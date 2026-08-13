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
  jump_rope/
    Icon.png
    jump_rope.json
Cooldown/
  Icon.png
  breathing/
    Icon.png
    breathing.json
Other/
  Icon.png
  pause/
    Icon.png
    pause.json
```

Each JSON file is the structured exercise record downloaded by the app. `Icon.png` is downloaded with the record and stored in the local SwiftData catalog. `icon-registry.json` records which SF Symbol was used to create each icon.

The `1.1.1` branch is the source for app version `1.1.1`. `manifest.json` lists every tag identity and every JSON/icon pair. The app first reads the latest commit SHA of that branch from GitHub. If that SHA matches the revision stored in its local database, no catalog files are downloaded. When the SHA is newer, the app downloads the complete manifest, tag icons, JSON files, and exercise icons from that exact commit and replaces the local catalog in one sync.

The supported JSON fields are:

```json
{
  "key": "jab",
  "title": "Jab",
  "description": "Exercise instructions",
  "target": "head",
  "color": "#E63946",
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

Only the fields relevant to an exercise need to be present. `key`, `title`,
`description`, `color`, `workoutType`, `valueType`, `availableFrom`, and
`availableUntil` are required in the `1.1.1` branch; `difficulty` is optional.
The icon path is authoritative in `manifest.json`, and the app derives its
technical `id` from `key`.
