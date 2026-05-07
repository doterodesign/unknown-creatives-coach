# Services Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Implement the services discovery skill — a coaching conversation that helps creatives define what they offer, how they deliver it, and how their services relate as an ecosystem.

**Architecture:** Core flow + adaptive lenses pattern. Four conversation stages (inventory → architecture → delivery → fit check) with three coaching lenses (builder/sharpener/restructurer) that shift dynamically based on user signals. Reference knowledge base provides curated patterns the coach draws from rather than defaulting to training data.

**Tech Stack:** Markdown skill files, markdown reference files, markdown eval files. No code — this is a prompt engineering implementation within the existing Claude Code plugin structure.

**Spec:** `docs/specs/2026-05-07-services-skill-design.md`

---

## File Structure

### New files

| File | Responsibility |
|------|---------------|
| `plugins/unknown-creatives-coach/skills/services/SKILL.md` | The skill file — coaching flow, lenses, stage instructions, output template |
| `plugins/unknown-creatives-coach/references/services/ecosystem-types.md` | Reference: how services relate to each other (tiered, ladder, modular, etc.) |
| `plugins/unknown-creatives-coach/references/services/engagement-models.md` | Reference: how clients buy and work with you (retainer, project, embedded, etc.) |
| `plugins/unknown-creatives-coach/references/services/delivery-patterns.md` | Reference: how work gets delivered (phased, sprint, workshop, etc.) |
| `plugins/unknown-creatives-coach/references/services/scope-patterns.md` | Reference: scope boundaries, IC trap, handoff patterns |
| `plugins/unknown-creatives-coach/evals/services/inventory.md` | Eval: Stage 1 — surfacing raw material |
| `plugins/unknown-creatives-coach/evals/services/architecture.md` | Eval: Stage 2 — organizing into ecosystem |
| `plugins/unknown-creatives-coach/evals/services/delivery.md` | Eval: Stage 3 — defining how services work |
| `plugins/unknown-creatives-coach/evals/services/fit-check.md` | Eval: Stage 4 — alignment with upstream knowledge |

### Modified files

| File | Change |
|------|--------|
| `plugins/unknown-creatives-coach/agents/shared/agent-protocol.md` | Add Single Source of Truth and Cross-Reference Format principles |
| `plugins/unknown-creatives-coach/agents/creative-business-consultant/abilities.md` | Add reference knowledge base read instructions for services skill |
| `README.md` | Move services from "Coming Soon" to skills table |

---

## Task 1: Add DRY Cross-Referencing Principles to Agent Protocol

**Files:**
- Modify: `plugins/unknown-creatives-coach/agents/shared/agent-protocol.md:46-49` (after Knowledge Folder Ownership section)

This goes first because the services skill and its evals reference these principles. Every subsequent task depends on this being in place.

- [ ] **Step 1: Add Single Source of Truth and Cross-Reference Format sections**

Open `plugins/unknown-creatives-coach/agents/shared/agent-protocol.md` and add the following after the `## Knowledge Folder Ownership` section (after line 49) and before `## Cross-Agent Handoff`:

```markdown
## Single Source of Truth

Each knowledge file is authoritative for its domain. When information from one domain appears in another skill's output, it must be referenced — never restated. This prevents drift between files and ensures both you and the user treat the knowledge system as an interconnected whole.

Rules:
- If information has a home in another knowledge file, reference the file and specific section
- ICP references must point to the specific segment when multiple ICPs exist
- Use the format: `file.md` → "Section Name"
- Never copy content from a referenced file into the current file

Examples:
- In `services.md`, do not restate who a service is for. Write: `See ideal-client-profile.md → "Growth-stage SaaS teams"`
- In the Gaps and Tensions section, do not restate energy patterns. Write: `See business-identity.md → "Energy Map"`
- If positioning misaligns with services, do not quote the positioning statement. Write: `See positioning.md — may need revision to reflect current service architecture`
```

- [ ] **Step 2: Verify the edit reads correctly in context**

Read the full file to confirm the new section integrates cleanly between Knowledge Folder Ownership and Cross-Agent Handoff. Confirm no duplicate headings or broken formatting.

- [ ] **Step 3: Commit**

```bash
git add plugins/unknown-creatives-coach/agents/shared/agent-protocol.md
git commit -m "feat(agent-protocol): add single source of truth cross-referencing rules"
```

---

## Task 2: Create the Ecosystem Types Reference File

**Files:**
- Create: `plugins/unknown-creatives-coach/references/services/ecosystem-types.md`

Start with reference files because the SKILL.md will instruct the coach to read them. Build the palette before writing the instructions that use it.

- [ ] **Step 1: Create the references/services/ directory**

```bash
mkdir -p plugins/unknown-creatives-coach/references/services
```

- [ ] **Step 2: Write ecosystem-types.md**

Create `plugins/unknown-creatives-coach/references/services/ecosystem-types.md` with the following content:

