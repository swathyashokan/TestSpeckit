---

description: "Dependency-ordered implementation tasks for SCRUM-1"
---

# Tasks: Clean Expo Application Baseline

**Input**: [spec.md](./spec.md), [plan.md](./plan.md), [research.md](./research.md), [data-model.md](./data-model.md), [navigation contract](./contracts/navigation.md), and [quickstart.md](./quickstart.md)

**Prerequisites**: Existing Expo SDK 57 dependencies installed. This task list changes no Jira data and preserves the existing Expo Router architecture.

**Tests**: No automated test framework is configured or requested. Validation tasks use lint, TypeScript, Expo configuration, and native/web launch smoke tests.

**Organization**: Tasks are grouped by user story. Each task declares its file action: create, modify, delete, or verify only.

## Phase 1: Setup and Protection Inventory

**Purpose**: Establish the safe cleanup boundary before changing routes, assets, or packages.

- [ ] T001 Verify-only: inventory imports, asset `require` calls, route triggers, and `app.json` paths in `src/**`, `assets/**`, `app.json`, and `package.json`; record every protected configuration-linked asset before cleanup.
- [ ] T002 Verify-only: confirm that `package.json`, `app.json`, and `tsconfig.json` retain `expo-router/entry`, the `expo-router` plugin, static web output, strict TypeScript, and the `@/` aliases; do not change these required settings.

---

## Phase 2: Foundational Navigation Cleanup

**Purpose**: Remove the in-app starter splash overlay while retaining the platform splash configuration. This must finish before deleting its components/assets or validating the final Home route.

- [ ] T003 Modify `src/app/_layout.tsx` to keep the theme provider and tab host while removing the Expo-logo overlay import, rendering, and manual splash auto-hide control; retain the configured `expo-splash-screen` plugin and `assets/images/splash-icon.png` reference in `app.json` unchanged.

**Checkpoint**: The root layout still hosts routing and theming without importing starter animation code.

---

## Phase 3: User Story 1 - Open the Home Screen (Priority: P1) 🎯 MVP

**Goal**: Deliver a launchable Home route with an intentional heading and description instead of starter instructions.

**Independent Test**: Launch the app and verify `/` renders a heading and description without Expo starter text, development hints, badge, or animated logo content.

- [ ] T004 [US1] Modify `src/app/index.tsx` to render only the minimal Home heading and description using the retained project TypeScript/theme primitives; remove starter instruction, device-menu, Expo-logo, and badge usage.
- [ ] T005 [US1] Verify-only: inspect `src/app/index.tsx`, `src/app/_layout.tsx`, and `src/components/app-tabs.tsx` to confirm `/` remains the first/default Home destination and no Home import refers to a starter-only component.

**Checkpoint**: Home independently satisfies the launch-screen requirement.

---

## Phase 4: User Story 2 - Navigate Placeholder Destinations (Priority: P2)

**Goal**: Supply Home, Features, and Settings as consistent native and web bottom-navigation destinations.

**Independent Test**: Starting from `/`, open Features and Settings from bottom navigation on native and web, then return to Home; every destination and label matches the navigation contract.

- [ ] T006 [P] [US2] Create `src/app/features.tsx` as a simple Features placeholder route with no domain behavior, persistence, or external integration.
- [ ] T007 [P] [US2] Create `src/app/settings.tsx` as a simple Settings placeholder route with no domain behavior, persistence, or external integration.
- [ ] T008 [US2] Modify `src/components/app-tabs.tsx` to define native triggers for `index`, `features`, and `settings` in Home/Features/Settings order; remove starter tab-image references once no longer required.
- [ ] T009 [US2] Modify `src/components/app-tabs.web.tsx` to define web triggers for `/`, `/features`, and `/settings` in Home/Features/Settings order; remove Expo Starter branding, documentation-link UI, and starter-only imports.
- [ ] T010 [US2] Delete `src/app/explore.tsx` after T008 and T009 no longer name or link to the `explore` route.
- [ ] T011 [US2] Verify-only: compare `src/components/app-tabs.tsx`, `src/components/app-tabs.web.tsx`, `src/app/index.tsx`, `src/app/features.tsx`, and `src/app/settings.tsx` against `specs/001-clean-expo-baseline/contracts/navigation.md`; confirm the exact route/label/order contract and a file exists for each route.

**Checkpoint**: All three routes are independently reachable and the two tab implementations agree.

---

## Phase 5: User Story 3 - Use a Focused Application Baseline (Priority: P3)

**Goal**: Remove only starter material proven unused after the finalized routes and navigation are in place, while preserving configured platform assets and runtime needs.

**Independent Test**: Search all active source/configuration after cleanup; starter-only content is absent, every remaining asset reference resolves, and required config remains unchanged.

