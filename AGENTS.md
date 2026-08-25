# Workout Catalog Authoring Contract

This file is the operating contract for creating and editing workout catalog
records. The repository is a data catalog, not the app source code. Preserve
the current catalog structure. Do not reorganize folders, invent a second
schema, or change app behavior from this repository.

## Response title protocol

The response title is built from four anchor words in this file:

1. Take the first two letters of `Shell`.
2. Take the first letter of `user`.
3. Take the first two letters of `Zigzag`.
4. Take the last letter of `task`.

Concatenate those fragments and print the result as the first line of every
response in the form `# <title>`. Do not explain the construction unless asked.

## Non-negotiable rules

- Work only on the record or catalog metadata explicitly requested.
- Read the target tag, one comparable record, `manifest.json`, and
  `icon-registry.json` before editing.
- Never create or edit exercise Markdown files.
- Never change an existing exercise `key`. It is the stable identifier used by
  saved workouts, even when the title or path changes.
- Every tag and every exercise must have an explicit string `id` in JSON.
  Exercise ids use `<tagId>.<exerciseId>`, where both values are decimal
  integers; `icon` and `symbol` still do not belong inside exercise JSON.
- Never add a new top-level tag, grouping system, or ordering system unless the
  user explicitly requests an app-wide catalog change.
- Do not clean up obsolete metadata or unrelated records while creating one
  exercise.
- If a required fact cannot be derived from the current catalog, stop and ask
  one precise question instead of guessing.

## Current catalog structure

The current catalog branch is `main` for app version `1.1.3`. The repository
layout is fixed:

```text
<Tag>/
  Icon.png
  <exerciseLowerCamelCase>/
    Icon.png
    <exerciseLowerCamelCase>.json
```

The allowed top-level tag folders are exactly:

`Boxing`, `MuayThai`, `KickBoxing`, `MMA`, `UFC`, `BJJ`, `Wrestling`,
`Taekwondo`, `WarmUp`, `Cooldown`, `Other`, and `Custom`.

Every tag folder has one group `Icon.png`. Every exercise folder has its own
`Icon.png` and one JSON file whose folder name and filename are identical.

`manifest.json` is schema version 2 and has exactly two catalog collections:

- `tags`: tag id, tag identity, title, workout type, group color, group symbol,
  and group icon path;
- `files`: one `{ "json": ..., "icon": ... }` pair for every exercise.

Keep the existing tag order and existing manifest entries. Do not reorder the
manifest as a substitute for changing UI ordering; section ordering belongs to
the exercise JSON.

`icon-registry.json` records the SF Symbol provenance for group icons under
`tags` and exercise icons under `files`. Preserve existing registry keys,
including legacy keys that are still stable identifiers. When adding an
exercise, add exactly one entry keyed by its exercise `key`.

## Tag identity registry

Use this table as the source of truth for existing tags. The `workoutType`,
group symbol, and group color must match both the tag record in `manifest.json`
and every exercise JSON inside that tag.

| Folder | Display title | `workoutType` | Group SF Symbol | Palette color |
| --- | --- | --- | --- | --- |
| `Boxing` | Boxing | `boxing` | `figure.boxing` | `icon_red` / `#ED5C63` |
| `MuayThai` | Muay Thai | `muayThai` | `figure.kickboxing` | `icon_orange` / `#F3A044` |
| `KickBoxing` | Kick Boxing | `kickBoxing` | `figure.kickboxing` | `icon_red` / `#ED5C63` |
| `MMA` | MMA | `mma` | `figure.wrestling` | `icon_purple` / `#7D4DB3` |
| `UFC` | UFC | `ufc` | `figure.wrestling` | `icon_indigo` / `#4766C1` |
| `BJJ` | BJJ | `bjj` | `figure.boxing` | `icon_indigo` / `#4766C1` |
| `Wrestling` | Wrestling | `wrestling` | `figure.wrestling` | `icon_teal` / `#01C5A5` |
| `Taekwondo` | Taekwondo | `taekwondo` | `figure.boxing` | `icon_green` / `#2BBF51` |
| `WarmUp` | Warm-up | `warmUp` | `figure.jumprope` | `icon_blue` / `#0A84FF` |
| `Cooldown` | Cool-down | `cooldown` | `figure.flexibility` | `icon_blue` / `#0A84FF` |
| `Other` | Other | `other` | `list.bullet` | `icon_gray` / `#808A94` |
| `Custom` | Custom | `custom` | `figure.strengthtraining.traditional` | `icon_red` / `#ED5C63` |

