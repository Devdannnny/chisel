# Pre-Install Questionnaire — Flutter

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
- If existing: any `ThemeData`, `ThemeExtensions`, or widget conventions to preserve.

---

## Section 2 — Stack

**5. Platform target** (default: **adaptive (Material 3 + Cupertino)**)
- iOS only — Cupertino design language
- Android only — Material 3
- Both — adaptive widgets, or Material 3 cross-platform
- Web / desktop (Flutter web/macOS/Windows)

**6. Design system** (default: **Material 3**)
- Material 3 (recommended for cross-platform and Android)
- Cupertino (iOS-native feel)
- Custom design system (you have one)

**7. State management** (default: **Bloc / Cubit**)
- Bloc / Cubit (recommended for this codebase)
- Provider (optional — typically for snapshot caches / notifiers)
- Riverpod (optional)
- GetX (optional)
- setState only (only for very small widgets)
- Other

**8. Routing** (default: **auto_route**)
- auto_route (recommended for this codebase)
- go_router (optional)
- Navigator 1.0 (legacy)
- Other

**9. Animation approach** (default: **flutter_animate for composition, AnimationController for fine control**)
- `flutter_animate` package (declarative, composable)
- Raw `AnimationController` + `Tween`
- `Rive` (designer-led named-state animations)
- `Lottie` (after-effects exports)
- `ImplicitlyAnimatedWidget` only (built-in)

**10. Image handling** (default: **`cached_network_image`**)
- `cached_network_image` (recommended for network images)
- `Image.network` (basic)
- `Image.asset` (local only)
- Custom solution

---

## Section 3 — Brand & aesthetic

**11. Existing brand?** (Y / N)
- If Y: list locked items (logo asset path, brand fonts, palette, voice).

**12. Tone direction** (pick 1–2)
- Playful · Professional · Minimal · Bold · Luxe · Technical · Editorial · Organic · Retro · Other

**13. Reference apps that inspire the vibe** (3 if possible)
- e.g., *"Linear app, Notion mobile, Apple Music"* or *"Things 3, Calm, Headspace"*

**14. Typography preference** (default: skill picks via `google_fonts`)
- Specific fonts (e.g., "Fraunces display + Lexend body")
- A vibe ("editorial serif + clean sans")
- Free-only? (Y / N — `google_fonts` is free)

**15. Color seed** (default: skill picks a non-default seed)
- Specific hex (e.g., `#B54B2A` for editorial terracotta)
- A direction (warm earth, cool monochrome, vibrant electric, muted pastels)
- Hard constraints ("no blue," "no purple")

---

## Section 4 — Design preferences

**16. Theme modes** (default: **System (supports Light + Dark)**)
- System (supports Light + Dark) (recommended)
- Light only
- Dark only

**17. Motion budget** (default: **medium — 2–3 intentional motions**)
- None / Subtle / Medium / Energetic

**18. Accessibility level** (default: **WCAG AA with semantic widgets**)
- WCAG AA + screen reader support (`Semantics` widgets)
- WCAG AAA (high contrast, larger touch targets)
- Not specified

---

## Section 5 — Constraints

**19. Target device classes** (default: **phone primary, tablet supported**)
- Phone only
- Phone + tablet (responsive)
- Phone + tablet + foldables (advanced)

**20. Performance targets** (default: **60fps on mid-range mobile**)
- 60fps on mid-range
- 60fps on high-end only
- 120fps on capable devices

**21. Flutter SDK version** (default: **latest stable**)
- Specify if you're on a pinned version (affects Material 3 defaults, new architecture)

**22. Anything to avoid?** (free-text)
- e.g., "no Roboto," "no bottom nav," "no FAB," "no AI-generated illustrations"

---

## Defaults applied if unanswered

- Flutter latest stable, Material 3 cross-platform
- Bloc/Cubit + auto_route
- `flutter_animate` for motion, `cached_network_image` for images, `google_fonts` for typography
- Non-default `ColorScheme.fromSeed` (warm terracotta seed)
- ThemeMode.system (light + dark both polished)
- Intermediate codebase level
- Phone primary, tablet responsive
- 60fps target on mid-range
- WCAG AA with `Semantics`
- Distinctive Google Fonts pair (NOT Roboto, NOT default Material font)
