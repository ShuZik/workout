# Workout catalog

This repository is the source of truth for the workout exercise catalog used by Fighting AI.

Each exercise has its own directory inside one of the four tag directories:

```text
Boxing/
  jab/
    Icon.png
    jab.md
WarmUp/
  jump_rope/
    Icon.png
    jump_rope.md
Cooldown/
  box_breathing/
    Icon.png
    box_breathing.md
Other/
  pause/
    Icon.png
    pause.md
```

The Markdown front matter is the structured record downloaded by the app. `Icon.png` is downloaded with the record and stored in the local SwiftData catalog.

`manifest.json` lists every Markdown/icon pair. The app first reads the latest commit SHA of `main` from GitHub. If that SHA matches the revision stored in its local database, no catalog files are downloaded. When the SHA is newer, the app downloads the complete manifest, Markdown files, and icons from that exact commit and replaces the local catalog in one sync.

The supported front matter fields are:

```yaml
---
id: jab
key: jab
title: Jab
description: Exercise instructions
target: head
color: boxing
icon: Icon.png
symbol: figure.boxing
workoutType: boxing
valueType: time
durationSeconds: 180
durationUnit: seconds
repeatCount: 1
actions: ["number", "choose"]
actionPlacement: trailing
sequence: ["jab", "cross"]
---
```

Only the fields relevant to an exercise need to be present. `id`, `key`, `title`, `description`, `color`, `icon`, `workoutType`, and `valueType` are required.
