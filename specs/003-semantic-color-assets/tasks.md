# Tasks: Semantic Catalog Color Assets

## Dependencies

Foundation work must finish before catalog data uses `colorType`. The app and
catalog updates then form one compatibility-safe release unit.

## Phase 1: Foundation

- [x] T001 Replace the prefixed palette asset names with approved bare color names and add their light/dark values in `/Users/shuzik/Developer/Fighting-AI/FNKTimer/Resources/Assets.xcassets/Colors/IconColor/`.
- [x] T002 Update the application design-system palette to expose the approved bare color names in `/Users/shuzik/Developer/Fighting-AI/FNKTimer/Shared/DesignSystem/Primitives/AppColor.swift`.
- [x] T003 Add optional catalog `colorType` parsing and the legacy-color fallback in `/Users/shuzik/Developer/Fighting-AI/FNKTimer/Shared/Services/WorkoutCatalogSync.swift`.
- [x] T004 Connect catalog tag and exercise rendering to the semantic palette with fallback in `/Users/shuzik/Developer/Fighting-AI/FNKTimer/Shared/Models/WorkoutCatalogTag+Style.swift` and `/Users/shuzik/Developer/Fighting-AI/FNKTimer/Shared/Models/WorkoutBuildItem+Style.swift`.

## Phase 2: User Story 1 — Consistent appearance

- [x] T005 [US1] Add a `colorType` matching each tag's existing color family in `/Users/shuzik/Developer/workout/manifest.json`.
- [x] T006 [US1] Add a matching `colorType` to every current exercise JSON in `/Users/shuzik/Developer/workout/` without altering another field.

## Phase 3: User Story 2 — Compatibility

- [x] T007 [US2] Validate valid semantic types, catalog fallback records, and retained legacy colors across `/Users/shuzik/Developer/workout/manifest.json` and every exercise JSON.

## Phase 4: Polish

- [x] T008 Validate the implementation against `/Users/shuzik/Developer/workout/specs/003-semantic-color-assets/spec.md` and record completion in this file.
