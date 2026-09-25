# Progymnasmata

*(Named Progymnasmata during initial development; renamed to Treatise on 2026-09-25, then reverted back to Progymnasmata the same day after "Treatise" hit an App Store Connect naming conflict — see decision #1 below. The bundle ID stays `com.treatise` (App Store Connect only rejected the *name*, not the identifier). The progymnasmata content itself — the 15 classical exercise types, the values, the 75 bundled exercises — is unchanged throughout.)*

A daily workout for the rhetorical mind. Choose the values you care about, and once a day you receive one of the fifteen classical progymnasmata — a fable, a maxim, a speech — matched to a value you selected. Read it, see how it's built, and practice the form.

Built with Expo, React Native, and TypeScript. **iOS only. Fully offline — no account, no server, no third-party services of any kind.**

This app was built from [`progym_instructions.md`](docs/progym_instructions.md) (the full v1.0 specification) and [`progym_sdlc.md`](docs/progym_sdlc.md) (the original product brief), adapted per the product-owner decisions below.

[Progymnasmata Wiki](https://en.wikipedia.org/wiki/Progymnasmata) — background on the classical exercises the app is built around (not the app itself).

## Simple Definition of Done

> User selects value(s). Progymnasmata sends relevant daily notifications matched to those values.

This is met: the selection engine (`src/domain/selection.ts`) picks an exercise tagged to a selected value, the scheduler (`src/services/scheduler.ts`) plans a rolling 14-day horizon of local notifications at the user's chosen time, and the reader (`app/exercise/[id].tsx`) presents the exercise with its full classical structure.

## Decisions (2026-09-24)

The product owner made these calls, which take precedence over `progym_instructions.md` where they conflict with it:

| # | Decision | Where it lives |
|---|---|---|
| 1 | App name: ~~Progymnasmata~~ → ~~Treatise~~ → **Progymnasmata** (renamed to Treatise 2026-09-25, reverted the same day after an App Store Connect "name already in use" rejection, pending the Treatise trademark; the progymnasmata content is unchanged throughout) | `app.json` |
| 2 | **Principles dropped** — not a canonical progymnasmata exercise. 16 types → 15, renumbered I–XV | `src/domain/content/types.ts`, `exercise-types.ts` |
| 3 | Bundle ID: ~~`com.treatise.progymnasmata`~~ → ~~`com.treatise.treatise`~~ → `com.treatise` | `app.json` |
| 4 | **iOS only** for now — no Android build, config, or assets | `app.json` (no `android` key) |
| 5 | **72 values**, not 74 — "baseline cooperation" and "value convictions" dropped | `src/domain/content/values.ts` |
| 6 | Adjective value labels kept verbatim (Punctual, Useful, Thorough, Moderate, Exotic, Counter) | `values.ts` |
| 7 | Value descriptions approved as drafted | `values.ts` |
| 8 | Invective subject policy approved: historical figures dead ≥ 50 years, legendary/fictional figures, or personified vices — never living persons | `src/domain/content/schemas.ts` (`invectiveBody` validation) |
| 9 | Personal Apple Developer account (not an organization) | Affects App Store Connect setup, not the codebase |
| 10 | Free, no monetization | — |
| 11 | **Fully local — no third-party vendors or cloud services of any kind.** No Sentry, no PostHog, no CDN, no content sync. All 75 exercises ship inside the app bundle as static TypeScript | `src/domain/content/`, no `src/services/analytics.ts` or equivalent exists |
| 12 | Default notification time: 8:00 AM | `src/data/settings.ts` (`DEFAULTS.scheduleTime`) |
| 13 | Brand: **White and Gold** | `src/ui/theme.ts` — contrast-checked to WCAG 2.2 AA |
| 14 | Living public figures **allowed** in advisor-role suasoria (Declamation), as analysis of a real, documented decision | `src/domain/content/types.ts` (`DeclamationBody.livingFigure`), used in `declamation-advise-nadella-2014` |
| 15 | No iPad support, iPhone only | `app.json` (`ios.supportsTablet: false`) |
| 16 | Minimum OS: iOS 16.4 (Expo SDK 57 default) | — |
| 17 | Content limited to **5 examples per exercise type** (75 total) for initial review | `src/domain/content/exercises/*.ts` |

Content hosting/domain (§19 OQ-14 in the original spec) is **moot** under decision #11 — there is no content hosting; everything ships in the binary and updates only via new app versions.

Several further simplifications were made to fit the reduced scope and keep the codebase small enough for a first, fully-verified build — see [AGENTS.md](AGENTS.md) for the complete list (single daily slot instead of 1–3, no ORM, no background task, `node --test` instead of Jest, etc.).

## Getting started

```bash
npm install
npm run ios          # starts Metro and opens the iOS Simulator (via Expo Go)
```

The first run installs Expo Go on the simulator automatically. Everything — SQLite, notifications, fonts, symbols — works in Expo Go; no custom dev client or native build is required for development.

### Useful commands

```bash
npm run typecheck          # tsc --noEmit
npm test                   # domain unit tests (node --test), 22 tests
npm run content:validate   # schema + structural checks on all 75 exercises
npm run doctor             # expo-doctor — project health check
```

## What's actually built

- **Onboarding**: welcome → value picker (72 values, 9 categories, search, long-press for descriptions) → schedule (native time picker) → notification permission priming → ready (creates the day-one "welcome" exercise).
- **Today**: shows the day's exercise once its time has arrived (or the welcome exercise on day one), with an "Open early" state before then, plus a streak counter and a "notifications are off" banner when relevant.
- **Exercise reader**: a single shared component (`ExerciseBody`) renders all 15 exercise types faithfully to their classical anatomy (a maxim's clarity/plausibility/benefit test, a narrative's six elements, a thesis's topics, CRAC for law, delivery cues for declamation, etc.), plus sources with confidence labels, favorite, share, and mark-complete.
- **Practice**: an autosaving practice editor per exercise, with the exercise's own prompt and steps.
- **Library**: every exercise the user has already received, with an All/Favorites filter.
- **Handbook**: all 15 types explained (Definition, Anatomy, How to practice, Pitfalls), with a live example pulled from the bundled content.
- **Settings**: edit values, exercise types (at least one must stay on), schedule, notification toggle (with a link to system settings), appearance (System/Light/Dark), About, and Delete all data.
- **Selection engine**: direct match on a selected value → related value (same category) → revisit a stale pick → explore anything, fully deterministic per install+date (unit-tested: no repeats within the content pool, type rotation, respects disabled types).
- **Scheduling**: DST-correct local-time → UTC conversion (tested against real fall-back/spring-forward/half-hour-offset cases in three time zones), a rolling 14-day horizon topped up on every foreground, and OS notification reconciliation.

## Content

75 exercises (5 per type × 15 types), each with real citations:

- **Fables**: 5 Aesop fables (public domain, Perry Index cited).
- **Anecdotes**: Diogenes, Archimedes, Socrates, Newton — each with a primary or well-attested traditional source; disputed attributions are labeled as such.
- **Historical exercises** (Encomium, Invective, Narrative, Declamation): real figures and events, cited (Cicero's *In Catilinam*, Newton's 1675 letter, Shackleton's *Endurance* voyage, Amelia Earhart's 1935 flight, etc.).
- **Maxims, Refutations, Confirmations, Commonplaces, Vivid Descriptions, Comparisons, Personifications, Theses, Law**: original compositions written for this app, following the classical form.

Run `npx tsx tools/validate-content.ts` after any content edit — it checks schema shape, exactly-one-primary-value, cross-references to known value/type slugs, the invective living-person guard, and warns (doesn't fail) on word-count range and value coverage.

## Architecture

```
app/                        Expo Router screens
  (onboarding)/              welcome(index) → values → schedule → notifications → ready
  (tabs)/                    Today, Library, Handbook, Settings (each a nested stack)
  exercise/[id].tsx          the reader
  practice/[exerciseId].tsx  the practice editor (form sheet)
src/
  domain/                    pure TypeScript: content, selection engine, schedule math, streaks
    content/                 values, exercise types, 75 exercises, Zod schemas
  data/                      expo-sqlite repositories (settings, deliveries, favorites, practice)
  services/                  notifications adapter, scheduler (selection + schedule-math + repos + notifications)
  ui/                        design tokens (White & Gold), theme context, shared components
  features/exercise/         the type-specific exercise body renderer
tools/validate-content.ts    content validator (run with tsx)
```

No Redux/MobX/Zustand — state is either local component state (refetched via `useFocusEffect` from `expo-router`) or the small `AppProvider` context that holds settings and a `ready` gate.

## Known gaps (fast follows, not required by the DoD)

- **Not yet tested**: dark mode end-to-end, a real device (only the iOS Simulator via Expo Go so far), an actual EAS/TestFlight build, VoiceOver.
- **Notification permission** could not be fully exercised in Expo Go on the simulator (iOS Simulator + Expo Go notification permission prompts are unreliable); the code path is the standard `expo-notifications` API and should be re-verified on a development build or TestFlight build before shipping.
- **No multi-slot schedule** (1–3 times/day) — single daily slot only, per the simplified scope.
- **No export/delete-my-data file**, no streak-longest tracking, no store-review prompt — all deferred; "Delete all data" (which the spec does require) is implemented.
- 16 of 72 values have no dedicated exercise yet at this 75-exercise scale; the selection engine's related-value fallback covers them.

## Before submitting to the App Store

1. Set up the app in App Store Connect under your personal Apple Developer account, bundle ID `com.treatise`.
2. Build with EAS (`npx eas-cli build --platform ios`) or locally via Xcode (`npx expo prebuild` then open `ios/*.xcworkspace`) — this project has never been built outside Expo Go, so budget time for first-build native issues.
3. Test on a real device / TestFlight build specifically for notification permission and delivery — this is the one system this Expo-Go-based verification pass could not fully exercise.
4. Fill in the App Privacy questionnaire: no data collected at all (decision #11 makes this the easiest possible answer).
5. Complete Apple's age-rating questionnaire; expect a 9+–13+ range given historical violence in a few exercises (Nero, Catiline, Caesar).
6. Host [`docs/treatise_privacy_policy.md`](docs/treatise_privacy_policy.md) and [`docs/treatise_terms_of_use.md`](docs/treatise_terms_of_use.md) somewhere with a public URL (GitHub Pages works well), and fill in the placeholder URLs inside those two files and in [`docs/treatise_app_store_ad_copy.json`](docs/treatise_app_store_ad_copy.json) before submission — Apple requires a live Privacy Policy URL. [`docs/treatise_eula.md`](docs/treatise_eula.md) documents that Treatise uses Apple's Standard EULA as-is, so no custom EULA needs to be uploaded unless you want one.
7. Copy the App Store listing fields (name, subtitle, description, keywords) straight from [`docs/treatise_app_store_ad_copy.json`](docs/treatise_app_store_ad_copy.json) into App Store Connect.
# progymnasmata
