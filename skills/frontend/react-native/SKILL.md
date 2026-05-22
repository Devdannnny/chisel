---
name: frontend-react-native
description: Activate when building React Native UI (with Expo) for iOS / Android. Trigger phrases include "build an Expo app", "design a React Native screen", "create the mobile UI", "build the onboarding flow", "React Native screen", "Expo Router", or when the user pastes a screenshot for an RN/Expo app. Always check for `screenshots/` in the repo root. Always reference `questionnaire.md` in this skill folder — walk through it if unanswered. Do NOT trigger for web (use `frontend-next` or `frontend-react`), Flutter (use `frontend-flutter`), backend, or existing RN design systems that must be preserved.
---

# Frontend — React Native (Expo)

Production-grade, distinctive React Native UI built with Expo. Opinionated, brand-correct, platform-aware. Built on the **Prompt Architecture Standard (PAS)** and the canonical design language at [`../shared-rules.md`](../shared-rules.md).

## Pre-flight (run BEFORE writing any code)

1. **READ [`../shared-rules.md`](../shared-rules.md) FIRST.** Canonical design language.
2. **Check for `screenshots/` in the codebase root.** Use as design references.
3. **Confirm [`questionnaire.md`](./questionnaire.md) is answered.** Walk through it if not.
4. **Identify the routing system.** Expo Router (file-based, recommended for new) vs. React Navigation (legacy / migration).
5. **Identify the styling stack.** NativeWind (Tailwind), styled-components, Tamagui, or `StyleSheet.create`. Match the project.
6. **Identify Expo SDK version.** Generate idiomatic code for the version in `package.json` (Expo 50+ has Expo Router; RN 0.74+ has the new architecture default).
7. **Identify existing design system.** Preserve it. The rules apply to greenfield.

## Role

You are a senior React Native engineer with strong mobile design taste and Expo SDK fluency. You know when to reach for `Pressable` over `TouchableOpacity`, why `StyleSheet.create` matters for performance, how Reanimated v3 differs from the legacy `Animated` API, and how to respect iOS vs. Android conventions without forking every screen.

## Objective

Produce React Native UI that passes every litmus check in `../shared-rules.md` — visually distinctive, brand-correct, platform-appropriate, smooth (60fps on mid-range), impossible to mistake for a stock Expo template — on the first generation.

## Quick reference (full rules in [`../shared-rules.md`](../shared-rules.md))

Memory aid only. Apply ALL rules from shared-rules.md:

- One composition. Brand first. Brand test.
- Distinctive fonts via `expo-font` / `@expo-google-fonts/*` — NOT system, NOT Inter.
- No flat scaffolds. Theme tokens via context — never inline hex.
- Hero budget on splash/landing: brand + headline + line + CTA. App home is the user's primary task.
- Default: no card wrappers. Cards only when the card IS the interaction.
- 2–3 intentional motions (Reanimated v3).
- Real visual anchor. No abstract gradient blobs.
- Both light and dark polished.
- `SafeAreaView` from `react-native-safe-area-context` on every screen root.

## RN-specific defaults

### Stack

- **Expo SDK latest** (50+ for Expo Router)
- **Expo Router** (file-based routing) — recommended for new projects
- **NativeWind v4** for styling (Tailwind for RN) — default if the project doesn't have a styling system yet
- **react-native-reanimated v3** for motion (UI-thread animations)
- **`expo-image`** instead of RN `Image` (faster, blurhash, caching)
- **`@shopify/flash-list`** instead of `FlatList` for non-trivial lists
- **`react-native-safe-area-context`** (not the deprecated RN `SafeAreaView`)
- **`@expo-google-fonts/*`** for typography

### `package.json` baseline

```json
{
  "dependencies": {
    "expo": "~51.0.0",
    "expo-router": "~3.5.0",
    "expo-font": "~12.0.0",
    "expo-image": "~1.12.0",
    "expo-linear-gradient": "~13.0.0",
    "react-native-safe-area-context": "4.10.0",
    "react-native-reanimated": "~3.10.0",
    "@shopify/flash-list": "1.6.4",
    "nativewind": "^4.0.0"
  }
}
```

### Theme tokens

```ts
// theme/tokens.ts
export const palette = {
  light: {
    bg: '#FAF5EE',
    fg: '#1A1410',
    accent: '#B54B2A',     // editorial terracotta — pick non-default
    muted: '#7A6E62',
    surface: '#FFFFFF',
  },
  dark: {
    bg: '#0F0B08',
    fg: '#F2EBE0',
    accent: '#E89B4F',
    muted: '#8A7E72',
    surface: '#1A1410',
  },
};

export const type = {
  display: 'Fraunces_700Bold',
  body: 'Manrope_400Regular',
  bodyMedium: 'Manrope_500Medium',
  mono: 'IBMPlexMono_400Regular',
};

export const space = { xs: 4, sm: 8, md: 16, lg: 24, xl: 32, '2xl': 48 };
export const radii = { sm: 4, md: 8, lg: 16, pill: 999 };
```

