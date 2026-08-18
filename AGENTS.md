# Workout Catalog Rules

These rules apply to every exercise record in this repository.

## JSON-only authoring contract

The `1.1.1` branch is JSON-only. Never create or edit an exercise Markdown
file. Every exercise must be stored as `<tag>/<title>/<title>.json` beside its
real `<tag>/<title>/Icon.png`; the title path uses lowerCamelCase and the tag
icon remains `<tag>/Icon.png`.

Every new or edited exercise JSON record must contain these fields:

- `key`
- `title`
- `description`
- `color`
- `workoutType`
- `valueType`
- `section`
- `availableFrom: "1.1.0"`
- `availableUntil: null` unless the exercise has an explicit removal version

Timed exercises (`valueType: "time"`) must additionally contain `timerRole`.

Optional fields are `difficulty`, `level` for boxing, `subtitle`, `target`,
`durationSeconds`, `durationUnit`, and `sequence`.
Add only fields supported by the app schema. Do not add `id`, `symbol`, or
`icon` to an exercise JSON file. The app derives `id` from `key`, and icon
provenance belongs to `icon-registry.json` while the asset itself is
`Icon.png`.

`manifest.json` must use schema `version: 2` and its exercise entries must use
the `json` path field, never `markdown`. When adding an exercise, update the
manifest and validate the JSON, icon pair, tag, color, and registry together.

## Catalog structure

- A top-level folder is a workout tag: `Boxing`, `MuayThai`, `KickBoxing`, `MMA`, `UFC`, `BJJ`, `Wrestling`, `Taekwondo`, `WarmUp`, `Cooldown`, or `Other`.
- Every top-level tag folder contains its group `Icon.png`.
- Every exercise has its own folder inside exactly one tag folder.
- Every exercise folder must contain the matching JSON file and `Icon.png`.
- The JSON filename and folder name use the same lowerCamelCase form of the
  exercise title; single-word titles use lowercase.
- `manifest.json` must contain one tag record and one JSON/icon pair for every exercise folder.
- `icon-registry.json` records the exact SF Symbol used to create each `Icon.png`.
- `key` is the required stable catalog identifier and is independent of the
  title-based folder and filename. Do not add an `id` field to JSON; the app
  derives its technical `Identifiable` id from `key`.

## JSON file format

Each exercise file is a single JSON object. Preserve all existing structured
fields required by the app. The `difficulty` field is optional, but when it is
present it must use one of these machine-readable values:

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
- `1 Main` is reserved for the single tag-named starter entry shown first in
  its tag; all other sections start at `1`.

Store the section value only in the JSON `section` field. Do not add a second
grouping scheme or duplicate the value in a text body.

### Availability by app version

`availableFrom` and `availableUntil` are inclusive bounds using a numeric
dot-separated app version such as `1.1.1`. In the `1.1.1` branch every current
exercise uses `availableFrom: "1.1.0"` and `availableUntil: null`. A `null`
upper bound means no end. An exercise is shown in the new-exercise picker only
when the current app version is within these bounds. Keep the record in the
catalog after it becomes unavailable so saved workouts that reference its
stable `key` continue to resolve. Do not use the catalog manifest `version`
field for this; that field is the catalog schema version.

### Boxing levels

The `level` field is required for every `workoutType: boxing` exercise and must be an integer from `0` to `5`:

- `0` — the single `Boxing` entry (`key: free_boxing`) shown in the untitled first section.
- `1` — `База`: fundamental punches and the basic jab-cross.
- `2` — `Новичок`: simple body punches and overhands.
- `3` — `Уверенный`: short combinations, defense, and simple angle work.
- `4` — `Продвинутый`: multi-phase combinations, level changes, defense counters, or footwork.
- `5` — `Профи`: unusually technical work such as the bolo punch.

Non-boxing records omit `level`. The app displays the JSON `level` value
directly.

### Named combat-style tags

Each named combat style is its own top-level tag and currently contains exactly
one timed starter record. The tag and `workoutType` pairs are:

