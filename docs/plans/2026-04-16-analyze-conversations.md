# Analyze Conversations Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a skill that reads past prospect/client conversations, produces a hypothesis-based observation map, and feeds richer context into the existing coaching flow.

**Architecture:** One new skill file (`analyze-conversations`), a routing update and skill listing update in the coach agent, and a version bump. The skill writes temporary observations to `conversation-insights.md` and hands off to existing coaching skills.

**Tech Stack:** Claude Code plugin system (markdown skill + agent edits). No code — all prompt engineering.

---

## File Map

### New files to create
- `plugins/contra-coach/skills/analyze-conversations/SKILL.md` — the skill definition

### Existing files to modify
- `plugins/contra-coach/agents/creative-business-consultant.md` — add skill to Available Skills list + add routing logic for conversation materials
- `plugins/contra-coach/.claude-plugin/plugin.json` — bump version to 0.3.0

---

## Task 1: Create the analyze-conversations Skill

The core skill file. This is the main deliverable.

**Files:**
- Create: `plugins/contra-coach/skills/analyze-conversations/SKILL.md`

- [ ] **Step 1: Create the skill file**

```markdown
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
```

- [ ] **Step 2: Commit**

```bash
cd ~/Documents/GITHUB/contra-coach
git add plugins/contra-coach/skills/analyze-conversations/SKILL.md
git commit -m "feat: add analyze-conversations skill for evidence-based coaching"
```

---

## Task 2: Update Coach Agent — Available Skills List

Add the new skill to the coach agent's skill listing so it knows the skill exists.

**Files:**
- Modify: `plugins/contra-coach/agents/creative-business-consultant.md`

- [ ] **Step 1: Add analyze-conversations to the Discovery Skills list**

In `plugins/contra-coach/agents/creative-business-consultant.md`, find the section that reads:

```markdown
### Discovery Skills
These are self-exploration modules. Each one guides a structured conversation to help the user articulate a specific aspect of their business identity and the life it supports.

- **design-your-business** — Explore who they are professionally. Their arena, distinct strengths, and what types of work energize or drain them. Answers: "What am I built for?"
- **design-your-life** — Explore what they want the business to do for their life. Values, working style, and the personal life the business needs to support. Answers: "What do I need the business to do for me?"
- **positioning** — Define what makes them different and where they stand in the market. Sharpens the arena and strengths into a market-facing statement.
- **icp** — Identify their ideal client profile. Who they serve best and want more of.
- **services** — Clarify what they offer, how they deliver it, and what outcomes they create.
- **voice** — Articulate their tone, communication style, and how they want to come across.
```

Replace it with:

```markdown
### Discovery Skills
These are self-exploration modules. Each one guides a structured conversation to help the user articulate a specific aspect of their business identity and the life it supports.

- **analyze-conversations** — Read past prospect/client conversations and surface patterns. Produces an observation map with evidence-based hypotheses that feed into any of the skills below. Start here when the user brings conversation materials.
- **design-your-business** — Explore who they are professionally. Their arena, distinct strengths, and what types of work energize or drain them. Answers: "What am I built for?"
- **design-your-life** — Explore what they want the business to do for their life. Values, working style, and the personal life the business needs to support. Answers: "What do I need the business to do for me?"
- **positioning** — Define what makes them different and where they stand in the market. Sharpens the arena and strengths into a market-facing statement.
- **icp** — Identify their ideal client profile. Who they serve best and want more of.
- **services** — Clarify what they offer, how they deliver it, and what outcomes they create.
- **voice** — Articulate their tone, communication style, and how they want to come across.
```

The only change is adding the `analyze-conversations` line at the top of the list with the note "Start here when the user brings conversation materials."

- [ ] **Step 2: Commit**

```bash
cd ~/Documents/GITHUB/contra-coach
git add plugins/contra-coach/agents/creative-business-consultant.md
git commit -m "feat: add analyze-conversations to coach agent skill listing"
```

---

## Task 3: Update Coach Agent — Routing Logic

