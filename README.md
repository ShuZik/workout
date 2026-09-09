# Workout catalog

This repository is the source of truth for the workout exercise catalog used by Fighting AI.

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

The `main` branch is the source for app version `1.1.3`. `manifest.json` lists every tag identity and every JSON/icon pair. The app first reads the latest commit SHA of that branch from GitHub. If that SHA matches the revision stored in its local database, no catalog files are downloaded. When the SHA is newer, the app downloads the complete manifest, tag icons, JSON files, and exercise icons from that exact commit and replaces the local catalog in one sync.

## Catalog compatibility boundaries

Keep authoring in `main`. Before the first incompatible change, publish a Git tag
`catalog-stop/<maximum-stopped-app-version>` on the **last compatible commit**.
For example, `catalog-stop/1.1.3` allows apps through 1.1.3 to download that
commit, but no later catalog. Apps above 1.1.3 continue following main until
a later applicable boundary. Versions are compared numerically (1.1.10 > 1.1.3).

In GitHub, create a tag with that name on the last compatible main commit and
publish it before pushing incompatible data. From a clean checkout of that commit:

```sh
git tag catalog-stop/1.1.3
git push origin refs/tags/catalog-stop/1.1.3
```

This is an example, not a boundary automatically installed by this change.
Both lightweight and annotated tags are supported. Ordinary release tags are ignored.
With boundaries 1.1.3 at A and 1.4.0 at B, apps through 1.1.3 use A,
apps above 1.1.3 through 1.4.0 use B, and later apps use main.
Compatible additions need no tag or new app release.

Boundaries must have strictly increasing numeric versions and follow main history.
Do not create numerically equivalent duplicate versions (for example 1.1 and 1.1.0),
move/delete published boundaries, or rewrite their history. Keep tagged data available.
The app reads all tag pages and verifies boundary ancestry before choosing a catalog;
invalid boundaries, API limits and network errors retain the existing local catalog.
A clean installation needs a successful compatible download. Catalog files are always
read from one selected commit, never mixed with a moving main.

**Rollout limitation:** this requires an app containing boundary support. Already
released 1.1.3 binaries request main directly and ignore these tags. Keep their
main catalog compatible while those clients remain supported; publishing a tag does
not make incompatible changes safe for them. Per-exercise `availableFrom` and
`availableUntil` do not replace whole-catalog compatibility boundaries.

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
