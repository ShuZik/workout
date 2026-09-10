# Catalog Audit: 2026-09-09

## Inventory

| Item | Result |
| --- | ---: |
| Tags in catalog index | 27 |
| Exercise records | 583 |
| Manifest file/icon pairs | 583 |
| Orphan exercise JSON files | 0 |
| Missing manifest JSON files | 0 |
| Duplicate exercise keys | 0 |
| Duplicate exercise IDs | 0 |
| Missing exercise icon files | 0 |
| Missing icon-registry entries | 0 |

## Current Order

The catalog index currently lists Custom, Cardio, Warm-up, Cool-down, then
combat, conditioning, mind-body, and utility categories. The client
uses the index order for non-Custom filters and explicitly pins Custom first.
Therefore, a tag reorder is sufficient for a safe linear presentation; tag IDs
are not the sort key.

## Safe Section Rules

The client accepts exercise section values in the form `N Title` or
`N Two Words`. It sorts sections by `N`, then keeps the catalog file order
inside each section. Existing exercise sections are the correct compatibility
mechanism for grouping exercises in 1.1.3.

The client has no concept of a cross-tag category header. Such headers cannot
be made visible in 1.1.3 through catalog data alone. A newer client may add
optional presentation metadata while 1.1.3 continues to use the same linear
order.

## Record Shape

| `valueType` | Records | Current behavior |
| --- | ---: | --- |
| `time` | 537 | Creates a timed row and timer segment. |
| `countAndWeight` | 42 | Creates a non-timer weight row. |
| `repeatCount` | 1 | Creates the Repeat stepper. |
| `none` | 3 | Creates a note-style non-timer row. |

Availability distribution: 44 records begin at 1.1.0, 469 at 1.1.2, 4 at
1.1.3, and 66 at 1.1.4.

## Field Use Review

| Field group | Current status |
| --- | --- |
| `id`, `key`, `title`, `workoutType`, `valueType`, `default`, `timerRole` | Active: identity, selection, row type, and timer construction. |
| `description`, `color`, icon asset, tag symbol | Active: information sheet and visual presentation. |
| `section` | Active: groups and orders exercises inside a selected tag. |
| `availableFrom`, `availableUntil` | Active for visibility after records are parsed. They do not protect older clients from unknown record shapes. |
| `energyProfile` | Active for completed-workout energy calculation. |
| `durationSeconds`, `durationUnit`, `repeatCount` | Used to persist and edit timed and repeat rows. |
| `subtitle` | Parsed, but catalog descriptions are mandatory and take precedence in the information sheet. |
| `target`, `difficulty`, `level` | Parsed and stored; no current catalog UI or workout behavior reads them. |
| `sequence` | Parsed and stored; no current catalog behavior reads it. |
| `default2` and count/weight initial values | Validated and stored, but not passed into the shortcut reference or rendered in the weight row. |

## Content and Contract Findings

- The Note record intentionally has a yellow exercise color rather than the
  Other tag's gray. This is a deliberate visual exception requested for 1.1.4.
- No other catalog structural mismatch was found in the checked files.

## Compatibility Constraints

- The catalog is fully downloaded and parsed before availability filtering.
- Existing legacy wire values therefore remain necessary while their client
  versions are supported.
- Reordering existing index entries and changing existing exercise section
  values preserve the current schema and are safe for 1.1.3 when all IDs,
  keys, paths, and supported value types remain unchanged.
