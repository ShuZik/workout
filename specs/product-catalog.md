# Product catalog manifest v1

The legacy root `manifest.json` remains the compatibility catalog for older
clients. `box/manifest.json`, `fitness/manifest.json`, and `gym/manifest.json`
are the catalogs for the three 1.1.5 products. Each product owns local copies
of every JSON and PNG asset it exposes under `<product>/exercises/`.

## Manifest shape

```json
{
  "schemaVersion": 1,
  "productID": "boxing",
  "minimumAppVersion": "1.1.5",
  "tags": [],
  "files": [],
  "workouts": []
}
```

Required fields:

- `schemaVersion`: integer `1`.
- `productID`: exactly `boxing`, `fitness`, or `gym`, matching its directory.
- `minimumAppVersion`: the earliest app version that understands this schema.
- `tags`: explicit copies of allowed tag metadata. Each icon is at
  `<product>/exercises/<Tag>/Icon.png`.
- `files`: explicit `{ "json": "<product>/exercises/<Tag>/<exercise>/<exercise>.json",
  "icon": "<product>/exercises/<Tag>/<exercise>/Icon.png" }` pairs.
- `workouts`: explicit relative paths to product workout definitions.

The tags and files arrays are allowlists. An app must reject a manifest when a
path escapes the repository, an icon or JSON file is missing, an exercise ID
is duplicated, a file belongs to a tag outside `tags`, or a workout reference
does not resolve to an allowlisted exercise.

## Validation coverage

Every product manifest must be checked before a product release tag is made.

| Check | Expected result |
| --- | --- |
| Product directory, `productID`, and schema version agree | Reject the manifest on any mismatch. |
| Every tag, JSON, icon, and workout path exists at the selected revision | Reject the whole update; do not partially replace cache. |
| Every file belongs to an allowlisted tag and every stable exercise ID is unique | Reject the manifest. |
| Every workout has the same `productID` as its manifest | Reject the manifest. |
| Every workout segment references an allowlisted exercise ID | Reject the workout and manifest. |
| Timed segments have a positive `durationSeconds`; manual segments have positive `reps` and non-negative `weight` | Reject the workout and manifest. |
| Gym CrossTraining references are a strict subset of the source folder | Confirm excluded conditioning-only entries remain absent. |
| A failed update | Preserve only the last valid cache for the same product. |

## Product ownership and exercise rules

- Product manifests may reference only assets below their own
  `<product>/exercises/` directory. A manifest must never read another
  product's assets or the legacy root catalog.
- Local copies keep the original JSON `id`, `key`, record contents, and icon
  bytes so saved workouts remain stable across catalog updates.
- The root catalog is not removed while older apps can still read it. New
  product clients never use it.
- `WarmUp`, `Cooldown`, `Other`, and `Custom` are common structural tags when
  included explicitly. `Custom` has local app persistence; its root tag does
  not make another product's user-created records visible.
- Box uses every combat discipline: `Boxing`, `KickBoxing`, `MuayThai`, `MMA`,
  `BJJ`, `Karate`, `Taekwondo`, `Judo`, and combat-specific warm-ups.
- Fitness uses `HIIT`, `Tabata`, `CrossTraining`, `Cardio`, `CoreTraining`,
  `SuspensionTraining`, `Breathwork`, and `Meditation`, plus common structural
  tags.
- Gym uses `Strength` plus an exercise-level selection of weight-bearing
  CrossTraining/CoreTraining records and common structural tags. It must not
  import all of `CrossTraining`, `HIIT`, or `Tabata`.

## Workout definitions

Product workout definitions live at `<product>/workouts/<workout>.json`. They
must declare `schemaVersion`, `id`, `productID`, `title`, `description`, and
an ordered `segments` array. Each segment references one allowlisted stable
exercise ID and declares either a timed value or a manual count-and-weight
value. Manual segments are supported by the Gym product only after the app
runtime supports them.

## Revision and release rules

The client resolves a catalog revision before it downloads a product manifest
or any referenced path. All files for one sync use that immutable revision.
Legacy `stop/<version>` tags remain immutable. New product stop tags use
`stop/<product>/<version>` and are independent of the moving `main` branch.
