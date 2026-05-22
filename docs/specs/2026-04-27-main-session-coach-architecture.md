# Main Session Coach Architecture

Move the Creative Business Consultant from a named subagent to a skill-based identity in the main Claude Code session. Keep the evaluation pipeline agents as subagents, with fork-based orchestration as the primary path and named subagent orchestration as fallback.

## Problem Statement

The current architecture spawns the consultant as a named subagent via the Agent tool. Each spawn is stateless — the agent re-reads its identity files (soul.md, personality.md, abilities.md, agent-protocol.md) on every message, including SendMessage continuations. This costs ~32KB / 8K tokens plus 4 tool round-trips per turn. Combined with knowledge folder reads, skill file reads, and memory reads, a single coaching turn loads ~55-65KB of context before processing the user's input.

Additional issues:
- **Conversational amnesia.** Each subagent spawn has no memory of prior coaching turns beyond what the main agent serializes into the prompt or what's persisted in the knowledge folder.
- **Triple-redundant instructions.** The same knowledge-folder and memory-protocol instructions appear in abilities.md, agent-protocol.md, and every SKILL.md.
- **Architecture rationale in runtime instructions.** agent-protocol.md is 11KB, but only ~3-4KB are actionable runtime rules. The rest is design documentation the agent doesn't need at runtime.
- **Eval pipeline cost multiplication.** With eval active, a single turn spawns 5-7 subagents, each building context from scratch. The orchestrator receives a lossy context summary rather than the full coaching conversation.

## Design

### 1. Coaching-Mode Skill

The consultant subagent entry point (`agents/creative-business-consultant.md`) is replaced by a skill (`skills/coaching-mode/SKILL.md`). When invoked, it transforms the main Claude Code session into the Creative Business Consultant.

#### Frontmatter

```yaml
---
name: coaching-mode
description: >
  Activates the Creative Business Consultant — a direct, experienced business
  coach for creative professionals. Use when the user wants help with business
  identity, positioning, ideal clients, services, voice, or turning any of those
  into client-facing documents. Also activates when the user provides past
  conversations for analysis.

  Triggers on: "help me with my business," "I can't explain what I do," "who
  should I be working with," "help me with positioning," "design my business,"
  or any request related to creative business identity and coaching.
---
```

#### Body

The skill body does four things in order:

1. **Read identity files.** Read `agents/creative-business-consultant/soul.md`, `personality.md`, and `abilities.md`. These are the consultant's working self — identity, voice, and capabilities. Read once per session; content persists in the main conversation.

2. **Read knowledge context.** Scan `unknown-creatives-coach-knowledge/` for existing discovery files (`business-identity.md`, `positioning.md`, `ideal-client-profile.md`, `life-design.md`, `voice.md`, `services.md`). Read `memory.md` and `coach-feedback.md` if they exist. Read `agents/shared/agent-protocol.md` for cross-agent rules.

3. **Set the coaching contract.** Instruct the main agent: "You are now the Creative Business Consultant. Your voice, behavior, and coaching approach are defined by the identity files you just read. Every response from this point forward is in the consultant's voice, delivered directly to the user. Do not announce the skill activation or narrate the identity loading."

4. **Route to the appropriate coaching skill.** Based on the user's intent, invoke the relevant skill (positioning, icp, design-your-business, design-your-life, analyze-conversations) or begin open-ended coaching if the user's need isn't skill-specific yet.

#### Identity files stay split

The soul, personality, and abilities files remain as separate files in `agents/creative-business-consultant/`. They are read once when the coaching-mode skill loads. After context compaction, the skill is re-injected and the agent re-reads the files. This trades a rare re-read (on compaction) for organizational clarity.

### 2. Eval Pipeline — Fork-Based with Named Subagent Fallback

The evaluation pipeline runs when `evaluate-session` is active. The main session (acting as the coach) generates a response, then runs it through the pipeline before delivering.

#### Fork-based flow (primary)

When `CLAUDE_CODE_FORK_SUBAGENT=1` is set and the environment is interactive:

1. Main session generates a coaching response internally (does not deliver yet).
2. Main session forks: "Evaluate this coaching response against the eval pipeline."
3. The fork inherits the full coaching conversation and consultant identity.
4. The fork identifies the active skill and loads only the relevant evals:
   - Behavioral evals that apply to "all exchanges" (from `evals/behavioral/`)
   - Thread-level evals for the active skill (from `evals/[skill-name]/`)
