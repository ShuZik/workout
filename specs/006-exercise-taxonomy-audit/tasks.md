# Tasks: Exercise Taxonomy Audit

## Phase 1: Audit Record

- [x] T001 Record the 21-tag audit verdict and approved corrections in `/Users/shuzik/Developer/workout/specs/006-exercise-taxonomy-audit/audit.md`.

## Phase 2: Remove Generic Strength Noise

- [x] T002 [US1] Remove the `functional_strength_training` record, icon pair, manifest entry and icon registry entry from `/Users/shuzik/Developer/workout/Strength/`, `/Users/shuzik/Developer/workout/manifest.json` and `/Users/shuzik/Developer/workout/icon-registry.json`.

## Phase 3: Correct Record Placement and Naming

- [x] T003 [US2] Keep Rest's `breathing` record titled Breathing in `/Users/shuzik/Developer/workout/Cooldown/breathing/breathing.json`.
- [x] T004 [US3] Move `tabata_plank_jacks` to Conditioning in `/Users/shuzik/Developer/workout/Tabata/plankJacks/plankJacks.json`.
- [x] T005 [US3] Move `cross_training_box_jump` to Bodyweight in `/Users/shuzik/Developer/workout/CrossTraining/boxJump/boxJump.json`.

## Phase 4: Validation

- [x] T006 Validate catalog pairs, registry coverage, count and no-whitespace diff; mark completion in `/Users/shuzik/Developer/workout/specs/006-exercise-taxonomy-audit/tasks.md`.

## Dependencies

- T001 → T002–T005 → T006.