```markdown
# Ecosystem Types

How services relate to each other. The coach presents relevant patterns as options — never prescribes one.

---

## Ladder

Services arranged from low-commitment to high-commitment. Clients enter at the bottom and progress upward as trust and need grow.

**When it works well:**
Creatives who serve a consistent client type across different engagement depths. The entry point (a workshop, an audit, a sprint) proves value and naturally leads to larger work. Works especially well when the creative's ICP has a long consideration cycle — the ladder gives prospects a low-risk way to start.

**When it doesn't work:**
When the entry-point service and the flagship serve fundamentally different buyers. A $500 workshop for junior designers and a $50K strategy engagement for VPs of Product are not a ladder — they're two separate businesses.

**Example:**
A design systems consultant offers: (1) a half-day design systems health check, (2) a 6-week architecture engagement, (3) an ongoing advisory retainer. Clients start with the health check, which surfaces the problems the architecture engagement solves, which creates the ongoing relationship the retainer sustains.

---

## Tiered (Papa Bear)

Multiple versions of the same core service at different scope levels. The client picks the tier that matches their budget and commitment. Typically three tiers: comprehensive (everything the creative can do), standard (what the client came for), and lightweight (minimal viable engagement for budget-constrained clients).

**When it works well:**
Creatives whose core offering can genuinely scale up or down without changing its nature. A brand identity engagement that can be a full system or a focused sprint is a good candidate. Works well for creatives who want to serve a range of budgets without creating entirely different services.

**When it doesn't work:**
When the tiers are artificially constructed — the creative removes value from the top tier to create lower ones, rather than each tier being genuinely useful at its scope. Also fails when the lowest tier creates more work than it's worth or sets wrong expectations.

**Example:**
A brand designer offers: (1) Brand Foundation — logo, color palette, type selection, one-page guidelines, (2) Brand System — everything in Foundation plus component library, templates, and detailed usage guidelines, (3) Brand Universe — everything in System plus motion guidelines, environmental design, and team training.

---

## Modular (Mix and Match)

Independent services that clients combine based on their specific needs. No fixed progression — the client picks what they need.

**When it works well:**
Creatives with genuinely distinct capabilities that different clients need in different combinations. A creative who does brand strategy, content design, and web development might have clients who need all three or just one. Works well when the creative's ICP is broad enough to have varied needs.

**When it doesn't work:**
When the modules actually depend on each other but are presented as independent. If the web development only works well after the brand strategy, that's a ladder, not a modular system — and selling them independently creates bad outcomes.

**Example:**
A creative studio offers: (1) Brand Strategy, (2) Visual Identity, (3) Web Design & Development, (4) Content Strategy. A startup might buy all four. An established company rebranding might buy only Visual Identity and Web. Each module has its own scope, timeline, and deliverables.

---

## Hub and Spoke

One central service (the hub) that all clients go through, with specialized follow-on services (spokes) based on what the hub reveals.

**When it works well:**
Creatives whose diagnostic or discovery process is their core differentiator. The hub is the discovery — it surfaces what the client actually needs, and the spokes are the solutions. Works well when clients often don't know what they need before engaging.

**When it doesn't work:**
When the hub is thin or perfunctory — a sales call disguised as discovery. The hub needs to deliver genuine value on its own, even if the client never engages a spoke.

**Example:**
A design systems consultant offers a Design Systems Audit (the hub) that evaluates the client's current system maturity, identifies gaps, and recommends a path forward. Based on the audit findings, the client can engage for: (a) Token Architecture, (b) Component Library Strategy, (c) Governance & Process Design, or (d) Team Training. The audit stands on its own as a deliverable — not every client proceeds to a spoke.

---

## Singular (One Offering, Scaled)

One core service that flexes in scope based on the client's situation. Not tiered — the creative shapes the engagement to fit, adjusting time, intensity, and depth.

**When it works well:**
Creatives who do one thing exceptionally well and want to keep their business focused. The offering is the same expertise applied differently depending on context. Works well for specialists who want to avoid the complexity of managing multiple service lines.

**When it doesn't work:**
When the creative actually does substantively different types of work but is forcing them under one umbrella. Also struggles when prospects can't understand what "it depends" means in practice — some structure helps clients self-select.

**Example:**
A design systems consultant positions as: "I help teams build and scale design systems." The engagement might be a 2-day workshop, a 3-month embedded partnership, or a year-long advisory relationship — but it's all the same core expertise applied at different scales. Scope is shaped in conversation, not selected from a menu.

---

## Parametric (Slider Model)

Services defined by adjustable dimensions rather than fixed packages. The creative has a set of sliders — time, intensity, involvement, focus area — and each engagement is configured by adjusting them.

**When it works well:**
Creatives who serve a wide range of client situations and budgets. The parametric model lets them say "how much budget and time do you have? I can configure something that fits." Works especially well for consultants and advisors whose value scales with involvement.

**When it doesn't work:**
When the creative needs clear boundaries to prevent scope creep. The flexibility that makes parametric models attractive can also make scope hard to pin down. Requires the creative to be skilled at scoping on the fly.

**Example:**
A design consultant has four dimensions: (1) Duration — from a single session to 12 months, (2) Involvement — from async advisory to embedded team member, (3) Focus — from strategy only to strategy + hands-on execution, (4) Team scope — from working with one lead to training the whole team. A prospect says they have a $15K budget and 6 weeks. The consultant configures: 6 weeks, advisory involvement, strategy focus, working with the design lead. Another prospect has $80K and 6 months — same consultant, different configuration.

---

## Independent (No Ecosystem)

Services that don't connect into a progression or system. Each offering stands alone, serves potentially different clients, and doesn't lead to the others.

**When it works well:**
Creatives whose skills span genuinely different domains or who are still exploring what they want to focus on. Acknowledging independence is more honest than forcing a narrative. Also valid for creatives in transition — they may consolidate later, but right now the services don't connect.

**When it doesn't work:**
As a permanent state for someone who wants referrals and repeat business. Independent services make it hard for clients to come back for more or to refer others, because there's no clear "next step." If the creative wants an ecosystem, independent services are a starting point to evolve from, not a destination.

**Example:**
A creative offers: (1) Brand identity design for startups, (2) Workshop facilitation for corporate teams, (3) Illustration for editorial publications. These serve different clients, require different sales approaches, and don't naturally lead to each other. The creative enjoys all three and isn't ready to drop any.
```

- [ ] **Step 3: Commit**

```bash
git add plugins/unknown-creatives-coach/references/services/ecosystem-types.md
git commit -m "feat(references): add ecosystem types reference for services skill"
```

---

## Task 3: Create the Engagement Models Reference File

**Files:**
- Create: `plugins/unknown-creatives-coach/references/services/engagement-models.md`

- [ ] **Step 1: Write engagement-models.md**

Create `plugins/unknown-creatives-coach/references/services/engagement-models.md` with the following content:

