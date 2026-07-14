# Memory Protocol

This document defines how the Unknown Creatives Coach plugin captures, stores, and revises memory. Every skill and the agent follow this protocol.

## What Memory Is

Memory is a single file — `unknown-creatives-coach-knowledge/memory.md` — that lives in the user's workspace alongside the structured knowledge files. It captures conversational signals that don't belong in a structured knowledge file yet but are important to remember across sessions.

Memory is a **working scratchpad**, not a log. It stays lean and high-signal through active revision.

## What Memory Is Not

- Not a transcript or conversation log
- Not a duplicate of what's already in the knowledge files (business-identity.md, positioning.md, etc.)
- Not Claude's platform-level memory — this is the plugin's own file-based memory, completely separate from Claude's native memory system. Do not write to or interfere with Claude's built-in memory features.

## When to Write to Memory

Append an entry to `memory.md` when any of the following happen during a coaching conversation:

**Direct corrections to generated content**
The user rejects specific language, framing, or tone. Examples: "I don't talk like that," "that's too corporate," "I'd never say 'leverage.'"

**Explicitly stated preferences**
The user names something they always want or never want. Examples: "I never do weekly standups," "I always want to lead with the problem," "I only work with companies under 50 people."

**Factual corrections about their business**
The user corrects something the agent got wrong about their situation. Examples: "Actually I have two contractors, not employees," "My retainer starts at $5k, not $3k."

**Strong reactions that reveal how they think**
The user reacts strongly (positive or negative) to something in a way that reveals an underlying preference or value. Examples: lighting up when talking about a certain type of project, shutting down when a certain topic comes up.

**Self-discoveries**
The user realizes something about themselves mid-conversation that surprises them. Examples: "I just realized I actually hate the execution phase," "Huh, I guess I do have a type when it comes to clients."

**Repeated patterns**
If the user says the same thing more than once across different conversations or skill contexts, that's a high-signal memory. Note the repetition.

## When NOT to Write to Memory

- General conversation back-and-forth
- One-off clarifications ("let's focus on this for now")
- Questions the user asks
- Temporary or session-specific context
- Anything already captured in a structured knowledge file — don't duplicate
- Routine coaching flow that doesn't reveal a preference or correction

## Entry Format

Each entry is a single line or short block with a date, context tag, and the observation:

```markdown
- **2026-04-10 | positioning** — Rejected "I leverage design systems to optimize product velocity." Prefers plain language: "I help teams build design systems so they ship faster."

- **2026-04-10 | voice** — "Stop saying 'streamline.' I'd say 'simplify' or 'clean up.'"

- **2026-04-11 | icp** — Best clients have always been 10-30 person product teams. Larger orgs feel bureaucratic, smaller ones don't have budget.

- **2026-04-11 | self-discovery** — Surprised to realize they enjoy the selling phase. Always assumed they hated it, but what they hate is cold outreach — warm conversations energize them.
```

Context tags use the skill name or a general category — the canonical tag list lives in `agents/shared/agent-protocol.md` → "Knowledge File Registry". Do not maintain a copy here.

Keep entries concise. One to three sentences maximum. Capture the signal, not the full story.

## Reading Memory

At the start of every coaching conversation or skill invocation, read `memory.md` if it exists. Use what's there to:

- Avoid repeating mistakes (don't use language they've already rejected)
- Build on known preferences (if they prefer direct language, lead with that)
- Reference past discoveries ("Last time we talked, you mentioned your best clients are small product teams — does that still feel right?")

If `memory.md` doesn't exist yet, that's fine — it gets created on the first write.

## Revising Memory

Memory revision happens at the start of a new session when the agent notices the file is getting long, or when the user asks for a review. The revision process has three operations:

**Consolidate** — If multiple entries capture the same signal from different conversations, merge them into one clearer statement. Three separate corrections about avoiding business jargon become one preference statement: "Prefers plain, direct language. Specifically avoids: leverage, optimize, streamline, synergy. Uses instead: use, improve, simplify, work together."

**Graduate** — If a memory entry has matured into a clear, stable insight that belongs in a structured knowledge file, move it there. Repeated voice corrections graduate into `voice.md`. A crystallized ICP pattern graduates into `ideal-client-profile.md`. Once graduated, remove the entry from memory.

**Prune** — Remove entries that are no longer relevant, were session-specific, or have been superseded by newer information. If the user changed their mind about something captured earlier, remove the outdated entry.

After revision, memory.md should be short — ideally under 30 entries. If it's longer than that, it needs more aggressive consolidation and graduation.

## User Access

The user owns this file. If they ask to see their memory, show them the contents. If they ask to edit it, help them. If they ask to export it, move it wherever they want. If they ask to turn memories into a guideline or SOP, extract the relevant entries and help them build a structured document from the patterns.

## File Location

The memory file lives at:
```
unknown-creatives-coach-knowledge/memory.md
```

This is inside the same knowledge folder as all other discovery outputs. It gets created automatically on the first memory-worthy event.
