# Feature Specification: Semantic Catalog Color Assets

**Feature Branch**: `1.1.4`
**Created**: 2026-09-10
**Status**: Draft
**Input**: Move the approved catalog palette into the app's color assets. A
catalog record names its color with `colorType`; the app resolves that name to
the correct light or dark appearance color. Do not add `colorLight` or
`colorDark` to catalog records.

## User Scenarios & Testing

### User Story 1 — See a consistent training color in either appearance (Priority: P1)

When a user opens the workout catalog in light or dark appearance, each
training tag and each exercise retains its assigned color family. The color is
appropriate for the current surface without the catalog having to duplicate
two hex values for every record.

**Why this priority**: A tag's color is the visual link between a training
family and its exercises, and the appearance choice must be consistent across
the catalog.

**Independent Test**: Open a tag and one of its exercises in light appearance,
then in dark appearance. Both use the same named color family, while the
appearance-appropriate asset value is shown in each mode.

**Acceptance Scenarios**:

1. **Given** a catalog tag or exercise with `colorType: "red"`, **When**
   light appearance is active, **Then** it shows the light value of the
   `red` color asset.
2. **Given** the same record, **When** dark appearance is active, **Then** it
   shows the dark value of the same `red` color asset.
3. **Given** a tag and an exercise assigned the same `colorType`, **When**
   either appearance is active, **Then** their color family is identical.

### User Story 2 — Keep released catalog clients and saved workouts usable (Priority: P1)

When catalog records gain a named color type, older supported releases still
load the catalog using their existing compatibility color. Saved workouts and
locally created content remain visible even if they do not carry a color type.

**Why this priority**: Publishing a catalog update must not make workouts
unavailable to a user who has not updated the application.

**Independent Test**: An older supported release loads a record containing
`color` and `colorType`; a current release shows a record that has only the
legacy color without failing or hiding it.

**Acceptance Scenarios**:

1. **Given** a supported 1.1.2 or 1.1.3 release, **When** it downloads a
   record containing `colorType`, **Then** it continues to use `color` and
   loads the catalog normally.
2. **Given** a 1.1.4 record without a recognized `colorType`, **When** it is
   rendered, **Then** the record remains visible using its existing `color`.
3. **Given** a saved workout or locally created exercise with an existing
   color but no color type, **When** it is opened in either appearance,
   **Then** its identity and visible color remain usable.

### User Story 3 — Maintain one approved palette (Priority: P2)

When a catalog editor assigns a color to a tag or exercise, they choose one
stable palette name rather than copying color values. The palette definition is
the single place where its light and dark values live.

**Why this priority**: A named palette prevents color drift across hundreds of
catalog records and keeps appearance changes centralized.

**Independent Test**: Every published `colorType` matches one approved color
asset, and changing the dark value of one approved asset changes the dark
appearance for all records using that type without editing their catalog data.

## Requirements

### Named Palette

- **FR-001**: The application MUST provide one named color asset and one
  corresponding design-system color type for each approved name: `red`,
  `coral`, `orange`, `yellow`, `green`, `teal`, `cyan`, `blue`, `indigo`,
  `purple`, `violet`, `pink`, `gray`, `sage`, and `tan`.
- **FR-002**: Each approved color asset MUST define both a light-appearance
  value and a dark-appearance value. The approved light and dark values from
  the preceding palette decision are the source of truth for those assets.
- **FR-003**: The app MUST resolve a recognized `colorType` through the
  design-system color type with the identical bare name, which obtains the
  identically named color asset. Its light or dark value follows the active
  system appearance automatically.
- **FR-004**: A catalog record's `colorType` MUST be one of the approved
  palette names exactly; free-form names and hex values are not valid color
  types.

### Catalog Data and Compatibility

- **FR-005**: Every published catalog tag and exercise MUST retain its current
  `color` field unchanged as the compatibility color for released clients and
  legacy records.
- **FR-006**: Every published catalog tag and exercise intended for 1.1.4
  rendering MUST include one `colorType` that names its approved palette
  color.
- **FR-007**: The catalog MUST NOT add `colorLight` or `colorDark` fields.
- **FR-008**: A current application MUST treat a missing, empty, or unapproved
  `colorType` as a per-record fallback to `color`; it MUST NOT reject the
  entire catalog, hide the record, or change its identity.
- **FR-009**: Older supported releases that only understand `color` MUST keep
  loading records that additionally contain `colorType`.

### Consistent Rendering

- **FR-010**: The named palette rule MUST apply anywhere a catalog-derived tag
  or exercise color is rendered, including browsing, search, selection,
  workout construction, and the workout sequence. Each surface MUST obtain
  the color through the design system, not from a catalog hex value.
- **FR-011**: Changing the system appearance while the catalog is visible MUST
  update catalog-derived colors without requiring the user to download the
  catalog again or recreate a workout.
- **FR-012**: User-created content and historical saved content that has no
  catalog color type MUST continue to use its existing stored color.

### Boundaries

- **FR-013**: This feature MUST NOT change exercise or tag identity, title,
  icon, order, section, availability, timer behavior, default values, or
  energy profile.
- **FR-014**: The catalog color field remains a compatibility value; 1.1.4
  catalog rendering MUST prefer a valid `colorType` over `color`.

## Key Entities

- **Color type**: A stable, approved bare palette name stored on a catalog tag
  or exercise, such as `red`.
- **Color asset**: The centrally maintained light and dark values belonging to
  one color type.
- **Design-system color**: The application-level color identified by the same
  bare name as `colorType`, which obtains the appearance-appropriate asset.
- **Compatibility color**: The existing `color` value that keeps older clients
  and content without a color type usable.
- **Catalog-derived content**: A tag or exercise downloaded from the workout
  catalog and assigned a palette color type.

## Assumptions

- The 15 approved bare color names are the full palette for this feature. The
  same name is used by `colorType`, the design system, and the color asset;
  prefixes such as `icon_` are not part of this contract.
- The approved light and dark values from the preceding color-palette work are
  applied centrally to those assets rather than duplicated in catalog JSON.
- `colorType` is additive catalog metadata. Older clients ignore unknown
  metadata and continue to read the retained `color` field.
- Content not originating from the catalog may have only a stored color and is
  intentionally covered by the compatibility fallback.

## Out of Scope

- Adding `colorLight` or `colorDark` to the manifest or exercise records.
- Creating new color families beyond the approved palette.
- Changing user-selected custom colors, exercise content, identities,
  availability rules, workout calculations, or timer behavior.
- Retrofitting 1.1.2 or 1.1.3 to render appearance-specific assets.

## Success Criteria

- 100% of approved color types resolve to one asset with usable light and dark
  appearance values.
- 100% of catalog tags and exercises targeted for 1.1.4 have a valid approved
  `colorType` and retain their existing `color` compatibility value.
- In both appearances, 100% of catalog visual surfaces show the color resolved
  from the same record's valid `colorType`.
- A record lacking a valid `colorType` remains visible and usable in 100% of
  tested catalog views through the compatibility color.
- Supported 1.1.2 and 1.1.3 clients load the expanded catalog without a
  catalog-unavailable error caused by `colorType`.