5. The fork spawns the evaluator as a named subagent, passing: coach response, conversation context, and the pre-filtered eval set.
6. The fork spawns the skeptic as a named subagent, passing: coach response, conversation context, eval set, and the evaluator's scores and reasoning.
7. The fork compares scores:
   - Within 10 points: averages them for the final score.
   - Diverge by >10 points: spawns the tiebreaker as a named subagent with both analyses anonymized.
8. The fork returns to the main session:
   - **PASS (≥92):** Behavioral note for `coach-feedback.md`.
   - **FAIL (<92):** Behavioral feedback describing what to fix.

On PASS: Main session delivers the response. Writes feedback to `coach-feedback.md`.

On FAIL: Main session rewrites using the behavioral feedback (full conversation context available — no re-serialization). Forks again to evaluate the rewrite. Maximum 2 attempts.

#### Named subagent fallback

When forks are unavailable (headless mode, Agent SDK, older Claude Code versions):

1. Main session generates a coaching response.
2. Main session spawns the orchestrator as a named subagent, passing: the coach response and a structured context summary (active skill, recent user message, key conversation points, relevant knowledge file references).
3. The orchestrator runs the same judge logic (evaluator → skeptic → tiebreaker if needed).
4. The orchestrator returns: PASS/FAIL + behavioral feedback.

The `evaluate-session` skill detects which mode is available and sets the pipeline accordingly.

#### Clean-room preservation

The main session (coach) never sees:
- Eval file names or content (loaded only inside the fork or orchestrator)
- Judge scores or reasoning
- Whether a fork or named subagent was used

The coach receives only behavioral feedback: "Your response asked two questions — ask one." Never: "You failed the one-question-at-a-time eval."

#### Eval pre-filtering

The orchestrator (or fork) identifies the active skill before passing evals to judges. Instead of loading all 16 eval files, it loads:
- All behavioral evals (~8 files, ~10KB)
- Thread-level evals for the active skill only (~2-4 files, ~3-5KB)

Judges receive only the evals that could apply to this exchange. This reduces eval loading from ~20KB to ~13-15KB per judge and eliminates wasted N/A reasoning.

### 3. Coaching Skills Deduplication

Every coaching skill currently contains a "Knowledge and Memory" section that repeats instructions already present in `abilities.md` and `agent-protocol.md`.

#### What gets removed from each SKILL.md

The following boilerplate sections are removed from all coaching skills (positioning, icp, design-your-business, design-your-life, analyze-conversations):

- "Before starting: check `unknown-creatives-coach-knowledge/` for existing files"
- "Read `unknown-creatives-coach-knowledge/memory.md` if it exists"
- "Follow the memory protocol in `references/memory-protocol.md`"
- "During the conversation: follow the memory protocol"
- Generic "ask before saving" instructions

#### What stays in each SKILL.md

- All coaching threads and thread-specific questions
- Output format and structure
- Coaching principles specific to this skill
- A one-line upstream reference declaring which files this skill builds on and writes to

#### Upstream reference format

Each skill replaces the boilerplate with a concise declaration:

```markdown
## Context
Builds on: business-identity.md, life-design.md
Output: positioning.md
```

The canonical knowledge-folder and memory-protocol instructions live in `abilities.md` and are read once when the coaching-mode skill loads. Skills focus purely on their coaching domain.

#### Estimated savings

| Skill | Before | After | Removed |
|---|---|---|---|
| positioning | 15.2KB | ~13.5KB | ~1.7KB |
| icp | 14.2KB | ~12.5KB | ~1.7KB |
| design-your-life | 14.2KB | ~12.8KB | ~1.4KB |
| design-your-business | 10.5KB | ~9.2KB | ~1.3KB |
| analyze-conversations | 10.1KB | ~8.8KB | ~1.3KB |
| **Total** | **64.2KB** | **56.8KB** | **~7.4KB** |

### 4. agent-protocol.md Compression

The shared protocol is compressed from ~11KB to ~3-4KB of actionable runtime rules.

#### What stays in agent-protocol.md

- **Output delivery rule.** Your response is the user-facing response, verbatim. The calling context must not summarize, paraphrase, or announce you. (~10 lines)
- **Eval pipeline blindness.** You never see evals. Feedback is behavioral only. Do not reference scores, evals, or the pipeline to the user. (~5 lines)
- **Memory tagging.** When writing to memory, tag entries with the skill context. (~3 lines)
- **Knowledge folder ownership.** The user owns `unknown-creatives-coach-knowledge/`. Ask before saving. Read before overwriting. (~3 lines)
- **Cross-agent handoff.** If the user's need is in another agent's domain, offer the handoff by name. No silent switching. (~5 lines)
- **When to break character.** Safety/crisis topics, user asks about the system, persistent system failure. (~5 lines)

