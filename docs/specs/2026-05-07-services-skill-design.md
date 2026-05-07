# Services Skill Design

Define what creatives offer, how they deliver it, and how their services relate to each other as a system. The services skill is the bridge between upstream identity and positioning work and downstream documents and pricing — it turns "who you are and who you serve" into something concrete and sellable.

## Problem Statement

A creative professional can have clear positioning and a well-defined ideal client profile, yet still struggle to articulate what they actually offer in concrete terms — packages, deliverables, process, engagement models, scope boundaries. Without defined services, every new project is scoped from scratch, pricing is ad hoc, scope creep is constant, and the creative risks becoming an embedded IC rather than a consultant.

The existing skill chain (design-your-business, design-your-life, positioning, ICP) surfaces identity and market position but stops short of turning those into an actionable service architecture. The services skill fills that gap.

## Context

```
Builds on: positioning.md, ideal-client-profile.md, business-identity.md, life-design.md
Output:     services.md
Feeds into: pricing (future skill), document builders (questionnaire, pitch, outbound, inbound)
```

### What this skill does

- Guide the user through defining what they offer and how they deliver it
- Help them see how their services relate to each other as an ecosystem
- Produce a concrete service architecture document a client could understand and the creative can sell

### What this skill does NOT do

- Set specific prices or rates (future pricing skill)
- Define value propositions or market positioning (positioning skill)
- Define who the services are for (ICP skill)
- Build client-facing documents from the services (document builder skills)

### The feedback loop

Defining services may reveal that positioning needs updating or that the ICP is too broad. The skill surfaces these signals explicitly and notes them in the output for the user to act on, without forcing a detour mid-session. Example: "Your positioning says you do X, but looking at your services, you're really offering Y. That's worth revisiting in `positioning.md`."

## Design

### Approach: Core Flow + Adaptive Lenses

The skill has a four-stage core flow that every user moves through, with three coaching lenses that adjust the tone and questions based on where the user actually is. The lens is not selected once at the start — the coach continuously reassesses which lens fits based on the user's responses and shifts mid-conversation when needed.

### Core Flow

#### Stage 1: Inventory

Surface the raw material — what does the user currently do (or want to do) for clients?

The coach's job is to get everything on the table before organizing anything. Some users arrive with a clear list; others describe work in terms of activities ("I design websites") rather than services ("I offer a 6-week brand-to-web engagement for early-stage startups"). The coach captures both — what they name and what they describe — because the gap between those two is often where the real work is.

Key coaching moves:

- "Walk me through the last 3-5 projects you did. What did the client hire you for, and what did you actually deliver?"
- "If someone asked you right now what you offer, what would you say?" (then probe the gaps)
- If upstream work exists: "Your positioning says you do [X] for [Y]. What does that actually look like as a service someone can buy?"

The output of this stage is a rough inventory — not organized yet, just named and described.

#### Stage 2: Architecture

Organize the inventory into a service ecosystem. The coach helps the user see how their services relate to each other — and identifies what's missing or doesn't belong.

Key questions the architecture answers:

- **What's the flagship?** The primary offering — highest value, most representative of their best work.
- **What's the entry point?** Low-commitment offerings that get new clients in the door (a talk, a workshop, an audit).
- **What's the progression?** How clients move from entry point to flagship to ongoing relationship.
- **What doesn't fit?** Services that don't connect to the rest of the ecosystem — either integrate or consciously drop.

The coach reads `references/services/ecosystem-types.md` before this stage and uses it as a palette of patterns to present — not a prescription. If the user's services don't form a natural ecosystem (some creatives genuinely have independent offerings), the coach names that honestly rather than forcing a narrative.

#### Stage 3: Delivery

For each service in the architecture, define how it actually works. The coach helps the user articulate the client experience from first contact to final deliverable.

Dimensions to cover for each service:

- **Process/phases** — stages of the engagement from kickoff to handoff
- **Deliverables** — what the client receives (specific: "a brand book" not "branding")
- **Timeline** — typical duration and what affects it
- **Engagement model** — how the client works with the creative during this service (embedded, advisory, async, workshop-based, etc.)
- **Scope boundaries** — what's explicitly included and excluded. This is the IC trap protection. If the user can't articulate where a service ends, they're at risk of scope creep.

The coach reads `references/services/delivery-patterns.md`, `references/services/engagement-models.md`, and `references/services/scope-patterns.md` selectively during this stage. Every service gets the same structural depth — the amount of content comes from what the user provides, not from the coach deciding which services matter more.

#### Stage 4: Fit Check

Step back and evaluate the whole service architecture against upstream knowledge. This is where the feedback loop lives.

The coach checks:

