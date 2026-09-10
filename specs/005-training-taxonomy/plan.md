# Implementation Plan: Training Taxonomy Reset

**Catalog repository**: `/Users/shuzik/Developer/workout`
**Target release**: Current catalog schema, compatible with app 1.1.3

## Technical Context

- The visible tag order is `manifest.json.tags` order.
- A saved exercise resolves by its stable `key`; a moved record therefore
  retains its `key`, while `id`, `workoutType`, colors, section and manifest
  path are updated for its destination tag.
- The 1.1.3 client reads legacy `color` and the current client reads
  `colorType`; keep the legacy colors unchanged and set semantic green only
  through `colorType` for the renamed Rest tag.
- The current app recognises `workoutType: "cooldown"`; renaming the visible
  Cool-down tag to Rest must not change that wire value.

## Design

1. Reorder `manifest.json.tags` to the approved 21-tag sequence. Rename
   `Core Training` to `Core` and `Cool-down` to `Rest`, retaining their stable
   folder names, ids and wire workout types.
2. Reorganise all current Cooldown records into Active Rest, Recovery
   Breathing, Static Stretching and Release Work. Preserve
   each record's key, path and energy profile; change only its title where the
   tag-named Main record becomes Rest, colors and section metadata.
3. Remove the `FunctionalTraining` and `Calisthenics` tag records. Move each
   non-duplicate exercise to CoreTraining or Strength with its stable key and
   a next-free destination id; retain the original icon file with the moved
   record. Retire exact duplicate records where an equivalent canonical Core
   or Strength record already exists.
4. Update `manifest.json.files`, `icon-registry.json`, JSON metadata and
   folders together, so every referenced record and icon resolves exactly
   once.

## Reclassification Decisions

### Move to Core

- Functional: `functional_dumbbell_wood_chop`,
  `balance_feet_together_stand`, `balance_single_leg_stand`.
- Calisthenics: `calisthenics_l_sit`, `calisthenics_hanging_leg_raise`,
  `calisthenics_superman_hold`.

### Move to Strength

- Functional: `functional_hip_hinge`, `functional_bodyweight_squat`,
  `functional_step_up`, `functional_reverse_lunge`,
  `functional_lateral_lunge`, `functional_push_up`,
  `functional_dumbbell_step_up`, `functional_dumbbell_split_squat`,
  `functional_training_wall_sit`.
- Calisthenics: `calisthenics_knee_push_up`, `calisthenics_split_squat`,
  `calisthenics_walking_lunge`, `calisthenics_calf_raise`,
  `calisthenics_pull_up`, `calisthenics_chin_up`, `calisthenics_dip`,
  `calisthenics_burpee`, `calisthenics_wall_handstand`,
  `calisthenics_inverted_row`, `calisthenics_pike_push_up`.

### Retire as duplicate or vague

- Functional: `functional_training`, `functional_bird_dog`,
  `functional_dead_bug`, `functional_glute_bridge`,
  `functional_goblet_squat`, `functional_dumbbell_romanian_deadlift`,
  `functional_training_plank`.
- Calisthenics: `calisthenics`, `calisthenics_push_up`,
  `calisthenics_bodyweight_squat`, `calisthenics_glute_bridge`,
  `calisthenics_plank`, `calisthenics_side_plank`,
  `calisthenics_hollow_hold`, `calisthenics_mountain_climber`.

## Boundaries

- Do not add or alter exercise descriptions, MET values, defaults, icons or
  timer behaviour beyond the fields required by a move or Rest rename.
- Do not alter app source code, tests or legacy `color` values.
- Yoga and Pilates are already absent and are not reintroduced.