#### What moves to docs/agent-architecture.md

- Agent anatomy explanation (file structure theory)
- Why output delivery matters (the justification behind the rule)
- Detailed eval pipeline integration mechanics (the coach stays blind anyway)
- The 10-point summary list (redundant with the compressed rules)
- "When to Stay Silent" catalog (covered by abilities.md)
- Architecture decisions and design rationale

## File Changes

### New files

| File | Purpose |
|---|---|
| `skills/coaching-mode/SKILL.md` | Replaces consultant subagent as entry point |
| `docs/agent-architecture.md` | Design rationale moved from agent-protocol.md |

### Removed files

| File | Reason |
|---|---|
| `agents/creative-business-consultant.md` | Replaced by coaching-mode skill |

### Modified files

| File | Change |
|---|---|
| `agents/creative-business-consultant/abilities.md` | Remove knowledge/memory boilerplate (~1.5KB) |
| `agents/shared/agent-protocol.md` | Compress to ~3-4KB actionable rules |
| `agents/orchestrator.md` | Rewrite: fork-aware, two modes (fork primary, named subagent fallback), no coach relay logic |
| `agents/evaluator.md` | Minor: accept pre-filtered evals |
| `agents/skeptic.md` | Minor: accept pre-filtered evals |
| `skills/evaluate-session/SKILL.md` | Rewrite: fork detection, updated activation flow |
| `skills/positioning/SKILL.md` | Remove knowledge/memory boilerplate, add upstream reference |
| `skills/icp/SKILL.md` | Same deduplication |
| `skills/design-your-business/SKILL.md` | Same deduplication |
| `skills/design-your-life/SKILL.md` | Same deduplication |
| `skills/analyze-conversations/SKILL.md` | Same deduplication |
| `README.md` | Reflect skill-based architecture |
| `.claude-plugin/plugin.json` | Version bump to 0.5.0 |

### Unchanged files

| File | Reason |
|---|---|
| `agents/creative-business-consultant/soul.md` | Identity content unchanged |
| `agents/creative-business-consultant/personality.md` | Identity content unchanged |
| `agents/tiebreaker.md` | Already clean |
| `agents/eval-creator.md` | Standalone tooling, not part of runtime |
| `references/eval-protocol.md` | Protocol unchanged |
| `references/memory-protocol.md` | Protocol unchanged |
| `evals/**` (16 files) | Eval content unchanged |

## Cost Impact

### Per coaching turn (no eval pipeline)

| Metric | Before | After |
|---|---|---|
| Subagent spawns | 1 per turn | 0 |
| Identity file reads | 4 per turn (32KB) | 3 once per session (21KB) |
| Knowledge folder reads | Every turn | Once per session |
| Active skill reads | Every turn | Once when skill activates |
| Per-turn context overhead | ~55-65KB | ~0KB (after initial load) |

### Per coaching turn (with eval pipeline)

| Metric | Before | After (fork) | After (fallback) |
|---|---|---|---|
| Subagent spawns | 5-7 | 1 fork + 2-3 subagents | 1 orchestrator + 2-3 subagents |
| Coach re-read on rewrite | Full ~55-65KB | 0 (main session) | 0 (main session) |
| Eval file loading per judge | ~20KB | ~13-15KB (pre-filtered) | ~13-15KB (pre-filtered) |
| Orchestrator context quality | Lossy summary | Full conversation (inherited) | Structured summary |

## Implementation Order

1. **Create `skills/coaching-mode/SKILL.md`** — the new entry point
2. **Create `docs/agent-architecture.md`** — move rationale from agent-protocol.md
3. **Compress `agents/shared/agent-protocol.md`** — keep only actionable rules
4. **Deduplicate `agents/creative-business-consultant/abilities.md`** — remove knowledge boilerplate
5. **Deduplicate all coaching skills** — remove boilerplate, add upstream references
6. **Rewrite `agents/orchestrator.md`** — fork-aware, two modes
7. **Update `agents/evaluator.md` and `agents/skeptic.md`** — pre-filtered eval acceptance
8. **Rewrite `skills/evaluate-session/SKILL.md`** — fork detection and updated flow
9. **Remove `agents/creative-business-consultant.md`** — old subagent entry point
10. **Update `README.md`** — reflect new architecture
11. **Bump `plugin.json` version** — 0.4.0 → 0.5.0
12. **Verify** — test coaching-mode skill activation, skill routing, eval pipeline (both modes)
