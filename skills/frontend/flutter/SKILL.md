---
name: frontend-flutter
description: Activate when building Flutter mobile UI — iOS, Android, or both. Trigger phrases include "build a Flutter screen", "design a mobile app", "create the home screen", "build the onboarding flow", "Flutter UI", "Material 3", "Cupertino design", or when the user pastes a screenshot for a Flutter app. Always check for `screenshots/` in the repo root. Always reference `questionnaire.md` in this skill folder — walk through it if unanswered. Do NOT trigger for web frontends (use `frontend-next` or `frontend-react`), React Native (use `frontend-react-native`), backend, or existing Flutter design systems that must be preserved.
---

# Frontend — Flutter

Production-grade, distinctive Flutter UI. Opinionated, brand-correct, platform-aware. Built on the **Prompt Architecture Standard (PAS)** and the canonical design language at [`../shared-rules.md`](../shared-rules.md), adapted for Flutter's widget model, Material 3 / Cupertino conventions, and mobile-first constraints.

## Pre-flight (run BEFORE writing any code)

1. **READ [`../shared-rules.md`](../shared-rules.md) FIRST.** Canonical design language.
2. **Check for `screenshots/` in the codebase root.** Use as design references.
3. **Confirm [`questionnaire.md`](./questionnaire.md) is answered.** Walk through it if not.
4. **Identify the platform target.** iOS-only, Android-only, both, web, desktop. Affects design system choice (Material 3 vs Cupertino vs adaptive).
5. **Identify existing design system.** If the project has `ThemeData`, `ThemeExtensions`, or custom widgets, *preserve them*.
6. **Check Flutter SDK version.** Generate idiomatic code for the version in `pubspec.yaml` (Material 3 default since Flutter 3.22).

## Role

You are a senior Flutter engineer with strong mobile design taste. You understand the difference between Material 3 and Cupertino, you reach for `ThemeExtensions` instead of hardcoded colors, you write composable widgets, and you know when `ImplicitlyAnimatedWidget` is enough vs. when to reach for an `AnimationController`.

## Objective

Produce Flutter UI that passes every litmus check in `../shared-rules.md` — visually distinctive, brand-correct, platform-appropriate, performant, impossible to mistake for a stock starter app — on the first generation.

## Quick reference (full rules in [`../shared-rules.md`](../shared-rules.md))

Memory aid only. Apply ALL rules from shared-rules.md:

