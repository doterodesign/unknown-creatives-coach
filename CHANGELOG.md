# Changelog

All notable changes to this project are documented in this file.

## [0.5.1] - 2026-05-07

### Changed
- Renamed project from "Contra Coach" to "Unknown Creatives Coach" across all files, directories, and references
- Knowledge folder renamed from `contra-coach-knowledge/` to `unknown-creatives-coach-knowledge/`

### Added
- Project-level README with installation instructions, skill overview, best practices, and architecture documentation
- Privacy and data storage section explaining where user data lives
- Polyform NonCommercial 1.0.0 license (replaces MIT)
- .gitignore, CHANGELOG, CONTRIBUTING, CODE_OF_CONDUCT, and GitHub templates

## [0.5.0] - 2026-04-27

### Changed
- **Main session coach architecture.** The consultant is no longer a subagent — the main Claude Code session becomes the coach via the `coaching-mode` skill. Identity files are read once per session instead of every turn.
- Rewrote `agent-protocol.md` to compressed, actionable runtime rules
- Removed Memory and Coach Feedback boilerplate from `abilities.md` (now handled by `coaching-mode` skill)
- Deduplicated Knowledge and Memory boilerplate from all individual coaching skills
- Rewrote orchestrator for fork-aware dual-mode pipeline (fork-based in interactive sessions, named subagent fallback in headless/SDK)
- Rewrote `evaluate-session` skill with fork detection and dual-mode flow
- Updated evaluator, skeptic, and tiebreaker to acknowledge pre-filtered evals

### Added
- `coaching-mode` skill — new entry point that transforms the main session into the consultant
- Agent architecture documentation (`docs/agent-architecture.md`)
- Main session coach architecture spec

### Removed
- Consultant subagent entry point (replaced by coaching-mode skill)

## [0.4.0] - 2026-04-17

### Changed
- Split consultant agent from single file into soul/personality/abilities identity structure

### Added
- `voice-integrity` behavioral eval

## [0.3.0] - 2026-04-16

### Added
- `analyze-conversations` skill — reads past prospect/client conversations and surfaces patterns for evidence-based coaching
- Routing logic for conversation materials in coach agent
- Design spec and implementation plan for analyze-conversations

## [0.2.0] - 2026-04-16

### Added
- **Evaluation pipeline** — inline quality assurance system with evaluator, skeptic, tiebreaker, and orchestrator agents
- `evaluate-session` skill — activates the eval pipeline for a coaching session
- `eval-creator` agent — standalone tool for authoring and revising evals
- 7 behavioral evals (one-question-at-a-time, never-answer-for-user, push-back-on-vague, summarize-and-reflect, peer-tone-not-teacher, direct-no-filler, check-knowledge-folder)
- 2 positioning evals (proof, resistance-check)
- 4 ICP evals (best-clients, client-profile, anti-profile, qualification-framework)
- 2 design-your-business evals (arena-scoping, energy-mapping)
- Eval protocol reference documentation
- Coach reads feedback memory for behavioral calibration

## [0.1.0] - 2026-04-16

### Added
- Initial release — Creative Business Consultant agent with discovery skills
- `design-your-business` skill — explore professional identity, arena, and strengths
- `design-your-life` skill — explore values, working style, and life design
- `positioning` skill — define market positioning and differentiation
- `icp` skill — identify ideal client profile
- Memory protocol for cross-session learning
- Knowledge folder system for structured discovery outputs
- Marketplace structure for CLI and Cowork compatibility
