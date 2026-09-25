# Progymnasmata - Daily Classical Rhetoric Exercise App

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen.svg)](#getting-started)
[![TypeScript](https://img.shields.io/badge/TypeScript-6.x-blue.svg)](https://www.typescriptlang.org/)
[![Framework](https://img.shields.io/badge/Framework-React%20Native%20%2F%20Expo-000000.svg)](https://expo.dev/)
[![License](https://img.shields.io/badge/License-Proprietary-red.svg)](#license)

Progymnasmata is an offline-first iOS app that delivers one classical rhetoric exercise a day, matched to the values you care about. Built with React Native, Expo, and TypeScript, it's fully local — no account, no server, and no third-party services of any kind — bringing the fifteen **progymnasmata** exercise types (fable, maxim, narrative, declamation, and more) to a modern daily-practice format.

---

## Key Features

1. **Daily Exercise Delivery Engine:** A local, on-device scheduler delivers one exercise a day at a time you choose, matched to your selected values. DST-correct local-time scheduling with a rolling 14-day notification horizon, no server round-trip required.
2. **15 Classical Types, 75 Exercises:** A single shared reader renders all fifteen progymnasmata types faithfully to their classical anatomy — a maxim's clarity/plausibility/benefit test, a narrative's six elements, CRAC for law, delivery cues for declamation — each with cited sources and a confidence label.
3. **Value-Matched Selection Engine:** 72 values across 9 categories drive a deterministic selection engine (direct match → related value → stale-pick revisit → explore), unit-tested against repeats, type rotation, and disabled-type handling.
4. **Offline-First, Zero Backend:** Fully local architecture powered by `expo-sqlite` for user state; all 75 exercises ship inside the app binary as static TypeScript. No account, no analytics, no CDN, no content sync.
5. **Practice Workspace & Handbook:** An autosaving practice editor per exercise (prompt, steps, checklist) alongside a streak counter, plus a full Handbook reference covering all 15 types — definition, anatomy, how to practice, and common pitfalls.

## Docs
- [Privacy Policy](docs/treatise_privacy_policy.md)
- [EULA](docs/treatise_eula.md)
- [Terms of Use](docs/treatise_terms_of_use.md)

## License

Copyright © 2026 Abraham Doe. All rights reserved.
Unlawful copying, distribution, or modifications of this software via any medium is strictly prohibited without explicit written consent.