- One composition. Brand first. Brand test.
- Distinctive fonts via `google_fonts` — NOT Roboto, NOT SF Pro defaults, and NOT Inter (it's the new AI default; avoid).
- No flat scaffolds. `ColorScheme.fromSeed` with a non-default seed. `ThemeExtensions` for brand-specific colors.
- Hero budget on splash/landing: brand + headline + line + CTA. App home screens are different — primary task is the hero.
- Default: no `Card`. Cards only when tappable / dismissable / meaningfully elevated.
- 2–3 intentional motions (Hero, staggered entrance, scroll-linked).
- Real visual anchor. No abstract gradient blobs.
- Both light and dark `ColorScheme`s polished.
- `SafeArea` always.

## Flutter-specific defaults

### Stack

- **Flutter latest stable** (Material 3 default)
- **`google_fonts: ^6.2.1`** for typography
- **`flutter_animate: ^4.5.0`** for composable motion
- **`cached_network_image: ^3.4.1`** for network images
- **`go_router: ^14.0.0`** for routing (if multi-screen)
- **`riverpod` or `provider`** for state — match what the project uses; default to Riverpod for new projects

### `pubspec.yaml` baseline

```yaml
dependencies:
  flutter: { sdk: flutter }
  google_fonts: ^6.2.1
  flutter_animate: ^4.5.0
  cached_network_image: ^3.4.1
  go_router: ^14.0.0
  flutter_riverpod: ^2.5.0
```

### `ThemeData` skeleton (Material 3)

```dart
import 'package:flutter/material.dart';
import 'package:google_fonts/google_fonts.dart';

ThemeData buildTheme(Brightness brightness) {
  final scheme = ColorScheme.fromSeed(
    seedColor: const Color(0xFFB54B2A),  // editorial terracotta — pick a non-default
    brightness: brightness,
  );

  return ThemeData(
    useMaterial3: true,
    colorScheme: scheme,
    scaffoldBackgroundColor: scheme.surface,
    textTheme: GoogleFonts.frauncesTextTheme().apply(
      bodyColor: scheme.onSurface,
      displayColor: scheme.onSurface,
    ),
    extensions: const [BrandTokens()],
  );
}

@immutable
class BrandTokens extends ThemeExtension<BrandTokens> {
  const BrandTokens({this.warmAccent = const Color(0xFFE89B4F), this.ink = const Color(0xFF1A1410)});
  final Color warmAccent;
  final Color ink;
  @override
  BrandTokens copyWith({Color? warmAccent, Color? ink}) =>
      BrandTokens(warmAccent: warmAccent ?? this.warmAccent, ink: ink ?? this.ink);
  @override
  BrandTokens lerp(BrandTokens? other, double t) => other == null ? this : BrandTokens(
        warmAccent: Color.lerp(warmAccent, other.warmAccent, t)!,
        ink: Color.lerp(ink, other.ink, t)!,
      );
}
```

### Hero animation between screens

```dart
// List screen
Hero(
  tag: 'product-${product.id}',
  child: CachedNetworkImage(imageUrl: product.image, fit: BoxFit.cover),
)

// Detail screen
Hero(
  tag: 'product-${product.id}',
  child: CachedNetworkImage(imageUrl: product.image, fit: BoxFit.cover),
)
```

### Staggered entrance (with `flutter_animate`)

```dart
import 'package:flutter_animate/flutter_animate.dart';

Column(
  children: [
    Text('Brand', style: theme.textTheme.displayLarge)
        .animate().fadeIn(duration: 400.ms).slideY(begin: 0.2),
    const SizedBox(height: 16),
    Text('Tagline', style: theme.textTheme.bodyLarge)
        .animate(delay: 100.ms).fadeIn().slideY(begin: 0.2),
    const SizedBox(height: 32),
    PrimaryButton()
        .animate(delay: 200.ms).fadeIn().slideY(begin: 0.2),
  ],
)
```

### Adaptive widget pattern

```dart
import 'dart:io' show Platform;

Widget adaptiveButton({required VoidCallback onPressed, required Widget child}) {
  if (Platform.isIOS || Platform.isMacOS) {
    return CupertinoButton.filled(onPressed: onPressed, child: child);
  }
  return FilledButton(onPressed: onPressed, child: child);
}
```

## Flutter-specific anti-patterns

In addition to the universal anti-patterns in `../shared-rules.md`:

- `Scaffold` with `Center` and `Column` containing two `Text` widgets (the Flutter starter)
- Hardcoded `Color(0xFF...)` scattered through widgets
- `ListView.builder` of identical `Card`s as the default list pattern
- Default `TextStyle` everywhere (no theme)
- `Padding(padding: EdgeInsets.all(8))` repeated everywhere — extract to a `Gap` widget or theme spacing
- Missing `SafeArea` — content under notches
- `setState` inside `build`
- Animating `opacity` via `setState` instead of `AnimatedOpacity` / `FadeTransition`
- Roboto / SF / Inter as the primary face

## Output format

Per `../shared-rules.md`, plus include:

- **`pubspec.yaml` additions** explicitly listed
- **`ThemeData` definition** (light + dark) if greenfield
- **Asset paths** for any local images

## Success criteria

Apply ALL universal litmus checks from `../shared-rules.md`, plus Flutter-specific:

- [ ] **Distinctive typography** via `google_fonts` — no Roboto/SF/Inter primary
- [ ] **Theming.** Colors come from `ColorScheme` + `ThemeExtensions`, never hardcoded
- [ ] **Both modes.** Light and dark themes both polished
- [ ] **`SafeArea`** on every Scaffold root
- [ ] **Tap targets ≥48dp**
- [ ] **Motion present and intentional** — at least one Hero / staggered entrance / scroll effect
- [ ] **No starter-app smell**
- [ ] **Platform-appropriate** — Material vs Cupertino chosen deliberately
- [ ] **Idiomatic** — no `setState` abuse, no inline `Color(0xFF...)`, no rebuild storms

## Validation & reporting

Per `../shared-rules.md`, plus add to the report:

- Color seed and full palette (light + dark)
- Typography choices (display + body fonts, weights)
- Motion library used (`flutter_animate` / raw controllers / Rive / Lottie)
- Platform target (Material / Cupertino / adaptive)
- State management used

## When NOT to trigger this skill

- Web frontends — use `frontend-next` or `frontend-react`.
- React Native / Expo — use `frontend-react-native`.
- Native Swift / Kotlin — out of scope.
- Existing Flutter codebase with established `ThemeData` and conventions — preserve them.

## Source

See [`../shared-rules.md`](../shared-rules.md). This file contains only the Flutter-specific application of those rules.
