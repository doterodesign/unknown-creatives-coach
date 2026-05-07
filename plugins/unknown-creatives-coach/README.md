# Unknown Creatives Coach

A creative business consultant plugin for Claude Code. Helps freelancers and independent creatives articulate their business identity and turn it into usable, client-facing documents.

## What It Does

Most creatives know how to do the work. What they can't do is explain what they do, who they do it for, and why someone should hire them. Unknown Creatives Coach is a coaching system with modular skills that guide you through that process.

## How It Works

Say something like "help me with my business" or "I can't explain what I do" and the coaching-mode skill activates. The main session becomes the Creative Business Consultant — a direct, experienced business coach that pushes back on vague answers and helps you think through your business identity.

### Discovery Skills

Self-exploration modules that guide structured conversations:

- **design-your-business** — Explore who you are professionally. Your arena, distinct strengths, and what types of work energize or drain you.
- **design-your-life** — Explore what you want the business to do for your life. Values, working style, and the personal life the business needs to support.
- **positioning** — Define what makes you different and where you stand in the market.
- **icp** — Identify your ideal client profile.
- **analyze-conversations** — Read past prospect/client conversations and surface patterns for evidence-based coaching.
- **services** — Define what you offer, how you deliver it, and how your services relate as an ecosystem.
- **voice** — *(coming soon)* Articulate your tone and communication style.

### Document Builder Skills *(coming soon)*

- **questionnaire** — Client intake questionnaire tailored to your services.
- **pitch** — Pitch document based on your positioning and ICP.
- **outbound** — Outbound outreach messaging grounded in your voice and value prop.
- **inbound** — Inbound response templates for when prospects come to you.

### Evaluation Pipeline

Optional quality assurance system. When activated (`evaluate-session`), every coaching response passes through an evaluator → skeptic judge loop before you see it. Invisible when working — responses just get sharper.

## Knowledge Folder

All discovery outputs are saved to `unknown-creatives-coach-knowledge/` in your workspace. This folder is your complete business identity — portable, human-readable, and editable.

## Memory

The plugin captures corrections, preferences, and insights during coaching. Stored in `unknown-creatives-coach-knowledge/memory.md` and used to improve future sessions.

## Setup

1. Install the plugin in Claude Code
2. Select a folder where you want your knowledge files saved
3. Start a conversation — the coach will guide you from there

## Tips

- Bring existing materials (past proposals, SOWs, client briefs, website copy) for better results
- Push back on the coach if something doesn't sound like you — that's how the memory learns
- You don't need to go in order — start with whatever area feels most unclear
