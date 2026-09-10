# Feature Specification: Training Taxonomy Reset

**Feature Branch**: `main`
**Created**: 2026-09-10
**Status**: Draft
**Input**: Simplify and reorder the workout catalog. The product is a training
timer, so tags must be concise and obvious. Combat sports come first after
Custom and Warm-up; Rest must not be confused with static stretching or an
in-round Rest block.

## User Scenarios & Testing

### User Story 1 — Find the workout in a predictable order (Priority: P1)

A user opens the catalog and encounters Custom first, then preparation,
combat sports, core work, cardio, recovery and finally specialist workout
methods. They do not need to understand an abstract fitness taxonomy to find a
training timer.

**Why this priority**: The existing flat list mixes sport, recovery,
equipment, movement style and timer controls. The order should match the
user's typical training flow and the app's combat focus.

**Independent Test**: A user locates Custom, Warm-up, Boxing, Core, Cardio,
recovery work, HIIT and an in-round Rest timer block without searching or opening an
unrelated tag.

**Acceptance Scenarios**:

1. **Given** the catalog root, **When** a user opens it, **Then** Custom is
   first and Warm-up is second.
2. **Given** the catalog root, **When** a user scrolls after Warm-up, **Then**
   all combat sports appear consecutively before Core and Cardio.
3. **Given** a user reaches the end of a hard session, **When** they open Rest
   **Then** they can choose a light walk, recovery breathing,
   stretching or self-release work in clear sections.
4. **Given** a user is building intervals, **When** they need a break between
   rounds, **Then** they use Other → Rest rather than the post-workout tag.

### User Story 2 — Understand recovery without mixing its parts (Priority: P1)

A user sees that Rest has several valid recovery activities, but they
are separated by intent: active rest, breathing, static stretching and release
work. They can choose a suitable recovery action without treating
all post-workout movement as the same exercise.

**Why this priority**: Current Cool-down combines unrelated exercises in one
undifferentiated list, making it unclear what to do after a workout.

**Independent Test**: Review every current Cool-down record. Each has one
unambiguous destination section under Rest, and no static stretch
appears in Active Rest.

**Acceptance Scenarios**:

1. **Given** a user wants to gradually reduce effort, **When** they choose
   Active Rest, **Then** they see low-intensity movement such as easy walking,
   marching, cycling or easy sport-specific movement.
2. **Given** a user wants flexibility work, **When** they choose Static
   Stretching, **Then** they see stretches and not foam rolling or interval
   controls.
3. **Given** a user wants to loosen muscles with a roller, **When** they open
   Release Work, **Then** foam rolling is available and is not labelled as a
   stretch.

### User Story 3 — Avoid vague and duplicate training tags (Priority: P2)

A user sees only tags with a distinct purpose. Functional Training and
Calisthenics no longer compete with Strength or Core as vague parallel homes
for the same movement.

**Why this priority**: A movement cannot be classified reliably when multiple
top-level tags claim it for different marketing reasons.

**Independent Test**: Audit every Functional Training and Calisthenics record.
Each retained record is placed in either Core or Strength, without a duplicate
technical identity.

**Acceptance Scenarios**:

1. **Given** a bodyweight or loaded strength movement, **When** it is retained,
   **Then** it belongs to Strength unless its primary purpose is core control.
2. **Given** an exercise whose primary purpose is trunk stability, bracing,
   rotation or anti-rotation, **When** it is retained, **Then** it belongs to
   Core.
3. **Given** a Yoga, Pilates or Calisthenics tag, **When** the revised catalog
   is published, **Then** that tag is not present.

## Requirements

### Visible Tag Order

- **FR-001**: The catalog MUST show these tags in this exact order:
  1. Custom
  2. Warm-up
  3. Boxing
  4. Kickboxing
  5. Muay Thai
  6. MMA
  7. BJJ
  8. Karate
  9. Taekwondo
  10. Judo
  11. Core
  12. Cardio
  13. Rest
  14. Breathwork
  15. Strength
  16. HIIT
  17. Tabata
  18. CrossFit
  19. TRX
  20. Meditation
  21. Other
- **FR-002**: The display title MUST be Core, not Core Training.
- **FR-003**: The current Cool-down tag MUST be renamed Rest. It
  replaces the visible Cool-down category without changing the meaning of the
  existing Other → Rest timer control.
- **FR-004**: Any visual group headings added by a newer client MUST preserve
  the required flat tag order for the currently released client.

### Combat and Training Tags

