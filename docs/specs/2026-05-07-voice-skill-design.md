# Voice Skill Design

**Date:** 2026-05-07
**Status:** Approved
**Output file:** `unknown-creatives-coach-knowledge/voice.md`
**Builds on:** `business-identity.md`, `positioning.md`, `ideal-client-profile.md`, `services.md`, `life-design.md`
**Reference files:** None

## Purpose

Guide a creative professional through discovering and defining their studio's communication voice — how it sounds, what it values, how it adapts across contexts, what it filters out. The output is a structured voice specification (`voice.md`) that downstream tools consume as a runtime dependency when producing content.

The voice skill does NOT change how the coach speaks. The coach remains in its own personality (personality.md) throughout. It builds voice.md *for* the user's studio, as a portable asset they take with them. The coach never adopts the user's discovered voice.

## Coaching Approach

**Emergent discovery with coverage tracking.** The conversation is free-form — the coach follows the user's thread, surfaces patterns, names what it hears, and builds understanding organically. The coach internally tracks coverage across the five discovery threads (stances, principles, modes, vocabulary, filtering) and steers toward gaps when the natural flow doesn't reach them. No rigid stages. The user never feels like they're filling out a form.

### Coaching Lenses

The coach continuously assesses which lens fits and shifts without announcing:

- **Discoverer** — User has no articulated voice and limited source material. The coach works primarily from the conversation itself, asking about communication preferences, what they like/dislike about how they currently sound, and drawing patterns from how the user is speaking right now.
- **Extractor** — User has source material (emails, proposals, posts, client conversations, past plugin conversations) but hasn't articulated rules. The coach analyzes material and surfaces patterns, then validates with the user.
- **Refiner** — User has some voice sense ("I want to sound direct but not cold") but needs structure and depth. The coach takes their intuitions and builds them into a complete system.

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
The coach doesn't know whether the user is typing or dictating, but it's aware that the input modality can influence vocabulary and rhythm. When analyzing the conversation as source material, look for signals (filler words, run-on structures, conversational cadence) and don't over-index on artifacts that might be dictation-specific rather than intentional voice choices. If patterns seem inconsistent with stated preferences, name it without judgment: "You mentioned you want the studio to sound concise, but in conversation you tend to develop ideas across longer runs. That's natural in spoken language — do you want the written voice to compress those, or does that longer rhythm feel right for the studio too?"

## Discovery Threads

The coach tracks five threads internally. These are not stages the user walks through in order — they are dimensions the coach is building toward. The conversation flows naturally, and the coach captures insights for the right thread as they surface. When a thread has enough material, the coach moves on. When one is thin, the coach steers toward it.

### Thread 1: Stances

The non-negotiable identity markers — how the studio shows up regardless of context, audience, or format. Values expressed as communication behaviors.

Surface these from patterns across the user's material and conversation. "You keep coming back to being honest about what you don't know. Is that something you want the studio to be known for — naming uncertainty rather than papering over it?" The goal is 3-6 stances, each expressed as a short directive ("Direct. Name what we see.") with enough context that someone applying them knows what they mean in practice.

Push when stances are generic ("be authentic," "be professional") — those describe everyone and guide no one. Good stances are specific enough to exclude their opposite. "Direct" excludes hedging. "Willing to be wrong" excludes defensiveness. If it doesn't exclude anything, it's not doing work.

### Thread 2: Core Voice Principles

The communication philosophy that holds across all modes — how the studio builds ideas, establishes credibility, and treats the reader. Deeper than stances: stances say what the studio *is*, principles say how the studio *thinks and communicates*.

Draw these from how the user actually reasons, explains, and persuades. "When you were explaining that to me just now, you grounded the abstract concept in a specific client scenario. Is that something you want the studio to do consistently — anchor abstractions in concrete situations?" Principles get tested against the user's material: does their best writing actually follow this principle? If so, it's real. If not, it's aspirational — which is fine, but the coach names the gap.

### Thread 3: Communication Modes

The user's own set of modes, discovered from their actual communication surfaces. Identified from the user's services, outreach patterns, content strategy, and client delivery.

Start by mapping communication surfaces: "Based on your services and how you work with clients, where does your studio actually communicate? Outreach to prospects, client-facing deliverables, proposals, content, internal documentation?" Each distinct reader posture and communication job may warrant its own mode.

For each mode, explore:
- **Reader posture** — What is the reader's relationship to you here?
- **Sentence rhythm** — Short and compressed, or room to develop ideas?
- **Idea development** — Stated and moved on, or introduced-grounded-developed-landed?
- **Patterns to use** — What works in this context?
- **Patterns to avoid** — What doesn't belong here?
- **Tone calibration** — What should this feel like? What's the reference point?
- **Vocabulary exceptions** — Any mode-specific overrides to global vocabulary?

Not every user needs 5 modes. Some need 2. Some need 7. The modes come from the user's actual communication needs, not from a template. The coach can reference common patterns (outreach, conversational, teaching, research, proposal) as prompts when the user is stuck, but never prescribes them.

Each mode gets a stable snake_case ID so downstream tools can reference them.

### Thread 4: Vocabulary Rules

Global words and phrases to use or avoid, with rationale. These surface naturally throughout the conversation — the user will reject specific words, prefer others, or have domain-specific vocabulary that carries weight.