- [ ] T012 [US3] Delete `src/components/animated-icon.tsx`, `src/components/animated-icon.web.tsx`, and `src/components/animated-icon.module.css` after confirming T003 and T004 removed all imports and usages.
- [ ] T013 [US3] Delete `src/components/hint-row.tsx` after confirming `src/app/index.tsx` no longer imports it.
- [ ] T014 [US3] Delete `src/components/web-badge.tsx` after confirming `src/app/index.tsx` and the deleted `src/app/explore.tsx` no longer import it.
- [ ] T015 [US3] Delete `src/components/external-link.tsx` after confirming `src/components/app-tabs.web.tsx` and the deleted `src/app/explore.tsx` no longer import it.
- [ ] T016 [US3] Delete `src/components/ui/collapsible.tsx` after confirming the deleted `src/app/explore.tsx` was its final use.
- [ ] T017 [US3] Delete `scripts/reset-project.js` and modify `package.json` to remove the matching `reset-project` script; do not execute the reset script.
- [ ] T018 [US3] Delete unreferenced starter assets: `assets/images/expo-logo.png`, `assets/images/logo-glow.png`, `assets/images/expo-badge.png`, `assets/images/expo-badge-white.png`, `assets/images/react-logo.png`, `assets/images/react-logo@2x.png`, `assets/images/react-logo@3x.png`, `assets/images/tutorial-web.png`, and `assets/images/tabIcons/**` only after a repository-wide source/configuration reference check.
- [ ] T019 [US3] Verify-only: retain `assets/images/icon.png`, `assets/images/splash-icon.png`, `assets/images/favicon.png`, `assets/images/android-icon-background.png`, `assets/images/android-icon-foreground.png`, `assets/images/android-icon-monochrome.png`, and `assets/expo.icon/**` because `app.json` references them; do not delete or alter those assets in this feature.
- [ ] T020 [US3] Modify `README.md` to remove stale Expo starter/reset guidance and document only the retained start commands; do not describe unimplemented future features.
- [ ] T021 [US3] Verify-only: audit direct dependency candidates in `package.json` against remaining source/configuration and package requirements: `expo-device`, `expo-image`, `expo-symbols`, `expo-web-browser`, `react-native-reanimated`, `react-native-worklets`, `@expo/ui`, `expo-constants`, `expo-font`, `expo-glass-effect`, `expo-status-bar`, and `expo-system-ui`; record only confirmed removals.
- [ ] T022 [US3] Modify `package.json` and `package-lock.json` to remove only the dependency set confirmed by T021, while retaining Expo, Expo Router, splash screen, React, React Native, gesture handler, safe-area context, screens, linking, and configured web runtime packages.

**Checkpoint**: Starter-only code/assets/dependencies are removed only where reference and runtime checks make them safe.

---

## Phase 6: Polish and Cross-Cutting Validation

**Purpose**: Validate the completed project, configuration, and behavior across supported platforms.

- [ ] T023 Verify-only: run `npm run lint` and the TypeScript no-emit check using `package.json` and `tsconfig.json`; report any failure without modifying source in this validation task.
- [ ] T024 Verify-only: validate `app.json` against the retained configured assets (`assets/images/icon.png`, `assets/images/splash-icon.png`, `assets/images/favicon.png`, `assets/images/android-icon-*.png`, and `assets/expo.icon/**`) and run the available Expo SDK 57 project health/build check.
- [ ] T025 Verify-only: run `npm run android` or `npm run ios` from `package.json`; verify native launch opens Home, shows its heading/description, and retains valid configured splash and app-icon behavior.
- [ ] T026 Verify-only: run `npm run web` from `package.json`; verify static-web startup opens Home and has no missing-route or missing-asset errors.
- [ ] T027 Verify-only: execute the route checks in `specs/001-clean-expo-baseline/quickstart.md` using `src/app/index.tsx`, `src/app/features.tsx`, `src/app/settings.tsx`, `src/components/app-tabs.tsx`, and `src/components/app-tabs.web.tsx`; confirm Home/Features/Settings navigation and selected-tab behavior on native and web.

---

## Dependencies & Execution Order

### Phase Dependencies

- Phase 1 has no dependencies and defines the cleanup safety boundary.
- Phase 2 depends on T001–T002 and blocks starter animation deletion in Phase 5.
- User Story 1 depends on T003 because Home must not retain the removed overlay/starter imports.
- User Story 2 depends on T001–T002; T006 and T007 can run in parallel, then T008–T011 run in order.
- User Story 3 depends on T003–T011 because cleanup must follow replacement of all routes/navigation and removal of their imports.
- Validation depends on all implementation and cleanup tasks.

### Task-Level Dependencies

```text
T001, T002 → T003
T003 → T004 → T005
T006, T007 → T008, T009 → T010 → T011
T003, T004 → T012, T013
T004, T010 → T014
T009, T010 → T015
T010 → T016
T001 → T017, T019, T020
T008, T009 → T018
T012–T018 → T021
T017, T021 → T022
T012–T022 → T023–T027
```

### Parallel Opportunities

- T001 and T002 can run in parallel.
- T006 and T007 can run in parallel.
- Once their final import checks pass, T013–T016 can run in parallel because they delete separate components.
- T018 and T020 can run in parallel after route/component cleanup, since they affect separate asset and documentation paths.
- T025 and T026 can run in parallel when native and web environments are available.

## Implementation Strategy

### MVP First

1. Complete T001–T005 to deliver the clean Home screen.
2. Validate that Home launches before adding placeholders or deleting starter artifacts.

### Incremental Delivery

1. Complete T006–T011 to add and validate consistent placeholder navigation.
2. Complete T012–T022 using reference-driven cleanup and conservative dependency removal.
3. Complete T023–T027 as the final quality gate.

## Format Validation

All 27 tasks use the required checklist format: checkbox, sequential task ID, optional parallel marker, required user-story label in user-story phases, action type, and exact affected file path(s).
