# Unknown Creatives Coach

A [Claude Code plugin](https://docs.anthropic.com/en/docs/claude-code) that acts as a business coach for freelancers and independent creatives. It helps you articulate what you do, who you do it for, and why someone should hire you — then turns that thinking into usable, client-facing documents.

## Who This Is For

Designers, design engineers, illustrators, video producers, 3D artists, and other independent creatives who are skilled at their craft but struggle to explain the business side. If you've ever frozen when someone asks "so, what do you do?" — this is for you.

## How It Works

Install the plugin, open Claude Code, and say something like "help me with my business" or "I can't explain what I do." The plugin activates a direct, experienced business coach that guides you through structured discovery conversations. It doesn't generate language for you — it asks the questions that surface your own.

Everything you work through gets saved to a `unknown-creatives-coach-knowledge/` folder in your workspace. That folder is your complete business identity: portable, human-readable, and yours to edit.

## Installation

**Claude Code CLI:**

```bash
claude install-plugin doterodesign/unknown-creatives-coach
```

**Claude Cowork (desktop/web):**

1. Open Settings and navigate to the Marketplace section
2. Paste the repo URL into the "Add Marketplace" input:
   ```
   https://github.com/doterodesign/unknown-creatives-coach
   ```
3. Install the plugin from the marketplace listing

Then start a conversation in any workspace. The coach will guide you from there.

## Skills

### Discovery Skills

Structured conversations that help you articulate different aspects of your business identity.

| Skill | What It Answers | Output |
|-------|----------------|--------|
| **design-your-business** | "What am I built for?" — your arena, strengths, and energy patterns | `business-identity.md` |
| **design-your-life** | "What do I need the business to do for me?" — values, working style, life constraints | `life-design.md` |
| **positioning** | "How do I want the market to see me?" — what makes you different and where you stand | `positioning.md` |
| **icp** | "Who do I serve best?" — your ideal client profile and red flags | `ideal-client-profile.md` |
| **services** | "What do I actually offer?" — packages, deliverables, process, engagement models, scope boundaries | `services.md` |
| **voice** | "How should my studio sound?" — stances, principles, communication modes, vocabulary, filtering | `voice.md` |
| **analyze-conversations** | "What do my real conversations reveal?" — pattern analysis from past prospect/client interactions | `conversation-insights.md` |

### Coming Soon

- **Document builders** — Intake questionnaires, pitch documents, outbound messaging, and inbound response templates built from your discovery work

## What to Expect

The coach is direct. It pushes back on vague answers. When you say "I help companies with design," it will ask: *What kind of companies? What were they struggling with before you showed up? What changed after you left?*

This is by design. The first answer is almost never the real one. The value is in the follow-up questions.

A session goes well when you leave with language that feels like yours — not a template. If you can say it to a friend at dinner without wincing, the coach did its job.

## Best Practices

**Bring materials.** Past proposals, scopes of work, client briefs, case studies, website copy, pitch decks, notes from client calls — even rough ones. The more context the coach has, the sharper the questions.

**Push back.** If something doesn't sound like you, say so. "That doesn't sound like me" or "I don't want to narrow that much" are some of the most valuable signals in a session. The coach captures them and adjusts.

**Start anywhere.** You don't need to go in order. Start with whatever area feels most unclear. If you already have strong positioning but no idea who your ideal client is, go straight to ICP.

**Use analyze-conversations first if you have them.** If you have transcripts, call recordings, or email threads from past prospect/client conversations, lead with those. The pattern analysis produces evidence-based hypotheses that make every other skill sharper.

**Trust the process on hard questions.** Some questions don't have quick answers. Sitting with them is part of the work. The coach won't rush you, but it won't let you off the hook with a safe, generic answer either.

## Project Structure

```
unknown-creatives-coach/
├── plugins/unknown-creatives-coach/
│   ├── .claude-plugin/
│   │   └── plugin.json              # Plugin manifest
│   ├── agents/
│   │   ├── creative-business-consultant/
│   │   │   ├── soul.md              # Identity, purpose, philosophy
│   │   │   ├── personality.md       # Voice, tone, conversation rules
│   │   │   └── abilities.md         # Skills, tools, routing logic
│   │   ├── shared/
│   │   │   └── agent-protocol.md    # Cross-agent rules
│   │   ├── orchestrator.md          # Eval pipeline orchestrator
│   │   ├── evaluator.md             # First-pass judge
│   │   ├── skeptic.md               # Adversarial second judge
│   │   ├── tiebreaker.md            # Disagreement resolver
│   │   └── eval-creator.md          # Eval authoring agent
│   ├── skills/                      # Discovery and coaching skills
│   ├── evals/                       # Quality evaluation criteria
│   └── references/                  # Protocol documentation
├── docs/
│   ├── specs/                       # Design specifications
│   └── plans/                       # Implementation plans
└── .claude-plugin/
    └── marketplace.json             # Marketplace listing
```

## Architecture

The plugin uses a **skill-based architecture** rather than spawning subagents. When coaching activates, the main Claude Code session *becomes* the consultant — reading identity files once and maintaining the full conversation context throughout. This means no context loss between turns and no re-loading overhead.

### Agent Identity System

The consultant's behavior is defined by three files that can be versioned and evolved independently:

- **Soul** — Why it exists and what it believes. The philosophical foundation.
- **Personality** — How it shows up in conversation. Voice rules, coaching behaviors, when to push and when to hold space.
- **Abilities** — What tools and skills are available. Routing logic for different user needs.

### Evaluation Pipeline

An optional quality assurance system. When activated, every coaching response passes through an evaluator/skeptic judge loop before you see it. The judges score against behavioral criteria (one question at a time, never answer for the user, push back on vague, etc.) and skill-specific criteria. The pipeline is invisible when working — responses just get sharper.

### Knowledge System

All outputs are saved to `unknown-creatives-coach-knowledge/` in your workspace. The coach reads existing files before starting any skill so you never repeat yourself across sessions. Memory captures corrections, preferences, and insights to improve future coaching.

## Privacy and Data Storage

Your coaching data stays on your device. The plugin does not include a backend, database, or cloud storage of its own.

When the coach saves knowledge files (`positioning.md`, `ideal-client-profile.md`, etc.) or memory (`memory.md`), it writes them to a `unknown-creatives-coach-knowledge/` folder inside your local workspace using Claude Code's standard file system tools. In Cowork, that's whatever folder you have selected. In the CLI, it's your current working directory.

Here's what lives where:

| Data | Where it lives | Who controls it |
|------|---------------|-----------------|
| Plugin files (skills, agents, evals) | Downloaded from GitHub at install | You can inspect, fork, or modify |
| Conversations | Processed through Anthropic's API, same as any Claude session | Subject to [Anthropic's privacy policy](https://www.anthropic.com/privacy) |
| Knowledge files and memory | Your local filesystem, in your workspace folder | Entirely yours — plain markdown, readable, editable, deletable |

The plugin never sends your coaching data to any third-party service. Installing from the Claude Marketplace works the same way — the marketplace distributes the plugin instructions, not your data.

## Contributing

This plugin is actively developed. The main areas for contribution:

- **New discovery skills** — `voice` is next on the roadmap
- **Document builder skills** — Turning discovery outputs into client-facing documents
- **Evaluation criteria** — New evals that improve coaching quality
- **Bug reports** — If the coach gives generic advice, that's a bug. Open an issue.

## License

This project is licensed under the [Polyform NonCommercial 1.0.0](LICENSE.md) license. You are free to use, modify, and share this plugin for personal and non-commercial purposes.

For commercial licensing, contact [support@unknowncreatives.studio](mailto:support@unknowncreatives.studio).

## Author

[Dimitri Otero](https://github.com/doterodesign)
