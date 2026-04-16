---
name: evaluate-session
description: >
  Activates the inline evaluation pipeline for Contra Coach sessions. When this skill
  is active, every response from the creative-business-consultant agent is intercepted
  by the orchestrator, judged against pre-written evals, and either passed through or
  rewritten before the user sees it.

  Use this skill when: testing the coach agent's quality, running a coaching session
  with quality assurance enabled, or when the plugin maintainer wants to evaluate
  coach performance.

  Triggers on: "evaluate session", "enable quality gate", "run with evaluation",
  "test the coach", "enable eval pipeline", or any request to activate the
  evaluation system during a coaching session.
---

# Evaluate Session

This skill activates the inline evaluation pipeline for the current coaching session. When active, every coach response passes through the orchestrator → evaluator → skeptic flow before reaching the user.

## Activation

When this skill is invoked:

1. Read `contra-coach-knowledge/coach-feedback.md` if it exists — this provides context on the coach's recent performance patterns
2. Read `contra-coach-knowledge/eval-health.md` if it exists — this flags any evals currently under review
3. Confirm to the user: "Evaluation pipeline is active. Responses may take a moment longer while they're reviewed."
4. Hand off to the orchestrator agent for the remainder of the session

## What the Orchestrator Needs

The orchestrator manages the full pipeline as defined in `references/eval-protocol.md`. It needs access to:
- The coach agent's output (intercepted before delivery)
- The `evals/` directory (all skill-specific and behavioral evals)
- The evaluator, skeptic, and tiebreaker agents
- The `contra-coach-knowledge/` directory (for writing feedback and health logs)

## Deactivation

The pipeline stays active for the entire session once activated. It cannot be partially disabled. At session end, the orchestrator produces a session-end report if any defaults occurred.

## For the Plugin Maintainer

To review pipeline health:
- Check `contra-coach-knowledge/eval-health.md` for flagged evals and threshold metrics
- Check `contra-coach-knowledge/coach-feedback.md` for recurring patterns in judge feedback
- Use the eval-creator agent to revise any evals flagged as REVIEW NEEDED
