# Changelog

All notable changes to this project are documented in this file.

## [0.8.0] - 2026-07-14

Architecture hardening release: single-sourced judge mechanics, tightened skill interfaces, and closed eval-coverage gaps. Based on an adversarially verified architecture review.

### Added
- ADR record: `docs/adr/0001-clean-room-judge-separation.md` — why judges stay separate agents, what may be shared, and what must stay role-private
- Thread-level evals for design-your-life (values-discovery, working-style, life-support, thread-tension)
- Behavior-level evals for analyze-conversations (hypothesis-framing, observation-integrity, coaching-handoff)
- Knowledge File Registry in `agent-protocol.md` — single authority for knowledge filenames and memory context tags
- Positioning reference catalogs (`references/positioning/differentiation-types.md`, `resistance-patterns.md`), loaded selectively per stage like the services references
- Identity check in all seven coaching skills — standalone invocation (without coaching-mode) now loads the consultant persona and protocol

### Changed
- `eval-protocol.md` is now the single source of truth for judge mechanics (pass threshold, 2-attempt loop, disagreement definition, eval pre-filtering); orchestrator, judge agents, and architecture docs point to it instead of restating
- Resolved pipeline drift between protocol and orchestrator: tiebreak triggers on overall weighted scores; behavioral feedback is written after every judged exchange; session-end defaults produce a one-line chat note plus full detail in `eval-health.md`
- Skill frontmatter descriptions rewritten as triggers-only and deduplicated (down from up to 1,093 chars to 461–674); the "what makes me different" trigger collision between positioning and design-your-business resolved
- Discovery skill Format blocks now name their target knowledge files and follow the knowledge-folder rules
- Document-builder routes marked as planned (not yet installed) with an explicit ad-hoc-draft fallback
- `memory-protocol.md` wired into runtime via agent-protocol's memory-tagging rule

### Fixed
- Clean-room leak: the orchestrator wrote scores into `coach-feedback.md`, a file the coach reads at session start — score tracking now lives only in `eval-health.md`
- Orchestrator never referenced `eval-protocol.md` despite the protocol claiming it did
- Memory context-tag drift (6 tags in agent-protocol vs 8 in memory-protocol) — canonical 8-tag list now in one place
- Missing eval directory for the active skill is now logged to `eval-health.md` instead of silently degrading coverage
- Stale "voice (coming soon)" in the plugin README and "voice skill (when available)" in analyze-conversations

## [0.7.0] - 2026-05-07

### Added
- **Voice skill** — discover and define the studio's communication voice (stances, principles, modes, vocabulary, filtering) with emergent discovery and three coaching lenses (Discoverer, Extractor, Refiner); outputs `voice.md` as a portable, machine-consumable spec
- 4 voice evals (discovery, stances-principles, modes, output)
- Voice routing in coaching-mode and design spec/implementation plan

### Changed
- CONTRIBUTING roadmap updated — services and voice moved off the roadmap into shipped skills

## [0.6.0] - 2026-05-07

### Added
- **Services skill** — define offerings, delivery, and the service ecosystem through three coaching lenses (Builder, Sharpener, Restructurer) and four stages (Inventory, Architecture, Delivery, Fit Check); outputs `services.md`
- `references/services/` reference library (ecosystem-types, engagement-models, delivery-patterns, scope-patterns) loaded selectively per stage
- 4 services evals (inventory, architecture, delivery, fit-check)

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
