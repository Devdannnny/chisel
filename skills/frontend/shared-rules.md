# Frontend — Shared Design Rules

Canonical design language for every `frontend/*` skill in this directory. Every sub-skill (`next`, `react`, `flutter`, `react-native`, and any future addition) inherits from this document.

**When you load any `frontend/*` skill, READ THIS FILE FIRST.** Each platform-specific SKILL.md assumes these rules are already in context.

## Source

This document synthesizes:
- **Anthropic's `frontend-design` skill** — bold-aesthetic-direction emphasis, design thinking framework.
- **Claude's frontend aesthetics cookbook** — the `<frontend_aesthetics>` distilled prompt; typography/color/motion/background dimensions.
- **OpenAI's GPT-5 frontend design guide** — composition rules, hero discipline, anti-patterns, litmus checks.
- The **Prompt Architecture Standard (PAS)** — `skills/prompt-architect/`.

---

## Design thinking (before any code)

Generic outputs come from skipping this step. Commit to a clear aesthetic direction first.

1. **Purpose.** What problem does this interface solve? Who uses it? What do they need to do or feel?
2. **Tone.** Pick an extreme and execute precisely. Real options: *brutally minimal, maximalist editorial, retro-futuristic, organic/natural, luxury/refined, playful/toy-like, brutalist/raw, art deco/geometric, soft/pastel, industrial/utilitarian, magazine/editorial, glassmorphic, neo-skeuomorphic.* Anchors only — design something specific to the brand.
3. **Constraints.** Framework, performance, accessibility, browser/OS support, motion budget. State them.
4. **Differentiation.** What is the one thing someone will remember about this interface? If you cannot answer in a sentence, the design is not specific enough.
5. **Visual thesis.** Write one paragraph describing the interface: type system, color palette, motion strategy, hero treatment. Write it *before* you code.

**CRITICAL:** Bold maximalism and refined minimalism both work. The discriminator is intentionality, not intensity. Generic is worse than ugly.

---

## Hard rules (apply universally)

### Composition

- **One composition.** The first viewport must read as one composition, not a dashboard of pieces (unless it *is* a dashboard).
- **One job per section.** Each section has one purpose, one headline, usually one short supporting sentence.
- **One dominant idea per section.** No section should need many tiny UI devices to explain itself.

### Brand

- **Brand first.** On branded interfaces, the brand or product name is a hero-level signal — not just nav text or an eyebrow. No headline overpowers the brand.
- **Brand test.** If the first viewport could belong to another brand after removing the nav/header, the branding is too weak. Fix it.

### Typography

- **Avoid default stacks.** Never use Inter, Roboto, Open Sans, Lato, Arial, SF, or system-font defaults. They signal "AI slop" instantly.
- **Choose distinctive fonts.** Real picks from Google Fonts:
  - **Code aesthetic:** JetBrains Mono, Fira Code, Berkeley Mono
  - **Editorial:** Playfair Display, Crimson Pro, Fraunces, Newsreader
  - **Startup:** Clash Display, Satoshi, Cabinet Grotesk, Lexend, Manrope
  - **Technical:** IBM Plex (Sans / Mono / Serif), Source Sans 3
  - **Distinctive:** Bricolage Grotesque, Migra, Tobias, Editorial New, Unbounded, Recursive
- **Pair with high contrast.** Display + monospace, serif + geometric sans, variable font across weight extremes.
- **Use extremes.** Weight 100/200 vs 800/900, not 400 vs 600. Size jumps of 3x+, not 1.5x.
- **Two typefaces max** without a clear reason. State your choices before coding.
- **Don't converge on "AI-safe" fonts.** Specifically, Space Grotesk has become a tell.
- **Define a type scale.** Reference it everywhere; never inline `font-size: 16px` or `fontSize: 16`.

### Color & atmosphere

- **No flat single-color backgrounds.** Use gradients, images, atmospheric patterns, subtle textures, layered transparencies.
- **One accent color** by default. Multiple accents only with an established system.
- **Avoid purple-on-white defaults.** Avoid dark-mode-by-default unless the brief explicitly calls for it.
- **Commit to a cohesive palette.** Dominant colors with sharp accents outperform timid, evenly-distributed palettes.
- **Define theme tokens** — CSS variables, ThemeData/ThemeExtensions, theme objects. Reference them everywhere; never hardcode colors in widgets/components.

### Hero

- **Full-bleed hero only.** On landing pages and promotional surfaces, the hero image is a dominant edge-to-edge visual plane. No inset images, side-panel hero, rounded media cards, tiled collages, or floating image blocks unless the existing design system requires them.
- **Hero budget.** The first viewport contains only: the brand, one headline, one short supporting sentence, one CTA group, one dominant image. Nothing else. No stats, schedules, event listings, address blocks, promos, metadata rows, or secondary marketing content.
- **No hero overlays.** No detached labels, floating badges, promo stickers, info chips, or callout boxes on top of hero media.
- **Headlines fit.** 2–3 lines on desktop, readable in one glance on mobile.

### Cards

- **Default: no cards.** Never use cards in the hero. Cards are allowed only when the card is the container for a user interaction.
- **Test:** If removing border, shadow, background, or radius does not hurt interaction or understanding, it is not a card. Strip it.

### Imagery

- **Real visual anchor.** Imagery shows the product, place, atmosphere, or context. Decorative gradients and abstract backgrounds are not the main visual idea.
- **In-situ over abstract.** Prefer real photography over fake 3D, generic illustrations, or generated noise.
- **No embedded UI in generated images.** Do not generate images with UI frames, splits, cards, or panels — that's the interface's job.
- **Crop for text overlay.** When text sits on imagery, choose stable tonal areas with strong contrast.