- **Positioning fit** — Do these services deliver on the positioning statement? Misalignments are named and referenced back to `positioning.md`.
- **ICP fit** — Are these services designed for the ideal client segments? Would the ideal client buy these? Reference specific segments in `ideal-client-profile.md`.
- **Energy fit** — Do these services align with what energizes the user? Reference energy patterns in `business-identity.md`.
- **Life design fit** — Do the engagement models respect the user's life constraints? Reference constraints in `life-design.md`.

The fit check surfaces tensions and asks the user what they want to do about them. Some tensions are intentional trade-offs; others are blind spots. The coach does not force changes.

### Lenses

Three coaching lenses that adjust how the coach navigates the core flow. The lens is continuously assessed — the coach watches for signals that the user is in a different place than they initially presented and shifts naturally without announcing the change.

#### Builder

For creatives who don't have defined services. The coach leads more, provides more scaffolding, and draws more heavily from the reference knowledge base to show what's possible.

Tone: "Let's design this."

Signals:

- User can't name their services, only activities
- No consistent delivery process across projects
- Every project was scoped from scratch
- User describes themselves by skills, not outcomes

#### Sharpener

For creatives who have services but they're vague, overlapping, or inconsistent. The coach follows more, works with what exists, and helps the user see the shape that's already there.

Tone: "Let's make this clearer."

Signals:

- User can name services but can't describe the delivery model
- Services overlap or have unclear boundaries
- User has a process but hasn't articulated it
- Scope creep is a recurring problem

#### Restructurer

For creatives whose business is changing — new ICP, new positioning, new market conditions, new life constraints. The coach starts with what changed and works forward.

Tone: "Let's redesign this."

Signals:

- User explicitly says things have changed or need to change
- Upstream knowledge files have been recently revised
- Services don't match current positioning or ICP
- User is entering or leaving a specific niche

#### Lens shifting

If the coach starts in sharpener mode but discovers the user's services are actually just activities with names on them, it shifts to builder mode. The shift is expressed through different questions — the coach never announces "switching to builder lens." The skill instructs the coach to watch for these transitions throughout the conversation.

### Reference Knowledge Base

A set of reference files in `references/services/` that the skill reads selectively based on the current conversation stage. Each file covers one knowledge domain to keep context focused.

| File | Domain | Read when |
|------|--------|-----------|
| `engagement-models.md` | How clients buy and work with you — retainer, project-based, embedded, advisory, productized, parametric, value-based, and more | Stage 2 and Stage 3 when discussing how clients engage |
| `delivery-patterns.md` | How work gets delivered — phased, sprint-based, async deliverable, workshop-driven, train-the-team, embedded partnership, and more | Stage 3 when defining process and phases |
| `ecosystem-types.md` | How services relate — tiered, ladder, modular, singular, hub-and-spoke, and more | Stage 2 when organizing the service ecosystem |
| `scope-patterns.md` | Scope boundaries, the IC trap, engagement-ending criteria, handoff patterns, in/out definitions | Stage 3 when defining scope boundaries |

Each reference file follows a consistent format per pattern:

- Name and brief description
- When it works well (what kind of creative, what kind of client)
- When it doesn't work (anti-patterns, warning signs)
- Example of what it looks like in practice

The reference knowledge base is extensible — new files can be added without changing the skill structure.

Reference file reads are triggered by the SKILL.md instructions — the skill file specifies which reference file to read before entering each stage. The coach does not decide on its own which reference files to load; the skill file makes this explicit (e.g., "Before Stage 2, read `references/services/ecosystem-types.md`").

### Output Structure

When all four stages have been explored, the skill produces a Service Architecture Summary. The coach asks before generating: "Ready for me to put together your service architecture, or is there more you want to work through?"

#### services.md structure

```markdown
# Service Architecture

## My Services

[2-3 sentence narrative overview of what the user offers and how their services
fit together. Natural voice — something they could say to a peer.]

## Service Ecosystem

| Role | Service |
|------|---------|
| Flagship | [name] |
| Entry Point | [name] |
| Ongoing | [name] |

Ecosystem type: [type from reference material] — [brief explanation of why
this pattern fits the user's situation].

[If services don't form a natural ecosystem, state that honestly.]

## Services

### [Service Name]

- **ICP segment:** See `ideal-client-profile.md` → "[specific segment name]"
- **Deliverables:** [specific, tangible outputs]
- **Process:** [phases with durations]
- **Timeline:** [typical duration and variables]
- **Engagement model:** [how the client works with the creative]
- **Scope boundaries:**
  - Includes: [what's in]
  - Excludes: [what's explicitly out]

### [Service Name]

[same structure, same depth — every service gets equal treatment]

### [Service Name]

[same structure, same depth]

## Gaps and Tensions

[Honest assessment of misalignments. Each gap references the relevant upstream
knowledge file rather than restating its content.]

- [tension] — see `positioning.md`
- [tension] — see `business-identity.md` → "Energy Map"
- [tension] — see `life-design.md` → "[specific constraint]"

## Signals for Downstream Work

- **Pricing:** [notes on pricing considerations for future pricing skill]
- **Document builders:** [which services need client-facing materials first]
- **Voice:** [if service language diverged from positioning language, note it]
```

