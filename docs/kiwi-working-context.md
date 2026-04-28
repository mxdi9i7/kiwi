# Kiwi — Working Context

Living document. Update as decisions are made. Companion to the PRD.

> **Name change:** This project was drafted as "DayFlow" in early PRD versions. The official name is **Kiwi**. The PRD file should be renamed and references updated when convenient.

---

## 1. Tech Stack

> **Decided** items are fixed. **Leaning** items are defaults that can be challenged with reason. **Open** items must be asked about before committing.

### Client (Phase 1: iOS)
- **Framework:** ✅ Decided — **React Native**
- **Styling:** ✅ Decided — **NativeWind** (Tailwind for RN)
- **Speech-to-text:** ✅ Decided (Phase 1) — **iOS Speech framework, on-device**
  - Open: cross-platform / cloud fallback strategy when Android & HarmonyOS land
- **Local storage (read-only cache):** Open — SQLite (via op-sqlite or expo-sqlite) leading

### Backend
- ✅ Decided — **Supabase** (Postgres, auth, storage, edge functions)
- ✅ Decided — Auth providers in Phase 1: **Sign in with Apple, Sign in with Google, email magic link**
- Edge functions handle the LLM proxy + auth + rate limiting; client never holds provider keys directly

### AI
- **Model:** ✅ Decided — **DeepSeek v4 Pro**, committed for the foreseeable future
- **No BYOT.** Single provider only. Build directly against the DeepSeek API; no abstraction layer for portability.
- **Implication:** prompts, evals, and tooling can be tuned specifically for DeepSeek without keeping a portability budget
- **Eval harness:** ✅ Decided — committed as a Phase 1 deliverable. Repo-stored labeled dataset, CLI runner, CI hook on classification-prompt PRs. See PRD §3.4.9.

### Infra & Distribution
- **Self-hosting:** Deferred — supported once cloud service stabilizes
- **License:** ✅ Decided — **MIT**

---

## 2. Conventions

### Naming
- Buckets capitalized in user-facing copy: **Task**, **Lightbulb**, **Project Plan**
- Internal: `task`, `lightbulb`, `plan` (singular, lowercase)
- "Top 3" never "top three" or "top-3-task-list"
- "Signal vs. Noise" with the "vs." in copy (acceptable as "Signal/Noise" internally)
- Product name: **Kiwi**, never "kiwi" mid-sentence in copy unless stylistically intentional

### Data model invariants
- A task may have 0+ steps; a project plan may have 0+ tasks; a task belongs to ≤1 plan
- Step due dates, when present, replace the parent task on the calendar (PRD §3.4.4)
- Completion % rolls up: steps → task → plan
- Links are bidirectional and surfaced both directions in UI

### Copy / tone
- Direct, slightly opinionated, never cute
- No exclamation points in default UI strings
- Empty states should teach the system, not motivate

---

## 3. Build-in-Public Policy

- All non-secret work happens in the public repo
- Changelogs published per release; build logs welcome but not required
- Design decisions of any weight get a short write-up + entry in the decision log below
- No private feature branches for "wow moments" — ship visibly
- Secrets, keys, and personal data never committed; `.env.example` only

---

## 4. Decision Log

| Date | Area | Decision | Rationale |
|---|---|---|---|
| 2026-04-28 | Product | Renamed from DayFlow → **Kiwi** | Shorter, more memorable, available |
| 2026-04-28 | Client framework | **React Native + NativeWind** | Cross-platform leverage for Phase 5; familiar Tailwind ergonomics |
| 2026-04-28 | Backend | **Supabase** | Postgres + auth + edge functions in one; minimizes ops for a solo project |
| 2026-04-28 | LLM | **DeepSeek v4 Pro**, single provider, no BYOT | Cost-performance ratio; one provider keeps prompts, evals, and tooling lean for a solo project |
| 2026-04-28 | Speech-to-text (Phase 1) | **iOS Speech framework on-device** | Latency, privacy, zero per-call cost |
| 2026-04-28 | License | **MIT** | Maximum permissiveness; matches solo-project + low-friction adoption goals |
| 2026-04-28 | Self-hosting | Deferred until cloud stabilizes | Ship one good path before maintaining two |
| 2026-04-28 | Auth | **Apple + Google + email magic link** in Phase 1 | Apple required by App Store when sign-in exists; Google has high real-user demand; magic link as no-OAuth fallback |
| 2026-04-28 | Eval harness | Committed as **Phase 1 deliverable** (was open question) | Triage accuracy is the bet — can't ship prompt changes blind |

---

## 5. Open Questions Tracker

- [ ] LLM cost model for cloud version (subsidized? free with cap? paid tier? sponsor-funded?)
- [ ] Top-3 hard cap vs. configurable 1–5
- [ ] Plan templates: ship full set at Phase 2 vs. project + trip first
- [ ] Calendar integration depth: own calendar vs. overlay on Apple/Google Calendar
- [ ] Cross-platform / cloud STT fallback for Phase 5 (Android, HarmonyOS, web)
- [ ] Local cache library: op-sqlite vs. expo-sqlite vs. WatermelonDB
- [ ] Finance encryption posture (Phase 4)

---

## 6. People

- **Builder / PM / primary user:** me
- **Reviewers / advisors:** _add as engaged_
- **Sponsors:** GitHub Sponsors (page TBD)

---

## 7. Recurring Risks to Watch

- **LLM latency** breaking the <5s capture target (PRD §3.4.1) — measure end-to-end early; DeepSeek + Supabase edge function adds two hops, profile both
- **Triage accuracy** — eval harness (PRD §3.4.9) gates prompt/model changes; watch for dataset drift as real-world capture vocabulary expands
- **Top-3 trust** — if the daily top-3 is wrong often, the whole opinionated UX collapses
- **DeepSeek single-provider lock-in** — accepted risk. Outages, pricing changes, geopolitical access issues, or capability regressions hit the product directly with no fallback. Watch for: rate-limit changes, latency drift, model deprecation announcements. Revisit only if a meaningful incident occurs.
- **Cost per active user** in the cloud version — model this before opening signups
- **Scope creep into team features** — the PRD non-goal exists for a reason; flag every adjacent request
- **React Native + iOS-native APIs** — Speech framework integration likely needs a custom native module; budget time
