# Research: Semantic Catalog Color Assets

## Decision: Use bare semantic color names

Catalog values are `red`, `coral`, `orange`, `yellow`, `green`, `teal`,
`cyan`, `blue`, `indigo`, `purple`, `violet`, `pink`, `gray`, `sage`, and
`tan`.

**Rationale**: These names express the palette family, avoid coupling catalog
data to an old resource prefix, and match the design-system type and asset
name.

**Alternatives considered**: Storing `colorLight` and `colorDark` in every
record duplicates palette data and was rejected. Keeping the `icon_` prefix
was rejected because the requested public type is the bare color name.

## Decision: Keep legacy color as fallback

**Rationale**: Catalog releases 1.1.2 and 1.1.3 already decode `color` and
ignore additive metadata. Content without a semantic type therefore remains
visible.

**Alternatives considered**: Removing `color` would break compatibility and
was rejected.
