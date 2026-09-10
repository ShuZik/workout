# Tasks: Persist Semantic Colors

## Phase 1: Foundation

- [x] T001 Add semantic type normalization and a runtime legacy-color resolver in `/Users/shuzik/Developer/Fighting-AI/FNKTimer/Shared/Models/WorkoutBuildPreset.swift`.
- [x] T002 Write new exercise payloads without `colorHex` and migrate stored payloads at load in `/Users/shuzik/Developer/Fighting-AI/FNKTimer/Persistence/BuildItemRecord.swift` and `/Users/shuzik/Developer/Fighting-AI/FNKTimer/Bootstrap/AppDataStore.swift`.

## Phase 2: User Story 1 — Picker persistence

- [x] T003 [US1] Create custom exercises with the selected `colorType`, not `colorHex`, in `/Users/shuzik/Developer/Fighting-AI/FNKTimer/Workout/CustomExercises/CustomExerciseDraft.swift`.

## Phase 3: User Story 3 — Runtime compatibility

- [x] T004 [US3] Derive a timer color from `colorType` when no item hex exists in `/Users/shuzik/Developer/Fighting-AI/FNKTimer/Timer/Shared/Models/WorkoutSessionBuilder.swift`.

## Phase 4: Validation

- [x] T005 Validate migrated payload shape and the 15 picker types; mark completion in this file.
