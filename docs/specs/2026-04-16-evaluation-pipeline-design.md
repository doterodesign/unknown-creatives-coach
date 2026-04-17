# Contra Coach Evaluation Pipeline — Design Spec

## Overview

An inline quality gate that intercepts every coach agent response, evaluates it against pre-written evals, and either passes it through or forces a rewrite. Judge feedback flows into the coach's memory to improve performance over time. The system degrades gracefully — if judging fails after 2 attempts, the coach's natural output reaches the user and eval health is tracked.

## Goals

1. Ensure every coach response meets a 92% quality threshold before the user sees it
2. Maintain clean-room separation — the coach never sees the evals, only behavioral feedback from the judges
3. Build a compounding feedback loop — judge feedback improves the coach over time through its memory system
4. Detect miscalibrated evals through health tracking rather than letting bad evals trap the system
5. Degrade gracefully — never block the user's experience

## Architecture

### Runtime Agents (3 new + Tiebreaker on-demand)

| Agent | Role | Receives | Does Not Receive |
|-------|------|----------|------------------|
| **Orchestrator** | Intercepts coach output, routes to judges, enforces the 2-attempt loop, compiles session-end report, writes to coach-feedback and eval-health | Everything — full pipeline visibility | N/A |
| **Evaluator** | Scores coach output against evals for the active skill + behavioral evals | Coach output, relevant evals | Skeptic's analysis |
| **Skeptic** | Challenges the evaluator's scoring, looks for weaknesses the evaluator missed | Coach output, relevant evals, evaluator's score and reasoning | How the evaluator weighted individual evals |
| **Tiebreaker** | Resolves when evaluator and skeptic diverge by >10 points | Both scores, both reasoning (anonymized — doesn't know which is evaluator vs. skeptic), coach output, evals | Which score came from which judge |

### Standalone Agent (1 new, not in runtime loop)

| Agent | Role | When It Runs |
|-------|------|-------------|
| **Eval Creator** | Writes thread-level and behavioral evals for each skill | Invoked manually by the plugin maintainer when building or updating skills. Writes to the `evals/` directory. Evals are pre-written and committed to the plugin — they are not generated at runtime. This agent exists for authoring and revising evals, not for inline evaluation. |

### Information Flow

```
User message arrives
        │
        ▼
Coach Agent generates response
        │
        ▼
Orchestrator intercepts (user sees "Reviewing...")
  - Identifies active skill
  - Pulls all evals for that skill + behavioral evals
  - Judges determine which evals are relevant to this exchange
        │
        ├──► Evaluator
        │      Input: coach output + evals
        │      Output: score (0-100) + reasoning per eval
        │
        ├──► Skeptic
        │      Input: coach output + evals + evaluator's score/reasoning
        │      Output: score (0-100) + counterarguments
        │
        ▼
Orchestrator compares scores:
  - Within 10 points → average the scores → final score
  - >10 points apart → Tiebreaker decides
        │
        ▼
Final score decision:

  ≥ 92% → PASS
    → Response delivered to user
    → Feedback summary written to coach-feedback.md

  < 92% → REWRITE (attempt 2)
    → Coach receives: specific feedback on what to fix
      (behavioral guidance, not the evals themselves)
    → New output re-judged through same flow
    → Pass → delivered + feedback to memory
    → Fail → DEFAULT to best-scored attempt of the two
           → Log to eval-health.md
           → Session-end report queued
```

### Cross-Stage Eval Pulling

The orchestrator does not try to detect which specific thread the coach is in. It pulls all evals for the active skill plus all behavioral evals and passes the full set to the judges. The judges determine which evals are relevant to the current exchange based on conversation context. This reflects how real coaching conversations flow — they don't follow clean thread boundaries.

## Eval Structure

### Thread-Level Evals (per skill, per thread)

These evaluate whether the coach's output meets the structural and content expectations for a given coaching thread within a skill.

```
evals/
├── positioning/
│   ├── positioning-statement.md
│   ├── differentiation.md
│   ├── proof.md
│   └── resistance-check.md
├── icp/
│   ├── best-clients.md
│   ├── client-profile.md
│   ├── anti-profile.md
│   └── qualification-framework.md
├── design-your-business/
│   ├── arena-scoping.md
│   └── energy-mapping.md
└── design-your-life/
    └── (threads defined when skill content is finalized)
```

### Behavioral Evals (cross-cutting, every exchange)

These evaluate coaching discipline regardless of which skill is active. They're binary or near-binary — the coach either followed the behavior or didn't.

```
evals/
└── behavioral/
    ├── one-question-at-a-time.md
    ├── never-answer-for-user.md
    ├── push-back-on-vague.md
    ├── summarize-and-reflect.md
    ├── peer-tone-not-teacher.md
    ├── direct-no-filler.md
    ├── check-knowledge-folder.md
    └── voice-integrity.md
```

### Eval File Format

Each eval file contains criteria the judges score against:

```markdown
# Eval: [Name]

## Applies To
[Skill/thread or "all exchanges"]

## Criteria
- [Specific, measurable criterion]
- [Specific, measurable criterion]

## Pass Indicators
[What good looks like for this eval]

## Fail Indicators
[What bad looks like for this eval]

## Weight
[How heavily this eval should factor into the overall score]
```

## Memory Architecture

### coach-feedback.md (new — judge feedback to coach)

Written by the orchestrator after each judged exchange. The coach reads this file at session start and uses it to improve its behavior. The coach never sees the underlying evals — only the behavioral feedback.

```markdown
## 2026-04-16 — Positioning session

### Exchange 3 — Positioning Statement thread
- Score: 94/100
- Feedback: Strong pushback on vague language. One note: when
  transitioning from the user's first attempt to sharpening
  questions, two questions were asked in sequence. Splitting
  them would give the user more space to think.

### Exchange 7 — Differentiation thread
- Score: 88/100 → Rewrite triggered
- Feedback: The coach suggested a differentiation angle before
  the user had a chance to articulate their own. Rewrite let
  the user lead. Passed on attempt 2 at 93/100.

### Pattern (cumulative)
- Recurring: tendency to bundle questions when transitioning
  between threads. Single-question discipline holds within threads.
- Improving: earlier sessions showed coach generating positioning
  language for the user. Now consistently letting user lead.
```

### eval-health.md (new — eval reliability tracking)

Written by the orchestrator when the 2-attempt loop exhausts without passing, or when the tiebreaker is called repeatedly for the same eval.

```markdown
## Flagged Evals

### behavioral/summarize-and-reflect.md
- Default count: 4 sessions
- Tiebreaker invocations: 3
- Pattern: Eval expects explicit summary after every 3 exchanges.
  Coach summarizes at natural conversational breaks instead.
  Both judges consistently disagree on whether implicit
  summarization counts.
- Status: REVIEW NEEDED — eval may be too rigid

### positioning/resistance-check.md
- Default count: 1 session
- Pattern: User didn't resist, so coach didn't coach resistance.
  Eval scored low because the thread wasn't covered. May need
  a "not applicable" condition.
- Status: MONITOR
```

### Threshold Calibration Signal

Track the rewrite rate in eval-health.md. If more than ~30% of responses hit the rewrite loop and pass on the second attempt with minor changes, the 92% threshold may be too tight. This data informs whether to adjust to 88-90%. The threshold is a tunable parameter, not a permanent decision.

```markdown
## Threshold Health

### Current: 92%
- Sessions evaluated: 12
- Total exchanges judged: 87
- Rewrite rate: 22% (19/87)
- Default rate: 5% (4/87)
- Second-attempt pass rate: 79% (15/19 rewrites passed)
- Assessment: Threshold is performing within acceptable range.
  Monitor if rewrite rate exceeds 30%.
```

## Session-End Report

When any defaults occurred during the session, the orchestrator presents a brief, non-alarming summary:

> "Quick note: during this session, [N] of my responses went through a quality review that couldn't fully resolve. I used my best judgment for those. The details have been logged — you can review them in `contra-coach-knowledge/eval-health.md` if you'd like to see what was flagged."

When no defaults occurred, no report is shown. The system is invisible when it's working well.

## UX

- The user sees "Reviewing..." briefly while the orchestrator runs the judges. No play-by-play, no scores, no judge commentary visible to the user.
- The coach's response appears as normal once it passes.
- On rewrite, the user sees a slightly longer "Reviewing..." pause but no indication that a rewrite occurred.
- The session-end report only appears when defaults happened.

## Plugin File Structure (updated)

```
contra-coach/
├── .claude-plugin/marketplace.json
└── plugins/contra-coach/
    ├── .claude-plugin/plugin.json
    ├── agents/
    │   ├── creative-business-consultant.md    (existing — the coach)
    │   ├── orchestrator.md                    (new)
    │   ├── eval-creator.md                    (new — standalone tool)
    │   ├── evaluator.md                       (new)
    │   ├── skeptic.md                         (new)
    │   └── tiebreaker.md                      (new)
    ├── skills/
    │   ├── design-your-business/              (existing)
    │   ├── design-your-life/                  (existing)
    │   ├── positioning/                       (existing)
    │   ├── icp/                               (existing)
    │   └── evaluate-session/                  (new — orchestration skill)
    ├── evals/                                 (new — pre-written by eval creator)
    │   ├── positioning/
    │   │   ├── positioning-statement.md
    │   │   ├── differentiation.md
    │   │   ├── proof.md
    │   │   └── resistance-check.md
    │   ├── icp/
    │   │   ├── best-clients.md
    │   │   ├── client-profile.md
    │   │   ├── anti-profile.md
    │   │   └── qualification-framework.md
    │   ├── design-your-business/
    │   │   ├── arena-scoping.md
    │   │   └── energy-mapping.md
    │   ├── design-your-life/
    │   │   └── (TBD)
    │   └── behavioral/
    │       ├── one-question-at-a-time.md
    │       ├── never-answer-for-user.md
    │       ├── push-back-on-vague.md
    │       ├── summarize-and-reflect.md
    │       ├── peer-tone-not-teacher.md
    │       ├── direct-no-filler.md
    │       ├── check-knowledge-folder.md
    │       └── voice-integrity.md
    └── references/
        ├── memory-protocol.md                 (existing)
        └── eval-protocol.md                   (new — how judges operate)
```

## What This Spec Does NOT Cover (future work)

- **Demo personas for manual testing** — the original request that led here. Once the pipeline is built, we can create personas that exercise specific skills and edge cases.
- **Eval content** — the actual criteria inside each eval file. The eval creator agent will generate these based on the skill definitions.
- **Services and Voice skills** — referenced in the coach agent but not yet built. Evals for these will be added when the skills are created.
- **Document builder skills** — referenced in the coach agent (questionnaire, pitch, outbound, inbound). These will need their own eval structure.
- **Contra MCP integration** — mentioned in the coach agent as a future capability.
