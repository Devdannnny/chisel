# Pre-Install Questionnaire — Frontend / Next.js

Answer these before invoking the skill. Output quality scales directly with how completely you answer. Skip a question only if you genuinely have no preference — the skill applies sensible defaults.

## How to use

Pick one:
1. **Inline** — paste your answers as a prefix to your first prompt.
2. **Persistent** — save your answers to `.claude/context.md` (gitignored) and reference it.
3. **Conversational** — invoke the skill and let it walk you through the questions.

---

## Section 1 — Project basics

**1. Do you have a `screenshots/` folder at your repo root?** (Y / N)
- If **Y**: the skill reads it and extracts typography, layout rhythm, color, spacing, and atmosphere as design references.
- If **N**: the skill works from your prompt and any existing design tokens in the codebase.

**2. Codebase structure level** (default: **Intermediate**)
- **Beginner** — more scaffolding, more inline comments, fewer advanced patterns
- **Intermediate** — modern idioms, clean code, sparse comments
- **Senior** — terse, idiomatic, no scaffolding noise, no comments unless encoding a non-obvious invariant

**3. Project surface type** (pick one)
- Landing page (marketing)
- Marketing site (multi-page)
- Product UI (signed-in app screens)
- Dashboard (data-dense, analytical)
- E-commerce (PDP, listings, checkout)
- Portfolio / case study
- Documentation site
- Blog / editorial
- Other (describe)

**4. New project or existing codebase?**
- New: skill applies all defaults below.
- Existing: list conventions, design system, or patterns you want preserved. The skill preserves those instead of imposing fresh defaults.

---

## Section 2 — Stack

**5. Next.js router** (default: **App Router**)
- App Router (Next 14+, recommended)
- Pages Router (legacy / migration)

**6. Utility CSS framework** (default: **Tailwind CSS**)
- Tailwind CSS
- UnoCSS
- Panda CSS
- vanilla-extract
- CSS Modules
- Plain CSS
- styled-components / Emotion

**7. Component library** (default: **none — build from primitives**)
- shadcn/ui (recommended if not none)
- Radix Primitives
- Headless UI
- Material UI
- Chakra UI
- Mantine
- None — most distinctive results

**8. Animation library** (default: **Framer Motion**)
- Framer Motion
- Motion One
- GSAP (heavy but powerful)
- CSS animations only
- Lottie / Rive (for complex)
- None

---

## Section 3 — Brand & aesthetic

**9. Existing brand?** (Y / N)
- If **Y**: list locked items (logo path, brand fonts, color palette, voice).
- If **N**: the skill picks a direction from your tone preference.

**10. Tone direction** (pick 1–2)
- Playful · Professional · Minimal · Bold · Luxe · Technical · Editorial · Brutalist · Organic · Retro-futuristic · Glassmorphic · Other

**11. Reference brands or sites that inspire the vibe** (list 3 if possible)
- Used to anchor the aesthetic — not to copy. Examples: *"Linear, Stripe, Pitch"* or *"Aesop, Hermès, Apple"* or *"Vercel, Resend, Cal.com"*.

**12. Typography preference** (default: **distinctive pairing chosen by the skill**)
- Specific fonts (e.g., "Clash Display + Satoshi")
- A vibe ("editorial serif + monospace") — skill picks
- Free-only constraint? (Y / N)

**13. Color direction** (default: **chosen from tone + references**)
- Specific palette (paste hex codes)
- A direction ("warm earth tones," "high-contrast monochrome," "muted pastels")
- Hard constraints ("no purple," "no neon green")

---

## Section 4 — Design preferences

**14. Dark mode** (default: **light default, optional dark**)
- Required (both modes equally polished)
- Optional (light is hero, dark supported)
- Dark only
- Light only

**15. Motion budget** (default: **medium — 2–3 intentional motions**)
- None / static
- Subtle (hovers, smooth transitions)
- Medium (entrance + scroll-linked + interaction)
- Energetic (multiple synchronized motions, scroll storytelling)

**16. Accessibility level** (default: **WCAG AA**)
- WCAG AA (recommended)
- WCAG AAA (strict)
- Not specified (still ships ≥AA)

---

## Section 5 — Constraints

**17. Target devices** (default: **desktop + mobile, both polished**)

**18. Performance targets** (default: **Core Web Vitals green**)
- LCP < 2.5s, INP < 200ms, CLS < 0.1

**19. Anything to avoid?** (free-text)
- e.g., "no dark mode," "no purple," "no carousels," "no AI-art imagery"

---

## Defaults applied if unanswered

- Next.js 14+ App Router, Tailwind CSS, no component library, Framer Motion
- Intermediate codebase level
- Greenfield landing page
- No existing brand — picks tone "bold and distinctive"
- Distinctive Google Fonts pair (NOT Inter, NOT Roboto)
- Light mode default, dark mode optional
- 2–3 intentional motions
- WCAG AA
- Desktop + mobile both polished