- **FR-005**: Combat sports MUST stay consecutive and retain their distinct
  techniques. A combat exercise MUST not be moved into a generic fitness tag
  solely because it also develops conditioning.
- **FR-006**: Cardio MUST remain a dedicated tag and MUST appear immediately
  after Core.
- **FR-007**: Core MUST remain a dedicated tag because trunk control is a
  primary training need for combat sports and is distinct from general strength
  work.
- **FR-008**: Strength MUST contain retained general resistance and bodyweight
  strength movements whose primary purpose is not core control.
- **FR-009**: HIIT, Tabata, CrossFit and TRX MUST remain distinct specialist
  methods or equipment-led training tags after Strength.

### Rest

- **FR-010**: Rest MUST use a green semantic color in the current
  app appearance. Its legacy color remains available for the currently
  released client.
- **FR-011**: After its mandatory Main starter, Rest MUST contain these
  sections in order: Active Rest, Recovery Breathing, Static Stretching and
  Release Work.
- **FR-012**: Active Rest MUST contain gradual low-intensity movement only,
  including easy walking, marching, easy cycling and, where appropriate, easy
  sport-specific movement.
- **FR-013**: Recovery Breathing MUST guide comfortable, unforced breathing
  after activity. It MUST NOT promise a particular heart-rate result without a
  measured heart-rate input.
- **FR-014**: Static Stretching MUST contain flexibility holds. It MUST NOT be
  represented as Active Rest.
- **FR-015**: Release Work MUST contain foam rolling and comparable self-release
  activities. It MUST NOT be represented as stretching.
- **FR-016**: Other → Rest MUST remain a green in-workout timer block for
  breaks between active rounds. It is not a post-workout recovery session.

### Tag Retirement and Exercise Reclassification

- **FR-017**: Pilates and Yoga MUST remain absent from the revised catalog.
- **FR-018**: The Calisthenics tag MUST be retired. Each of its exercises MUST
  be audited and either moved to Core or Strength, or retired when it does not
  have a clear purpose in the revised catalog.
- **FR-019**: The Functional Training tag MUST be retired. Each retained
  exercise MUST move to Core when its primary purpose is trunk control, or to
  Strength when its primary purpose is general resistance work.
- **FR-020**: Every retained exercise MUST have one primary tag and one
  primary section. Equivalent duplicate records MUST not remain in multiple
  tags.
- **FR-021**: Stable identities of retained exercises MUST be preserved so
  saved workouts continue to resolve.

## Key Entities

- **Tag**: A concise, user-visible training family or timer-control family.
- **Section**: A clear purpose-based grouping inside a tag.
- **Other → Rest**: The in-round timer control used between active rounds.
- **Top-level Rest**: The post-workout tag used to transition out of a session.
- **Core**: Training whose primary purpose is trunk stability, bracing,
  rotation or anti-rotation.
- **Release Work**: Foam rolling and comparable self-release activity; it is
  distinct from static stretching.

## Assumptions

- A gradual active cool-down is useful as a distinct post-workout flow; major
  health guidance recommends reducing intensity rather than stopping abruptly.
  [American Heart Association](https://www.heart.org/en/healthy-living/exercise-and-physical-activity/fitness-basics/warm-up-cool-down), [NHS](https://www.nhsinform.scot/illnesses-and-conditions/muscle-bone-and-joints/how-to-reduce-your-risk-of-injury-from-exercise-or-physical-activity)
- The timer should distinguish Other → Rest (an in-round interval) from the
  top-level Rest recovery flow, even though both use the same display word.
- The app's combat focus justifies placing Core before Cardio and Rest, then
  Breathwork and Strength before generic conditioning methods.
- All reclassification decisions are made from an exercise's primary intent,
  not merely its equipment, body part or whether it can raise heart rate.

## Out of Scope

- Adding health sensors, recovery scores or a heart-rate claim.
- Changing timer interval behaviour or the Rest block's timer role.
- Deleting a retained exercise identity before a valid destination is assigned.
- Changing timer interval behaviour, the current app's compatibility parser or
  committing and publishing the catalog.

## Success Criteria

- The revised catalog displays exactly the 21 tags in the required order.
- 100% of current Cool-down records receive one unambiguous Rest
  section.
- 100% of current Functional Training and Calisthenics records receive a keep,
  move or retire decision.
- 100% of retained exercises have one primary tag and one primary section.
- A user can find Custom, a combat drill, Core, Cardio, Rest and an
  in-round Rest block without using search.
- No Active Rest section contains a static stretch, foam-rolling activity or
  interval timer control.
