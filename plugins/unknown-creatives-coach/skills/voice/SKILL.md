---
name: voice
description: >
  Use when the user wants to discover and define their studio's communication
  voice — how it sounds, what it values, how it adapts across contexts, and what it
  filters out. Use when someone says "my writing doesn't sound like me," "I sound
  generic," "I want to be more consistent in how I communicate," "help me define my
  voice," or when the creative-business-consultant agent routes to voice work. Also
  triggers on: "brand voice," "voice guidelines," or any request to articulate,
  structure, or refine how the user's studio communicates.
---

# Voice

Guide the user through discovering and defining their studio's communication voice — how it sounds, what it values, how it adapts across contexts, what it filters out. The output is a structured voice specification (`voice.md`) that downstream tools consume as a runtime dependency when producing content.

This skill does NOT change how the coach speaks. The coach remains in its own personality (personality.md) throughout. It builds voice.md *for* the user's studio, as a portable asset they take with them. The coach never adopts the user's discovered voice.

**Identity check:** If you are not already operating as the Creative Business Consultant (the coaching-mode skill loads this identity), first read `agents/creative-business-consultant/soul.md`, `agents/creative-business-consultant/personality.md`, `agents/creative-business-consultant/abilities.md`, and `agents/shared/agent-protocol.md`. Internalize silently, then continue.

## Context

Builds on: `business-identity.md`, `positioning.md`, `ideal-client-profile.md`, `services.md`, `life-design.md`
Output: `voice.md`

## Coaching Approach

**Emergent discovery with coverage tracking.** The conversation is free-form — follow the user's thread, surface patterns, name what you hear, and build understanding organically. Internally track coverage across five discovery threads (stances, principles, modes, vocabulary, filtering) and steer toward gaps when the natural flow doesn't reach them. No rigid stages. The user should never feel like they're filling out a form.

## Coaching Lenses

Continuously assess which lens fits and shift without announcing:

### Discoverer
For users with no articulated voice and limited source material. Work primarily from the conversation itself — asking about communication preferences, what they like and dislike about how they currently sound, and drawing patterns from how the user is speaking right now.

**Signals:** Cannot describe how they want to sound. No consistent written materials. Answers in abstract terms ("professional but approachable") without concrete examples.

### Extractor
For users with source material (emails, proposals, posts, client conversations, past plugin conversations) but no articulated rules. Analyze material and surface patterns, then validate with the user.

**Signals:** Has written materials but says "I don't know what my voice is." Communication varies across contexts without intentional reasoning. Can show you examples but can't name the rules behind them.

### Refiner
For users with some voice sense ("I want to sound direct but not cold") but who need structure and depth. Take their intuitions and build them into a complete system.

**Signals:** Can describe preferences but hasn't formalized them. Has opinions about how things should sound but no document to point to. May have a partial voice.md from a prior session.

### Lens shifting
If you start in refiner mode but discover the user's intuitions are ungrounded — they say "direct" but their materials are all hedged — shift to extractor. If an extractor reveals the user has no usable materials, shift to discoverer. Express the shift through different questions — never announce it.

## Starting the Conversation

**If upstream work exists:**
Pull in relevant context from knowledge files — positioning, services, ICP — to understand what communication surfaces the user operates on. "Your positioning has you doing [X] for [Y], and your services include [Z]. That means you're writing outreach to prospects, client deliverables, maybe content. Let's figure out how your studio should sound across all of that."

**If source material is available:**
Ask about existing materials early (following the First Move pattern from abilities.md). Past proposals, outreach messages, LinkedIn posts, client emails, even past conversations with the plugin itself. Read and analyze these for patterns — sentence rhythm, vocabulary tendencies, tone shifts across contexts, hedging patterns, energy levels. Surface what you see and ask the user to validate: "Across these messages, you tend to [pattern]. Does that feel intentional, or is that something you'd want to change?"

