# Kiwi

A voice-first personal productivity app. Press to speak — AI routes your thoughts into tasks, ideas, or plans, and surfaces the daily top 3 that matter most.

Open-source. Cloud-first. iOS first. Built in public.

> **Status:** Early development. The PRD is settled; building Phase 1 (capture core) now. Expect rough edges, breaking changes, and gaps until v0.1 ships.

## Why Kiwi?

Most thoughts die between mind and screen because existing tools demand you pre-categorize them. Where does this go — todo app, notes app, calendar, scratchpad? That friction kills capture in the moment.

And even when capture works, todo lists become noise. The 3 things that actually matter today drown in the 40 things that don't.

Kiwi removes both frictions:

- **One button.** Speak. AI sorts it.
- **Top 3 a day.** Inspired by Steve Jobs' insistence that focus means saying no to the hundred other good ideas. Everything else stays visible, but de-emphasized.

## The model

```
idea → project (requires) → tasks (include) → steps
```

Voice capture routes every thought into one of three buckets:

- **Task** — concrete and executable. Lands on the calendar.
- **Lightbulb** — a spark, not yet actionable. Goes to the notes list. Resurfaces during open blocks.
- **Project Plan** — needs thinking time before doing time. Opens an AI dialogue with a selectable persona (skeptical investor, supportive coach, ruthless editor, systems thinker). Output is a plan, plus tasks, plus more notes.

Tasks are the atomic unit. They link to notes, belong to projects, can be decomposed into steps with their own deadlines, and (in Phase 4) link to finance entries.

Each morning, Kiwi scores all active tasks on a Signal-vs-Noise axis and surfaces the **Top 3** for the day. It re-prioritizes daily, on new captures, or on demand.

## Roadmap

| Phase | Focus |
|---|---|
| **1** | Capture core — voice → triage → tasks/notes/plans, calendar placement, offline browsing |
| **2** | Top-3 prioritization, project dialogues with personas, memory layer, AI search |
| **3** | Daily shutdown, weekly review, velocity recap, focus mode |
| **4** | Personal finance module — assets, investments, budgets, savings goals |
| **5** | Android, HarmonyOS, web (read/search) |

Full PRD: [`/docs/prd.md`](./docs/prd.md)

## Stack

- **Client** — React Native, Tailwind via NativeWind. iOS first.
- **Backend** — Supabase (Postgres, auth, storage, edge functions)
- **AI** — DeepSeek v4 Pro by default. BYOT (bring your own token) on the roadmap so users can plug in any compatible provider.
- **Speech-to-text** — iOS Speech framework (on-device) at launch. Cross-platform and cloud fallbacks evaluated as Android and HarmonyOS land.

Self-hosting will be supported once the cloud service stabilizes. Setup docs to follow.

## Built in public

Development happens here. Issues, design notes, and changelogs are public. Watch the repo to follow along.

## Sponsor

Sponsoring Kiwi supports development and earns a spot on the early-access list:

- Beta invites during Phase 1 and Phase 2 development
- Priority invite when the public cloud launch opens
- Direct line for product feedback

[Become a sponsor →](https://github.com/sponsors/USERNAME) *(placeholder — sponsor page coming soon)*

## Contributing

Kiwi is primarily a personal project — it's built for the way I think. That said, bug reports, design critique, and small PRs are welcome. For larger contributions, open an issue first to talk through the idea before writing code.

## License

MIT — see [LICENSE](./LICENSE).
