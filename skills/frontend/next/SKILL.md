---
name: frontend-next
description: Activate when building Next.js (App Router or Pages Router) web frontends — landing pages, marketing pages, product UI, dashboards, portfolios. Trigger phrases include "build a Next.js page", "design a Next.js landing page", "create the UI in Next", "build the marketing site", "App Router", "server components", or when the user pastes a screenshot for a Next.js project. Always check for `screenshots/` in the repo root. Always reference `questionnaire.md` in this skill folder — walk the user through it if unanswered. Use `frontend-react` for non-Next React (Vite, CRA, Remix). Do NOT trigger for Flutter (use `frontend-flutter`), React Native (use `frontend-react-native`), backend, or existing design systems that must be preserved.
---

# Frontend — Next.js

Production-grade, distinctive Next.js UI. Opinionated, brand-correct, free of "AI slop" aesthetics. Built on the **Prompt Architecture Standard (PAS)** and the canonical design language at [`../shared-rules.md`](../shared-rules.md).

## Pre-flight (run BEFORE writing any code)

1. **READ [`../shared-rules.md`](../shared-rules.md) FIRST.** It is the canonical design language. This SKILL.md assumes you have those rules in context.
2. **Check for `screenshots/` in the codebase root.** If present, extract typography, layout rhythm, color, spacing, atmosphere as design references.
3. **Confirm [`questionnaire.md`](./questionnaire.md) is answered.** Walk through it if not.
4. **Identify the router.** App Router (Next 14+, preferred) vs. Pages Router (legacy). Generate matching patterns.
5. **Identify the surface.** Landing / marketing / product UI / dashboard / portfolio — see surface-specific defaults in `../shared-rules.md`.
6. **Identify existing design system.** Preserve it if present. The rules apply to greenfield work.

## Role

You are a senior Next.js engineer with strong design taste. You think in Server Components first, you reach for `next/font` and `next/image` by default, and you ship distinctive, brand-correct UI on the first generation.

## Objective

Produce a Next.js frontend that passes every litmus check in `../shared-rules.md` — visually distinctive, brand-correct, technically sound, impossible to mistake for generic AI output — on the first generation.

## Quick reference (full rules in [`../shared-rules.md`](../shared-rules.md))

Memory aid only. Apply ALL rules from shared-rules.md:

- One composition. Brand first. Brand test.
- Distinctive Google Fonts via `next/font` — NOT Inter, NOT Roboto.
- No flat backgrounds. One accent. CSS variables.
- Full-bleed hero only. Hero budget: brand + headline + line + CTA + image.
- No cards by default. Cards only when card IS the interaction.
- 2–3 intentional motions (Framer Motion).
- Real visual anchor. No abstract gradient blobs.
- Both light and dark polished.
- Delete 30% of copy.

## Next-specific defaults

### Stack

- **Next 14+** with App Router (use Pages Router only if the codebase requires it)
- **Tailwind CSS** for utility styling (default)
- **CSS variables** for theming (defined in `:root` and a `dark` selector)
- **Framer Motion** for animation (client components only)
- **`next/font`** for self-hosted Google Fonts (no FOUT)
- **`next/image`** for image optimization
- **Server Components first** — client only where interactivity demands

### Loading fonts (App Router)

```ts
// app/fonts.ts
import { Fraunces, Manrope } from 'next/font/google';

export const display = Fraunces({
  subsets: ['latin'],
  weight: ['400', '700', '900'],
  variable: '--font-display',
  display: 'swap',
});

export const body = Manrope({
  subsets: ['latin'],
  weight: ['400', '500', '700'],
  variable: '--font-body',
  display: 'swap',
});
```

```tsx
// app/layout.tsx
import { display, body } from './fonts';

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" className={`${display.variable} ${body.variable}`}>
      <body>{children}</body>
    </html>
  );
}
```

### CSS variables baseline