```markdown
# Engagement Models

How clients buy and work with you during a service. The coach presents relevant models as options — never prescribes one.

---

## Project-Based (Fixed Scope)

A defined scope of work with a clear start, deliverables, and end date. The client pays for a specific outcome.

**When it works well:**
Creatives who do distinct, completable work — a brand identity, a website, a design system architecture. Works when both sides can agree on scope upfront and the creative can estimate timeline and effort accurately. Clients who value predictability prefer this.

**When it doesn't work:**
When the work is inherently exploratory or the scope can't be defined upfront. Also struggles when the creative underestimates scope — fixed-scope with vague boundaries is a recipe for scope creep.

**Example:**
"This is a 6-week engagement. You get a token architecture, component API spec, and migration plan. We kick off with a 2-hour discovery session, I present the architecture in week 3, and we finalize documentation by week 6."

---

## Retainer (Ongoing Access)

The client pays a recurring fee for ongoing access to the creative's time and expertise. Scope is defined by hours or availability rather than specific deliverables.

**When it works well:**
Creatives whose clients need ongoing guidance, not just one-time deliverables. Works well after a project engagement — the project builds the system, the retainer maintains it. Also works for advisory relationships where the value is access to expertise.

**When it doesn't work:**
When the retainer becomes an excuse for undefined scope. "I'm on retainer" can easily turn into "I'm an IC doing whatever comes up." Retainers need clear boundaries around availability, response time, and what types of work are included.

**Example:**
"After the architecture engagement, I offer a monthly advisory retainer. Four hours per month — we do a standing weekly call where I review your team's progress, unblock decisions, and keep the system on track. Anything beyond the four hours is scoped as a separate project."

---

## Embedded Partnership

The creative works alongside the client's team as a temporary team member. They're in the meetings, in the Slack channels, pairing on decisions.

**When it works well:**
Complex work where context matters more than deliverables — organizational change, design system adoption, team capability building. Works when the creative's value is in real-time judgment, not just artifacts. Clients with a strong internal team who need augmentation, not replacement.

**When it doesn't work:**
When "embedded" becomes "employee without benefits." The creative must maintain their advisory role — they're there to guide and build capability, not to execute tickets. Without clear scope boundaries, this model is the highest risk for the IC trap.

**Example:**
"I embed with your team 3 days a week for 3 months. I attend your design critiques, pair with engineers on token implementation, and facilitate the governance model. By the end, your team can maintain and extend the system without me."

---

## Advisory (Strategic Guidance)

The creative provides expert guidance without doing the execution work. They consult, review, advise, and direct — but the client's team does the building.

**When it works well:**
Experienced creatives whose judgment and pattern recognition are their primary value. Works well for clients who have capable teams but lack senior expertise or an outside perspective. Lower time commitment allows the creative to serve more clients simultaneously.

**When it doesn't work:**
When the client actually needs execution help but bought advisory because it was cheaper. Also fails when the client's team lacks the skill to execute on the advice — the creative ends up frustrated watching their recommendations get misimplemented.

**Example:**
"I serve as your external design systems advisor. Biweekly 90-minute sessions where we review your team's work, I make architectural recommendations, and we align on direction. I don't build components — your team does. I make sure they're building the right things the right way."

---

## Workshop-Based

Engagement delivered as a facilitated workshop — half-day, full-day, or multi-day. Structured, time-boxed, and designed to produce a specific output by the end.

**When it works well:**
Creatives who are strong facilitators and can compress value into a focused session. Works as an entry-point offering — low commitment, high energy, produces a tangible artifact the client can use immediately. Also works for team alignment problems where getting everyone in the same room is the primary value.

**When it doesn't work:**
When the problem requires sustained work over time, not a burst of activity. Workshops produce starting points, not finished products. If the client expects a complete deliverable from a half-day session, expectations are misset.

**Example:**
"A full-day Design Systems Strategy Workshop. Your leadership team, your design leads, and your engineering leads in a room for 8 hours. We map your current state, align on principles, define the token architecture, and leave with a prioritized roadmap. You can execute it yourselves or bring me in for the next phase."

---

## Productized Service

A standardized service with fixed scope, fixed price, and a repeatable process. The creative delivers the same thing to every client with minimal customization.

**When it works well:**
Creatives who have refined their process enough to make it predictable. Works when the client problem is common enough that a standardized solution fits. Scales well — the creative can serve more clients because each engagement is efficient.

**When it doesn't work:**
When the creative's value is in customization and deep understanding of each client's unique situation. Also fails when the productized version is too rigid to handle the real variation in client needs.

**Example:**
"Brand Sprint: a 5-day brand identity engagement. Day 1: discovery session. Day 2-3: I design three directions. Day 4: we pick and refine. Day 5: I deliver final files and a usage guide. $8,000 flat fee. Same process every time."

---

## Async Deliverable

The creative works independently and delivers artifacts without synchronous collaboration. Communication happens through written briefs, recorded feedback, and document exchanges.

**When it works well:**
Creatives who do their best work independently and clients who are busy, time-zone-distant, or prefer written communication. Works well for execution-heavy work where the brief is clear and the creative doesn't need ongoing input.

**When it doesn't work:**
When the work requires tight collaboration, frequent course corrections, or the client can't articulate what they want in writing. Also struggles when the creative needs real-time context to make good decisions.

**Example:**
"You fill out my intake questionnaire and share your existing brand materials. I spend two weeks designing your component library. You get a Loom walkthrough of every decision plus the files. One round of written feedback, then final delivery."

---

## Train-the-Team

The engagement is focused on building the client team's capability rather than delivering a work product. The creative teaches, mentors, and coaches.

**When it works well:**
Experienced creatives who enjoy teaching and whose clients need to own the work long-term. Works well as a complement to other models — after building the system, train the team to maintain it. Also works as a standalone offering for teams that have the talent but lack the knowledge.

**When it doesn't work:**
When the client wants deliverables, not education. Also fails when the team doesn't have the baseline skill to absorb the training — there's a minimum capability threshold below which training doesn't stick.

**Example:**
"A 4-week Design Tokens Bootcamp for your team. Week 1: foundations — what tokens are, how they work, why they matter. Week 2: hands-on — your team builds the first token layer with me pairing. Week 3: advanced — theming, responsive tokens, multi-brand architecture. Week 4: governance — how to maintain and evolve the system. By the end, your team owns the system."

---

## Hybrid / Phased

An engagement that combines multiple models across phases. Typically starts with one model and transitions to another as the work evolves.

**When it works well:**
Complex engagements where different phases require different working styles. The discovery phase might be workshop-based, the build phase might be embedded, and the sustain phase might be advisory. Lets the creative match the model to the moment.

**When it doesn't work:**
When the transitions between phases aren't clearly defined. The client needs to understand what changes at each phase — expectations, involvement level, deliverables, and cost. Without clarity, phase transitions feel arbitrary.

**Example:**
"Phase 1 (weeks 1-2): Discovery Workshop — we align on goals and map current state. Phase 2 (weeks 3-8): Embedded Build — I work with your team 3 days/week to build the architecture. Phase 3 (months 3-6): Advisory Retainer — biweekly check-ins to support your team as they extend the system."
```

