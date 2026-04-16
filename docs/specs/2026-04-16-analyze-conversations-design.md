# Analyze Conversations Skill — Design Spec

## Overview

A new skill that reads past prospect/client conversations (any format), produces a hypothesis-based observation map organized by coaching themes, then hands off to the appropriate existing coaching skill for validation. The skill gives the coach evidence-based starting questions instead of cold discovery.

## Goals

1. Let the coach start from what the user has actually said in real conversations, not just self-reporting
2. Surface patterns the user can't see themselves — inconsistencies, energy shifts, recurring themes
3. Frame every observation as a hypothesis to validate, not a conclusion — preserving the coaching philosophy
4. Produce a temporary working document that feeds the session, not a permanent artifact
5. Hand off seamlessly to existing coaching skills with richer context

## How It Works

### Input

The user provides past conversation materials. The skill handles any format:

| Input Type | What the Skill Looks For |
|-----------|------------------------|
| **Call transcripts** | How the user naturally talks — phrasing, confidence, energy shifts. The unfiltered version of who they are. |
| **Emails/DMs** | How the user presents themselves intentionally — the gap between natural speech and performed professionalism is itself a signal. |
| **Proposals/SOWs** | How the user frames their value, prices their work, describes their process. Reveals positioning and services clarity. |
| **Meeting notes/summaries** | Compressed signal — topics discussed, decisions made, follow-ups. Less about voice, more about ICP and energy patterns. |
| **Mixed** | Cross-reference across types. The user who sounds confident on calls but hedges in proposals has a clarity gap worth naming. |

The skill adapts its analysis approach based on what it receives. Transcripts get voice and energy analysis. Proposals get positioning and pricing analysis. Mixed inputs get cross-referenced.

### The Observation Map

After reading the materials, the skill produces an observation map organized into themes. Only themes where actual signal was found are included — no padding.

**Positioning signals:**
How the user described themselves across conversations. Inconsistencies between framings. Which versions got traction with prospects vs. which fell flat. Comparison against existing `positioning.md` if it exists.

Example hypothesis: *"I noticed you described yourself as 'a product designer who builds design systems' to Prospect A but 'a consultant who helps teams scale their design practice' to Prospect B. The second version is broader — was that intentional, or were you still figuring out how to say it?"*

**ICP patterns:**
Who the user was talking to. Which conversations had energy and depth. Which felt transactional or forced. Common attributes of the prospects they engaged most with. Comparison against existing `ideal-client-profile.md` if it exists.

Example hypothesis: *"Three of your five conversations were with Series B startups, and those were the longest and most detailed. The two agency conversations were shorter and more surface-level. Does that match how you felt about those conversations?"*

**Energy/drain patterns:**
Topics where the user became more engaged vs. where they went flat. Types of work discussed enthusiastically vs. reluctantly. Project phases that generated excitement vs. dread.

Example hypothesis: *"In your call with [Client], you spent 20 minutes on the strategy discussion and barely touched the execution timeline. That's an energy signal — does strategic work energize you more than execution?"*

**Voice patterns:**
How the user naturally communicates. Formal vs. casual tone. Jargon vs. plain language. Patterns in word choice. Gap between how they talk on calls vs. how they write in proposals.

Example hypothesis: *"On calls you're direct and casual — 'I just help teams not hate their design system.' But your proposals shift to formal language — 'leveraging systematic design approaches.' Which version sounds more like you?"*

**Red flags:**
Things the user said that undermined their positioning, underpriced themselves, or signaled misalignment with the prospect. These are observations, not judgments — the coach presents them for the user to assess.

Example hypothesis: *"In two conversations, you offered to reduce scope when the prospect hesitated on price. That might be a pattern worth examining — was that a strategic choice, or a reflex?"*

### Hypothesis Framing

Every observation MUST be framed as a hypothesis to validate. The skill does not tell the user who they are — it tells them what it noticed and asks if it resonates.

The pattern is always:
1. State the specific observation with evidence ("I noticed X in your conversation with Y")
2. Name what it might mean ("That could suggest Z")
3. Ask the user to validate or correct ("Does that resonate, or is something else going on?")

This preserves the core coaching philosophy: the thinking comes from the user. The skill just gives the coach better questions to ask.

### Handoff to Coaching

After presenting the observation map, the coach asks: **"Where do you want to start?"**