```css
/* app/globals.css */
:root {
  --bg: oklch(98% 0.01 90);
  --fg: oklch(15% 0.02 250);
  --accent: oklch(60% 0.18 25);
  --muted: oklch(70% 0.02 250);
  --header-h: 4rem;
}

.dark, [data-theme="dark"] {
  --bg: oklch(15% 0.02 250);
  --fg: oklch(95% 0.01 90);
}

body {
  background: var(--bg);
  color: var(--fg);
  font-family: var(--font-body);
}
```

### Tailwind config

```ts
// tailwind.config.ts
import type { Config } from 'tailwindcss';

export default {
  content: ['./app/**/*.{ts,tsx}', './components/**/*.{ts,tsx}'],
  darkMode: ['class', '[data-theme="dark"]'],
  theme: {
    extend: {
      colors: {
        bg: 'var(--bg)',
        fg: 'var(--fg)',
        accent: 'var(--accent)',
        muted: 'var(--muted)',
      },
      fontFamily: {
        display: 'var(--font-display)',
        body: 'var(--font-body)',
      },
    },
  },
} satisfies Config;
```

### Framer Motion entrance pattern (Client Component)

```tsx
// components/Hero.tsx — must be 'use client' because Framer Motion needs browser APIs
'use client';
import { motion } from 'framer-motion';

const stagger = { animate: { transition: { staggerChildren: 0.08 } } };
const fadeUp = {
  initial: { opacity: 0, y: 24 },
  animate: { opacity: 1, y: 0, transition: { duration: 0.6, ease: [0.16, 1, 0.3, 1] } },
};

export function Hero() {
  return (
    <motion.section
      variants={stagger}
      initial="initial"
      animate="animate"
      className="min-h-[calc(100svh-var(--header-h))]"
    >
      <motion.h1 variants={fadeUp} className="font-display text-7xl">Brand</motion.h1>
      <motion.p variants={fadeUp} className="font-body text-lg max-w-prose">
        One sentence that does the work.
      </motion.p>
      <motion.div variants={fadeUp}>{/* CTA */}</motion.div>
    </motion.section>
  );
}
```

### Image patterns

```tsx
import Image from 'next/image';

<Image
  src="/hero.jpg"
  alt="Editorial product photograph"
  fill
  priority
  sizes="100vw"
  className="object-cover"
/>
```

## Next-specific anti-patterns

In addition to the universal anti-patterns in `../shared-rules.md`:

- `'use client'` on every component without thinking about it — RSC is the default
- `useState` for derived values that should be computed
- `<img>` instead of `next/image`
- Inline `<style jsx>` instead of Tailwind / CSS modules
- Mixing App Router and Pages Router conventions in the same project without reason
- Importing client-only libraries (Framer Motion, Zustand) into Server Components

## React patterns (apply when the codebase uses them)

- `useEffectEvent`, `startTransition`, `useDeferredValue` when appropriate
- Do NOT add `useMemo` / `useCallback` by default — follow React Compiler guidance
- Suspense boundaries around async data fetches

## Output format

Per `../shared-rules.md`, plus include:

- **Required `next.config.mjs` changes** (if any — image domains, experimental flags)
- **Required `tailwind.config.ts` changes**
- **List of new Server vs Client components** in the diff

## Success criteria

Apply ALL universal litmus checks from `../shared-rules.md`, plus Next-specific:

- [ ] **Server Components by default**, `'use client'` only where interactivity needs it
- [ ] **`next/font`** for typography (not `<link>` tags to Google Fonts)
- [ ] **`next/image`** for any meaningful image
- [ ] **CSS variables** drive theming, both `:root` and `.dark`
- [ ] **Tailwind config extends** to reference CSS variables
- [ ] **No layout shift** — `priority` on above-the-fold images, font `display: swap`

## Validation & reporting

Per `../shared-rules.md`, plus add to the report:

- Router used (App / Pages)
- Server vs Client component split
- Image strategy
- Font loading method

## When NOT to trigger this skill

- Non-Next React projects — use `frontend-react`.
- Flutter, React Native — use the relevant frontend sub-skill.
- Existing Next codebase with established conventions — preserve them.
- Bug fixes without design choices.

## Source

See [`../shared-rules.md`](../shared-rules.md) for the canonical design language and synthesis sources. This file contains only the Next.js-specific application of those rules.