Capture these as they come up and also actively probe: "Are there words or phrases you see in your industry that make you cringe? Things you never want your studio to say?" and "Are there words that feel distinctly *yours* — things you reach for that other people in your space don't use?"

Grammar lives here too. Ask about intentional grammar choices: "Do you break any grammar rules on purpose? Sentence fragments for emphasis, starting sentences with 'And' or 'But,' dropping periods in certain contexts?" This is language-agnostic — work with whatever language and register the user operates in, including slang and colloquialisms that are intentional voice choices rather than errors.

### Thread 5: Filtering Principle

What to keep from natural speech and what to filter out in writing. This is where the aspiration-vs-reality tension gets resolved.

Surface patterns the user might not see: "You've used 'just' three times in the last few minutes — is that something you want in the studio's voice, or is that a habit you'd rather filter out?" Name these without judgment: filtering isn't about being fake, it's about intentional curation. The user's natural speech has patterns they might be actively growing out of, and the voice system should represent the intentional version.

Frame this as two lists:
- **Keep in writing:** Patterns from natural speech that are assets (transparent reasoning, building ideas across sentences, naming uncertainty when it genuinely bounds a claim)
- **Filter out in writing:** Patterns that are processing artifacts, not voice (hedges that don't change scope, restating the same idea from different angles, filler connectives)

## Output Structure

When the coach has surfaced enough material across the five threads, ask: "I think we have enough to put together your voice system. Ready, or is there more you want to work through?"

### voice.md Structure

The file opens with a YAML metadata block:

```yaml
id: studio_voice
last_updated: [date]
version: 1.0
basis:
  - [what the voice was derived from]
stable_mode_ids:
  - [user's discovered modes as snake_case IDs]
```

Then the structured content:

1. **Stances** — Non-negotiable identity markers. Each as a short directive with a one-sentence explanation. 3-6 stances.

2. **Core Voice Principles** — Communication philosophy. Each principle gets a name, a description of how it works, and when relevant, a grounded example from the user's own material or conversation.

3. **Global Vocabulary Rules** — Two tables: "Use these" and "Avoid these," each with the word/phrase and the rationale. Intentional grammar choices included.

4. **The Filtering Principle** — "Keep in writing" and "Filter out in writing" lists with explanations.

5. **Modes** — Each mode as its own section with:
   - Mode name and stable ID
   - Description (one sentence)
   - Reader posture
   - Sentence rhythm
   - Idea development
   - Patterns to use
   - Patterns to avoid
   - Vocabulary exceptions (if any override global rules)
   - Tone calibration

### Cross-referencing

Reference upstream knowledge files where relevant (linking modes to services or ICP segments) but never restate content from those files.

### The "Sounds Like Me" Gate

Before finalizing, read back key sections and ask: "Does this sound like your studio? If you read this cold in six months, would you recognize it as yours?" Rejections at this stage are the most valuable — they reveal voice dimensions the discovery didn't surface.

## Fit Check

After the output is drafted, evaluate against upstream knowledge where those files exist:

- **Positioning fit** — Does the voice deliver on the positioning?
- **ICP fit** — Does the voice resonate with the ideal client?
- **Services fit** — Do the modes cover the communication surfaces the services require?
- **Life design fit** — Does the voice feel sustainable, or does maintaining it require performing in a way that drains the user?

Surface tensions. The user decides what to do about each one. If upstream files may need revision based on what surfaced, note it in the output — don't detour.

## Coaching Principles

- Voice is the most personal discovery skill. Give space for surprise, discomfort, and changing minds mid-conversation.
- The coach never adopts the user's discovered voice. It stays in its own personality throughout.
- Evidence over intuition. When the user says "I want to sound X" but their material consistently shows Y, name the gap without judgment. Both directions are valid — the choice should be conscious.
- Filtering is not fakery. Frame it as intentional curation: the studio represents the version of the user they're building toward.
- Vocabulary captures happen in real time. When the user rejects or gravitates toward a word during conversation, capture it immediately. Organic reactions are often more revealing than direct questions.
- Modes emerge from use cases, not templates. Reference common patterns when the user is stuck, but never prescribe a mode they don't need.
- The output is a specification, not a journal entry. Structured enough for a content-generating tool to consume as frontmatter. Human-readable enough to hand to a collaborator.
- Grammar is a voice choice, not a correctness axis. Capture intentional grammar decisions without prescribing standard rules.
- The conversation itself is source material. Pay attention to how the user communicates in real time — not just what they say about their voice, but how they actually sound saying it.
- As complete as the evidence supports. Not artificially light, not padded beyond what the user has given. Depth scales with input quality.

## Voice Signals Across Sessions

The coach does NOT passively analyze voice patterns during non-voice coaching sessions. When the user is working on positioning, the coach is fully present for positioning.

However, when a user has an explicit voice reaction during any session — rejects a specific word, says "I'd never say it that way," corrects the coach's phrasing, or expresses a strong preference about how something sounds — the coach captures that to `memory.md` as a voice signal. This is not passive analysis; it's capturing a preference the user explicitly surfaced.

When the user later enters the voice skill, these memory captures become additional source material alongside past conversations, uploaded documents, and the live coaching exchange. The coach surfaces them: "In a previous session, you pushed back on the word [X] — that's a useful signal for your vocabulary rules."

The voice skill can also use past coaching conversations as retrospective source material, analyzing how the user communicated across prior sessions. This analysis happens explicitly within the voice skill, not silently in the background.
