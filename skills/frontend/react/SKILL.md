---
name: frontend-react
description: Activate when building React frontends WITHOUT Next.js — typically Vite + React, Create React App, or Remix (when used outside the App Router pattern). Trigger phrases include "build a React app with Vite", "create the UI in React", "build a React component", "Vite + React", or when the user pastes a screenshot for a non-Next React project. Always check for `screenshots/` in the repo root. Always reference `questionnaire.md` in this skill folder — walk the user through it if unanswered. Use `frontend-next` for Next.js specifically. Do NOT trigger for Flutter, React Native, backend, or existing design systems that must be preserved.
---

# Frontend — React (Vite, CRA, non-Next)

Production-grade, distinctive React UI built outside Next.js — typically Vite + React. Opinionated, brand-correct, free of "AI slop" aesthetics. Built on the **Prompt Architecture Standard (PAS)** and the canonical design language at [`../shared-rules.md`](../shared-rules.md).

## Pre-flight (run BEFORE writing any code)

1. **READ [`../shared-rules.md`](../shared-rules.md) FIRST.** Canonical design language. This SKILL.md assumes you have those rules in context.
2. **Check for `screenshots/` in the codebase root.** Use as design references if present.
3. **Confirm [`questionnaire.md`](./questionnaire.md) is answered.** Walk through it if not.
4. **Identify the build tool.** Vite (preferred for new), CRA (legacy), Remix (non-App-Router), other.
5. **Identify the surface** — see surface-specific defaults in `../shared-rules.md`.
6. **Identify existing design system.** Preserve it. The rules apply to greenfield work.

## Role

You are a senior React engineer with strong design taste. You ship distinctive UI in Vite-based React, you know modern React 18 patterns (`useDeferredValue`, `startTransition`), and you don't reach for `useMemo` unless you've measured a problem.

## Objective

Produce React UI that passes every litmus check in `../shared-rules.md` — visually distinctive, brand-correct, technically sound, impossible to mistake for a stock Vite template — on the first generation.

## Quick reference (full rules in [`../shared-rules.md`](../shared-rules.md))

Memory aid only. Apply ALL rules from shared-rules.md:

- One composition. Brand first. Brand test.
- Distinctive Google Fonts via `@fontsource/*` — NOT Inter, NOT Roboto.
- No flat backgrounds. One accent. CSS variables.
- Full-bleed hero only. Hero budget: brand + headline + line + CTA + image.
- No cards by default. Cards only when card IS the interaction.
- 2–3 intentional motions (Framer Motion).
- Real visual anchor. No abstract gradient blobs.
- Both light and dark polished.
- Delete 30% of copy.

## React-specific defaults

### Stack

- **Vite 5+** with React 18+
- **react-router-dom 6+** for routing (or TanStack Router if the team prefers)
- **Tailwind CSS** for utility styling (default)
- **CSS variables** for theming
- **Framer Motion** for animation
- **`@fontsource/*`** packages for self-hosted Google Fonts (avoids FOUT)
- **`vite-plugin-image-presets`** or a CDN for image optimization

### `package.json` baseline

```json
{
  "dependencies": {
    "react": "^18.3.0",
    "react-dom": "^18.3.0",
    "react-router-dom": "^6.26.0",
    "framer-motion": "^11.3.0",
    "@fontsource/fraunces": "^5.0.0",
    "@fontsource/manrope": "^5.0.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.3.0",
    "vite": "^5.4.0",
    "tailwindcss": "^3.4.0",
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0"
  }
}
```

### Loading fonts (Vite + `@fontsource`)

```ts
// src/main.tsx
import '@fontsource/fraunces/400.css';
import '@fontsource/fraunces/700.css';
import '@fontsource/manrope/400.css';
import '@fontsource/manrope/500.css';
import '@fontsource/manrope/700.css';
import './index.css';
```

### CSS variables baseline

```css
/* src/index.css */
:root {
  --bg: oklch(98% 0.01 90);
  --fg: oklch(15% 0.02 250);
  --accent: oklch(60% 0.18 25);
  --muted: oklch(70% 0.02 250);
  --header-h: 4rem;
  --font-display: 'Fraunces', serif;
  --font-body: 'Manrope', sans-serif;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg: oklch(15% 0.02 250);
    --fg: oklch(95% 0.01 90);
  }
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
  content: ['./index.html', './src/**/*.{ts,tsx}'],
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

### Framer Motion entrance pattern

```tsx
import { motion } from 'framer-motion';

const stagger = { animate: { transition: { staggerChildren: 0.08 } } };
const fadeUp = {
  initial: { opacity: 0, y: 24 },
  animate: { opacity: 1, y: 0, transition: { duration: 0.6, ease: [0.16, 1, 0.3, 1] } },
};

export function Hero() {
  return (
    <motion.section variants={stagger} initial="initial" animate="animate" className="min-h-[calc(100svh-var(--header-h))]">
      <motion.h1 variants={fadeUp} className="font-display text-7xl">Brand</motion.h1>
      <motion.p variants={fadeUp} className="font-body text-lg max-w-prose">One sentence that does the work.</motion.p>
      <motion.div variants={fadeUp}>{/* CTA */}</motion.div>
    </motion.section>
  );
}
```

## React-specific anti-patterns

In addition to the universal anti-patterns in `../shared-rules.md`:

- Default `App.tsx` with two `<Counter />` buttons (the Vite template)
- `<a href>` for in-app navigation — use `<Link>` from `react-router-dom`
- `useState` for derived values that should be computed
- Class components in new code
- Inline `style={{ }}` everywhere instead of Tailwind / CSS modules
- `<img>` without `width`/`height` or aspect-ratio container (causes layout shift)
- `useEffect` with empty deps for one-shot setup that could be done at module scope

## React patterns

- Modern React 18+ when the codebase uses them: `useEffectEvent`, `startTransition`, `useDeferredValue`
- Do NOT add `useMemo` / `useCallback` by default — follow React Compiler guidance
- `Suspense` + `ErrorBoundary` around async data loaders
- Functional components only

## Output format

Per `../shared-rules.md`, plus include:

- **Required `package.json` deps** added
- **Required `vite.config.ts` changes**
- **Required `tailwind.config.ts` changes**
- **Font imports list** (which `@fontsource/*` packages were added)

## Success criteria

Apply ALL universal litmus checks from `../shared-rules.md`, plus React-specific:

- [ ] **`@fontsource/*`** loads custom fonts (not `<link>` to Google Fonts CDN)
- [ ] **CSS variables** drive theming
- [ ] **Tailwind config extends** to reference CSS variables
- [ ] **`react-router-dom` `<Link>`** for in-app navigation
- [ ] **No layout shift** on image load
- [ ] **Functional components only**, no classes

## Validation & reporting

Per `../shared-rules.md`, plus add to the report:

- Build tool (Vite / CRA / Remix / other)
- Routing library
- Image optimization strategy
- Font loading method

## When NOT to trigger this skill

- Next.js projects — use `frontend-next` (App Router patterns differ).
- Flutter, React Native — use the platform-specific frontend skill.
- Existing React codebase with established design system — preserve it.
- Bug fixes without design choices.

## Source

See [`../shared-rules.md`](../shared-rules.md). This file contains only the Vite/React-specific application of those rules.
