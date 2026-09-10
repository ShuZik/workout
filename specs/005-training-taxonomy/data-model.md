# Catalog Data Model: Training Taxonomy Reset

## Tag

- `id`, root icon path and `workoutType` are stable wire identities.
- `title` is visible and changes from `Core Training` to `Core`, and from
  `Cool-down` to `Rest`.
- `color` remains legacy compatibility data; `colorType` is the semantic
  appearance type. The Rest tag uses `colorType: "green"` while retaining its
  legacy blue `color`.

## Exercise

- `key` is the stable saved-workout identity and never changes.
- A moved record adopts its destination tag's `workoutType`, `color`,
  `colorType` and tag-prefixed `id`.
- `energyProfile`, `default`, `valueType`, description, availability and icon
  stay unchanged unless the Rest tag's title or grouping requires metadata
  only.
- `section` is the display grouping and determines the order within Rest.

## Relationships

- Every `manifest.json.files` pair references one exercise JSON and its icon.
- Every manifest exercise must have one matching icon-registry file entry
  keyed by its unchanged exercise `key`.
- Each remaining exercise belongs to exactly one manifest tag.
