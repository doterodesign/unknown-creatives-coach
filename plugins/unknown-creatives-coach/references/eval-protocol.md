# Eval Protocol

This document defines how the Unknown Creatives Coach evaluation pipeline operates. The evaluator, skeptic, tiebreaker, and orchestrator agents all reference this protocol as their source of truth. Each agent can operate independently by reading this document.

## System Purpose

The evaluation pipeline is an inline quality gate. It runs automatically after the coach generates output, before that output reaches the user.

Three principles govern the system:

**Inline gate** — Evaluation happens in the same session, not as a post-hoc review. Output only reaches the user if it clears the threshold or has exhausted its attempts.

**Clean-room separation** — The coach and the judges operate in separate contexts. The coach generates output without knowledge of how it will be evaluated. The judges evaluate without coaching the user. The two roles never bleed into each other.

**Coach never sees evals** — Judge scores, reasoning, and disagreements are never fed back to the coach in evaluative form. Feedback to the coach is behavioral only (see Feedback to Coach section). This prevents the coach from gaming the rubric rather than improving its output.

## Eval Pre-Filtering

Judges never receive the full eval library. The orchestrator identifies the active coaching skill and passes only:

- All behavioral evals from `evals/behavioral/`
- Thread-level evals for the active skill from `evals/[skill-name]/`

Evals for other skills are excluded. If no `evals/[skill-name]/` directory exists for the active skill, the pipeline proceeds on behavioral evals alone and notes the gap in `eval-health.md` — degraded coverage must be visible, not silent.

## Scoring

Judges score on a 0–100 scale.

Each judge scores only the evals that are relevant to the output type. A judge does not score an eval if that eval does not apply to the content being reviewed.

The final score is a weighted average across all relevant evals. Weights are defined per eval in the individual eval files. If an eval does not specify a weight, it is treated as equal weight with the others.

Scores are integers. No decimals.

## Score Interpretation

| Range | Meaning |
|-------|---------|
| 92–100 | Strong — output meets the bar |
| 80–91 | Acceptable with weaknesses — passes only if below threshold after max attempts |
| 60–79 | Significant issues — needs rewrite |
| Below 60 | Fundamental failure — rewrite required |

## Pass Threshold

The current pass threshold is **92**.

This is a tunable parameter. If rewrite rates are consistently high (output rarely passes on the first attempt), consider whether the threshold is calibrated correctly or whether the coach instructions need updating. Track rewrite rates in `eval-health.md` to inform this decision.

A score of 92 or above on the weighted average passes. A score below 92 triggers a rewrite request on the first attempt, or defaults to best-scored on the second attempt.

## The 2-Attempt Loop

The evaluation pipeline runs as follows:

1. **Coach generates output** — The coach produces a response based on user input and its instructions.
2. **Judges evaluate** — Each judge scores the output against relevant evals and produces a weighted score.
3. **Pass or rewrite** — If the weighted average meets the threshold, the output is delivered to the user. If it does not, the coach receives behavioral feedback and generates a second attempt.
4. **Max 2 attempts** — The loop runs at most twice. There is no third attempt.
5. **Default to best-scored on failure** — If neither attempt passes, the output with the higher weighted average score is delivered to the user. This is a default, not a pass. The session-end report notes it.

The user never waits through more than two generation cycles.

## Judge Disagreement

Disagreement is defined as a divergence of more than 10 points between the evaluator's and skeptic's overall weighted scores.

When disagreement exceeds this threshold:

1. **Tiebreaker is invoked** — The tiebreaker agent forms its own independent judgment of the output against the evals.
2. **Analyses are anonymized** — The tiebreaker does not know which judge produced which analysis. It receives the output, the evals, and both analyses labeled without attribution.
3. **Tiebreaker score is final** — The tiebreaker's overall score is the final score for the exchange.
4. **Persistent disagreement is logged** — If tiebreaker invocations driven by the same eval recur across sessions, this is logged to `eval-health.md` as a signal that the eval rubric may need clarification.

## Feedback to Coach

After every judged exchange — pass or rewrite — feedback is written to `coach-feedback.md`.

Feedback is **behavioral, not evaluative**. It describes what the output did or did not do — never how it scored. It never references evals by name, scores by number, or judges by role. The coach reads `coach-feedback.md` at session start, so scores must never appear in it — score tracking belongs in `eval-health.md`, which the coach never reads.

Good feedback (behavioral):
- "The response used generic language where specificity was needed. Rewrite to ground the advice in the user's actual situation."
- "The output offered multiple options when the user asked for a recommendation. Rewrite to give a clear answer."

Bad feedback (evaluative — do not write this):
- "The specificity eval scored 61. The directness eval scored 58."
- "The skeptic judge flagged this as generic."
- "You failed the voice eval."

The coach reads the feedback and generates a second attempt without knowing it was evaluated.

## Eval Health

`eval-health.md` is the system's self-monitoring file. It tracks:

- **Defaults** — Each time the pipeline defaults to best-scored (neither attempt passed), log the date, skill context, and the highest score achieved.
- **Tiebreaker invocations** — Each time a tiebreaker is called, log the eval name and the two diverging scores (without judge attribution).
- **Rewrite rates** — Track what percentage of outputs require a second attempt. A high rewrite rate (above ~30%) is a signal to review coach instructions or threshold calibration.

`eval-health.md` is not shown to the user. It is a diagnostic tool for reviewing and improving the evaluation system over time.

## Session-End Report

If any defaults occurred during the session, two things happen at session end:

1. The user gets a one-sentence note in chat — that some responses went through a review that couldn't fully resolve, with a pointer to `eval-health.md`. One sentence, then move on.
2. The full detail — which skill was running, that the output was a default (best-scored, not passing), and the score achieved — is recorded in `eval-health.md`.

When the system works correctly — no defaults, no tiebreakers — the session-end report is invisible. Nothing is said, nothing is appended. The pipeline runs silently.

## UX

From the user's perspective:

- The coach generates a response.
- A brief "Reviewing..." indicator appears while evaluation runs.
- The response is delivered.

The user never sees:
- Scores or scoring breakdowns
- Judge names or roles
- Pipeline mechanics or attempt counts
- Whether a rewrite occurred
- Whether a default was used (visible only in post-session logs, not during the session)

The evaluation system is infrastructure. It improves output quality without surfacing complexity to the user.