- [ ] **Step 2: Commit**

```bash
git add plugins/unknown-creatives-coach/references/services/engagement-models.md
git commit -m "feat(references): add engagement models reference for services skill"
```

---

## Task 4: Create the Delivery Patterns Reference File

**Files:**
- Create: `plugins/unknown-creatives-coach/references/services/delivery-patterns.md`

- [ ] **Step 1: Write delivery-patterns.md**

Create `plugins/unknown-creatives-coach/references/services/delivery-patterns.md` with the following content:

```markdown
# Delivery Patterns

How work gets delivered within a service — the process, phases, and rhythm of an engagement. The coach presents relevant patterns as options — never prescribes one.

---

## Linear Phased

Work moves through sequential phases — each one completes before the next begins. Discovery → Design → Build → Deliver.

**When it works well:**
Work with clear dependencies where each phase informs the next. Clients who value predictability and want to approve at each gate before proceeding. Works well for project-based engagements with defined deliverables.

**When it doesn't work:**
When the work is iterative by nature and forcing linearity creates waste. Also fails when the creative can't reliably estimate phase durations — the linear promise becomes a linear fiction.

**Example:**
"Week 1-2: Discovery — I interview stakeholders and audit your current system. Week 3: Architecture — I present the token structure and get alignment. Week 4-6: Build — I implement the architecture and document everything. Week 7: Handoff — we walk through the system together and I transfer ownership."

---

## Sprint-Based

Work delivered in short, focused bursts (1-2 weeks) with a defined output per sprint. Scope is flexible — each sprint is scoped at the start based on what was learned in the previous one.

**When it works well:**
Exploratory or complex work where the full scope can't be known upfront. Clients comfortable with iterative progress rather than a fixed plan. Works well for embedded and advisory engagements where priorities shift.

**When it doesn't work:**
When the client expects a fixed price for a fixed deliverable. Sprint-based delivery is inherently flexible — if the client needs predictability, sprints create anxiety. Also fails if sprint reviews get skipped and the work drifts.

**Example:**
"We work in 2-week sprints. At the start of each sprint, we agree on 2-3 priorities. At the end, I present what's done and we adjust priorities for the next sprint. Typical engagement is 4-6 sprints."

---

## Workshop-Driven

The core delivery happens in facilitated workshop sessions. Between workshops, the creative synthesizes outputs and prepares the next session.

**When it works well:**
Work that requires group alignment, shared decision-making, or collaborative exploration. The value is in getting the right people together with the right structure. Works well as entry-point delivery because workshops feel low-commitment but produce high-value artifacts.

**When it doesn't work:**
When the work is primarily execution — you can't workshop your way to a finished component library. Also fails when key stakeholders can't commit to workshop attendance.

**Example:**
"Three half-day workshops over 3 weeks. Workshop 1: Current State Mapping — we document what exists. Workshop 2: Future State Design — we design the target architecture. Workshop 3: Roadmap & Governance — we plan how to get there and who owns what. Between sessions, I synthesize outputs and prepare materials."

---

## Async Delivery

Work delivered through documents, recordings, and written communication without requiring synchronous meetings. The creative works independently based on a brief and delivers artifacts.

**When it works well:**
Execution-heavy work where the creative has a clear brief and doesn't need ongoing input. Clients who are time-zone-distant, busy, or prefer written communication. Creatives who do their best work independently.

**When it doesn't work:**
When the work requires frequent course corrections, real-time decisions, or the brief is ambiguous. Also fails when feedback cycles are slow — async delivery depends on timely written feedback.

**Example:**
"You send me the brief and existing materials. I spend 10 business days on the work, sharing progress via Loom recordings at the halfway point. You provide written feedback within 3 business days. I deliver finals one week after feedback."

---

## Pair-and-Build

The creative works alongside a client team member in real time — pairing on decisions, co-creating artifacts, and building capability through doing.

**When it works well:**
When the goal is both the deliverable and the team's ability to maintain it. Works especially well for design systems, where the team needs to own the system long-term. The creative builds while teaching — the artifact and the capability are delivered simultaneously.

**When it doesn't work:**
When the client team member is unavailable, underqualified, or treats the pairing as observation rather than participation. Also fails when the creative bills for pairing time but could deliver faster solo — the client needs to value the capability transfer.

**Example:**
"I pair with your senior designer 3 days/week. We build the token architecture together — I lead the decisions, they learn the reasoning. By week 4, they're leading and I'm reviewing. By week 8, they own the system."

---

## Diagnostic-First

The engagement starts with a structured assessment or audit. The diagnostic deliverable stands on its own as valuable — the client can act on it independently or engage the creative for implementation.

**When it works well:**
Creatives whose primary value is seeing what others miss — pattern recognition, gap analysis, strategic assessment. Works as a hub-and-spoke entry point. Clients who aren't sure what they need get clarity before committing to a larger engagement.

**When it doesn't work:**
When the diagnostic is thin or generic — a sales pitch disguised as an audit. Also fails when the creative can't provide genuinely useful recommendations without doing the implementation work first.

**Example:**
"A 2-week Design Systems Audit. I review your current system, interview your team, and assess maturity across 6 dimensions. You get a 15-page report with specific findings, a maturity scorecard, and a prioritized roadmap of recommendations. The audit is $5K. If you want me to implement the recommendations, we scope that separately."

---

## Milestone-Based

Work organized around specific, measurable milestones rather than time or phases. Payment and progress are tied to hitting defined checkpoints.

**When it works well:**
Clients who want accountability and measurable progress. Creatives who are confident in their ability to hit targets. Works well for larger engagements where the client needs to justify ongoing investment.

**When it doesn't work:**
When milestones are poorly defined or the work is too exploratory to predict concrete checkpoints. Also creates perverse incentives if the creative rushes to hit milestones at the expense of quality.

**Example:**
"Milestone 1: Token architecture documented and approved by your team. Milestone 2: Core component library built and passing automated tests. Milestone 3: First product team migrated to the new system. Milestone 4: Governance model in place and team trained. Each milestone triggers a progress review and the next payment."
```

- [ ] **Step 2: Commit**