Add the routing rule for when the user provides conversation materials.

**Files:**
- Modify: `plugins/contra-coach/agents/creative-business-consultant.md`

- [ ] **Step 1: Add conversation materials routing**

In `plugins/contra-coach/agents/creative-business-consultant.md`, find the routing logic section. Locate this block:

```markdown
**When the user has existing documents in their workspace:**
If the user selected a folder with existing client documents, past proposals, scopes of work, or similar — reference them. Pull context from those files to inform the conversation. The more the user brings, the better the output.
```

Replace it with:

```markdown
**When the user provides past conversations:**
If the user shares transcripts, call recordings, emails, proposals, meeting notes, or other prospect/client conversation materials, offer the analysis skill: "I see you've shared some past conversations. Want me to read through them first and surface patterns before we start coaching?" If yes, invoke `analyze-conversations`. If no, proceed with normal coaching using the materials as background context.

**When the user has existing documents in their workspace:**
If the user selected a folder with existing client documents, past proposals, scopes of work, or similar — reference them. Pull context from those files to inform the conversation. The more the user brings, the better the output.
```

The new routing rule is inserted BEFORE the existing documents rule. This is intentional — conversation materials are a specific subset of documents that warrant the analysis skill, while general documents (case studies, website copy, etc.) are still handled as background context.

- [ ] **Step 2: Commit**

```bash
cd ~/Documents/GITHUB/contra-coach
git add plugins/contra-coach/agents/creative-business-consultant.md
git commit -m "feat: add routing for conversation materials to coach agent"
```

---

## Task 4: Bump Version and Push

**Files:**
- Modify: `plugins/contra-coach/.claude-plugin/plugin.json`

- [ ] **Step 1: Update plugin.json**

Replace the contents of `plugins/contra-coach/.claude-plugin/plugin.json` with:

```json
{
  "name": "contra-coach",
  "version": "0.3.0",
  "description": "A creative business consultant that helps freelancers and independent creatives articulate their business identity — positioning, ideal clients, services, voice — and turn it into usable documents. Includes an inline evaluation pipeline for quality assurance and conversation analysis for evidence-based coaching.",
  "author": {
    "name": "Dimitri Otero"
  },
  "keywords": ["coaching", "freelance", "creative", "business", "positioning", "contra", "evaluation", "conversations"]
}
```

Changes from 0.2.0: version bumped to 0.3.0, description updated to mention conversation analysis, "conversations" added to keywords.

- [ ] **Step 2: Commit**

```bash
cd ~/Documents/GITHUB/contra-coach
git add plugins/contra-coach/.claude-plugin/plugin.json
git commit -m "chore: bump version to 0.3.0 for analyze-conversations skill"
```

- [ ] **Step 3: Push**

```bash
cd ~/Documents/GITHUB/contra-coach
GITHUB_TOKEN="" git push
```

---

## Task 5: Verification

- [ ] **Step 1: Verify file structure**

Run:
```bash
cd ~/Documents/GITHUB/contra-coach
find plugins/contra-coach/skills -type f | sort
```

Expected: 6 skills including the new one:
```
plugins/contra-coach/skills/analyze-conversations/SKILL.md
plugins/contra-coach/skills/design-your-business/SKILL.md
plugins/contra-coach/skills/design-your-life/SKILL.md
plugins/contra-coach/skills/evaluate-session/SKILL.md
plugins/contra-coach/skills/icp/SKILL.md
plugins/contra-coach/skills/positioning/SKILL.md
```

- [ ] **Step 2: Verify coach agent has new skill and routing**

Run:
```bash
cd ~/Documents/GITHUB/contra-coach
grep -c "analyze-conversations" plugins/contra-coach/agents/creative-business-consultant.md
```

Expected: at least 2 matches (skill listing + routing logic)

- [ ] **Step 3: Verify version**

Run:
```bash
cat plugins/contra-coach/.claude-plugin/plugin.json | grep version
```

Expected: `"version": "0.3.0"`
