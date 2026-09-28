# Research: Clean Expo Application Baseline

## Decision: Retain Expo Router file-based routes and the existing platform tab split

- **Rationale**: The project already uses `expo-router/entry`, the Expo Router config plugin, `src/app/_layout.tsx`, native `NativeTabs`, and web `expo-router/ui` tabs. Expo SDK 57 documents Expo Router as a file-based router for React Native and web, with the Router config plugin in app configuration.
- **Alternatives considered**: Replacing navigation with a new navigation library or moving to a different route structure.
- **Decision**: Reuse the existing Router structure and update the existing native and web tab implementations together.

## Decision: Make Home the first root route and tab

- **Rationale**: `src/app/index.tsx` maps to the root destination, and the existing native and web tab controls already present Home first. Retaining that order keeps Home the initial tab without a new routing layer.
- **Alternatives considered**: Add redirects or a separate startup route.
- **Decision**: Keep Home at `/` and list it first in both tab controls.

## Decision: Replace the starter Explore screen with neutral placeholders

- **Rationale**: The existing Explore route is tutorial content, while the specification requires future-feature placeholders. Replacing it avoids retaining a starter route and keeps the change small.
- **Alternatives considered**: Delete Explore without replacement, or add a nested tab layout.
- **Decision**: Provide two simple root placeholder routes alongside Home; do not add a nested navigator or domain feature.

## Decision: Remove the in-app Expo-logo splash animation, retain configured splash assets

- **Rationale**: The in-app animation depends on starter logo assets and animation libraries. The platform splash screen remains configured in `app.json` and must be preserved.
- **Alternatives considered**: Redesign the animation or remove all splash configuration.
- **Decision**: Remove the in-app overlay and only its auto-hide control; keep the configured splash plugin and asset unchanged.

## Decision: Use reference-driven cleanup and dependency removal

- **Rationale**: Several assets are referenced by `app.json`, and some packages can be runtime requirements even when no direct source import remains.
- **Alternatives considered**: Bulk removal based only on starter-template membership.
- **Decision**: Confirm source/configuration references, update the lockfile through the package manager, and validate every removal via lint, type checks, and launch tests.

## Sources

- Expo SDK 57 Expo Router reference: https://docs.expo.dev/versions/v57.0.0/sdk/router/