```bash
git add plugins/unknown-creatives-coach/references/services/delivery-patterns.md
git commit -m "feat(references): add delivery patterns reference for services skill"
```

---

## Task 5: Create the Scope Patterns Reference File

**Files:**
- Create: `plugins/unknown-creatives-coach/references/services/scope-patterns.md`

- [ ] **Step 1: Write scope-patterns.md**

Create `plugins/unknown-creatives-coach/references/services/scope-patterns.md` with the following content:

```markdown
# Scope Patterns

How to define what's in and what's out of a service. Scope boundaries protect the creative from scope creep, the IC trap, and misaligned expectations. The coach helps the user build clear boundaries — not as bureaucratic overhead, but as protection.

---

## The IC Trap

The most common scope failure for creative consultants. It happens when a consulting engagement gradually becomes an employment relationship — the creative starts executing tasks rather than providing expertise, loses their advisory role, and ends up doing whatever the client's team needs done.

**Warning signs:**
- The creative is assigned tickets or tasks rather than shaping the work
- Scope discussions stop — the creative just picks up whatever is next
- The creative's unique expertise is underutilized — they're doing work anyone could do
- The client treats them as a team member rather than an external expert
- The creative feels they can't push back on requests without risking the engagement

**How to prevent it:**
- Define the creative's role explicitly in the service scope: "I architect and advise. I don't execute tickets."
- Include an "Excludes" section in every service description that names specific activities
- Build scope check-ins into the engagement rhythm — regular moments to confirm the work matches the service definition
- Have an escalation phrase for when scope drifts: "That sounds like execution work. Let me scope that as a separate engagement."

---

## In/Out Definition

Every service should have an explicit "Includes" and "Excludes" list. The Includes list defines what the client gets. The Excludes list defines what they don't — and this is the more important list, because it sets boundaries before the engagement starts.

**How to build an In/Out list:**
- Start with what the creative actually delivers (Includes)
- Then ask: "What do clients typically assume is included but isn't?" — those are the Excludes
- Common Excludes that are often assumed: unlimited revisions, ongoing maintenance, implementation of recommendations, content creation, third-party tool costs, stakeholder management

**Example:**
```
Includes:
- Token architecture design and documentation
- Two rounds of revision based on written feedback
- Team walkthrough of the architecture
- Migration guide for existing components

Excludes:
- Component build-out or implementation
- Ongoing maintenance after handoff
- Figma library construction
- Third-party tool setup or configuration
- Stakeholder alignment beyond the design team
```

---

## Engagement-Ending Criteria

Every service should have a clear definition of when it's done. Without this, engagements drag on indefinitely — the creative keeps polishing, the client keeps asking for "one more thing," and nobody knows when to stop.

**Types of endings:**
- **Deliverable-based:** The engagement ends when specific artifacts are delivered and accepted. "Done when you have the brand book and component library in hand."
- **Time-based:** The engagement ends after a fixed duration. "This is a 6-week engagement. Whatever we accomplish in 6 weeks is the deliverable."
- **Milestone-based:** The engagement ends when specific outcomes are achieved. "Done when three product teams are successfully using the new token system."
- **Event-based:** The engagement ends when a specific event occurs. "I'm here until your new design systems lead is onboarded and up to speed."

**How to coach this:**
Ask: "How does the client know you're done? And how do you know you're done?" If the answers are different, the ending criteria aren't clear enough.

---

## Handoff Patterns

How the creative transfers ownership to the client at the end of an engagement. A clean handoff prevents the engagement from lingering and ensures the client can maintain what was built.

**Documentation handoff:**
The creative delivers comprehensive documentation that the client's team can follow independently. Works for technical deliverables — design systems, token architectures, governance models.

**Training handoff:**
The creative trains the client's team on how to maintain and extend the work. Works when the deliverable requires ongoing management and the team has the baseline skill to learn.

**Overlap handoff:**
The creative works alongside their replacement (the client's new hire or internal team member) for a transition period. Works for embedded engagements where context transfer is critical.

**Clean break handoff:**
The creative delivers the final artifacts and the engagement ends. No transition period. Works for productized services and async deliverables where the work is self-contained.

**How to coach this:**
Ask: "When you leave, what does the client need to be able to do without you? And what's your plan for getting them there?" If the creative can't answer, the handoff pattern isn't defined.

---

## Revision and Feedback Boundaries

How many rounds of revision are included, how feedback is delivered, and what happens when the client wants more than what's scoped.

**Common patterns:**
- **Fixed rounds:** "Two rounds of revision included. Additional rounds at $X per round." Clear and predictable.
- **Time-boxed feedback:** "Feedback window is 5 business days per round. After that, we proceed with what we have." Prevents stalling.
- **Written-only feedback:** "All feedback in writing — consolidated, not piecemeal. One document per round." Prevents feedback chaos.
- **Stakeholder limits:** "Feedback from one designated point of contact. If you need to consolidate internal opinions, that happens before the feedback reaches me." Prevents design-by-committee.

**How to coach this:**
Ask: "What's the worst feedback experience you've had with a client? What would have prevented it?" The answer usually reveals which boundaries matter most to the creative.
```

- [ ] **Step 2: Commit**

```bash
git add plugins/unknown-creatives-coach/references/services/scope-patterns.md
git commit -m "feat(references): add scope patterns reference for services skill"
```

---

## Task 6: Create the Services SKILL.md

**Files:**
- Create: `plugins/unknown-creatives-coach/skills/services/SKILL.md`

This is the core deliverable — the skill file that drives the coaching conversation.

- [ ] **Step 1: Create the skills/services/ directory**

```bash
mkdir -p plugins/unknown-creatives-coach/skills/services
```

- [ ] **Step 2: Write SKILL.md**

Create `plugins/unknown-creatives-coach/skills/services/SKILL.md` with the following content:

