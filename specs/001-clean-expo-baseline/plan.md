# Implementation Plan: Clean Expo Application Baseline

**Branch**: `main` | **Date**: 2026-09-20 | **Spec**: [spec.md](./spec.md)

## Summary

Replace the current Expo starter experience with a minimal, extendable application shell: a Home route with a heading and description, plus consistent native and web bottom tabs for Home and future-feature placeholders. Retain the current Expo Router, TypeScript, platform configuration, and configuration-linked assets. Remove starter material only after reference checks prove it is no longer used.

## Technical Context

**Language/Version**: TypeScript 6.0.3; React 19.2.3; React Native 0.86.3; Expo SDK 57.0.24.

**Primary Dependencies**: `expo-router` 57.0.22, `expo-splash-screen`, `react-native-safe-area-context`, `react-native-gesture-handler`, and `react-native-screens`; native `NativeTabs` and web `expo-router/ui` tabs are already in use.

**Storage**: N/A. This feature introduces no persisted data or backend integration.

**Testing**: Existing `npm run lint`; TypeScript compiler validation; Expo native and web launch smoke tests. No automated test framework is currently configured.

**Target Platform**: iOS, Android, and static web output, as configured by `app.json` and existing scripts.

**Project Type**: Existing Expo React Native mobile application with web support.

**Performance Goals**: Launch to a usable Home screen without startup errors; no performance-specific change is required.

**Constraints**: Preserve `expo-router/entry`, the Expo Router config plugin, `src/app` file-based routes, TypeScript aliases, existing platform assets, and static web support. Do not introduce libraries or redesign navigation.

**Scale/Scope**: Three lightweight routes: Home plus two non-functional future-feature placeholders; no domain data, API, or authentication work.

## Constitution Check

**Pre-design gate: PASS.** `.specify/memory/constitution.md` remains the uncustomized template and does not contain enforceable project-specific principles. The feature constraints provide the governing limits: retain the current architecture, avoid new libraries, preserve configured assets, and validate launch/build behavior.

**Post-design gate: PASS.** The design retains one existing application structure, adds no service, data, or API layer, and has no constitution violations to justify.

## Architecture Approach

The project already uses Expo Router file-based routes from `src/app`, with `package.json` pointing to `expo-router/entry` and `app.json` enabling the Expo Router plugin. Preserve this foundation.

- Keep `src/app/_layout.tsx` as the root layout and theme boundary.
- Keep the platform split for the tab UI: `src/components/app-tabs.tsx` for native `NativeTabs` and `src/components/app-tabs.web.tsx` for web `expo-router/ui` tabs.
- Replace the starter Home implementation at `src/app/index.tsx` with the simple Home content.
- Replace the starter Explore route with a neutral placeholder route and add a second neutral placeholder route. Both tab implementations must use the same route names, labels, and ordering, with Home first.
- Use only built-in React Native primitives and the existing theme helpers for the minimal screen UI. No additional navigation, UI, state, or styling library is required.
- Remove the Expo-branded animated splash overlay rather than replace it with another in-app splash sequence. The configured `expo-splash-screen` asset and plugin remain intact.

## Files to Create

- `src/app/features.tsx` — simple future-feature placeholder route.
- `src/app/settings.tsx` — simple future-feature placeholder route.

## Files to Modify

- `src/app/_layout.tsx` — retain theme and tab rendering while removing the starter animated overlay and its startup control only if no longer needed.
- `src/app/index.tsx` — replace starter instructions, Expo animation, and badge with Home heading and description.
- `src/components/app-tabs.tsx` — align native triggers with the final Home and placeholder routes; remove starter image icons if no longer needed.
- `src/components/app-tabs.web.tsx` — align web triggers and labels with the native tab set; remove Expo Starter branding and documentation link.
- `package.json` and `package-lock.json` — remove the reset script and only dependencies proven unused after source and configuration cleanup.
- `README.md` — replace or remove starter-only instructions if documentation cleanup is included in the implementation tasks.

## Files to Delete After Reference Verification

- `src/app/explore.tsx`
- `src/components/animated-icon.tsx`
- `src/components/animated-icon.web.tsx`
- `src/components/animated-icon.module.css`
- `src/components/hint-row.tsx`
- `src/components/web-badge.tsx`
- `src/components/external-link.tsx`
- `src/components/ui/collapsible.tsx`
- `scripts/reset-project.js`
- `assets/images/expo-logo.png`
- `assets/images/logo-glow.png`
- `assets/images/expo-badge.png`
- `assets/images/expo-badge-white.png`
- `assets/images/react-logo.png`, `react-logo@2x.png`, `react-logo@3x.png`
- `assets/images/tutorial-web.png`
- `assets/images/tabIcons/` after native tabs no longer reference starter tab images.
- `assets/expo.icon/Assets/expo-symbol 2.svg` and `assets/expo.icon/Assets/grid.png` only if they are not required by `assets/expo.icon/icon.json` or the configured iOS icon set.

