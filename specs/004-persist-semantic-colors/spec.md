# Feature Specification: Persist Semantic Colors

**Feature Branch**: `1.1.4`
**Created**: 2026-09-10
**Status**: Draft
**Input**: The color picker shows the approved semantic palette. A user picks a
color by name, and saved exercise data stores only that color type—not a hex
color. Remove the old color parameter from saved exercise data and migrate
existing records safely.

## User Scenarios & Testing

### User Story 1 — Choose a color that follows the app appearance (Priority: P1)

When creating or editing a custom exercise, a user sees the approved palette
in Choose Icon. Selecting a color stores its stable name and the selected icon
automatically follows light or dark appearance everywhere it is rendered.

**Why this priority**: The chosen visual color must be both clear at selection
time and stable across future palette adjustments.

**Independent Test**: Choose `red` in Choose Icon, save the exercise, reopen
it, and view it in light and dark appearance. The saved selection remains
`red`, and the displayed color comes from the corresponding appearance value.

**Acceptance Scenarios**:

1. **Given** the Choose Icon color picker, **When** the user opens it, **Then**
   it presents exactly the approved bare color names: `red`, `coral`,
   `orange`, `yellow`, `green`, `teal`, `cyan`, `blue`, `indigo`, `purple`,
   `violet`, `pink`, `gray`, `sage`, and `tan`.
2. **Given** the user selects a color, **When** they save a custom exercise,
   **Then** its stored color selection is that bare color name and contains no
   stored hex color.
3. **Given** the user reopens the saved exercise in a different appearance,
   **When** it is rendered, **Then** the same color type resolves to that
   appearance's asset value.

### User Story 2 — Preserve existing exercises during the storage migration (Priority: P1)

When an existing user upgrades, exercises saved with the old hex parameter
remain available. Their known old color is converted to its matching semantic
type and the saved exercise data is rewritten without the old parameter.

**Why this priority**: Removing storage data without migration would discard a
user's color selection.

**Independent Test**: Seed an existing saved exercise with each legacy palette
hex value, load the app once, and inspect its rewritten record. It contains a
valid `colorType` and no old exercise-color parameter.

**Acceptance Scenarios**:

1. **Given** a saved exercise with a known legacy palette color and no type,
   **When** the app loads its exercise data, **Then** it assigns the matching
   semantic type and rewrites the record without the legacy exercise color.
2. **Given** a saved exercise with an unrecognized legacy color, **When** the
   migration runs, **Then** it uses `red`, matching the existing picker
   fallback, and rewrites the record without the legacy exercise color.
3. **Given** an exercise already stored with a valid type and no legacy
   color, **When** it loads, **Then** it remains unchanged.

### User Story 3 — Keep runtime integrations intact (Priority: P2)

When a workout starts, any runtime surface that still requires a concrete
color can derive it from the selected semantic type. Removing hex persistence
from exercise data does not break a timer, shared workout state, or a saved
workout sequence.

**Why this priority**: The storage cleanup must not make a valid workout fail
to start after an app upgrade.

**Independent Test**: Start a workout built from a migrated custom exercise;
it has a valid runtime color while its saved exercise record contains only the
semantic type.

## Requirements

### Picker and Palette

- **FR-001**: Choose Icon MUST use the one approved semantic palette and MUST
  not expose a separate legacy or hex-based color list.
- **FR-002**: The picker MUST store only the selected bare color type.
- **FR-003**: The visible picker swatches and every rendered selection MUST
  resolve through the same design-system palette and its appearance-aware
  assets.

### Exercise Persistence

- **FR-004**: Newly saved custom exercises MUST persist `colorType` and MUST
  NOT persist `colorHex` in the exercise-data payload.
- **FR-005**: Existing saved exercise payloads containing `colorHex` MUST be
  migrated to `colorType` before being rewritten without `colorHex`.
- **FR-006**: The migration MUST map each known legacy palette hex to its
  matching bare type. The known yellow Note legacy color maps to `yellow`.
- **FR-007**: An unrecognized legacy hex MUST migrate to `red`, the existing
  picker fallback, rather than preventing the exercise from loading.
- **FR-008**: A record already holding a valid `colorType` MUST not be changed
  by migration solely because it has no legacy hex.
- **FR-009**: Imported catalog `color` remains a catalog compatibility field;
  it MUST NOT be copied into newly persisted exercise data when `colorType` is
  available.

### Runtime Boundary

- **FR-010**: The app MUST derive any concrete runtime color needed for a
  timer or shared workout state from `colorType` when a stored exercise no
  longer has `colorHex`.
- **FR-011**: The change applies to saved exercise data only. Existing runtime
  timer, Live Activity, and snapshot contracts that require a concrete color
  are outside this migration and remain compatible.
- **FR-012**: No exercise ID, key, title, timer value, workout structure, or
  selected icon may change during color migration.

## Key Entities

- **Color type**: One approved bare palette name selected by the user and
  saved with an exercise.
- **Saved exercise payload**: The persisted exercise data that must contain
  `colorType` and no `colorHex` after migration.
- **Legacy exercise color**: The historic saved hex value used only to map an
  existing record into a semantic type during migration.
- **Runtime color**: A concrete color derived only when a running workout or
  system integration requires it.

## Assumptions

- The existing Choose Icon control already has the 15 approved semantic color
  choices; this feature makes those choices the sole persistence source.
- The current picker resolves unknown stored hex colors to `red`; migration
  preserves that established behavior.
- Catalog downloads retain `color` for 1.1.2 and 1.1.3 compatibility, while
  the local exercise database is owned by the current app version.

## Out of Scope

- Removing concrete colors from timer, Live Activity, or snapshot contracts.
- Adding new colors beyond the approved semantic palette.
- Changing the catalog's compatibility `color` field or supporting older app
  binaries' local database schemas.

## Success Criteria

- 100% of newly saved custom exercises contain a valid `colorType` and no
  `colorHex` in their persisted exercise payload.
- 100% of migrated saved exercises with a known legacy palette hex preserve
  their matching color family.
- 100% of migrated saved exercises with an unknown legacy hex load with the
  documented `red` fallback instead of failing to load.
- The picker displays exactly 15 approved semantic choices and no legacy color
  list.
- A migrated exercise can be added to and started in a workout without a
  missing-color failure.