- `MuayThai` / `muayThai` — `Muay Thai`
- `KickBoxing` / `kickBoxing` — `Kick Boxing`
- `MMA` / `mma` — `MMA`
- `UFC` / `ufc` — `UFC`
- `BJJ` / `bjj` — `BJJ`
- `Wrestling` / `wrestling` — `Wrestling`
- `Taekwondo` / `taekwondo` — `Taekwondo`

Use `valueType: time` and `section: 1 Main` for these starter records. Omit
`level` and do not add technique-specific fields.

### Visual identity and SF Symbols

The catalog has two visual layers. Keep them separate and keep both layers
stable:

1. Every top-level tag/group (`Boxing`, `MuayThai`, `KickBoxing`, `MMA`, `UFC`, `BJJ`, `Wrestling`,
   `Taekwondo`, `WarmUp`, `Cooldown`, and `Other`) must have a stable group
   icon and its own group color in the app. Combat groups intentionally share
   the exact SF Symbol when they belong to the same visual family: striking
   groups use `figure.boxing`, while grappling groups use `figure.wrestling`.
   The group identity is defined in the `tags` array of `manifest.json` and
   represented by the matching `WorkoutType`. Its root `Icon.png`, title, SF
   Symbol name, and hex color are downloaded together with the exercise
   catalog. Do not create a fake `MartialArts` container or add an unapproved
   JSON field.
2. Every exercise must have its own exercise icon. The required `Icon.png` is
   the asset downloaded into the catalog; `icon-registry.json` is the only
   source of its SF Symbol provenance. Do not add a `symbol` or `icon` field
   to JSON and do not copy one placeholder icon into unrelated exercises.
   Shared group icons are allowed only for the documented visual families. The
   app may keep an internal fallback symbol for bundled presets, but that
   fallback is not catalog metadata.