## Dependency Changes

Remove a dependency only after `rg` confirms no application/configuration import or reference remains, then update the lockfile with the project package manager and validate the app.

- Expected direct removals after starter cleanup: `expo-device`, `expo-image`, `expo-symbols`, `expo-web-browser`, `react-native-reanimated`, and `react-native-worklets`.
- Candidates requiring an explicit final verification before removal: `@expo/ui`, `expo-constants`, `expo-font`, `expo-glass-effect`, `expo-status-bar`, and `expo-system-ui`.
- Preserve the Expo/Router/runtime packages required by the remaining app: `expo`, `expo-router`, `expo-splash-screen`, `react`, `react-native`, `react-native-gesture-handler`, `react-native-safe-area-context`, `react-native-screens`, `expo-linking`, and web packages while static web output remains configured.

## Configuration Considerations

- Preserve `package.json` `main: "expo-router/entry"`.
- Preserve the `expo-router` plugin, `web.output: "static"`, typed routes, React Compiler experiment, app scheme, orientation, and interface-style settings in `app.json`.
- Preserve all assets referenced by `app.json`: app icon, iOS icon directory, Android adaptive icon layers, web favicon, and splash image. They may be replaced only by updating the reference and adding a valid replacement in the same change.
- Preserve `tsconfig.json` strict mode, Expo base config, and `@/` / `@/assets` aliases because active source imports use them.
- Do not run `scripts/reset-project.js`; it can move or delete source directories and is outside the controlled cleanup path.

## Validation and Testing Strategy

1. Before deleting each candidate, search source and configuration for imports, asset requires, route triggers, and config paths.
2. Run the configured lint command and a TypeScript no-emit check; resolve all errors.
3. Launch the app with the existing native workflow and verify: Home opens first; the heading and description render; every tab opens the intended route; Home can be reselected.
4. Launch web with `npm run web` and verify the same labels, route destinations, and selected-tab behavior.
5. Run the appropriate Expo project health/build validation available in the installed SDK, then confirm that app configuration still resolves every referenced icon and splash asset.
6. Re-search for Expo starter labels, tutorial text, Expo-logo assets, and removed dependency imports to ensure no active references remain.

## Risks and Mitigations

| Risk | Mitigation |
|---|---|
| A removed tab route leaves a broken trigger on native or web. | Update both tab implementations as one change and smoke-test every tab on both platforms. |
| A starter asset is also used by app configuration. | Treat `app.json` references as protected; delete only after configuration is deliberately updated or the asset is verified unreferenced. |
| Removing a direct dependency breaks Router/runtime behavior indirectly. | Separate confirmed application imports from package requirements; remove one verified set at a time and rerun lint, type checks, and launch tests. |
| The native and web tabs drift in names or order. | Define and validate a single route/label set in `contracts/navigation.md`; apply it to both tab files. |
| Removing the in-app splash overlay leaves the native splash screen open. | Remove `preventAutoHideAsync` only together with the overlay and verify startup on native targets. |
| Existing uncommitted files are overwritten. | Limit implementation edits to listed files and preserve unrelated worktree changes. |

## Project Structure

### Documentation (this feature)

```text
specs/001-clean-expo-baseline/
├── spec.md
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
└── contracts/
    └── navigation.md
```

### Source Code (repository root)

```text
src/
├── app/
│   ├── _layout.tsx          # Root Router layout and tab host
│   ├── index.tsx            # Home route
│   ├── features.tsx         # Future-feature placeholder
│   └── settings.tsx         # Future-feature placeholder
├── components/
│   ├── app-tabs.tsx         # Native tab UI
│   ├── app-tabs.web.tsx     # Web tab UI
│   ├── themed-text.tsx      # Retained theme helper
│   └── themed-view.tsx      # Retained theme helper
├── constants/theme.ts
├── hooks/
│   ├── use-color-scheme.ts
│   ├── use-color-scheme.web.ts
│   └── use-theme.ts
└── global.css

assets/
├── expo.icon/               # iOS icon configuration asset
└── images/                  # only app.json-referenced platform assets
```

**Structure Decision**: Keep the existing single Expo application and its `src/app` file-based route structure. This is the smallest change that satisfies SCRUM-1 and retains current native/web navigation behavior.

## Complexity Tracking

No complexity exceptions are needed.
