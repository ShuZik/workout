# Tasks: Training Taxonomy Reset

## Phase 1: Foundation

- [x] T001 Record the approved taxonomy design and reclassification map in `/Users/shuzik/Developer/workout/specs/005-training-taxonomy/plan.md` and `/Users/shuzik/Developer/workout/specs/005-training-taxonomy/data-model.md`.

## Phase 2: User Story 1 — Predictable catalog order

**Goal**: Display the approved 21 tags in a combat-first sequence.

**Independent Test**: Reading `manifest.json.tags` yields the exact sequence
from FR-001.

- [x] T002 [US1] Reorder the visible tag records; rename Core Training to Core and Cool-down to Rest in `/Users/shuzik/Developer/workout/manifest.json`.
- [x] T003 [US1] Rename the tag-named Rest exercise, apply the green semantic Rest color, and organise all recovery sections in `/Users/shuzik/Developer/workout/Cooldown/`.

## Phase 3: User Story 2 — Distinct recovery flow

**Goal**: Separate active recovery, breathing, stretching and release work.

**Independent Test**: Every Cooldown record has exactly one approved Rest section.

- [x] T004 [US2] Assign each Cooldown exercise to Active Rest, Recovery Breathing, Static Stretching or Release Work in `/Users/shuzik/Developer/workout/Cooldown/`.
- [x] T005 [US2] Preserve Other → Rest as the separate green timer block in `/Users/shuzik/Developer/workout/Other/rest/rest.json`.

## Phase 4: User Story 3 — Retire vague duplicate tags

**Goal**: Move purposeful records into Core or Strength and remove duplicate source records.

**Independent Test**: No manifest entry or root tag remains for Functional Training or Calisthenics; every retained source key has its planned destination.

- [x] T006 [US3] Move the planned Functional Training records into `/Users/shuzik/Developer/workout/CoreTraining/` and `/Users/shuzik/Developer/workout/Strength/`, retaining keys and icons.
- [x] T007 [US3] Move the planned Calisthenics records into `/Users/shuzik/Developer/workout/CoreTraining/` and `/Users/shuzik/Developer/workout/Strength/`, retaining keys and icons.
- [x] T008 [US3] Remove duplicate/vague Functional Training and Calisthenics records and their source tag roots from `/Users/shuzik/Developer/workout/`.
- [x] T009 [US3] Reconcile exercise ids, destination metadata, manifest file pairs and icon registry entries in `/Users/shuzik/Developer/workout/manifest.json` and `/Users/shuzik/Developer/workout/icon-registry.json`.

## Phase 5: Validation

- [x] T010 Validate catalog structure, identities, manifest pairs and no-whitespace diff; mark completion in `/Users/shuzik/Developer/workout/specs/005-training-taxonomy/tasks.md`.

## Dependencies

- T001 → T002–T005 → T006–T009 → T010.
- T003 and T004 use the same JSON records and run sequentially.
- T006 and T007 use shared destination tags and run sequentially.

## Implementation Strategy

1. Apply the visible taxonomy and recovery grouping.
2. Consolidate Functional Training and Calisthenics into the two explicit
   destination tags.
3. Validate manifest, paths, identities and metadata only after the final
   catalog state is complete.
