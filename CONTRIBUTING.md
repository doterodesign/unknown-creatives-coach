# Contributing

Thanks for your interest in contributing to Unknown Creatives Coach.

## How to Contribute

1. Fork the repository
2. Create a branch for your change
3. Make your changes
4. Open a pull request against `master`

## What We're Looking For

### New Discovery Skills

The `services` and `voice` skills are next on the roadmap. A good discovery skill:
- Guides a structured conversation through 2-3 threads
- Asks one question at a time (never stacks multiple questions)
- Produces a clear output format saved to the knowledge folder
- Builds on existing knowledge files when available

Look at `plugins/unknown-creatives-coach/skills/design-your-business/SKILL.md` for the pattern.

### Document Builder Skills

Skills that turn discovery outputs into client-facing documents (intake questionnaires, pitch documents, outbound messaging, inbound templates). These should pull from existing knowledge files and never generate content the user hasn't supplied.

### Evaluation Criteria

New evals that improve coaching quality. A good eval:
- Tests one specific behavior
- Has clear pass/fail criteria
- Includes example passing and failing responses
- Lives in `evals/behavioral/` (cross-cutting) or `evals/[skill-name]/` (thread-specific)

Look at `plugins/unknown-creatives-coach/evals/behavioral/one-question-at-a-time.md` for the pattern.

### Bug Reports

If the coach gives generic advice, accepts vague answers without pushing back, or generates content the user didn't supply — that's a bug. Open an issue with:
- What you said to the coach
- What the coach responded with
- What you expected instead

## Architecture Guidelines

- **Skills are markdown.** No code, no build step. A skill is a SKILL.md file with a frontmatter description and coaching instructions.
- **Agents follow the identity structure.** User-facing agents get soul/personality/abilities. Pipeline agents get a single file.
- **The knowledge folder is the user's.** Always ask before saving. Read before overwriting. Never rename files.
- **Memory is a scratchpad, not a log.** Capture corrections, preferences, and self-discoveries. Consolidate and prune regularly.

## Style

- Write in direct, plain language
- No corporate jargon (leverage, optimize, streamline, synergize)
- No filler phrases ("that's a great question," "it's important to note that")
- Skills and agents are written as instructions to Claude, not documentation for humans

## License

By contributing, you agree that your contributions will be licensed under the same [Polyform NonCommercial 1.0.0](LICENSE.md) license as the rest of the project.
