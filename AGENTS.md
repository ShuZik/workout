# Workout Catalog Rules

These rules apply to every exercise record in this repository.

## Catalog structure

- A top-level folder is a workout tag: `Boxing`, `MuayThai`, `KickBoxing`, `MMA`, `UFC`, `BJJ`, `Wrestling`, `Taekwondo`, `WarmUp`, `Cooldown`, or `Other`.
- Every top-level tag folder contains its group `Icon.png`.
- Every exercise has its own folder inside exactly one tag folder.
- Every exercise folder must contain the matching JSON file and `Icon.png`.
- The JSON filename and folder name use the same `snake_case` slug.
- `manifest.json` must contain one tag record and one JSON/icon pair for every exercise folder.
- `icon-registry.json` records the exact SF Symbol used to create each `Icon.png`.
- `key` is the required stable catalog identifier and must match the exercise
  slug. Do not add an `id` field to JSON; the app derives its technical
  `Identifiable` id from `key`.

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
- `0 Boxing` is reserved for the single `free_boxing` entry shown in the
  untitled first section; all other sections start at `1`.

Store the section value only in the JSON `section` field. Do not add a second
grouping scheme or duplicate the value in a text body.

### Availability by app version

`availableFrom` and `availableUntil` are inclusive bounds using a numeric
dot-separated app version such as `1.1.1`. In the `1.1.1` branch every current
exercise uses `availableFrom: "1.1.1"` and `availableUntil: null`. A `null`
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

Use `valueType: time` and `section: 1 Styles` for these starter records. Omit
`level` and do not add `repeatCount`, `countAndWeight`, or technique-specific
fields.

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
   format `#RRGGBB` (for example `#E63946`). Never write symbolic tokens such
   as `boxing`, `warmUp`, `round`, or `coolDown`, localized names, or arbitrary
   color formats. A group color must not be silently reused for another group.
   The app parser must validate the hex format and render the same value.
6. Before adding or editing a record, check `icon-registry.json` and the
   existing catalog. Reject duplicate group colors, duplicate exercise
   symbols, missing `Icon.png`, and mismatches between the registry and the
   exported image. Allow a duplicate group symbol only when it matches the
   documented visual-family mapping. If no suitable SF Symbol exists, stop and
   ask instead of reusing a placeholder.

The current group-color registry is:

| Top-level tag | SF Symbol | Required `color` |
| --- | --- | --- |
| `Boxing` | `figure.boxing` | `#E63946` |
| `MuayThai` | `figure.boxing` | `#F77F00` |
| `KickBoxing` | `figure.boxing` | `#D62828` |
| `MMA` | `figure.wrestling` | `#6A4C93` |
| `UFC` | `figure.wrestling` | `#1D3557` |
| `BJJ` | `figure.boxing` | `#457B9D` |
| `Wrestling` | `figure.wrestling` | `#2A9D8F` |
| `Taekwondo` | `figure.boxing` | `#2E7D32` |
| `WarmUp` | `figure.jumprope` | `#0057FF` |
| `Cooldown` | `figure.flexibility` | `#3A86FF` |
| `Other` | `list.bullet` | `#6C757D` |

Do not change an existing group color or assign the same hex to another group
without updating this registry and the app's group presentation together.

### Allowed `valueType` values

`valueType` is a required closed enum for exercises. Use only these machine-readable values:

- `time` — the exercise is measured by time. Use this for punches, combinations, warm-ups, cooldowns, and other timed activities.
- `countAndWeight` — the exercise needs both a count and a weight value. Use it only when this capability is explicitly supported by the app schema.
- `none` — the exercise has no measured value or action control.

Do not create other values such as `rounds`, `reps`, `duration`, `weight`, `distance`, or localized variants. If the exercise does not clearly require one of these types, stop and ask before changing the schema.

The JSON record must follow this structure:

```json
{
  "key": "jab",
  "title": "Jab",
  "description": "Short exercise description.",
  "color": "#E63946",
  "workoutType": "boxing",
  "valueType": "time",
  "difficulty": "basic",
  "level": 1,
  "section": "1 Base",
  "availableFrom": "1.1.1",
  "availableUntil": null
}
```

- Optional fields such as `subtitle`, `target`, `durationSeconds`,
  `durationUnit`, `actions`, and `sequence` are included only when the
  exercise needs them.
- Do not invent random fields, colors, icons, or workout tags. Reuse the existing catalog conventions.

## Creating or editing an exercise

Before creating or editing a record:

1. Inspect the target tag folder, `icon-registry.json`, and an existing matching record.
2. Decide the correct difficulty and, for boxing records, the correct level from the exercise complexity using the definitions above.
3. Keep the JSON file, exercise `Icon.png`, tag `Icon.png`, `manifest.json`,
   and `icon-registry.json` in sync.
4. Validate that every tag folder contains the required JSON/icon pair and
   every JSON file follows the shared schema.
5. Make only the requested catalog change; do not perform unrelated cleanup or app changes.
