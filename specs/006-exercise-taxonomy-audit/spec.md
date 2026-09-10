# Feature Specification: Exercise Taxonomy Audit

**Feature Branch**: `main`
**Created**: 2026-09-10
**Status**: Draft
**Input**: Audit every exercise in the approved 21-tag workout catalog. Decide
whether each exercise belongs in its current tag and section, whether it is a
useful timer exercise, and whether it is an intentional context-specific
variant or unnecessary noise. Do not change catalog data during the audit.

## User Scenarios & Testing

### User Story 1 — Choose a relevant exercise without noise (Priority: P1)

A user opens any tag and sees exercises that have a clear purpose for that
training family. They do not need to interpret generic labels, duplicate
presets or movements that belong to another type of workout.

**Why this priority**: The catalog is the product's exercise vocabulary. A
misplaced or vague record makes timer setup slower and makes the tag structure
meaningless.

**Independent Test**: An audit verdict exists for every current exercise:
Keep, Move, Retire/Merge or Needs evidence. Each non-Keep verdict states a
specific destination or reason.

**Acceptance Scenarios**:

1. **Given** a user opens a combat tag, **When** they browse its exercises,
   **Then** every record is a technique, drill, stance, defence, grappling
   action or sport-specific practice relevant to that sport.
2. **Given** a user opens a method tag such as HIIT, Tabata, CrossFit or TRX,
   **When** a familiar movement appears, **Then** its inclusion is justified
   by method, equipment or prescribed interval context rather than being an
   accidental duplicate of Strength.
3. **Given** an exercise has a vague title such as a generic training session,
   **When** it cannot be distinguished from its tag's Main starter,
   **Then** it is marked Retire/Merge.

### User Story 2 — Recover safely and find the right calm practice (Priority: P1)

A user can distinguish Rest, Breathwork and Meditation by their purpose:
post-workout recovery, breathing protocol and attentional practice.

**Why this priority**: The catalog intentionally contains related activities
in separate tags; the distinction must be clear rather than arbitrary.

**Independent Test**: Every record in Rest, Breathwork and Meditation has one
clear purpose and no record promises an unmeasured physiological outcome.

**Acceptance Scenarios**:

1. **Given** a user finishes a hard workout, **When** they open Rest, **Then**
   they see active recovery, recovery breathing, static stretching or release
   work—not an interval control or generic meditation session.
2. **Given** a user selects Breathwork, **When** they browse a protocol,
   **Then** the exercise is defined by its breathing pattern rather than a
   meditation theme.
3. **Given** a user needs a pause during a workout, **When** they select Other
   → Rest, **Then** it remains distinct from the top-level Rest recovery tag.

### User Story 3 — Retain intentional variants and correct genuine mistakes (Priority: P2)

A user benefits from a deliberate context-specific version of a movement while
the audit removes accidental duplicates and fixes wrong sections.

**Why this priority**: A Jab in Boxing and a Jab in Muay Thai are not an error;
a generic Functional Strength Training timer inside Strength is noise because
it has no distinct movement or method.

**Independent Test**: Every repeated display title across tags has an explicit
classification of intentional context variant or duplicate candidate.

**Acceptance Scenarios**:

1. **Given** the same movement appears in two combat tags, **When** its rules
   or technique context differ by sport, **Then** it is retained in both tags.
2. **Given** a movement appears in Strength and a method tag, **When** the
   method tag gives it a specific interval, equipment or training context,
   **Then** it is retained in both tags.
3. **Given** a section contradicts an exercise's primary demand, **When** the
   audit identifies it, **Then** it receives one specific corrected section.

## Requirements

### Audit Coverage

- **FR-001**: The audit MUST cover all 21 visible tags and every exercise
  currently referenced by the manifest.
- **FR-002**: Each record MUST receive exactly one verdict: Keep, Move,
  Retire/Merge or Needs evidence.
- **FR-003**: Every Move verdict MUST name one existing destination tag and
  section; it MUST preserve the record's stable key if implemented.
- **FR-004**: Every Retire/Merge verdict MUST identify the canonical retained
  exercise or explain why the record has no distinct timer purpose.
- **FR-005**: The audit MUST review title, description, section, workout type,
  value type, availability and energy profile for each record.

### Taxonomy Rules

- **FR-006**: Combat tags MAY repeat a movement only when it is a genuine
  sport-specific technique or drill; general conditioning belongs elsewhere.
- **FR-007**: Warm-up contains light preparation and sport-specific warm-up
  flows. Cardio contains sustained modalities and jump-rope technique.
- **FR-008**: Rest contains post-workout recovery only. Breathwork contains
  named breathing protocols. Meditation contains attentional or reflective
  practices.
- **FR-009**: Core contains trunk stability, bracing, rotation,
  anti-rotation and closely related control work. Balance-only exercises must
  be justified as trunk-control work or moved.
- **FR-010**: Strength contains resistance and bodyweight strength movements;
  it MUST NOT retain a generic Functional Strength Training session when the
  Main Strength starter already serves that purpose.
- **FR-011**: HIIT, Tabata, CrossFit and TRX retain method- or equipment-led
  variants. Their section labels MUST match the movement's primary demand.
- **FR-012**: Other contains timer-building controls only and cannot be used
  as a destination for physical exercises.

### Initial Findings to Verify in the Audit

- **FR-013**: `Functional Strength Training` in Strength is a retire candidate:
  it is generic and overlaps the Main Strength starter and Circuit Training.
- **FR-014**: `Feet-Together Stand` and `Single-Leg Stand` in Core require a
  trunk-control justification; otherwise they are move candidates rather than
  automatic Core exercises.
- **FR-015**: `Plank Jacks` in Tabata is a full-body or conditioning movement,
  not an upper-body movement; its section is a correction candidate.
- **FR-016**: `Box Jump` in CrossFit requires a section review because it is
  plyometric bodyweight work rather than a conventional gymnastics skill.
- **FR-017**: Rest's `Breathing` label is a rename candidate to make its
  recovery purpose clear without duplicating standalone Breathwork protocols.

## Key Entities

- **Audit verdict**: The decision and evidence for one exercise record.
- **Intentional context variant**: The same movement retained because a sport,
  method, equipment or interval context changes its user purpose.
- **Canonical record**: The single retained record that replaces an accidental
  duplicate.
- **Timer purpose**: The reason a movement belongs in a timer catalog rather
  than being a generic fitness concept.

## Assumptions

- The current catalog baseline is 21 tags and 520 referenced exercises.
- Repeated punches, kicks, grappling movements and method-specific versions
  are not automatically duplicates; their context must be evaluated first.
- The visible title `Rest` is intentionally used both for top-level recovery
  and Other's in-round control; the parent tag supplies the distinction.
- This audit must not revive Yoga, Pilates, Calisthenics or Functional
  Training as top-level tags.

## Out of Scope

- Changing icons, colors, keys, availability or app code.
- Adding new exercises before the audit identifies a specific coverage gap.
- Changing interval behaviour, calorie formulas or client compatibility.

## Success Criteria

- 100% of manifest exercises have a documented audit verdict.
- 100% of duplicate titles across tags are classified as intentional variants
  or correction candidates.
- No exercise is left in a section that contradicts its primary demand.
- A user can explain the difference between Rest, Breathwork and Meditation
  without reading implementation documentation.
- The audit identifies no more than one canonical generic session per physical
  training tag, excluding explicitly distinct methods such as Circuit
  Training.
