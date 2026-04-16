---
name: analyze-conversations
description: >
  This skill should be used when the user provides past prospect or client conversations
  for analysis — call transcripts, emails, DMs, proposals, meeting notes, or any record
  of how they've communicated with prospects or clients. The skill reads the materials,
  surfaces patterns the user may not see, and produces a hypothesis-based observation map
  that gives the coach evidence-based starting questions for coaching.

  Use when someone says "I have some past conversations," "here are my call transcripts,"
  "can you look at my emails with prospects," "analyze my sales calls," "read through
  these proposals," "what patterns do you see in my conversations," or when the
  creative-business-consultant agent routes to conversation analysis after the user
  shares conversation materials.

  Also triggers on: "review my prospect calls," "look at how I talk to clients,"
  "what can you learn from my conversations," "read my meeting notes," "analyze my
  outreach," or any request to extract coaching insights from past communications.
---

# Analyze Conversations

Read past prospect/client conversations and produce a hypothesis-based observation map that gives the coach evidence-based starting questions. This skill turns the user's real communication history into coaching fuel — surfacing patterns they can't see themselves.

This is not a summarization tool. It analyzes conversations through the lens of the existing coaching frameworks (positioning, ICP, energy, voice) and presents observations as hypotheses for the user to validate or correct.

## Knowledge and Memory

### Before starting
- Check `contra-coach-knowledge/positioning.md` — if it exists, compare how the user described themselves in conversations against their articulated positioning. Gaps between the two are high-value signals.
- Check `contra-coach-knowledge/ideal-client-profile.md` — did the user actually talk to the types of clients they said they wanted? Compare conversation partners against the stated ICP.
- Check `contra-coach-knowledge/business-identity.md` — does their natural conversation behavior match their stated identity?
- Read `contra-coach-knowledge/memory.md` if it exists. Use past corrections and preferences to shape the analysis — don't flag patterns the user has already addressed.

### During the session
- Write observations to `contra-coach-knowledge/conversation-insights.md` — this is a temporary working document for the session
- Follow the memory protocol in `references/memory-protocol.md` for any corrections, preferences, or self-discoveries the user reveals during the coaching that follows

### After the session
- Validated discoveries flow into permanent knowledge files through whichever coaching skill the user transitions to
- Key insights get captured in `memory.md` per the memory protocol
- `conversation-insights.md` can be overwritten on the next analysis — it is not a permanent artifact

## Reading the Materials

### Adapt to what you receive

The user may provide any combination of conversation types. Adapt your analysis based on what's there:

**Call transcripts** — the richest source. Look for how the user naturally talks: their phrasing, confidence level, energy shifts across topics, moments where they become more or less engaged. This is the unfiltered version of who they are. Pay attention to what they say unprompted versus what they say when asked.

**Emails and DMs** — intentional communication. The user was performing here — writing to impress or persuade. The gap between how they talk on calls and how they write in emails is itself a valuable signal. Look for how they frame their value, how they describe their work, and where they hedge or over-qualify.

**Proposals and SOWs** — reveals positioning and pricing clarity. How did they describe their process? How did they frame the value of the engagement? Did they lead with outcomes or activities? Was the pricing confident or apologetic?

**Meeting notes and summaries** — compressed signal. Less useful for voice analysis but valuable for ICP patterns (who were they meeting with?) and energy patterns (what topics got discussed in depth vs. glossed over?).

**Mixed inputs** — the most powerful. Cross-reference across types. The user who sounds confident on calls but hedges in proposals has a clarity gap worth naming. The user who describes themselves differently to different prospects hasn't locked in their positioning.

### What to look for

Read with these lenses:

1. **How does the user describe themselves?** Look for consistency, specificity, and confidence. Do they use the same framing across conversations, or do they reinvent their description each time?

2. **Who are they talking to?** Profile the prospects/clients. What types of companies, roles, industries? Which conversations went deeper and which stayed surface-level?

3. **Where does their energy shift?** Topics where they become more engaged, speak at length, use vivid language. Topics where they go flat, give short answers, or change the subject.