3. Select symbols only from Apple SF Symbols using the SF Symbols app:
   [SF Symbols](plugin://computer-use@openai-bundled?app=com.apple.SFSymbols).
   Search the app, choose an existing symbol that describes the group or
   exercise, and use its exact name. Never invent a symbol name, use emoji, or
   generate a replacement image when a suitable SF Symbol exists.
4. Export or render the selected symbol as `Icon.png` with a transparent
   background and keep the group asset in its tag folder or the exercise asset
   in its exercise folder. `manifest.json` and the app catalog parser must
   refer to that same downloaded asset; the JSON file does not repeat the
   asset path.
5. The JSON `color` field is a required six-digit hex color in the exact
   format `#RRGGBB` (for example `#ED5C63`). Never write symbolic tokens such
   as `boxing`, `warmUp`, `round`, or `coolDown`, localized names, or arbitrary
   color formats. Use only the exact sRGB hex values from the app's
   `AppColor.IconPicker` palette listed below. If a requested color is not in
   that palette, choose the nearest palette color by visual color family and
   record that palette choice; when candidates are close, preserve the
   original hue family (for example, orange maps to `icon_orange`, not
   yellow). Do not invent a new hex value. Every exercise in a tag must use
   the exact same color as its tag in `manifest.json`.
   The app parser must validate the hex format and render the same value.
6. Before adding or editing a record, check `icon-registry.json` and the
   existing catalog. Reject unapproved group-color assignments, duplicate
   exercise symbols, missing `Icon.png`, and mismatches between the registry
   and the exported image. Shared group colors are valid only when they use
   the documented palette mapping. Allow a duplicate group symbol only when
   it matches the documented visual-family mapping. If no suitable SF Symbol
   exists, stop and ask instead of reusing a placeholder.

The current group-color registry is:

| Top-level tag | SF Symbol | Palette | Required sRGB hex |
| --- | --- | --- | --- |
| `Boxing` | `figure.boxing` | `icon_red` | `#ED5C63` |
| `MuayThai` | `figure.boxing` | `icon_orange` | `#F3A044` |
| `KickBoxing` | `figure.boxing` | `icon_red` | `#ED5C63` |
| `MMA` | `figure.wrestling` | `icon_purple` | `#7D4DB3` |
| `UFC` | `figure.wrestling` | `icon_indigo` | `#4766C1` |
| `BJJ` | `figure.boxing` | `icon_indigo` | `#4766C1` |
| `Wrestling` | `figure.wrestling` | `icon_teal` | `#01C5A5` |
| `Taekwondo` | `figure.boxing` | `icon_green` | `#2BBF51` |
| `WarmUp` | `figure.jumprope` | `icon_blue` | `#0A84FF` |
| `Cooldown` | `figure.flexibility` | `icon_blue` | `#0A84FF` |
| `Other` | `list.bullet` | `icon_gray` | `#808A94` |

Shared colors are allowed when the same palette color is the nearest visual
match. The registry above is the required source of truth. Do not change a group
color or the palette mapping without updating this registry and the app's
group presentation together.

The app's `IconPicker` palette is:

| Palette name | Required sRGB hex |
| --- | --- |
| `icon_red` | `#ED5C63` |
| `icon_coral` | `#FD7C5D` |
| `icon_orange` | `#F3A044` |
| `icon_yellow` | `#E6BF00` |
| `icon_green` | `#2BBF51` |
| `icon_teal` | `#01C5A5` |
| `icon_cyan` | `#67C4ED` |
| `icon_blue` | `#0A84FF` |
| `icon_indigo` | `#4766C1` |
| `icon_purple` | `#7D4DB3` |
| `icon_violet` | `#AF6ED7` |
| `icon_pink` | `#EA7CC7` |
| `icon_gray` | `#808A94` |
| `icon_sage` | `#90A994` |
| `icon_tan` | `#B79E80` |

### Allowed `valueType` values

`valueType` is a required closed enum for exercises. Use only these machine-readable values:

- `time` — the exercise is measured by time. Use this for punches, combinations, warm-ups, cooldowns, and other timed activities.
- `countAndWeight` — the exercise needs both a count and a weight value. Use it only when this capability is explicitly supported by the app schema.
- `none` — the exercise has no measured value or action control.

Do not create other values such as `rounds`, `reps`, `duration`, `weight`, `distance`, or localized variants. If the exercise does not clearly require one of these types, stop and ask before changing the schema.

### Timer role

`timerRole` is a closed enum used only by the timer presentation. It is required
for every `valueType: "time"` record and must be one of:

- `active` — a regular active exercise;
- `intense` — an explicitly designated high-intensity exercise;
- `rest` — a recovery interval.

Every non-timed record (`valueType: "none"` or `"countAndWeight"`) must omit
`timerRole`. Do not infer a timer role from the title, tag, difficulty, color,
or duration. For the current catalog, `Other/rest/rest.json` is `rest`; all
other timed records are `active` unless an explicit catalog decision changes
them.

The JSON record must follow this structure:

```json
{
  "key": "jab",
  "title": "Jab",
  "description": "Short exercise description.",
  "color": "#ED5C63",
  "workoutType": "boxing",
  "valueType": "time",
  "timerRole": "active",
  "difficulty": "basic",
  "level": 1,
  "section": "1 Base",
  "availableFrom": "1.1.0",
  "availableUntil": null
}
```

- Optional fields such as `subtitle`, `target`, `durationSeconds`,
  `durationUnit`, and `sequence` are included only when the
  exercise needs them.
- Do not invent random fields, colors, icons, or workout tags. Reuse the existing catalog conventions.

## Creating or editing an exercise

Before creating or editing a record:

1. Inspect the target tag folder, `icon-registry.json`, and an existing matching record.
2. Decide the correct difficulty and, for boxing records, the correct level from the exercise complexity using the definitions above.
3. Keep the JSON file, exercise `Icon.png`, tag `Icon.png`, `manifest.json`,
   `icon-registry.json`, and `timerRole` contract in sync.
4. Validate that every tag folder contains the required JSON/icon pair and
   every JSON file follows the shared schema.
5. Make only the requested catalog change; do not perform unrelated cleanup or app changes.
