# Workout catalog

This repository is the source of truth for the workout exercise catalog used by Fighting AI.

## White-label catalog freeze

The legacy catalog for Boxing Timer `1.1.4` is frozen at
`fce1bac7becba9d73fe30ec5745e50f4a35a87fe` on `main`. The corresponding
release tag is `stop/1.1.4`.

Each exercise has its own directory inside exactly one top-level tag directory.
The folder contains the exercise JSON and its real SF Symbols-derived icon:

```text
Boxing/
  Icon.png
  jab/
    Icon.png
    jab.json
WarmUp/
  Icon.png
  jumpRope/
    Icon.png
    jumpRope.json
Cooldown/
  Icon.png
  breathing/
    Icon.png
    breathing.json
Other/
  Icon.png
  rest/
    Icon.png
    rest.json
```

Exercise folders and JSON filenames use the exercise title in lowerCamelCase;
the JSON `key` remains the stable catalog identifier.

Each JSON file is the structured exercise record downloaded by the app. `Icon.png` is downloaded with the record and stored in the local SwiftData catalog. `icon-registry.json` records which SF Symbol was used to create each icon.

The `main` branch is the source for any app version without a matching global stop tag. `manifest.json` lists every tag identity and every JSON/icon pair for legacy clients. The app first reads the selected revision SHA from GitHub. If that SHA matches the revision stored in its local database, no catalog files are downloaded. When the SHA is newer, the app downloads the complete manifest, tag icons, JSON files, and exercise icons from that exact commit and replaces the local catalog in one sync.

## Stop catalog updates for an app version

The app uses its own version, for example `1.1.4`:

- Tag `stop/1.1.4` exists: use the catalog at that tag.
- No matching tag: use `main`.
- That catalog is already stored: skip downloading its files.
- App updates to `1.1.5`: look for `stop/1.1.5` instead.

Publish the tag on the last compatible commit before changing the catalog:

```sh
git tag stop/1.1.4
git push origin refs/tags/stop/1.1.4
```

The match is exact: a tag for 1.1.4 does not stop 1.1.3 or 1.1.5.
Create a separate tag for each version you want to stop; they can point to
the same commit. Keep published tags and their data available and unchanged.
Both lightweight and annotated tags work. No tags are created automatically.

## White-label product catalogs

Box, Fitness, and Gym catalog manifests live at `box/manifest.json`,
`fitness/manifest.json`, and `gym/manifest.json` on `main`. Each product has
its own complete exercise folder:

```text
box/exercises/<Tag>/<exercise>/<exercise>.json
fitness/exercises/<Tag>/<exercise>/<exercise>.json
gym/exercises/<Tag>/<exercise>/<exercise>.json
```

Every manifest references only assets in its own product directory. The root
catalog remains only for older apps that still request `manifest.json`.

A single immutable `stop/<version>` tag freezes every white-label product for
that app version. The tag must point to a commit containing every product
manifest supported by the release. When no matching tag exists, every product
resolves the current `main` revision and then loads only its own manifest.

A fresh installation downloads the selected catalog once. Network errors do
not bypass a tag; only a missing tag allows `main`. Already released apps
without this support, including 1.1.3, continue reading `main` directly.

The supported JSON fields are:

```json
{
  "id": "1.1",
  "key": "jab",
  "title": "Jab",
  "description": "Exercise instructions",
  "target": "head",
  "color": "#ED5C63",
  "workoutType": "boxing",
  "valueType": "time",
  "difficulty": "basic",
  "durationSeconds": 180,
  "durationUnit": "seconds",
  "sequence": ["jab", "cross"],
  "availableFrom": "1.1.0",
  "availableUntil": null
}
```

Only the fields relevant to an exercise need to be present. `id`, `key`, `title`,
`description`, `color`, `workoutType`, `valueType`, `availableFrom`, and
`availableUntil` are required in the `main` catalog; `difficulty` is optional.
Exercise IDs use the numeric `tagId.exerciseId` format, such as `1.1`.
The icon path is authoritative in `manifest.json`, and the app derives its
technical identity from the explicit catalog `id` while keeping `key` as the
stable exercise identifier.


## App 1.1.7 schema

The current root catalog contains 1241 records. Product catalogs remain independent
subsets: Box 1002, Fitness 207, Gym 186, and Yoga 58. Product manifests use schema 3;
ready-made workouts use schema 2. Timed exercise values use `durationSeconds`, repeat
controls use `repeatCount`, and weighted sets use `initialValue` and `initialValue2`.
`default`, `default2`, `timerRole`, and legacy color hex fields are no longer emitted.
Use `state`, `colorType`, and the value types `time`, `stepper`, `countAndWeight`, `note`.
The app preserves saved workout definitions before its one-time 1.1.7 catalog reset.
Old active sessions are read with their original progress and saved with `durationSeconds`.
Published stop tags are retained unchanged; these working-tree changes require publication
before the app can download the new schema from GitHub.