4. **How do they naturally communicate?** Formal vs. casual, jargon vs. plain language, direct vs. hedging. This is voice data they didn't consciously produce.

5. **What patterns repeat?** Pricing hesitation, scope creep offers, underselling, inconsistent positioning, misaligned prospects. Things that happen more than once are patterns, not accidents.

## Producing the Observation Map

After reading all materials, produce an observation map organized by theme. Only include themes where you found actual signal — do not pad or manufacture observations.

### Themes

**Positioning signals**
How the user described themselves across conversations. Inconsistencies between framings. Which versions got traction with prospects vs. which fell flat. If `positioning.md` exists, note where conversation behavior aligns with or diverges from their stated positioning.

**ICP patterns**
Who the user was talking to. Which conversations had energy and depth. Which felt transactional or forced. Common attributes of the prospects they engaged most with. If `ideal-client-profile.md` exists, note whether they actually talked to their stated ideal clients.

**Energy/drain patterns**
Topics where the user became more engaged vs. where they went flat. Types of work discussed enthusiastically vs. reluctantly. Project phases that generated excitement vs. dread.

**Voice patterns**
How the user naturally communicates. Formal vs. casual tone. Jargon vs. plain language. Patterns in word choice. Gap between spoken and written communication.

**Red flags**
Things the user said that undermined their positioning, underpriced themselves, or signaled misalignment with the prospect. These are observations, not judgments.

### Hypothesis framing

Every observation MUST follow this pattern:
1. **Evidence** — State the specific observation with a direct reference to the conversation ("In your call with [Prospect], you said '...'")
2. **Interpretation** — Name what it might mean ("That could suggest you're more drawn to strategy work than execution")
3. **Question** — Ask the user to validate or correct ("Does that resonate, or is something else going on?")

Do NOT present observations as conclusions. Do NOT tell the user who they are. Present what you noticed and ask if it fits.

### Writing the insights file

Save the observation map to `contra-coach-knowledge/conversation-insights.md` so it persists during the session. Format:

```markdown
# Conversation Insights

Generated from: [list of materials analyzed]
Date: [session date]

## Positioning Signals
[observations with evidence and hypotheses]

## ICP Patterns
[observations with evidence and hypotheses]

## Energy/Drain Patterns
[observations with evidence and hypotheses]

## Voice Patterns
[observations with evidence and hypotheses]

## Red Flags
[observations with evidence and hypotheses]
```

Only include sections where signal was found.

## Handoff to Coaching

After presenting the observation map, ask: **"Where do you want to start?"**

If the user picks a theme, transition naturally into the appropriate coaching skill:
- Positioning signals → invoke **positioning** skill
- ICP patterns → invoke **icp** skill
- Energy/drain patterns → invoke **design-your-business** skill
- Voice patterns → invoke **voice** skill (when available)
- Red flags → depends on the specific flag — route to the most relevant skill

If the user doesn't know where to start, recommend based on where you found the strongest signal or the biggest gap.

The handoff must feel natural. Do NOT say "Now invoking the positioning skill." Instead: "Let's dig into how you're describing yourself. You had some interesting variations across these conversations — let's figure out which version is really you."

The coaching skill that picks up will read `conversation-insights.md` as additional context, giving it a warm start instead of cold discovery.

## Coaching Principles for This Skill

- The observation map is a starting point, not a deliverable. Its value is in the coaching conversation it enables, not the document itself.
- Observations based on 2-3 conversations are hypotheses. Observations based on 10+ conversations are patterns. Frame the confidence accordingly.
- The user may be surprised or defensive about what you surface — especially red flags like pricing hesitation or inconsistent positioning. Handle these the same way the coach handles resistance: acknowledge, don't argue, redirect to what they want to do about it.
- If the user provides only one conversation, you can still extract signal — but be transparent that patterns need more data. "This is one conversation, so I'm reading it as a snapshot, not a pattern."
- Cross-referencing against existing knowledge files is one of the most powerful moves. "Your positioning says you focus on design systems for scaling companies, but in three of these conversations you were pitching general product design. That gap is worth exploring."
