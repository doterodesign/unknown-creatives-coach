# Judges stay separate agents; only role-agnostic mechanics are shared

The evaluation pipeline's judges (evaluator, skeptic, tiebreaker) look like near-duplicate files, and architecture reviews will keep suggesting merging them or collapsing the tiebreaker into the orchestrator. We decided against both: each judge is a separate agent because the fresh context window *is* the isolation mechanism — the orchestrator creates the "Analysis A/B" anonymization labels, so it can never judge them blind, and a merged judge would anchor on scores it already saw. What judges may share is the role-agnostic protocol in `references/eval-protocol.md` (threshold, loop, disagreement definition, pre-filter), which every judge already reads — clean-room is enforced at dispatch time by what the orchestrator sends and withholds, not by keeping static files ignorant of each other.

## Consequences

The following must stay role-private — moving any of it into the shared protocol re-opens gaming incentives:

- `skeptic.md`: the challenge-generous-scores-more-aggressively bias, and the form-your-own-assessment-first process. (If the evaluator learned the audit's bias, it could pre-deflate scores to dodge challenges.)
- `tiebreaker.md`: the never-average / no-compromise principles. (If the other judges knew ties aren't split, exaggerating divergence becomes a strategy.)
- `orchestrator.md`: the within-10-points averaging rule and the full dispatch send/withhold matrix.

`coach-feedback.md` is read by the coach, so it must never contain scores — score tracking lives in `eval-health.md`, which coaching agents never read.

## Considered Options

- **Merge the tiebreaker into the orchestrator** ("it's only ~10 unique lines") — rejected: destroys anonymization by construction, and in fork mode would put tiebreak judgment inside the coach's own context.
- **Split eval-protocol.md into per-role sheets** for true static secrecy — rejected: the protocol already discloses the pipeline shape to every judge, and the isolation that matters is runtime data withholding, not static-file ignorance.