### Motion

- **Use motion for presence and hierarchy, not noise.**
- **Ship 2–3 intentional motions** for visually led work:
  - One entrance sequence (load reveal, staggered entrance, hero animation)
  - One scroll-linked or sticky depth effect (where the platform supports scroll)
  - One hover/press/reveal/layout transition for affordance
- **Constraints:** noticeable in a quick recording, smooth on mobile, fast and restrained, consistent across the interface. If a motion is purely ornamental, remove it.

### Clutter

- **Avoid:** pill clusters, stat strips, icon rows, boxed promos, schedule snippets, multiple competing text blocks.
- **Delete 30% of the copy.** If the interface still reads, keep deleting.

### Responsive / Adaptive

- **Both target form factors correct.** Not "primary device with secondary patched on."
- **Tap targets:** ≥44pt (iOS) / 48dp (Android) / 44×44px (web touch surfaces).
- **Safe areas:** account for notches, dynamic islands, home indicators, navigation bars.
- **No layout shift on load.** Reserve space for images, fonts, async content.

---

## Surface-specific defaults

### Landing / marketing surfaces

- Default narrative: hero → supporting imagery/context → product detail → social proof → final CTA.
- Visually led; image-forward; bold typography; ambitious motion budget.
- "Linear-style restraint" is wrong here. Marketing needs *presence*.

### Product UI / app surfaces

- Default to **Linear-style restraint:** calm surface hierarchy, strong typography, generous spacing, few colors, dense but readable, minimal chrome.
- Organize around: primary workspace, navigation, secondary context/inspector, one clear accent for action.
- Utility copy: state what the area is and what users can do. No aspirational hero lines, no metaphors, no executive banners unless requested.
- Litmus test: if an operator scans only headings, labels, and numbers, can they understand the interface immediately?
- Cards only when the card is itself the interaction.

### Dashboards

- Permissive of density. Information first, decoration second.
- Strong type hierarchy carries the load.
- Avoid: dashboard-card mosaics, decorative gradients behind data, multiple competing accent colors.

### Portfolios / editorial / narrative

- High typography contrast. Image-driven. Long-scroll narratives are fine.
- Motion can carry more weight here. Story-pacing matters.

---

## Universal anti-patterns (do NOT ship)

- Inter / Roboto / SF / system stack as the only font
- Purple-to-blue gradient on a white background
- Hero with 7 stat tiles
- Card grid as the first impression
- Dark mode by default with no light fallback (unless the brief explicitly calls for it)
- "Get started for free" CTA with no other context
- Decorative gradient pretending to be a visual anchor
- Three carousels stacked vertically
- Card-of-cards architecture
- Beautiful image, weak brand presence
- The starter-template smell (vanilla Vite / Expo / Flutter / CRA starter screens shipping)

---

## Universal litmus checks (apply before declaring done)

- [ ] **Brand test.** Could the first viewport belong to another brand after the nav/header is removed?
- [ ] **Single composition.** Does the first viewport read as one designed thing, not a dashboard of pieces?
- [ ] **Visual anchor.** Is there one strong visual anchor (product photo, atmosphere, signature element)?
- [ ] **Scannable headlines.** Can the interface be understood by reading only headlines and labels?
- [ ] **One job per section.** Does each section have a single clear purpose?
- [ ] **Cards justified.** Cards present *only* where the card is itself the interaction?
- [ ] **Motion improves hierarchy.** Does motion strengthen presence and orientation, not noise?
- [ ] **Type test.** Remove all decorative shadows, gradients, borders — does the interface still feel premium from typography and spacing alone?
- [ ] **Responsive correctness.** Layout works at primary mobile size without horizontal scroll, tap targets ≥44pt?
- [ ] **Anti-pattern audit.** No generic stack fonts, no purple-on-white, no card grids in hero, no overlays on hero media?

---

## Universal output format

When generating code, deliver in this order:

1. **One visual-thesis paragraph** before the code. State aesthetic choices: fonts, colors, motion strategy, hero treatment. This is the contract.
2. **Working code** as the primary artifact. Self-contained when possible. Production-quality, not pseudocode.
3. **Short list of choices made** after the code:
   - Anything skipped or simplified and why
   - Open questions for the user
   - Tradeoffs

---

## Universal validation & reporting

Before finishing, run through the universal litmus checks above. Then report:

- **What was built** (one paragraph)
- **Aesthetic choices** — fonts, palette, motion strategy, hero treatment
- **What was skipped/simplified** and why
- **Open questions** the user should answer
- **Anti-patterns actively rejected** — call out the trap-doors you noticed and avoided

(Platform-specific reports add stack-specific items — see each sub-skill's SKILL.md for the additions.)

---

## When NOT to apply these rules

- The user is working inside an existing design system or brand — *preserve* it.
- Bug fixes or non-design work.
- Migrations where the goal is faithful translation, not redesign.

For platform-specific application of these rules (code patterns, stack idioms, framework-specific anti-patterns), see each sub-skill:

- [`next/SKILL.md`](./next/SKILL.md) — Next.js App Router / Pages Router
- [`react/SKILL.md`](./react/SKILL.md) — React without Next (Vite, CRA, Remix)
- [`flutter/SKILL.md`](./flutter/SKILL.md) — Flutter mobile (Material 3 + Cupertino)
- [`react-native/SKILL.md`](./react-native/SKILL.md) — React Native with Expo

---

## Maintenance

This is the single source of truth for the frontend design language. To update a universal rule, edit this file and the change propagates to every sub-skill automatically. Platform-specific edits go in the relevant sub-skill's SKILL.md.

If you find yourself updating the same rule in multiple SKILL.md files, that rule belongs here instead.