```markdown
---
name: services
description: >
  This skill should be used when the user wants to define what they offer, how they deliver
  it, and how their services relate to each other as an ecosystem. Use when someone says
  "help me define my services," "what should I offer," "I need to figure out my packages,"
  "how should I structure my offerings," "I do a bunch of different things and I need to
  organize them," "what does my service look like," "I keep scoping things from scratch,"
  or when the creative-business-consultant agent routes to services work.
  Also triggers on: "service offerings," "packages," "deliverables," "engagement model,"
  "how I deliver," "scope of work," "what do I actually sell," "my menu of services,"
  "service ecosystem," "flagship offering," "entry point service," "scope creep,"
  or any request to define, structure, or refine what the user offers as a creative professional.
---

# Services

Guide the user through defining what they offer, how they deliver it, and how their services relate to each other as a system. The goal is a concrete service architecture — not a tagline or a pitch, but a working document that describes what a client can buy and what they experience when they buy it.

This skill turns upstream identity and positioning work into something sellable. If positioning answers "what do you do and why you?" and ICP answers "for whom?" — services answers "what exactly can someone hire you to do, and how does the engagement work?"

## Context

Builds on: `positioning.md`, `ideal-client-profile.md`, `business-identity.md`, `life-design.md`
Output: `services.md`

## Starting the Conversation

**If upstream work exists:**
Start by pulling in what you know: "Based on your positioning work, you're focused on [arena]. Your ideal clients are [ICP summary reference]. Let's figure out what you actually offer them — the concrete services someone can hire you for."

**If no upstream work exists:**
Start from experience: "Let's figure out what you offer. The fastest way is to look at what you've already done. Walk me through the last 3-5 projects — what did each client hire you for, and what did you actually deliver?"

**If services.md already exists:**
Read it first. Start with: "You've already defined some services. Let me read through what you have and we can sharpen, add, or restructure from there."

## Coaching Lenses

You have three coaching lenses. Do not select one and lock in — continuously assess which lens fits based on the user's responses. Shift naturally when the conversation reveals the user is in a different place than they initially presented.

### Builder
For users who don't have defined services. They describe activities, not offerings. Lead more, scaffold more, draw from references to show what's possible. Tone: "Let's design this."

**Signals:** Can't name services (only activities). No consistent process. Every project scoped from scratch. Describes skills, not outcomes.

### Sharpener
For users who have services but they're vague, overlapping, or inconsistent. Follow more, work with what exists, help them see the shape. Tone: "Let's make this clearer."

**Signals:** Can name services but can't describe delivery. Services overlap. Has a process but hasn't articulated it. Scope creep is recurring.

### Restructurer
For users whose business is changing. Start with what changed and redesign. Tone: "Let's redesign this."

**Signals:** User says things have changed. Upstream files recently revised. Services don't match positioning or ICP. Entering or leaving a niche.

### Lens shifting
If you start in sharpener mode but discover the user's "services" are actually just activities with names, shift to builder. If a restructurer reveals they don't have a baseline to restructure from, shift to builder. Express the shift through different questions — never announce it.

## Stage 1: Inventory

Surface the raw material. Get everything on the table before organizing.

Some users arrive with a clear list. Others describe work as activities ("I design websites") rather than services ("I offer a 6-week brand-to-web engagement"). Capture both — the gap between what they name and what they describe is where the work is.

**Key coaching moves:**

- "Walk me through the last 3-5 projects. What did the client hire you for, and what did you actually deliver?"
- "If someone asked you right now what you offer, what would you say?"
- "Are there services you want to offer but haven't yet? Things you've been thinking about adding?"
- If upstream work exists: "Your positioning says you do [X] for [Y]. What does that actually look like as a service someone can buy?"

**What you're listening for:**
- Named services vs. activity descriptions (the activity-to-service gap)
- Consistency across projects (same process or different every time)
- Energy signals (which services excite them vs. which they mention reluctantly)
- Pricing signals (capture for downstream work but don't resolve here)

Produce a rough inventory — named, described, not yet organized.

## Stage 2: Architecture

Before starting this stage, read `references/services/ecosystem-types.md`.

Organize the inventory into a service ecosystem. Help the user see how their services relate and identify what's missing or doesn't belong.

**Key questions:**

- "Looking at everything you've listed, which one is your flagship — the offering that represents your best work and highest value?"
- "What's the lowest-commitment way someone could start working with you? A workshop, an audit, a single session?"
- "How do clients move between these services? Does someone who does the workshop naturally progress to the bigger engagement?"
- "Is anything on this list that doesn't connect to the rest? Something that serves a different client or uses a different skill set?"

**Using reference material:**
Present ecosystem patterns that match what you're hearing. "What you're describing sounds like a [pattern] — here's how that typically works. Does that fit, or is something different going on?" Always present 2-3 options when the user is choosing. Never prescribe.

If the user's services don't form a natural ecosystem, name it: "These feel like independent offerings rather than a connected system. That can work — it just means each one needs to stand on its own." Do not force connections.

## Stage 3: Delivery

Before starting this stage, read the following references selectively based on the conversation:
- `references/services/engagement-models.md` — when discussing how clients engage
- `references/services/delivery-patterns.md` — when discussing process and phases
- `references/services/scope-patterns.md` — when discussing scope boundaries

For each service, define how it actually works. Cover these dimensions:

- **Process/phases** — What are the stages from kickoff to handoff?
- **Deliverables** — What does the client receive? Be specific.
- **Timeline** — How long does this typically take? What affects it?
- **Engagement model** — How does the client work with you during this service?
- **Scope boundaries** — What's included? What's explicitly excluded?

Every service gets the same depth. The amount of content comes from what the user provides, not from you deciding which services matter more.

**On scope boundaries specifically:**
Push if the user hand-waves. "What happens when a client asks for something outside the scope? How do you handle that now?" Scope boundaries are protection — frame them as the creative's safety net, not bureaucratic overhead. Reference the IC trap from `references/services/scope-patterns.md` when relevant.

**Using reference material:**
When the user is choosing an engagement model or delivery pattern, present relevant options. "For the way you work, this could be [model A] or [model B]. Here's the difference — [trade-off]. Which feels more like you?"

## Stage 4: Fit Check

Step back and evaluate the architecture against upstream knowledge. This is where the feedback loop lives.

**Checks to run (only where upstream files exist):**

- **Positioning fit** — Do these services deliver on the positioning statement? "Your positioning says [reference positioning.md]. Looking at your services, does this still hold, or has something shifted?"
- **ICP fit** — Are these services designed for the ideal client segments? "Your ICP describes [reference specific segment in ideal-client-profile.md]. Would they buy these services as structured?"
- **Energy fit** — Do these services align with what energizes the user? "Your energy map shows [reference business-identity.md]. This service leans heavily on [activity] — is that sustainable for you?"
- **Life design fit** — Do engagement models respect life constraints? "Your life design says [reference life-design.md]. Does an embedded 3-day/week engagement work with that?"

Surface tensions. Ask what the user wants to do about each one. Some are intentional trade-offs. Others are blind spots. Do not force changes.

Note any signals that upstream files may need revision: "Your positioning might need updating now that your services are clearer." Capture in the output — do not detour.

## Producing the Output

When all four stages have been explored, ask: "I think we've got enough to put together your service architecture. Ready, or is there more you want to work through?"

### Service Architecture Summary Structure

**My Services**
A 2-3 sentence narrative overview. Natural voice — something they could say to a peer.

**Service Ecosystem**
Table showing roles (flagship, entry point, ongoing, etc.) mapped to services. Ecosystem type named and briefly explained. If no ecosystem, say so.

**Services**
For each service, a structured card with equal depth:

- **ICP segment:** Reference to specific segment in `ideal-client-profile.md`
- **Deliverables:** Specific, tangible outputs
- **Process:** Phases with durations
- **Timeline:** Typical duration and variables
- **Engagement model:** How the client works with the creative
- **Scope boundaries:** Includes and Excludes lists

**Gaps and Tensions**
Honest assessment of misalignments. Each gap references the relevant upstream knowledge file — never restates content.

**Signals for Downstream Work**
Notes for pricing skill, document builders, and voice skill.

### Cross-referencing rules

- If information lives in another knowledge file, reference the file and specific section — never restate
- ICP references point to the specific segment when multiple ICPs exist
- Use the format: `file.md` → "Section Name"

### Format

Produce the summary in the conversation first. Save to `unknown-creatives-coach-knowledge/services.md` when the user is ready.

Use:
- Tables for ecosystem overview
- Lists for service card dimensions
- `###` headings for each service
- Prose for narrative sections

