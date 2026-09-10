# Feature Specification: Catalog Theme Colors

**Feature Branch**: `main`
**Created**: 2026-09-10
**Status**: Draft
**Input**: Preserve the current catalog color for older releases, add light and
dark color variants, use the current color in light mode, and use a darker
variant in dark mode.

## User Scenarios & Testing

### User Story 1 — Recognize a training family in either appearance (Priority: P1)

When a user opens the catalog in light or dark appearance, the color attached
to a training tag and its exercises remains recognizable as the same family.
The dark appearance avoids a bright color block that competes with the dark
surface.

**Why this priority**: Color is the primary visual cue connecting a tag to its
exercises.

**Independent Test**: Select any catalog tag in light appearance and dark
appearance. Its icon background uses the light and dark variants respectively,
while retaining the same hue family.

**Acceptance Scenarios**:

1. **Given** a catalog record with all three color fields, **When** the app is
   in light appearance, **Then** it uses `colorLight`.
2. **Given** the same record, **When** the app is in dark appearance, **Then**
   it uses `colorDark`.
3. **Given** a tag and an exercise within that tag, **When** either appearance
   is shown, **Then** both use the variant belonging to the same catalog color
   family.

### User Story 2 — Keep existing releases and saved content working (Priority: P1)

When an older supported release downloads the expanded catalog, it continues
to use the existing `color` field and loads the catalog normally. A locally
saved or older record that has no variants still has a usable color.

**Why this priority**: The catalog is downloaded by more than one application
version and must not make an existing catalog unavailable.

**Independent Test**: A record that contains only `color` resolves to that
color in either appearance; a supported older release can load a catalog record
that also contains the two new optional fields.

**Acceptance Scenarios**:

1. **Given** a supported older release, **When** it downloads the expanded
   catalog, **Then** the unchanged `color` value remains available to it.
2. **Given** a record without `colorLight` or `colorDark`, **When** either
   appearance is shown, **Then** the app falls back to `color`.
3. **Given** a saved workout with an existing exercise identity, **When** the
   color fields are introduced, **Then** its identity and exercise selection do
   not change.

### User Story 3 — Maintain a deterministic palette (Priority: P2)

When a catalog editor adds or reviews a color, the light and dark variants can
be reproduced without subjective per-record decisions.

**Why this priority**: A catalog with hundreds of exercises needs stable visual
rules rather than hand-tuned drift.

**Independent Test**: For every catalog color, the light variant is equal to
the legacy color and the dark variant is the documented darker counterpart.

## Requirements

### Catalog Color Data

- **FR-001**: The existing `color` field MUST remain present and unchanged on
  every current tag and exercise record. It remains the compatibility color for
  earlier releases.
- **FR-002**: Every published tag record and exercise record MUST include
  `colorLight` and `colorDark` as valid six-digit sRGB hex colors.
- **FR-003**: `colorLight` MUST exactly equal the record's existing `color`.
- **FR-004**: `colorDark` MUST retain the same hue family and be darker than
  `colorLight`. Its red, green, and blue channels MUST each be 70% of the
  corresponding `colorLight` channel, rounded to the nearest integer.
- **FR-005**: The same legacy source color MUST always produce the same pair of
  variants across tags and exercises.

### Appearance Behavior

- **FR-006**: Light appearance MUST select `colorLight`; dark appearance MUST
  select `colorDark` for both tag icons and exercise icons.
- **FR-007**: If either new field is missing or invalid, the app MUST use the
  existing `color` field rather than hiding the record, changing its identity,
  or failing the catalog load.
- **FR-008**: The color selection rule MUST apply consistently anywhere the
  catalog renders a tag or exercise color, including selection, search, list,
  and workout-building surfaces.

### Compatibility and Scope

- **FR-009**: Adding the two color fields MUST NOT require a catalog schema
  version change, change availability boundaries, alter icon assets, or change
  a tag/exercise ID, key, title, section, timer value, or energy profile.
- **FR-010**: A release that knows only `color` MUST remain able to load every
  record containing the new fields.

## Key Entities

- **Legacy color**: The existing `color` value, retained as a compatibility
  value and source for both appearance variants.
- **Light color**: The catalog color used when the interface is light.
- **Dark color**: The darker catalog color used when the interface is dark.
- **Color family**: A tag color and the matching colors of its exercises.

## Assumptions

- The catalog's current `color` values are the approved light palette.
- Dark variants are calculated from the legacy color using the 70% channel rule
  so all existing and future colors follow one rule.
- Existing user-created or historical records may lack the two new fields and
  use `color` as their fallback.

## Out of Scope

- Changing the current palette names, exercise icons, SF Symbols, IDs, keys,
  availability, timer behavior, or energy estimates.
- Retrofitting older released binaries to render dark variants.
- Adding theme-specific exercise groups, tags, or catalog schemas.

## Success Criteria

- 100% of the 23 current tags and 535 current exercise records have valid
  `colorLight` and `colorDark` values.
- 100% of `colorLight` values equal their record's legacy `color`.
- 100% of `colorDark` values are reproducible from the documented rule and are
  visually darker than their corresponding light variant.
- A record with only the legacy color remains visible and recognizable in both
  appearances.
- An older supported release loads the expanded catalog without a catalog
  availability error caused by the two new fields.