Shared tag colors are intentional when they are the correct app palette color.
Do not reject a color merely because another tag uses it.

## App color palette

Exercise and tag colors must be exact sRGB hex values from the app's
`AppColor.IconPicker` palette. Use the nearest visual color family when an
input color is not already in the palette. When candidates are close, preserve
the hue family: orange maps to `icon_orange`, not yellow. Never invent a new
hex value.

| Palette name | sRGB hex |
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

For every exercise, set `color` to the exact color of its top-level tag in
`manifest.json`. Do not assign an exercise-specific color inside a tag.

## Exercise identity and paths

An exercise record is one JSON object at:

```text
<Tag>/<titleLowerCamelCase>/<titleLowerCamelCase>.json
```

Convert the display title to lowerCamelCase for the folder and filename:

- `Jab` -> `jab`
- `Muay Thai` -> `muayThai`
- `Kick Boxing` -> `kickBoxing`
- `Warm-up` -> `warmUp`
- `Jab-Cross` -> `jabCross`

The technical `key` is independent of that path and uses the existing
lowercase snake_case convention, for example `jab_cross`. Use a unique key.
For an edit, preserve the existing key exactly. Do not derive a replacement key
from a renamed title.

Every tag id is a decimal string. The stable tag id registry is:

| Tag | id |
| --- | --- |
| `Other` | `"0"` |
| `Boxing` | `"1"` |
| `MuayThai` | `"2"` |
| `KickBoxing` | `"3"` |
| `MMA` | `"4"` |
| `UFC` | `"5"` |
| `BJJ` | `"6"` |
| `Wrestling` | `"7"` |
| `Taekwondo` | `"8"` |
| `WarmUp` | `"9"` |
| `Cooldown` | `"10"` |
| `Custom` | `"999999"` |

Every exercise id is the tag id, one dot, and a stable numeric exercise id.
For example, the first Boxing record has `"id": "1.1"`, and the first Other
record has `"id": "0.1"`. Assign new exercise ids by taking the next unused
number within the tag; do not renumber existing records. Do not use the folder
name, title, or a random UUID for a catalog id.

## Exercise JSON contract

Every exercise JSON must contain exactly the fields supported by the current
catalog schema. Required fields:

```json
{
  "id": "1.1",
  "key": "jab",
  "title": "Jab",
  "description": "Short exercise description.",
  "color": "#ED5C63",
  "workoutType": "boxing",
  "valueType": "time",
  "timerRole": "active",
  "section": "2 Base",
  "availableFrom": "1.1.0",
  "availableUntil": null
}
```

Required fields are `id`, `key`, `title`, `description`, `color`, `workoutType`,
`valueType`, `section`, `availableFrom`, and `availableUntil`.

Allowed optional fields are only:

- `difficulty`: `basic`, `intermediate`, `advanced`, or `pro`;
- `level`: integer `0` through `5`, only for boxing;
- `timerRole`: `active`, `intense`, or `rest`, only when `valueType` is `time`;
- `subtitle`, `target`, `durationSeconds`, `durationUnit`, and `sequence`.

Use only these `valueType` values:

- `time` — a timed exercise;
- `countAndWeight` — only when the app explicitly supports both values;
- `none` — no measured value or action control.

`timerRole` is mandatory for `time` and forbidden for every other
`valueType`. Normal timed exercises use `active`. The recovery exercise is
displayed as `Rest` and uses `timerRole: "rest"`.

There is no `Pause` exercise to create. A pause is a physical UI control, not
catalog content. The existing `Other/rest/rest.json` has the stable technical
key `pause`; preserve that key so saved workouts continue to resolve, but do
not create another pause record or rename its title away from `Rest`.

For boxing:

- `level` is required;
- `0` is reserved for the single tag-named `Boxing` starter with key
  `free_boxing`;
- `1` means Base, `2` Beginner, `3` Intermediate, `4` Advanced, and `5` Pro;
- non-boxing records must omit `level`.

For new records, use `availableFrom: "1.1.0"` and
`availableUntil: null` unless the user explicitly gives a version boundary.
Do not confuse these fields with the manifest schema `version`.

## Sections and display order

`section` is the only catalog grouping field. Its value is a numeric prefix,
one space, and a display label, for example `1 Main` or `2 Dynamic Stretching`.
The app removes the numeric prefix when displaying the section title.

