---
name: evaluate-session
description: >
  Activates the inline evaluation pipeline for Unknown Creatives Coach sessions. When active,
  every coaching response is evaluated against pre-written evals before the user
  sees it. Supports two modes: fork-based (primary, for interactive sessions) and
  named subagent fallback (for headless/SDK contexts).

  Use this skill when: testing the coach agent's quality, running a coaching session
  with quality assurance enabled, or when the plugin maintainer wants to evaluate
  coach performance.

  Triggers on: "evaluate session", "enable quality gate", "run with evaluation",
  "test the coach", "enable eval pipeline", or any request to activate the
  evaluation system during a coaching session.
---

# Evaluate Session

Activates the evaluation pipeline for the current coaching session. When active, every coaching response passes through the judge loop before reaching the user.

## Activation

When this skill is invoked:

1. Read `unknown-creatives-coach-knowledge/coach-feedback.md` if it exists — context on recent performance patterns.
2. Read `unknown-creatives-coach-knowledge/eval-health.md` if it exists — flags any evals under review.
3. Confirm to the user: "Evaluation pipeline is active. Responses may take a moment longer while they're reviewed."
4. Detect pipeline mode and begin evaluation on every subsequent coaching response.

## Pipeline Modes

### Fork-Based (Primary)

Use when running in an interactive Claude Code session where forks are available.

For each coaching response:
1. Generate the coaching response internally — do not deliver yet.
2. Fork: "Evaluate this coaching response against the eval pipeline. Read `agents/orchestrator.md` for the full pipeline instructions."
3. The fork inherits the full conversation context and consultant identity.
4. The fork follows the orchestrator pipeline: pre-filter evals → evaluator → skeptic → tiebreaker (if needed).
5. The fork returns: PASS with behavioral note, or FAIL with behavioral feedback.
6. On PASS: deliver the response. Write feedback to `unknown-creatives-coach-knowledge/coach-feedback.md`.
7. On FAIL: rewrite using the behavioral feedback (full context available — no re-serialization needed). Fork again to evaluate the rewrite. Maximum 2 attempts.

### Named Subagent Fallback

Use when forks are unavailable (headless mode, Agent SDK, older Claude Code versions).

For each coaching response:
1. Generate the coaching response.
2. Spawn the orchestrator as a named subagent, passing:
   - The coach's response
   - Active coaching skill name
   - User's most recent message
   - Key conversation points
   - Relevant knowledge file references
3. The orchestrator runs the judge pipeline and returns: PASS/FAIL + behavioral feedback.
4. On PASS: deliver the response.
5. On FAIL: rewrite and re-submit to the orchestrator. Maximum 2 attempts.

## Clean-Room Preservation

The coaching session (you, the consultant) never sees:
- Eval file names or content (loaded only inside the fork or orchestrator)
- Judge scores or reasoning
- Whether a fork or named subagent was used

You receive only behavioral feedback: "Your response asked two questions — ask one." Never: "You failed the one-question-at-a-time eval."

## Deactivation

The pipeline stays active for the entire session once activated. At session end, the orchestrator produces a session-end report if any defaults occurred.

## For the Plugin Maintainer

To review pipeline health:
- Check `unknown-creatives-coach-knowledge/eval-health.md` for flagged evals and threshold metrics
- Check `unknown-creatives-coach-knowledge/coach-feedback.md` for recurring patterns in judge feedback
- Use the eval-creator agent to revise any evals flagged as REVIEW NEEDED
