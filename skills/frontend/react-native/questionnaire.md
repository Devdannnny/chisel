# Pre-Install Questionnaire — Frontend / React Native (Expo)

Answer these before invoking the skill. Output quality scales directly with how completely you answer.

## How to use

1. **Inline** — paste your answers as a prefix to your first prompt.
2. **Persistent** — save to `.claude/context.md` and reference it.
3. **Conversational** — invoke the skill and let it walk you through.

---

## Section 1 — Project basics

**1. Do you have a `screenshots/` folder at your repo root?** (Y / N)

**2. Codebase structure level** (default: **Intermediate**) — Beginner / Intermediate / Senior

**3. Project surface type** (pick one)
- Onboarding flow
- App home / main screen
- Product detail screen
- Feed / list view
- Settings / profile
- Auth screens
- Splash / launch
- Other (describe)

**4. New project or existing codebase?**
- If existing: list theme tokens, navigation patterns, or component conventions to preserve.

---

## Section 2 — Stack

**5. Expo SDK version** (default: **latest stable**)
- Specify if pinned. Affects Expo Router availability and the new RN architecture default.

**6. Routing** (default: **Expo Router**)
- Expo Router (file-based, recommended for new projects)
- React Navigation (stack, tab, drawer — legacy or migrating)
- Custom

**7. Styling system** (default: **NativeWind v4** if you want Tailwind, else **`StyleSheet.create` with theme tokens**)
- NativeWind (Tailwind for React Native)
- styled-components (`/native` variant)
- Tamagui (cross-platform design system)
- StyleSheet.create + theme tokens
- Restyle (from Shopify)

**8. Animation library** (default: **react-native-reanimated v3**)
- Reanimated v3 (recommended — runs on UI thread)
- Moti (declarative, built on Reanimated)
- Legacy `Animated` API (avoid in new code)

**9. Image handling** (default: **`expo-image`**)
- `expo-image` (recommended — faster, caching, blurhash)
- RN `Image` (basic)
- `react-native-fast-image` (third-party)

**10. List component** (default: **`@shopify/flash-list`**)
- FlashList (recommended for non-trivial lists)
- FlatList (basic, fine for small static lists)
- ScrollView (for short, non-virtualized content)

**11. Platform target** (default: **iOS + Android, both polished**)
- iOS only
- Android only
- Both
- + Web (Expo for web — affects styling choices)

---

## Section 3 — Brand & aesthetic

**12. Existing brand?** (Y / N)
- If Y: list locked items (logo asset path, brand fonts, palette, voice).

**13. Tone direction** (pick 1–2)
- Playful · Professional · Minimal · Bold · Luxe · Technical · Editorial · Organic · Retro · Other

**14. Reference apps that inspire the vibe** (3 if possible)
- e.g., *"Linear app, Notion mobile, Apple Music"* or *"Things 3, Calm, Headspace, Roam"*

**15. Typography preference** (default: skill picks via `@expo-google-fonts/*`)
- Specific fonts (e.g., "Fraunces display + Manrope body")
- A vibe ("editorial serif + clean sans")
- Free-only? (Y / N — Google Fonts is free)

**16. Color direction** (default: skill picks based on tone)
- Specific palette (paste hex codes for light + dark)
- A direction (warm earth, cool monochrome, vibrant, muted pastels)
- Hard constraints ("no blue," "no purple")

---

## Section 4 — Design preferences

**17. Color scheme** (default: **both light and dark, both polished**)
- Both (recommended)
- Light only
- Dark only
- Match system

**18. Motion budget** (default: **medium — 2–3 intentional motions**)
- None / Subtle / Medium / Energetic

**19. Accessibility level** (default: **WCAG AA + VoiceOver/TalkBack labels**)
- WCAG AA + screen reader support
- WCAG AAA
- Not specified

---

## Section 5 — Constraints

**20. Device targets** (default: **iPhone + Android phone, all modern**)
- iPhone only (iOS 15+)
- Android only (API 28+)
- Both phone (recommended)
- + Tablet (iPad / Android tablet)

**21. Performance targets** (default: **60fps on mid-range, < 200ms cold start**)

**22. Bare RN or Expo?** (default: **Expo managed**)
- Expo managed workflow (recommended for new projects)
- Expo bare workflow (you've ejected or need native modules)
- Bare React Native (no Expo) — flag this; some patterns differ

**23. Anything to avoid?** (free-text)
- e.g., "no system fonts," "no bottom tabs," "no FAB," "no AI-art splash"

---

## Defaults applied if unanswered

- Expo SDK latest, Expo Router, NativeWind for styling
- react-native-reanimated v3 for motion, expo-image for images, @shopify/flash-list for lists
- `@expo-google-fonts/*` for typography (Fraunces + Manrope by default)
- Both light + dark polished, theme tokens consumed via context (no inline hex)
- `react-native-safe-area-context` on every screen root
- Intermediate codebase level
- iPhone + Android phone, both
- 60fps target on mid-range
- WCAG AA + VoiceOver/TalkBack labels
- Distinctive fonts (NOT system, NOT Inter)
- 2–3 intentional motions
