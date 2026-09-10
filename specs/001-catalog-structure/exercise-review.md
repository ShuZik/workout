# Exercise catalog review

## Scope and evidence

- Current catalog: 23 tags, 535 exercises.
- Every exercise JSON parses; every description is a numbered instruction list;
  no tag contains an exact duplicate title.
- Existing same-named exercises in different tags are intentional contextual
  duplicates, not candidates for mechanical deletion: a Jab in Boxing and a
  Jab in Muay Thai are selected from different training contexts.
- The category set already covers the useful timer-oriented classes used by
  mainstream workout platforms: cardio, strength, HIIT, cross-training,
  meditation, breathwork, recovery, and combat.

Sources used for category coverage:

- [Apple Watch workout types](https://support.apple.com/en-euro/105089)
- [Apple Fitness+ workout types](https://support.apple.com/en-ca/108761)
- [Google Fit activity types](https://developers.google.com/fit/rest/v1/reference/activity-types)
- [Samsung Health predefined exercise types](https://developer.samsung.com/health/android/data/api-reference/EXERCISE_TYPE.html)
- [Garmin activity profiles](https://support.garmin.com/en-US/?faq=1aLq0PzSib0C2TR9YAlLF9&productID=125677&tab=topics&topicTag=region_garminconnect6vi)

## Product decisions

1. Do not add a top-level category now. The existing 26 tags are already more
   useful than a broad competitor-derived list because each one has a timer
   purpose. A separate Mobility tag would duplicate the current Warm-up and
   Cool-down content without a clear new user job.
2. Keep `Work` in `Other / Structure`. It is an active interval in a workout
   plan, not a workout type or physical exercise.
3. `Work` is energy-neutral in the app. The catalog retains a valid MET profile
   only as a wire-format compatibility requirement: 1.1.2 and 1.1.3 validate
   every time record before applying `availableFrom`. The current app excludes
   all `Other` structure blocks from calorie estimation.
4. Custom must remain empty. `Custom/deleteIt` is a catalog control in a user
   exercise category and violates this rule. Deletion is application UI, not
   catalog content.

## Mandatory catalog repairs

| Priority | Change | Reason |
| --- | --- | --- |
| P0 | Remove `Custom/deleteIt` and its manifest/registry entries. | Custom must be empty. |
| P0 | Change all 42 `countAndWeight` records from `availableFrom: 1.1.5` to `1.1.4`. | The agreed availability boundary is 1.1.4. |
| P0 | Give every section within a tag a unique numeric prefix. | Equal prefixes are sorted alphabetically by the app, not in manifest order. |
| P1 | Remove Cool-down `Light Shadowboxing`. | It duplicates Warm-up `Light Boxing Shadowboxing` and belongs to warm-up. |
| P1 | Rename Judo `7 Osaekomi-waza` to a broader groundwork grouping. | It contains a hold, choke, and arm lock, not only osaekomi. |
| P1 | Keep all existing stable ids and keys when reorganizing. | Saved workouts resolve by these identifiers. |

Section prefixes that are currently ambiguous:

| Tag | Current conflict | Proposed order |
| --- | --- | --- |
| Boxing | `2 Base`, `2 Beginner` | `2 Base`, `3 Beginner`, `4 Intermediate` |
| Warm-up | `3 Cardio`, `3 Dynamic Movement` | `2 Joint Mobility`, `3 Dynamic Movement`, `4 Cardio`, then sport prep sections `5` through `8` |
| Cool-down | `1 Main`, `1 Recovery` | `1 Main`, `2 Recovery`, `3 Walking`, `4 Foam Rolling` |
| Tabata | `5 Conditioning`, `5 Core` | `2 Full Body`, `3 Lower Body`, `4 Upper Body`, `5 Core`, `6 Conditioning` |
| Meditation | skips from `3 Wellbeing` to `6 Mindfulness` | use contiguous `1` through `4` |

## Per-tag disposition

| Tag | Keep / remove / move | Structural change | Add only after prerequisites |
| --- | --- | --- | --- |
| Custom | Remove Delete It! | Keep tag empty | — |
| Cardio | Keep all current activities | Keep Outdoor, Indoor, Conditioning, Adaptive Cardio, Jump Rope | — |
| Warm-up | Keep all current mobility, dynamic, cardio and sport-prep work | Make numeric order unique | — |
| Cool-down | Remove Light Shadowboxing | Unique section order | — |
| Boxing | Keep set | Base, Beginner, Intermediate | — |
| Kickboxing | Keep all; move Axe, Crescent, Spinning Back Kick, Hook Kicks and Foot Sweep out of Base | Base and Intermediate | — |
| Muay Thai | Keep all; move Kick Catch, Sweep and elbow variations out of Base | Base, Elbows, Clinch, Intermediate | — |
| MMA | Keep all | Striking, Wrestling, Ground, Cage, Submissions | — |
| BJJ | Keep all | Positions, Escapes, Sweeps, Passes, Submissions | — |
| Karate | Keep all | Current stances/punches/blocks/kicks grouping is sound | — |
| Taekwondo | Keep all | Stances, Blocks, Punches, Kicks | — |
| Judo | Keep all | Ukemi, Movement, Throws, Groundwork | — |
| Core Training | Keep all | Current Base section is usable | Hollow Hold, Reverse Crunch, Bear Plank |
| HIIT | Keep all | Current Battle Ropes, Bootcamp, Agility grouping is sound | — |
| TRX | Keep all | Current Foundational, Lower, Upper, Core grouping is sound | Suspension Push-Up, Suspension Y-Fly |
| CrossFit | Keep all | Current categories are sound | Thruster, Power Clean, Clean and Jerk, Wall Ball, Box Jump, Toes-to-Bar |
| Tabata | Keep all | Fix section numbers | — |
| Strength | Keep all | Current equipment grouping is sound | Dumbbell Biceps Curl, Lateral Raise, Triceps Extension, Dumbbell Fly |
| Pilates | Removed | Removed with the top-level tag | — |
| Step Training | Removed | Removed with the top-level tag | — |
| Yoga | Removed | Removed with the top-level tag | — |
| Meditation | Keep all | Use contiguous section numbering | — |
| Breathwork | Keep all | Current library is sufficient | — |
| Functional Training | Keep all | Current Movement, Stability, Loaded, Balance grouping is sound | — |
| Calisthenics | Keep all | Current Base/Intermediate/Advanced grouping is sound | Inverted Row, Pike Push-Up |
| Other | Keep Rest, Preparation, Repeat, End Repeat, Note and Work | Keep `1 Rest`, `2 Structure` | Work energy-neutral support |

## App boundary before publishing weighted additions

`default` and `default2` are parsed into `WorkoutBuildItem.initialValue` and
`initialValue2`. The app now exposes both values in `ShortcutWeightRow` and
persists them in `WorkoutBuildReference`, so weighted exercises are available
from 1.1.4 without losing a user’s count or weight edits.

## Implementation order

1. Fix the app support for count-and-weight values and energy-neutral Work.
2. Apply the P0 catalog repairs and section-number cleanup without changing
   stable ids or keys.
3. Apply sport-section reorganization; this changes downloaded catalog order
   without requiring an application release.
4. Add the proposed time-based exercises from 1.1.2.
5. Add weighted Strength and CrossFit exercises from 1.1.4.