## Coaching Principles for This Skill

- Activities are not services. Help the user make the leap from "I do X" to "I offer Y as a defined engagement with scope, deliverables, and an ending."
- Present options from references, never prescribe models. "Here are a few patterns. Which fits?"
- Continuously reassess the lens. Shift when the conversation reveals a different starting point.
- Scope boundaries are protection. Push when the user hand-waves. Frame as safety, not paperwork.
- Don't force an ecosystem that isn't there. Name independence honestly.
- Cross-reference, never restate. Point to upstream files, don't copy from them.
- Draw from reference material, not defaults. Read the reference files. Use them.
- Services are hypotheses. "Let's design this based on what you know now. Adjust after a few engagements."
- If a service deflates the user, that's a signal. Name it. The services should energize.
```

- [ ] **Step 3: Commit**

```bash
git add plugins/unknown-creatives-coach/skills/services/SKILL.md
git commit -m "feat(skills): add services discovery skill"
```

---

## Task 7: Create Evaluation Criteria

**Files:**
- Create: `plugins/unknown-creatives-coach/evals/services/inventory.md`
- Create: `plugins/unknown-creatives-coach/evals/services/architecture.md`
- Create: `plugins/unknown-creatives-coach/evals/services/delivery.md`
- Create: `plugins/unknown-creatives-coach/evals/services/fit-check.md`

- [ ] **Step 1: Create the evals/services/ directory**

```bash
mkdir -p plugins/unknown-creatives-coach/evals/services
```

- [ ] **Step 2: Write inventory.md eval**

Create `plugins/unknown-creatives-coach/evals/services/inventory.md`:

```markdown
# Eval: Inventory Stage

## Applies To
Services skill — Stage 1: Inventory

## Criteria
- Coach surfaces the user's current offerings or activities before attempting to organize them
- Coach distinguishes between named services and activity descriptions
- Coach grounds the inventory in real experience (past projects), not hypothetical offerings
- Lens is appropriately selected and adjusted based on user's responses

## Pass Indicators
- Coach asks about 3-5 past projects or current offerings before moving to architecture
- Coach identifies the gap between activities and services when it exists ("You said you do branding — what does a client actually get?")
- Coach captures both what the user names and what they describe
- Energy signals are noticed and noted
- Builder lens engages when user can't name services; sharpener when they can but details are vague

## Fail Indicators
- Coach skips inventory and jumps straight to organizing or naming services
- Coach asks about services in the abstract without grounding in past work
- Coach doesn't probe when user gives activity descriptions ("I design websites" accepted without follow-up)
- Lens never shifts despite clear signals the user is in a different place

## Weight
High — the inventory is the raw material for everything downstream. Skipping it or doing it shallowly produces a service architecture built on assumptions.
```

- [ ] **Step 3: Write architecture.md eval**

Create `plugins/unknown-creatives-coach/evals/services/architecture.md`:

```markdown
# Eval: Architecture Stage

## Applies To
Services skill — Stage 2: Architecture

## Criteria
- Coach helps user see how services relate before defining delivery details
- Coach identifies flagship, entry point, and progression path (or names their absence)
- Coach draws from ecosystem-types.md reference material, not generic advice
- Coach presents ecosystem patterns as options, not prescriptions
- Coach acknowledges when services don't form a natural ecosystem

## Pass Indicators
- Coach reads ecosystem-types.md before or during this stage
- Flagship and entry point are explicitly identified (or their absence is named)
- Coach presents 2-3 ecosystem patterns and helps user choose
- Services that don't fit are surfaced: "This one doesn't connect to the rest — should we integrate it or keep it separate?"
- If services are genuinely independent, coach names it without forcing a narrative

## Fail Indicators
- Coach prescribes an ecosystem type without presenting alternatives
- Coach forces all services into a connected system when they're genuinely independent
- No reference to ecosystem-types.md — coach uses generic advice or training data
- Flagship and entry point not identified or discussed
- Coach organizes services by the creative's preference rather than client buying logic

## Weight
High — the architecture shapes the entire service offering. A misidentified flagship or a forced ecosystem creates a document the user won't trust.
```

- [ ] **Step 4: Write delivery.md eval**

Create `plugins/unknown-creatives-coach/evals/services/delivery.md`:

```markdown
# Eval: Delivery Stage

## Applies To
Services skill — Stage 3: Delivery

