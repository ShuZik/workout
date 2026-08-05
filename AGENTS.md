# Workout Catalog Rules

These rules apply to every exercise record in this repository.

## Catalog structure

- A top-level folder is a workout tag: `Boxing`, `WarmUp`, `Cooldown`, or `Other`.
- Every exercise has its own folder inside exactly one tag folder.
- Every exercise folder must contain the matching Markdown file and `Icon.png`.
- The Markdown filename and folder name use the same `snake_case` slug.
- `manifest.json` must contain one Markdown/icon pair for every exercise folder.

## Markdown file format

Keep the YAML front matter first. Preserve all existing structured fields required by the app. The `difficulty` field is optional, but when it is present it must use one of these machine-readable values:

- `basic` — `База`
- `intermediate` — `Средний`
- `advanced` — `Продвинутый`
- `pro` — `Профи`

### Named sections

The `section` field is required for every exercise. It is an ordered label in
the format `N Name` or `N Two Word Name`:

- `N` is a non-negative integer used only for sorting;
- the label is one or two words shown as the section title in the app;
- the app removes the numeric prefix before displaying the title;
- `0 Boxing` is reserved for the single `free_boxing` entry shown in the
  untitled first section; all other sections start at `1`.

Use the same section value in the Markdown body under `## Секция`. Do not add
extra section fields or invent a second grouping scheme.

### Boxing levels

The `level` field is required for every `workoutType: boxing` exercise and must be an integer from `0` to `5`:

- `0` — the single `Boxing` entry (`key: free_boxing`) shown in the untitled first section.
- `1` — `База`: fundamental punches and the basic jab-cross.
- `2` — `Новичок`: simple body punches and overhands.
- `3` — `Уверенный`: short combinations, defense, and simple angle work.
- `4` — `Продвинутый`: multi-phase combinations, level changes, defense counters, or footwork.
- `5` — `Профи`: unusually technical work such as the bolo punch.

Non-boxing records omit `level`. The displayed `## Уровень` value must match the front matter `level`.

### Allowed `valueType` values

`valueType` is a required closed enum for exercises. Use only these machine-readable values:

- `time` — the exercise is measured by time. Use this for punches, combinations, warm-ups, cooldowns, and other timed activities.
- `countAndWeight` — the exercise needs both a count and a weight value. Use it only when this capability is explicitly supported by the app schema.
- `none` — the exercise has no measured value or action control.

Do not create other values such as `rounds`, `reps`, `duration`, `weight`, `distance`, or localized variants. If the exercise does not clearly require one of these types, stop and ask before changing the schema.

The Markdown body must use this order and these headings:

```markdown
---
id: exercise_slug
key: exercise_slug
title: Exercise Title
description: Short exercise description.
color: boxing
icon: Icon.png
symbol: figure.boxing
workoutType: boxing
valueType: time
difficulty: basic
level: 1
section: 1 Base
---

## Title

Exercise Title

## Сложность

База

## Уровень

1

## Секция

1 Base

## Описание

Short exercise description.
```

- `## Title`, `## Сложность`, `## Секция`, and `## Описание` are required and must appear in this order. Boxing records keep `## Уровень` between `## Сложность` and `## Секция`.
- The title in the body must match front matter `title`.
- The displayed difficulty must match front matter `difficulty` when that field is present.
- The displayed section must match front matter `section`.
- The description in the body must match front matter `description`.
- Do not keep a duplicate `# Title` heading.
- Keep one blank line between headings and their values and between sections.
- Do not invent random fields, colors, icons, or workout tags. Reuse the existing catalog conventions.

## Creating or editing an exercise

Before creating or editing a record:

1. Inspect the target tag folder and an existing matching record.
2. Decide the correct difficulty and, for boxing records, the correct level from the exercise complexity using the definitions above.
3. Keep the Markdown file, `Icon.png`, front matter, body sections, and `manifest.json` in sync.
4. Validate that every tag folder contains the required Markdown/icon pair and every Markdown file follows the shared section order.
5. Make only the requested catalog change; do not perform unrelated cleanup or app changes.