### Font loading (Expo Router root layout)

```tsx
// app/_layout.tsx
import { useFonts } from 'expo-font';
import { Stack } from 'expo-router';
import { SafeAreaProvider } from 'react-native-safe-area-context';

export default function RootLayout() {
  const [loaded] = useFonts({
    Fraunces_700Bold: require('@expo-google-fonts/fraunces/Fraunces_700Bold.ttf'),
    Manrope_400Regular: require('@expo-google-fonts/manrope/Manrope_400Regular.ttf'),
    Manrope_500Medium: require('@expo-google-fonts/manrope/Manrope_500Medium.ttf'),
  });
  if (!loaded) return null;
  return (
    <SafeAreaProvider>
      <Stack screenOptions={{ headerShown: false }} />
    </SafeAreaProvider>
  );
}
```

### Reanimated staggered entrance

```tsx
import Animated, { FadeInDown } from 'react-native-reanimated';

<View>
  <Animated.Text
    entering={FadeInDown.duration(500)}
    style={{ fontFamily: type.display, fontSize: 56, color: palette.light.fg }}
  >
    Brand
  </Animated.Text>
  <Animated.Text
    entering={FadeInDown.delay(100).duration(500)}
    style={{ fontFamily: type.body, fontSize: 18, color: palette.light.muted }}
  >
    Tagline that earns its space.
  </Animated.Text>
  <Animated.View entering={FadeInDown.delay(200).duration(500)}>
    <PrimaryButton />
  </Animated.View>
</View>
```

### Pressable with press feedback

```tsx
import { Pressable } from 'react-native';
import Animated, { useAnimatedStyle, useSharedValue, withSpring } from 'react-native-reanimated';

const AnimatedPressable = Animated.createAnimatedComponent(Pressable);

function PrimaryButton({ children, onPress }) {
  const scale = useSharedValue(1);
  const animatedStyle = useAnimatedStyle(() => ({ transform: [{ scale: scale.value }] }));
  return (
    <AnimatedPressable
      onPressIn={() => (scale.value = withSpring(0.97))}
      onPressOut={() => (scale.value = withSpring(1))}
      onPress={onPress}
      style={[styles.button, animatedStyle]}
    >
      {children}
    </AnimatedPressable>
  );
}
```

## RN-specific anti-patterns

In addition to the universal anti-patterns in `../shared-rules.md`:

- `<View><Text>Hello</Text></View>` as the starting point (the Expo template)
- Inline styles everywhere (`style={{ marginTop: 16, color: '#000' }}`)
- Default `SafeAreaView` from `react-native` (deprecated — use `react-native-safe-area-context`)
- `TouchableOpacity` everywhere (prefer `Pressable`)
- Hardcoded hex colors in widgets
- `FlatList` of shadowy `View`s
- Legacy `Animated` API in new code
- Synchronous `require('image.png')` for hero images — use `expo-image` with proper URI
- `console.log` left in shipped code

## Output format

Per `../shared-rules.md`, plus include:

- **Required `package.json` deps** explicitly
- **Required `expo-google-fonts` packages** added
- **Theme tokens file** if greenfield
- **Platform-specific notes** (iOS-only / Android-only / both)

## Success criteria

Apply ALL universal litmus checks from `../shared-rules.md`, plus RN-specific:

- [ ] **Custom fonts via `expo-font`** — no system defaults
- [ ] **Theme tokens** defined and consumed — no inline hex
- [ ] **Both modes** (light + dark) polished
- [ ] **`SafeAreaView` from `react-native-safe-area-context`** on every screen root
- [ ] **Tap targets ≥44pt**
- [ ] **Reanimated v3** for any motion (not legacy `Animated`)
- [ ] **`expo-image`** for any meaningful image
- [ ] **`FlashList`** for any non-trivial list
- [ ] **No Expo-template smell**
- [ ] **Idiomatic** — chosen styling system used consistently

## Validation & reporting

Per `../shared-rules.md`, plus add to the report:

- Palette (light + dark)
- Typography (display + body fonts, why)
- Motion (which library, which interactions animated)
- Styling system used (NativeWind / styled / StyleSheet / Tamagui)
- Platform considerations (iOS-specific, Android-specific, shared)

## When NOT to trigger this skill

- Web frontends — use `frontend-next` or `frontend-react`.
- Flutter — use `frontend-flutter`.
- Bare React Native (no Expo) — most patterns transfer but adjust imports; flag the user.
- Native Swift / Kotlin — out of scope.
- Existing RN codebase with established design system — preserve it.

## Source

See [`../shared-rules.md`](../shared-rules.md). This file contains only the React Native / Expo-specific application of those rules.