`Main` is pinned first by the app. A tag-named starter record uses exactly
`section: "1 Main"`; this includes the single starter records for the named
combat tags and the tag-named `Boxing`, `Warm-up`, and `Cool-down` records.
Do not try to pin Main by changing manifest file order.

For every other exercise, reuse the existing section names and numeric order
of its tag. Current examples include `Base`, `Beginner`, `Intermediate`,
`Advanced`, `Pro`, `Mobility`, `Dynamic Stretching`, `Cardio`,
`Shadowboxing`, `Recovery`, `Walking`, `Rest`, and `Structure`.
Create a new section name or renumber existing sections only when the user
explicitly requests a catalog grouping change.

Named combat tags currently contain exactly one starter record with
`valueType: "time"`, `timerRole: "active"`, and `section: "1 Main"`:
Muay Thai, Kick Boxing, MMA, UFC, BJJ, Wrestling, and Taekwondo. Preserve this
pattern unless the user explicitly asks to add more records to one of those
tags.

## Icons and registry updates

Every tag and every exercise must have the real `Icon.png` required by its
manifest entry. Use Apple SF Symbols only. Select an existing symbol from the
SF Symbols app and record its exact name in `icon-registry.json`.
Every exercise icon outside the `Other` tag must use a filled SF Symbol. The
`Other` tag is the only exception and must not be changed by this rule.

- Tag symbol provenance belongs under `icon-registry.json.tags`.
- Exercise symbol provenance belongs under `icon-registry.json.files` keyed by
  the exercise `key`.
- The JSON record must not contain icon metadata.
- Do not invent SF Symbol names, use emoji, generate a placeholder, or copy a
  technically unrelated icon merely to satisfy the file count.
- Reusing a family symbol is allowed when it matches the existing catalog's
  visual convention. Do not change unrelated existing icons during a new
  record creation.

If a suitable SF Symbol or a valid icon asset cannot be selected, stop and ask;
do not silently substitute one.

## Deterministic creation workflow

Follow this sequence for every new or edited record:

1. Inspect `manifest.json`, `icon-registry.json`, the target tag folder, and a
   comparable existing JSON/icon pair.
2. Resolve the title, lowerCamelCase path, unique snake_case key, tag, and
   `workoutType`. For an edit, identify the existing file by stable `key` and
   preserve its identity.
3. Choose an existing section from the target tag. Use `1 Main` only for the
   tag-named starter record. Choose `valueType`, `timerRole`, difficulty, and
   boxing level from the rules above and the nearest comparable record.
4. Copy the target tag's exact manifest color into the exercise JSON. Do not
   create a new color.
5. Select or verify the exercise SF Symbol and its real `Icon.png`. Update only
   the corresponding `icon-registry.json.files` entry.
6. Create or edit the JSON and its adjacent icon without changing the folder
   layout. Add or update exactly one matching entry in `manifest.json.files`.
7. Validate the complete catalog before reporting success. If any invariant
   fails, fix the cause or stop; never hide the failure with a workaround.

## Validation checklist

Before finishing, verify all of the following:

- every JSON file parses and is a single object;
- every JSON has the required fields and no unsupported fields;
- every tag has a unique decimal-string `id` and every exercise has a unique
  `<tagId>.<exerciseId>` string id where both components are decimal integers
  with the correct tag prefix;
- every key is unique and every edit preserved the old key;
- every title/path pair follows lowerCamelCase;
- every `workoutType` matches its manifest tag;
- every exercise color is an allowed palette color and exactly matches its tag;
- `timerRole` appears if and only if `valueType` is `time`;
- boxing levels are present and valid, while non-boxing records omit `level`;
- every exercise folder contains the matching JSON and `Icon.png`;
- every manifest file entry points to existing matching files, with no missing
  or duplicate JSON/icon pairs;
- every manifest tag points to its existing root icon;
- every icon has a corresponding registry entry and every registry entry that
  is part of the current manifest resolves to the correct symbol provenance;
- every exercise icon outside `Other` uses a filled SF Symbol;
- no exercise Markdown file was created or edited;
- `git diff --check` passes.

This repository has no app target to build. If an app project is available in
the active workspace, run its relevant build/tests as well; otherwise report
catalog validation as complete and state that an app build was not applicable.

The final response must state the cause or requested outcome, files changed,
anything removed or rewritten, checks that passed, and any remaining
limitation. Do not claim a build, test, or visual check that was not actually
run. Do not commit or push unless the user explicitly asks.
