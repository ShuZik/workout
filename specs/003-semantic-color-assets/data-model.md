# Data Model: Semantic Catalog Color Assets

## Catalog tag and exercise

| Field | Meaning | Rule |
| --- | --- | --- |
| `color` | Existing compatibility color | Retained unchanged. |
| `colorType` | Named palette family | One approved bare name, optional for historical content. |

## Approved color types

`red`, `coral`, `orange`, `yellow`, `green`, `teal`, `cyan`, `blue`, `indigo`,
`purple`, `violet`, `pink`, `gray`, `sage`, `tan`.

## Resolution

For catalog content, a recognized `colorType` resolves through the design
system and the identically named light/dark asset. If it is absent or invalid,
the existing `color` is used. Local and historical content has no migration.
