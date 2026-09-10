# Implementation Plan: Semantic Catalog Color Assets

**Feature**: Semantic Catalog Color Assets
**Catalog repository**: `/Users/shuzik/Developer/workout`
**App repository**: `/Users/shuzik/Developer/Fighting-AI`
**Target release**: 1.1.4

## Technical Context

The catalog currently carries a legacy six-digit `color` value on tags and
exercise records. FNK Timer downloads and validates that value before it builds
catalog items. Its design system already exposes a named picker palette, but
the corresponding Assets colors currently use a prefixed resource name and
have identical values for light and dark appearance.

## Design

1. Define the approved palette under bare names (`red` through `tan`) in the
   application color assets. Each has the approved light and dark value.
2. Make the design system expose the same bare color names and resolve a
   catalog `colorType` through that one central palette.
3. Parse `colorType` as optional catalog metadata in the 1.1.4 app. Preserve
   the existing `color` value and use it whenever the type is absent or not an
   approved name.
4. Add `colorType` to every manifest tag and exercise JSON. Derive it from the
   existing approved tag color; do not alter other record fields.
5. Keep the new metadata additive so 1.1.2 and 1.1.3 continue using `color`.

## Constitution Check

- Only the catalog metadata and app color-rendering path are in scope.
- Existing exercise keys, IDs, fields, icons, and user changes remain intact.
- No custom workaround, dependency, or test rewrite is introduced.
- Existing custom and historical data retains the legacy color fallback.

## Validation

- Validate every catalog JSON and the manifest has a recognized `colorType`
  matching its legacy color family.
- Inspect that each approved bare color asset has light and dark appearances.
- Inspect the app parser and style resolution for valid-type and fallback
  paths.
- Build and test execution are not part of this task unless requested
  separately.
