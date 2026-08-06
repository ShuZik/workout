# Workout catalog

This repository is the source of truth for the workout exercise catalog used by Fighting AI.

Each exercise has its own directory inside exactly one top-level tag directory.
The folder contains the exercise Markdown and its real SF Symbols-derived icon:

```text
Boxing/
  Icon.png
  jab/
    Icon.png
    jab.md
WarmUp/
  Icon.png
  jump_rope/
    Icon.png
    jump_rope.md
Cooldown/
  Icon.png
  box_breathing/
    Icon.png
    box_breathing.md
Other/
  Icon.png
  pause/
    Icon.png
    pause.md
```

The Markdown front matter is the structured record downloaded by the app. `Icon.png` is downloaded with the record and stored in the local SwiftData catalog. `icon-registry.json` records which SF Symbol was used to create each icon.

`manifest.json` lists every tag identity and every Markdown/icon pair. The app first reads the latest commit SHA of `main` from GitHub. If that SHA matches the revision stored in its local database, no catalog files are downloaded. When the SHA is newer, the app downloads the complete manifest, tag icons, Markdown files, and exercise icons from that exact commit and replaces the local catalog in one sync.

The supported front matter fields are:

```yaml
---
key: jab
title: Jab
description: Exercise instructions
target: head
color: "#E63946"
workoutType: boxing
valueType: time
difficulty: basic
durationSeconds: 180
durationUnit: seconds
actions: ["number", "choose"]
actionPlacement: trailing
sequence: ["jab", "cross"]
---
```

Only the fields relevant to an exercise need to be present. `key`, `title`,
`description`, `color`, `workoutType`, and `valueType` are required;
`difficulty` is optional. The icon path is authoritative in `manifest.json`,
and the app derives its technical `id` from `key`.
