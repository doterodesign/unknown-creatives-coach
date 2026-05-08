# Eval: Output Quality and Fit Check

## Applies To
Voice skill — output generation, "sounds like me" gate, and fit check

## Criteria
- Output follows the voice.md structure defined in the skill (YAML metadata, stances, principles, vocabulary, filtering, modes)
- Output is specification-grade — structured enough for a content-generating tool to consume as frontmatter
- Output is also human-readable — someone could hand it to a collaborator
- The "sounds like me" gate is applied before finalizing
- Fit check evaluates against upstream knowledge files where they exist
- Cross-referencing rules are followed — references, never restates
- Vocabulary tables include rationale, not just word lists
- Filtering principle captures both keep and filter lists with explanations
- No content is generated that the user did not supply

## Pass Indicators
- YAML metadata block includes stable mode IDs and basis
- Stances read as short directives with explanations
- Principles include grounded examples where available
- Vocabulary tables have "Use these" and "Avoid these" with rationale columns
- Filtering lists explain why each pattern is kept or filtered
- Modes have all required dimensions (reader posture, rhythm, idea development, patterns, tone calibration)
- Coach asks "does this sound like your studio?" before finalizing
- Fit check references upstream files by name and section — doesn't restate their content
- Tensions between voice and upstream work are surfaced, not hidden

## Fail Indicators
- Output is a narrative summary rather than a structured specification
- Missing YAML metadata or stable mode IDs
- Vocabulary listed without rationale
- Filtering principle is vague ("filter out bad habits") rather than specific
- "Sounds like me" gate is skipped — output is presented as final without checking
- Fit check is skipped when upstream files exist
- Coach generates voice rules the user didn't supply — inventing rather than synthesizing
- Cross-referencing rules violated — content from positioning.md or services.md is restated in voice.md

## Weight
High — the output is the entire point of the skill. A voice specification that isn't structured enough to be consumed by downstream tools, or that doesn't feel like the user's studio, fails regardless of how good the coaching conversation was.
