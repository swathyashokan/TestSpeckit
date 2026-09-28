# Validation Quickstart: Clean Expo Application Baseline

## Prerequisites

- Node.js and project dependencies installed.
- An Android emulator, iOS simulator, or physical device available for native smoke testing.
- A browser available for web validation.

## Static Validation

1. Install dependencies after any package change using the project package manager.
2. Run `npm run lint`.
3. Run the project TypeScript no-emit validation.
4. Search for removed route names, deleted assets, and removed package imports to confirm no references remain.

## Native Validation

1. Run `npm run android` or `npm run ios`.
2. Confirm that launch opens Home.
3. Confirm that Home shows a heading and description.
4. Confirm that the bottom bar lists Home, Features, and Settings, in that order.
5. Select Features and Settings, then return to Home; confirm every selected tab opens the expected screen.
6. Confirm the configured platform splash and app icon assets still resolve without an error.

## Web Validation

1. Run `npm run web`.
2. Confirm Home is the initial route.
3. Confirm Home, Features, and Settings use the same labels and order as native.
4. Open each destination and confirm it renders without a missing-route error.

## Expected Result

The application launches successfully without Expo starter UI, all three navigation destinations work on supported platforms, and lint/type/build validation completes without errors.
