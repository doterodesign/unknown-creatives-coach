# Agent Architecture

Design documentation for the Unknown Creatives Coach plugin agent system. This file explains the "why" behind the operational rules in `agents/shared/agent-protocol.md`. Agents read the protocol; maintainers read this.

## Agent Anatomy

Every user-facing agent in this plugin follows the same identity structure:

```
agents/
└── [agent-name]/
    ├── soul.md          ← identity, purpose, philosophy
    ├── personality.md   ← voice, tone, conversation rules
    └── abilities.md     ← skills, tools, knowledge, routing
```

Identity files are not documentation — they are the agent's working self. The soul defines why it acts; the personality defines how; the abilities define with what.

**Pipeline agents** (orchestrator, evaluator, skeptic, tiebreaker) and **standalone tooling agents** (eval-creator) use a single-file pattern. They do not have a user-facing voice and do not need the soul/personality layer.

## Coaching-Mode Skill Architecture

The consultant was originally a named subagent spawned via the Agent tool. Each spawn was stateless — the agent re-read identity files on every message. This cost ~32KB / 8K tokens plus 4 tool round-trips per turn.

The coaching-mode skill replaces this by transforming the main Claude Code session into the consultant. Identity files are read once per session. Knowledge context is loaded once. The coach persists in the main conversation, retaining full coaching history without serialization.

### Why a skill, not a subagent

- **Context persistence.** The main session retains the full coaching conversation. No serialization loss between turns.
- **Single identity load.** Identity files read once, not every turn. Saves ~32KB per turn after the first.
- **Knowledge folder reads once.** Existing discovery files scanned once at session start.
- **Eval pipeline benefits.** The fork inherits the full coaching context when running evaluation, eliminating lossy context summaries.

## Why Output Delivery Matters

The thing that makes an agent feel real is that the user hears *its* voice, not a summary of its voice. If a reflection is paraphrased away, the user loses the crystallization. If a question is re-framed, the user answers a different question. The coaching contract breaks.

With the skill-based architecture, the main session *becomes* the consultant — there is no relay layer. The output delivery rule still matters because:

1. System prompt instructions or hooks might add wrappers or summaries.
2. The discipline of writing for direct delivery improves output quality.
3. If the agent is ever re-separated into a subagent, the rule must hold.

### How agents honor direct delivery

Write every response as if it is being delivered directly to the user. No internal notes, meta-commentary, or process narration. No third-person self-references. No cover notes.

Do not say: "I will now ask the user..." — just ask. "The user should consider..." — talk directly. "Here is my response:" — there is no wrapper.

### Caller-side enforcement

If the agent detects its output is being summarized (e.g., the user's next message suggests they saw only a question when a reflection + question was provided), surface it: "I want to check — are you seeing my full responses, or just the questions? The reflections are part of the work, not prelude to it."

## Evaluation Pipeline Architecture

The evaluation pipeline runs when the `evaluate-session` skill is active. It ensures coaching quality through a judge loop while preserving clean-room separation.

### Pipeline Modes

**Fork-based (primary).** In interactive Claude Code sessions, the main session (acting as the coach) generates a response, then forks. The fork inherits the full conversation context and runs the judge loop. This eliminates context loss from serialization.

**Named subagent fallback.** In headless or SDK contexts where forks are unavailable, the orchestrator runs as a named subagent. It receives a structured context summary rather than the full conversation. Some context fidelity is lost, but the pipeline mechanics are identical.

### Judge Loop

1. **Orchestrator** pre-filters evals to behavioral + active skill only.
2. **Evaluator** scores independently against the pre-filtered eval set.
3. **Skeptic** challenges the evaluator's scores (adversarial second opinion).
4. **Tiebreaker** resolves disagreements >10 points (invoked on demand, receives anonymized analyses).

### Clean-Room Separation

- The coach never sees evals — feedback is behavioral only.
- The evaluator never sees the skeptic's output.
- The skeptic sees evaluator scores but not weighting methodology.
- The tiebreaker doesn't know which analysis came from which judge.
- The user sees only the final coaching response.

### Eval Pre-Filtering

Instead of loading all eval files, the orchestrator identifies the active skill and loads only:
- All behavioral evals (~8 files from `evals/behavioral/`)
- Thread-level evals for the active skill only (~2-4 files from `evals/[skill-name]/`)

This reduces eval loading per judge from ~20KB to ~13-15KB and eliminates wasted N/A reasoning.

## When to Stay Silent

Agent behaviors that should be invisible to the user:

- Reading identity files, memory, or the knowledge folder
- Checking for existing discovery work
- Invoking skills internally
- Writing to memory or knowledge files (unless the user asked about persistence)
- Any evaluation pipeline activity
- Cross-referencing workspace files

If these processes shape the response, they do so invisibly. The user sees the result, not the mechanics.

## Design Principles

1. **The user hears one voice.** Not a narrator describing an agent. The voice itself.
2. **Identity lives in files, not in prompts.** Soul, personality, and abilities are maintained as separate files that can be versioned and evolved independently.
3. **The evaluation system is invisible.** When working correctly, the user has no idea it exists. When it defaults, it explains in one sentence and moves on.
4. **Memory is shared, context is not.** All agents write to the same memory file, but each agent maintains its own conversation context.
5. **Skills focus on coaching, not plumbing.** Individual coaching skills define threads, questions, and output formats. Knowledge-folder and memory protocols are loaded once by the coaching-mode skill.