#### Formatting rules

- Tables for ecosystem overview and structured comparisons
- Lists for service card dimensions
- `###` headings for each individual service
- Prose for narrative sections (My Services, Gaps and Tensions, Signals)
- Every service gets the same structural depth

#### Cross-referencing rules

- If information has a home in another knowledge file, reference the file and specific section — never restate
- ICP references must point to the specific segment when multiple ICPs exist
- Upstream references use the format: `file.md` → "Section Name"
- This prevents drift between files and keeps the knowledge system connected

## System-Wide Principles

These principles are introduced by the services skill but apply to all skills, current and future. They should be incorporated into `agents/shared/agent-protocol.md` and retrofitted to existing skills as separate work.

### Single Source of Truth

Each knowledge file is authoritative for its domain. When information from one domain appears in another skill's output, it must be referenced (with specific section pointers when applicable), never restated. This prevents drift between files and ensures both the user and the AI treat the knowledge system as an interconnected whole.

### Cross-Reference Format

When a knowledge file references another:

- Point to the file: `positioning.md`
- Point to a specific section when needed: `ideal-client-profile.md` → "Growth-stage SaaS teams"
- Never copy content from the referenced file into the current file

## Coaching Principles

Rules governing how the coach behaves within the services skill.

**Activities are not services.** Most creatives describe what they do in terms of activities. A service is an activity packaged into something a client can buy — with defined scope, deliverables, process, and an ending. The coach's core job is helping the user make that leap.

**Present options, never prescribe models.** The reference knowledge base gives the coach fluency across many models. The coach presents relevant options and lets the user choose. It never says "you should use a tiered model." It says "here are a few ways to structure this — here's what I think fits, but let me show you the alternatives."

**Continuously reassess the lens.** The coach watches for signals that the user is in a different place than they initially presented. Someone who says "I have three services" but can't describe the delivery model for any of them is in builder territory. The shift is expressed through different questions, never announced.

**Scope boundaries are protection, not paperwork.** If a user hand-waves on scope, the coach pushes. Every service needs a clear "in" and "out." The coach frames this as protection against scope creep and the IC trap, not as bureaucratic overhead.

**Don't force an ecosystem that isn't there.** Some creatives have independent services that don't connect. The coach names this honestly rather than forcing false connections.

**Cross-reference, never restate.** When the output touches information that lives in another knowledge file, reference the specific file and section. Never copy it into `services.md`.

**Draw from reference material, not defaults.** When presenting models or patterns, the coach reads from the relevant reference file in `references/services/`. It does not fall back on generic examples from training data.

**Services are hypotheses, not commitments.** Frame the service architecture as something the user is testing, not carving in stone. This reduces pressure and makes the user more willing to be specific.

**If defining a service deflates the user, that's a signal.** The energy map from `business-identity.md` matters here. If the user lights up on one service but goes flat on another, the coach names it. The services should energize the user, not just look good on paper.

## Implementation Notes

### New files to create

- `plugins/unknown-creatives-coach/skills/services/SKILL.md` — the skill file
- `plugins/unknown-creatives-coach/references/services/engagement-models.md`
- `plugins/unknown-creatives-coach/references/services/delivery-patterns.md`
- `plugins/unknown-creatives-coach/references/services/ecosystem-types.md`
- `plugins/unknown-creatives-coach/references/services/scope-patterns.md`
- `plugins/unknown-creatives-coach/evals/services/` — evaluation criteria (per-thread)

### Files to modify

- `plugins/unknown-creatives-coach/agents/creative-business-consultant/abilities.md` — services skill is already listed but needs reference knowledge base instructions added
- `plugins/unknown-creatives-coach/agents/shared/agent-protocol.md` — add Single Source of Truth and Cross-Reference Format principles
- `README.md` — move services from "Coming Soon" to the skills table

### Future work (not part of this implementation)

- Retrofit DRY cross-referencing to existing skills (positioning, ICP, design-your-business, design-your-life)
- Pricing skill (new skill, informed by services output)
- YAML frontmatter for knowledge files (system-wide improvement for machine scannability)
- ICP skill update to support named segments for multi-ICP cross-referencing
