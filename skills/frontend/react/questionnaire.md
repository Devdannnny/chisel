# Pre-Install Questionnaire — Frontend / React (non-Next)

Answer these before invoking the skill. Output quality scales directly with how completely you answer.

## How to use

1. **Inline** — paste your answers as a prefix to your first prompt.
2. **Persistent** — save to `.claude/context.md` and reference it.
3. **Conversational** — invoke the skill and let it walk you through.

---

## Section 1 — Project basics

**1. Do you have a `screenshots/` folder at your repo root?** (Y / N)

**2. Codebase structure level** (default: **Intermediate**) — Beginner / Intermediate / Senior

**3. Project surface type** — Landing / Marketing / Product UI / Dashboard / E-commerce / Portfolio / Docs / Blog / Other

**4. New project or existing codebase?**
- If existing: list conventions to preserve.

---

## Section 2 — Stack

**5. Build tool** (default: **Vite 5+**)
- Vite (recommended)
- Create React App (legacy)
- Remix (non-App Router)
- Other

**6. Routing** (default: **react-router-dom 6+**)
- react-router-dom
- TanStack Router
- Wouter
- None / single-page

**7. Utility CSS framework** (default: **Tailwind**)
- Tailwind · UnoCSS · Panda · vanilla-extract · CSS Modules · Plain CSS · styled-components / Emotion

**8. Component library** (default: **none — primitives only**)
- shadcn/ui · Radix Primitives · Headless UI · Material UI · Chakra · Mantine · None

**9. Animation library** (default: **Framer Motion**)
- Framer Motion · Motion One · GSAP · CSS only · None

**10. Font loading** (default: **`@fontsource/*` packages**)
- `@fontsource/*` (self-hosted, no FOUT)
- `<link>` tags to Google Fonts (simpler but FOUT risk)
- Custom font files in `/public`

---

## Section 3 — Brand & aesthetic

**11. Existing brand?** (Y / N)
- If Y: list locked items (logo, fonts, palette, voice).

**12. Tone direction** (pick 1–2)
- Playful · Professional · Minimal · Bold · Luxe · Technical · Editorial · Brutalist · Organic · Retro · Glassmorphic · Other

**13. Reference brands or sites** (3 if possible)

**14. Typography preference** (default: skill picks)
- Specific fonts, a vibe, or "free-only"?

**15. Color direction** (default: skill picks)
- Specific palette, a direction, or hard constraints

---

## Section 4 — Design preferences

**16. Dark mode** (default: **light default, optional dark**)
- Required / Optional / Dark only / Light only

**17. Motion budget** (default: **medium — 2–3 intentional motions**)
- None / Subtle / Medium / Energetic

**18. Accessibility level** (default: **WCAG AA**)

---

## Section 5 — Constraints

**19. Target devices** (default: **desktop + mobile, both polished**)

**20. Performance targets** (default: **Core Web Vitals green, < 200kB JS bundle initial**)

**21. Anything to avoid?** (free-text)

---

## Defaults applied if unanswered

- Vite 5+ with React 18+, react-router-dom 6, Tailwind, no component library, Framer Motion
- `@fontsource/*` for fonts (no FOUT)
- Intermediate codebase level
- Greenfield landing page
- No existing brand — tone "bold and distinctive"
- Distinctive Google Fonts pair (NOT Inter, NOT Roboto)
- Light mode default, dark mode optional
- 2–3 intentional motions
- WCAG AA
- Desktop + mobile both polished