The user picks a theme (or the coach suggests one based on where the biggest gaps or patterns appeared). The coach then invokes the appropriate existing skill — positioning, ICP, design-your-business, etc. — which picks up the conversation insights as additional context.

The handoff should feel natural, not mechanical. The coach doesn't say "Now invoking the positioning skill." It says something like "Let's dig into how you're describing yourself. You had some interesting variations across these conversations..."

## Knowledge and Memory

### Before Starting

- Check `contra-coach-knowledge/positioning.md` — if it exists, the analysis can compare what the user said in conversations against what they articulated in prior coaching. Gaps between the two are high-value signals.
- Check `contra-coach-knowledge/ideal-client-profile.md` — same comparison. Did the user actually talk to the types of clients they said they wanted?
- Check `contra-coach-knowledge/business-identity.md` — does their natural conversation behavior match their stated identity?
- Read `contra-coach-knowledge/memory.md` for language preferences, corrections, and known patterns.

### During the Session

- Write observations to `contra-coach-knowledge/conversation-insights.md` — this is the temporary working document for the session
- Follow the memory protocol in `references/memory-protocol.md` for any corrections, preferences, or self-discoveries the user reveals during the coaching conversation that follows

### After the Session

- Validated discoveries flow into permanent knowledge files through whichever coaching skill the user transitions to (positioning.md, ideal-client-profile.md, etc.)
- Key insights from the conversation analysis get captured in `memory.md` per the memory protocol — language preferences revealed by how they naturally talk, ICP patterns from who they actually engaged with, etc.
- `conversation-insights.md` is a session working document — it can be overwritten on the next analysis. It is not a permanent artifact.

## Coach Agent Routing Update

Add to the coach agent's routing logic section:

**When the user provides past conversations:**
If the user shares transcripts, call recordings, emails, proposals, or other prospect/client conversation materials, offer the analysis skill: "I see you've shared some past conversations. Want me to read through them first and surface patterns before we start coaching?" If yes, invoke `analyze-conversations`. If no, proceed with normal coaching using the materials as background context.

This routing update is a small addition to the existing routing logic section in `creative-business-consultant.md`.

## Eval Coverage

This skill needs its own evals, created via the eval-creator agent after implementation:

**Thread-level evals:**
- `observation-quality.md` — Are observations grounded in specific evidence from the conversations (direct quotes, specific moments), not generic coaching advice?
- `hypothesis-framing.md` — Are observations presented as hypotheses to validate, not conclusions? Does every observation follow the pattern: evidence → interpretation → question?
- `theme-relevance.md` — Does the observation map only include themes where actual signal was found? No padding or manufactured observations.
- `input-adaptation.md` — Does the analysis approach shift based on the type of material provided? Transcripts analyzed for voice; proposals analyzed for positioning; mixed inputs cross-referenced.
- `handoff-quality.md` — Does the transition from analysis to coaching feel natural? Does the coach reference specific observations when moving into a skill?

**Behavioral evals:**
The existing behavioral evals (one-question-at-a-time, never-answer-for-user, push-back-on-vague, etc.) still apply during the coaching phase that follows the analysis.

## Plugin File Changes

### New files
- `plugins/contra-coach/skills/analyze-conversations/SKILL.md` — the skill definition

### Modified files
- `plugins/contra-coach/agents/creative-business-consultant.md` — add routing for conversation materials
- `plugins/contra-coach/.claude-plugin/plugin.json` — bump version to 0.3.0

### Eval files (created later via eval-creator)
- `plugins/contra-coach/evals/analyze-conversations/observation-quality.md`
- `plugins/contra-coach/evals/analyze-conversations/hypothesis-framing.md`
- `plugins/contra-coach/evals/analyze-conversations/theme-relevance.md`
- `plugins/contra-coach/evals/analyze-conversations/input-adaptation.md`
- `plugins/contra-coach/evals/analyze-conversations/handoff-quality.md`

## What This Spec Does NOT Cover

- **Eval content** — the actual criteria for the 5 new evals. The eval-creator agent will generate these based on the skill definition.
- **Automated conversation ingestion** — the user manually provides materials. Future work could integrate with tools like Granola, Otter, or email clients to pull conversations automatically.
- **Conversation summarization** — the skill analyzes for coaching-relevant patterns, not general summarization. It's not a meeting notes tool.
