# Frontend Skills

Production-grade frontend skills, organized by platform. Each sub-skill is self-contained and triggered independently based on what you're building.

## Sub-skills

| Skill | Folder | When to use |
|---|---|---|
| **Next.js** | [`next/`](./next/) | Building Next.js (App Router or Pages) — landing pages, marketing, product UI, dashboards. |
| **React** | [`react/`](./react/) | Building React WITHOUT Next.js — typically Vite + React, CRA, or Remix outside App Router. |
| **Flutter** | [`flutter/`](./flutter/) | Building Flutter mobile (iOS / Android / both) with Material 3 or Cupertino. |
| **React Native** | [`react-native/`](./react-native/) | Building React Native (with Expo) for iOS / Android. |

All sub-skills share the same design language, defined once in [`shared-rules.md`](./shared-rules.md). Each sub-skill's `SKILL.md` references it and adds only the platform-specific application (code patterns, stack idioms, framework-specific anti-patterns).

**To refine the design language:** edit [`shared-rules.md`](./shared-rules.md). The change propagates to every sub-skill automatically.

**To add a new platform sub-skill:** create `<platform>/SKILL.md` + `<platform>/questionnaire.md`, reference `../shared-rules.md` at the top, and document only the platform-specific code patterns and anti-patterns.

## Shared design principles

Each sub-skill enforces:

- **One composition** in the first viewport
- **Brand first** — distinctive enough to fail the "could be any brand" test
- **Distinctive typography** — no Inter, Roboto, or system stacks
- **Atmospheric backgrounds** — no flat single-color fills
- **Full-bleed hero discipline** — edge-to-edge dominant visual; no inset cards, no overlays
- **Cards only when they're the interaction** — no card grids as default layout
- **2–3 intentional motions** — purpose-driven, not noise
- **Both light and dark modes** polished (where applicable)
- **Real visual anchors** — product, place, atmosphere — not abstract gradients

What differs between sub-skills is the **idiom**: component model, routing, styling, animation library, theming pattern, platform conventions.

## Pre-install questionnaire

Each sub-skill has a `questionnaire.md` that calibrates the skill's output to your stack and brand. **Answer it before invoking the skill.** Output quality scales directly with how completely you answer.

## Adding new sub-skills

Planned (not yet implemented):
- `frontend/vue/` — Vue 3 with Nuxt or Vite
- `frontend/svelte/` — Svelte 5 / SvelteKit
- `frontend/solid/` — SolidJS / Solid Start
- `frontend/astro/` — Astro for content-led sites
- `frontend/vanilla/` — HTML/CSS/JS, no framework

To contribute a new sub-skill, see [`../../CONTRIBUTING.md`](../../CONTRIBUTING.md) and follow the structure of an existing sub-skill (`SKILL.md` + `questionnaire.md`).