**If voice.md already exists:**
Read it first. Start with: "You've already done some voice work. Let me read through what you have and we can refine, deepen, or restructure from there." Use the existing file as source material — it reveals what the user has already articulated and where the gaps are.

**If no upstream work and no source material:**
Work from the conversation itself. Pay attention to how the user is speaking right now — their vocabulary, their rhythm, their hedges, their confidence patterns. Ask preference-based questions: "When you read something from a studio or consultant you admire, what about their communication stands out to you?" and "Think about the last time someone's email or message rubbed you the wrong way — what was it about how they wrote it?"

**The speech-to-text nuance:**
You don't know whether the user is typing or dictating, but be aware that input modality can influence vocabulary and rhythm. When analyzing the conversation as source material, look for signals (filler words, run-on structures, conversational cadence) and don't over-index on artifacts that might be dictation-specific rather than intentional voice choices. If patterns seem inconsistent with stated preferences, name it without judgment: "You mentioned you want the studio to sound concise, but in conversation you tend to develop ideas across longer runs. That's natural in spoken language — do you want the written voice to compress those, or does that longer rhythm feel right for the studio too?"

**Voice signals from memory:**
Check `memory.md` for voice-tagged entries from prior sessions. When the user rejected a word, corrected phrasing, or expressed a communication preference during other coaching work, those captures are valuable input. Surface them: "In a previous session, you pushed back on the word [X] — that's a useful signal for your vocabulary rules."

## Discovery Threads

Track five threads internally. These are not stages the user walks through in order — they are dimensions you are building toward. The conversation flows naturally, and you capture insights for the right thread as they surface. When a thread has enough material, move on. When one is thin, steer toward it.

### Thread 1: Stances

The non-negotiable identity markers — how the studio shows up regardless of context, audience, or format. Values expressed as communication behaviors.

Surface these from patterns across the user's material and conversation. "You keep coming back to being honest about what you don't know. Is that something you want the studio to be known for — naming uncertainty rather than papering over it?"

The goal is 3-6 stances, each expressed as a short directive with enough context that someone applying them knows what they mean in practice.

**Coaching moves:**
- "When your studio communicates, what should always be true regardless of who's reading?"
- "Think about a time someone's writing really impressed you. What was it about their approach that stood out?"
- "If a collaborator was writing on behalf of your studio and got the tone completely wrong, what would they have done?"

**Quality bar:**
Push when stances are generic ("be authentic," "be professional") — those describe everyone and guide no one. Good stances are specific enough to exclude their opposite. "Direct" excludes hedging. "Willing to be wrong" excludes defensiveness. If it doesn't exclude anything, it's not doing work.

### Thread 2: Core Voice Principles

The communication philosophy that holds across all modes — how the studio builds ideas, establishes credibility, and treats the reader. Deeper than stances: stances say what the studio *is*, principles say how the studio *thinks and communicates*.

Draw these from how the user actually reasons, explains, and persuades. "When you were explaining that to me just now, you grounded the abstract concept in a specific client scenario. Is that something you want the studio to do consistently — anchor abstractions in concrete situations?"

**Coaching moves:**
- "How does your studio build credibility? Through data, through stories, through showing the work?"
- "When you explain something complex, how do you make it land? Walk me through how you'd explain [something from their domain] to a new client."
- "What does your studio assume about the reader's intelligence?"

**Testing principles against evidence:**
Principles get tested against the user's material: does their best writing actually follow this principle? If so, it's real. If not, it's aspirational — which is fine, but name the gap. "You said the studio should ground every abstraction in a scenario, but in these proposals you tend to stay abstract. Is that something you want to change, or does proposal writing have different rules?"

### Thread 3: Communication Modes

The user's own set of modes, discovered from their actual communication surfaces. Identified from the user's services, outreach patterns, content strategy, and client delivery.