## Criteria
- Coach covers all five dimensions for each service: process, deliverables, timeline, engagement model, scope boundaries
- Every service gets equal structural depth
- Coach draws from reference material (engagement-models.md, delivery-patterns.md, scope-patterns.md)
- Scope boundaries are pushed on, not accepted when vague
- The IC trap is addressed when relevant

## Pass Indicators
- Each service has process, deliverables, timeline, engagement model, and scope boundaries defined
- Coach reads from reference files when presenting engagement model or delivery pattern options
- Scope boundaries include explicit "Includes" and "Excludes" for each service
- When user hand-waves on scope, coach pushes: "What happens when a client asks for something outside this?"
- IC trap referenced when the engagement model is embedded or the user has history of scope creep

## Fail Indicators
- Some services get less detail than others based on coach's judgment of importance
- Scope boundaries are missing or accepted as vague ("I just handle whatever comes up")
- No reference to engagement-models.md or delivery-patterns.md — coach uses generic models
- IC trap not addressed for embedded or high-involvement engagement models
- Coach defines scope for the user rather than coaching them to articulate it

## Weight
High — delivery details are what make services concrete. Without them, the service architecture is a list of names, not an actionable document.
```

- [ ] **Step 5: Write fit-check.md eval**

Create `plugins/unknown-creatives-coach/evals/services/fit-check.md`:

```markdown
# Eval: Fit Check Stage

## Applies To
Services skill — Stage 4: Fit Check

## Criteria
- Coach evaluates service architecture against available upstream knowledge files
- Tensions are surfaced as observations, not directives
- Cross-references point to specific files and sections, never restate upstream content
- Coach asks what the user wants to do about each tension rather than forcing changes

## Pass Indicators
- At least 2 upstream dimensions checked (positioning, ICP, energy, life design) — limited to files that actually exist
- Misalignments named specifically: "Your positioning says X but your flagship service is Y"
- Cross-references use correct format: `file.md` → "Section Name"
- Upstream content is referenced, not restated or copied
- Coach asks user's preference on each tension: "Is this an intentional trade-off, or something to fix?"
- Signals for upstream revision are captured for the output, not forced as immediate detours

## Fail Indicators
- Fit check skipped or perfunctory ("Everything looks aligned")
- Coach restates positioning, ICP, or energy content rather than referencing the file
- Tensions presented as problems to fix rather than observations to evaluate
- Coach forces changes based on misalignments without asking the user
- No cross-references to upstream files in the tensions identified

## Weight
Medium — the fit check adds strategic value but builds on the core work of stages 1-3. A weak fit check doesn't ruin the service architecture; it just misses an opportunity to connect it upstream.
```

- [ ] **Step 6: Commit**

```bash
git add plugins/unknown-creatives-coach/evals/services/
git commit -m "feat(evals): add evaluation criteria for services skill stages"
```

---

## Task 8: Update Abilities and README

**Files:**
- Modify: `plugins/unknown-creatives-coach/agents/creative-business-consultant/abilities.md:29-46`
- Modify: `README.md:48-52`

- [ ] **Step 1: Add reference knowledge base instructions to abilities.md**

In `plugins/unknown-creatives-coach/agents/creative-business-consultant/abilities.md`, find the services entry in the Discovery Skills list (line ~42):

```markdown
- **services** — Clarify what they offer, how they deliver it, and what outcomes they create.
```

Replace it with:

```markdown
- **services** — Define what they offer, how they deliver it, and how their services relate as an ecosystem. This skill reads from `references/services/` — a set of reference files covering engagement models, delivery patterns, ecosystem types, and scope patterns. The skill file specifies which reference to read before each conversation stage. Do not load all references at once — read selectively as instructed by the skill.
```

- [ ] **Step 2: Update README.md — move services from Coming Soon to skills table**

In `README.md`, find the skills table (around line 40-47) and add the services row:

```markdown
| **services** | "What do I actually offer?" — packages, deliverables, process, engagement models, scope boundaries | `services.md` |
```

Then find the Coming Soon section (around line 49-52) and remove the services bullet:

```markdown
### Coming Soon

- **voice** — Articulate your tone and communication style
- **Document builders** — Intake questionnaires, pitch documents, outbound messaging, and inbound response templates built from your discovery work
```

- [ ] **Step 3: Commit**

```bash
git add plugins/unknown-creatives-coach/agents/creative-business-consultant/abilities.md README.md
git commit -m "feat: register services skill in abilities and README"
```

---

## Task 9: Add Services Route to Coaching Mode

**Files:**
- Modify: `plugins/unknown-creatives-coach/skills/coaching-mode/SKILL.md:49-55`

- [ ] **Step 1: Add services routing**

In `plugins/unknown-creatives-coach/skills/coaching-mode/SKILL.md`, find the routing section (around line 48-55) and add the services route. The list should now include:

```markdown
- User wants positioning help → invoke `positioning`
- User wants to define ideal clients → invoke `icp`
- User wants to explore professional identity → invoke `design-your-business`
- User wants to design their life/lifestyle → invoke `design-your-life`
- User wants to define their services or offerings → invoke `services`
- User provides past conversations → invoke `analyze-conversations`
```

- [ ] **Step 2: Commit**

```bash
git add plugins/unknown-creatives-coach/skills/coaching-mode/SKILL.md
git commit -m "feat(coaching-mode): add services skill routing"
```

---

## Self-Review Checklist

**Spec coverage:**
- ✅ Core flow (4 stages): Task 6 (SKILL.md)
- ✅ Lenses (builder/sharpener/restructurer + shifting): Task 6 (SKILL.md)
- ✅ Reference knowledge base (4 files): Tasks 2-5
- ✅ Output structure with cross-referencing: Task 6 (SKILL.md)
- ✅ Coaching principles: Task 6 (SKILL.md)
- ✅ System-wide DRY principles: Task 1 (agent-protocol.md)
- ✅ Evals (per-stage): Task 7
- ✅ Abilities update: Task 8
- ✅ README update: Task 8
- ✅ Coaching-mode routing: Task 9

**Placeholder scan:** No TBDs, TODOs, or "implement later" in any task.

**Type consistency:** File paths are consistent across all tasks. Reference file names match between the reference tasks (2-5), the SKILL.md (Task 6), and the evals (Task 7). The `services.md` output format matches between the spec and SKILL.md.
