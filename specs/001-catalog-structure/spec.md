# Feature Specification: Catalog Structure

**Feature Branch**: `main`
**Created**: 2026-09-09
**Status**: Draft
**Input**: Reassess the workout catalog, put Custom first, place martial arts before other training, introduce useful sections, and audit current catalog data without breaking app 1.1.3.

## User Scenarios & Testing

### User Story 1 — Find a training family quickly (Priority: P1)

When opening the exercise catalog, a user sees Custom first. The remaining
categories begin with martial arts and then continue through the non-combat
training families in a predictable order.

**Why this priority**: The catalog is the entry point to creating every workout.

**Independent Test**: On an empty search, the visible category list starts with
Custom, followed by the agreed martial-arts sequence, without relying on tag
IDs.

**Acceptance Scenarios**:

1. **Given** an empty search, **When** the catalog opens, **Then** Custom is the
   first category.
2. **Given** an empty search, **When** the catalog opens, **Then** every combat
   category appears before every non-combat category.
3. **Given** a saved workout that references an existing exercise, **When** the
   catalog is reorganized, **Then** the saved workout still resolves the same
   exercise.

### User Story 2 — Understand exercise groups inside a category (Priority: P1)

When a user opens a category, they see concise section headers that progress
from the most foundational work to more specific work.

**Independent Test**: Opening Boxing shows the Main and Base sections before
the later technical sections; opening any other category preserves its intended
section order.

**Acceptance Scenarios**:

1. **Given** a category with multiple exercise groups, **When** it is opened,
   **Then** its section headers and exercises appear in the documented order.
2. **Given** an older supported app, **When** it opens the same category,
   **Then** it can still display every exercise and section without a catalog
   loading failure.

### User Story 3 — Trust catalog information (Priority: P2)

When a user opens exercise information, the description is clear, structured,
and relevant to the selected exercise. Fields that are shown or that affect the
workout have one documented meaning.

**Independent Test**: Every catalog description is either a numbered sequence
of instructions or is explicitly exempt as intentional copy; every field that
affects selection, display, timing, availability, or energy has an owner.

## Requirements

### Catalog Navigation

- **FR-001**: Custom MUST remain first whenever it is available.
- **FR-002**: The catalog MUST order the current categories as follows after
  Custom:

  1. Boxing
  2. Kickboxing
  3. Muay Thai
  4. Karate
  5. Taekwondo
  6. MMA
  7. BJJ
  8. Judo
  9. Cardio
  10. HIIT
  11. Tabata
  12. CrossFit
  13. TRX
  14. Strength
  15. Functional Training
  16. Calisthenics
  17. Core Training
  18. Step Training
  19. Warm-up
  20. Cool-down
  21. Pilates
  22. Yoga
  23. Meditation
  24. Breathwork
  25. Other

- **FR-003**: The grouping concept for the visible order MUST be:
  Custom; Martial Arts; Conditioning; Movement & Recovery; Utilities. These
  group labels are presentation metadata, not replacements for existing tags.
- **FR-004**: The current 1.1.3 experience MUST remain a single ordered list.
  A later client may render the group labels as headers, but their absence MUST
  not hide, reject, or move an exercise in 1.1.3.

### Exercise Sections and Identity

- **FR-005**: Exercise sections MUST remain scoped to one tag. Each section
  label MUST have an explicit numeric order and a concise visible title.
- **FR-006**: Main content MUST remain first within a tag; foundational skills
  MUST precede combinations, advanced work, and specialty work.
- **FR-007**: Existing exercise IDs and keys MUST remain stable. Reorganizing
  tags, files, or sections MUST NOT change the identity resolved by a saved
  workout.
- **FR-008**: The order of exercises inside one section MUST be intentional and
  documented rather than inferred from an ID.

### Data Governance

- **FR-009**: Every exercise MUST keep a non-empty description. Standard
  instructional exercises MUST use sequential, numbered steps.
- **FR-010**: Every field in an exercise record MUST be classified as one of:
  actively used, retained only for compatibility, intentionally unused pending
  product work, or removable in a versioned migration.
- **FR-011**: Fields that presently have no user-visible or runtime effect MUST
  NOT receive new catalog data until an owner and use case are defined.
- **FR-012**: Version availability MUST gate visibility only; it MUST NOT be
  relied on to make unsupported record shapes safe for an older client.

### Compatibility

- **FR-013**: The catalog reorganization MUST preserve the existing catalog
  schema and required fields consumed by 1.1.3.
- **FR-014**: New cross-tag grouping metadata, if introduced later, MUST be
  optional for older clients and MUST have a linear ordering fallback.
- **FR-015**: Every catalog record and asset referenced by the catalog index
  MUST remain loadable after the reorganization.

## Current-State Audit Summary

The audit is recorded in [audit.md](audit.md). Its key conclusions are:

- The catalog is structurally complete: 27 tags and 583 exercise records, with
  no orphan exercise JSON, missing manifest pair, duplicate exercise key, or
  duplicate exercise ID.
- The existing order is already read from the catalog index; the application
  also pins Custom first.
- Existing exercise sections are usable for this project and are the safe way
  to group exercises in 1.1.3.
- Several fields are accepted and stored but do not currently change the
  catalog UI or workout execution. Their ownership must be resolved before a
  broad content rewrite.

## Key Entities

- **Catalog category**: A user-visible training family such as Boxing or Yoga.
- **Category group**: A presentation-only collection of categories such as
  Martial Arts or Conditioning.
- **Exercise**: A stable, selectable workout building block.
- **Exercise section**: An ordered group of exercises within one category.
- **Catalog availability**: The app-version interval in which an exercise is
  shown to a user.

## Assumptions

- Custom remains a special empty-capable category and stays pinned first.
- This feature reorganizes and governs catalog content; it does not add or
  remove training categories or exercises.
- Cross-tag headers are deferred until a client can render them; the linear
  category order remains the compatibility fallback.

## Out of Scope

- Changing exercise IDs or keys.
- Adding, removing, or renaming training categories.
- Changing timer controls, workout calculations, or availability semantics.
- Publishing raw value types that older supported clients cannot parse.

## Success Criteria

- Users can reach any martial-arts category from the first eight non-Custom
  positions in the catalog.
- 100% of existing saved exercise identities resolve after reorganization.
- 100% of catalog files and referenced icon assets remain loadable.
- 100% of standard instructional descriptions conform to the approved
  structured-description rule or have a documented exemption.
- No supported 1.1.3 catalog load fails because of the new ordering or section
  structure.