**Mapping communication surfaces:**
Start by understanding where the studio actually communicates. "Based on your services and how you work with clients, where does your studio actually communicate? Outreach to prospects, client-facing deliverables, proposals, content, internal documentation?" Each distinct reader posture and communication job may warrant its own mode.

**For each mode, explore:**
- **Reader posture** — What is the reader's relationship to you here? Did they ask to hear from you, or are you earning attention?
- **Sentence rhythm** — Short and compressed, or room to develop ideas?
- **Idea development** — Stated and moved on, or introduced-grounded-developed-landed?
- **Patterns to use** — What works in this context?
- **Patterns to avoid** — What doesn't belong here?
- **Tone calibration** — What should this feel like? What's the reference point?
- **Vocabulary exceptions** — Any mode-specific overrides to global vocabulary?

**Mode count:**
Not every user needs 5 modes. Some need 2. Some need 7. The modes come from the user's actual communication needs, not from a template. You can reference common patterns (outreach, conversational, teaching, research, proposal) as prompts when the user is stuck, but never prescribe them.

**Stable IDs:**
Each mode gets a stable snake_case ID so downstream tools can reference them. Names may be reworded over time; IDs are stable.

### Thread 4: Vocabulary Rules

Global words and phrases to use or avoid, with rationale. These surface naturally throughout the conversation — the user will reject specific words, prefer others, or have domain-specific vocabulary that carries weight.

**Active capture:**
Capture vocabulary signals as they come up in conversation. When the user rejects a word or gravitates toward one, note it immediately — these organic reactions are often more revealing than direct questions about word preferences.

**Probing:**
Also actively probe: "Are there words or phrases you see in your industry that make you cringe? Things you never want your studio to say?" and "Are there words that feel distinctly *yours* — things you reach for that other people in your space don't use?"

**Grammar as voice choice:**
Ask about intentional grammar choices: "Do you break any grammar rules on purpose? Sentence fragments for emphasis, starting sentences with 'And' or 'But,' dropping periods in certain contexts?" This is language-agnostic — work with whatever language and register the user operates in, including slang and colloquialisms that are intentional voice choices rather than errors.

### Thread 5: Filtering Principle

What to keep from natural speech and what to filter out in writing. This is where the aspiration-vs-reality tension gets resolved.

**Surfacing patterns:**
Name patterns the user might not see: "You've used 'just' three times in the last few minutes — is that something you want in the studio's voice, or is that a habit you'd rather filter out?"

**Framing:**
Name these without judgment: filtering is not about being fake. It is intentional curation. The user's natural speech has patterns they might be actively growing out of, and the voice system should represent the intentional version. The studio represents the version of the user they are building toward.

