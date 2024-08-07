# Managment of Theme - 070824

## Description

Currently, I am developing a mobile application of my client using React Native.
Luckily one of my team member was a senior full-stack developer who has experience on react native,
so I got a chance to get my code reviewed.

The first ticket that was assigned to me was - integrating a form screen to create a purchase order. While I was working on it, I was to raise a PR to get my code review, and this is one of the comments I got.

## Problem

```ts
import { StyleSheet, View, useColorScheme, ... } from 'react-native'
```

"Use this `import { useColorScheme } from '@/hooks/useColorScheme'` to import `useColorScheme`."

I was importing `useColorScheme` from `react-native` instead of `@/hooks/useColorScheme`. What is the difference between the two?

## `useColorScheme` from react-native

```ts
type ColorSchemeName = "light" | "dark" | null | undefined;
export function useColorScheme(): ColorSchemeName;
```

The built-in useColorScheme hook from React Native provides and subscribes to color scheme updates based on the user's system settings. The return value indicates the current color scheme preference. The value can be updated if the user changes their system theme or if there are scheduled theme changes (e.g., light/dark themes following the day/night cycle).

## Custom hook `useColorScheme` by Elvin

```ts
export enum ColorSchemeType {
  LIGHT = "light",
  DARK = "dark",
}
export type ColorScheme = ColorSchemeType.LIGHT | ColorSchemeType.DARK;
```

Elvin’s custom useColorScheme hook provides similar functionality but uses a more explicit and type-safe approach by defining an enum and type alias for color schemes.

1. `ColorSchemeType` Enum:

   - `ColorSchemeType` is an enum that explicitly defines the color scheme options as string constants:
     - `LIGHT`: Represents light mode.
     - `DARK`: Represents dark mode.
       This enum helps in managing color scheme values more safely and clearly in the code.

2. `ColorScheme` Type:
   - The `ColorScheme` type is a union type that allows only `ColorSchemeType.LIGHT` or `ColorSchemeType.DARK` values. This enforces that the color scheme values are one of the defined `enum` members, improving type safety and code clarity.

## It was then used like this

```tsx
export default function NewPurchaseOrder() {
  const theme = useColorScheme(); // assigned to variable theme
  return (
    <TextInput
      style={styles.textInput}
      outlineColor={
        theme === ColorSchemeType.LIGHT // was theme === 'light'? before
          ? CustomMD3Theme.light.colors.outlineVariant
          : CustomMD3Theme.dark.colors.outlineVariant
      }
    />
  );
}
```
By using Elvin’s useColorScheme, you benefit from type safety and clarity by leveraging the enum to define color schemes. This approach reduces the risk of errors and makes the code more maintainable compared to using plain strings.