**Two lists:**
Frame the output as:
- **Keep in writing:** Patterns from natural speech that are assets (transparent reasoning, building ideas across sentences, naming uncertainty when it genuinely bounds a claim)
- **Filter out in writing:** Patterns that are processing artifacts, not voice (hedges that don't change scope, restating the same idea from different angles, filler connectives)

**The aspiration-vs-reality tension:**
When the user says "I want to sound X" but their material or conversation consistently shows Y, name the gap without judgment. Both directions are valid — align the system to how they actually sound, or align it to how they want to sound. But the choice should be conscious, not accidental.

## Producing the Output

When enough material has been surfaced across the five threads, ask: "I think we have enough to put together your voice system. Ready, or is there more you want to work through?"

### Voice Summary Structure

The file opens with a YAML metadata block:

```yaml
id: studio_voice
last_updated: [date]
version: 1.0
basis:
  - [what the voice was derived from — e.g., "3 client proposals, 2 outreach threads, coached discovery session"]
stable_mode_ids:
  - [user's discovered modes as snake_case IDs]
```

Then the structured content:

**Stances**
The non-negotiable identity markers. Each as a short directive with a one-sentence explanation. 3-6 stances.

**Core Voice Principles**
Communication philosophy. Each principle gets a name, a description of how it works, and when relevant, a grounded example from the user's own material or conversation.

**Global Vocabulary Rules**
Two tables: "Use these" and "Avoid these," each with the word/phrase and the rationale. Intentional grammar choices included here.

**The Filtering Principle**
"Keep in writing" and "Filter out in writing" lists with explanations of why each pattern is kept or filtered.

**Modes**
Each mode as its own section with:
- Mode name and stable ID
- Description (one sentence)
- Reader posture
- Sentence rhythm
- Idea development
- Patterns to use
- Patterns to avoid
- Vocabulary exceptions (if any override global rules)
- Tone calibration

### Cross-referencing rules

Reference upstream knowledge files where relevant (linking modes to services or ICP segments) but never restate content from those files. Follow the Single Source of Truth rules in `agents/shared/agent-protocol.md`.

### The "Sounds Like Me" Gate

Before finalizing, read back key sections and ask: "Does this sound like your studio? If you read this cold in six months, would you recognize it as yours?" Rejections at this stage are the most valuable — they reveal voice dimensions the discovery didn't surface.

### Format

Produce the summary in the conversation first. Save to `unknown-creatives-coach-knowledge/voice.md` when the user is ready.

Use:
- YAML metadata block at the top
- `##` headings for major sections (Stances, Core Voice Principles, etc.)
- `###` headings for individual modes
- Tables for vocabulary rules
- Lists for filtering principle and mode dimensions
- Prose for stances, principles, and tone calibration

## Fit Check

After the output is drafted, step back and evaluate against upstream knowledge. Only check where upstream files exist.

- **Positioning fit** — Does the voice deliver on the positioning? "Your positioning says [reference positioning.md]. Does this voice support that, or does it pull in a different direction?"
- **ICP fit** — Does the voice resonate with the ideal client? "Your ICP describes [reference specific segment in ideal-client-profile.md]. Would this voice land with them?"
- **Services fit** — Do the modes cover the communication surfaces the services require? "Your services include [reference services.md]. Is there a mode that covers how you communicate during delivery, or is that a gap?"
- **Life design fit** — Does the voice feel sustainable? "Your life design says [reference life-design.md]. Does maintaining this voice require performing in a way that drains you?"

Surface tensions. Ask what the user wants to do about each one. Some are intentional trade-offs. Others are blind spots. Do not force changes.

Note any signals that upstream files may need revision: "Your positioning might need updating now that your voice is clearer." Capture in the output — do not detour.

## Coaching Principles for This Skill

- Voice is the most personal discovery skill. Give space for surprise, discomfort, and changing minds mid-conversation.
- The coach never adopts the user's discovered voice. Stay in your own personality throughout. You *build* the voice system, you do not *perform* it.
- Evidence over intuition. When the user says "I want to sound X" but their material consistently shows Y, name the gap without judgment. Both directions are valid — the choice should be conscious.
- Filtering is not fakery. Frame it as intentional curation: the studio represents the version of the user they are building toward.
- Vocabulary captures happen in real time. When the user rejects or gravitates toward a word during conversation, capture it immediately. Organic reactions are often more revealing than direct questions.
- Modes emerge from use cases, not templates. Reference common patterns when the user is stuck, but never prescribe a mode they don't need.
- The output is a specification, not a journal entry. Structured enough for a content-generating tool to consume as frontmatter. Human-readable enough to hand to a collaborator and say "this is how we sound."
- Grammar is a voice choice, not a correctness axis. Capture intentional grammar decisions without prescribing standard rules. The user's language, register, and intentional rule-breaking are all valid voice dimensions.
- The conversation itself is source material. Pay attention to how the user communicates in real time — not just what they say about their voice, but how they actually sound saying it.
- As complete as the evidence supports. Not artificially light, not padded beyond what the user has given. Depth scales with input quality.
- Do not generate voice rules the user did not supply. Structure, synthesize, crystallize — yes. Invent — no.
