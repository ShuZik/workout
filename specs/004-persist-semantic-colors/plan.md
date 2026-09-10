# Implementation Plan: Persist Semantic Colors

**App repository**: `/Users/shuzik/Developer/Fighting-AI`
**Target release**: 1.1.4

## Design

1. Keep `colorType` as the single persisted exercise color field.
2. Normalize each stored exercise item during database load. A valid stored
   type is retained; a known legacy hex maps to its type; an unknown legacy
   hex maps to `red`.
3. Re-encode migrated and newly written exercise payloads without `colorHex`.
4. Make custom-exercise creation use the selected picker type directly.
5. Let runtime timer construction derive its concrete hex from `colorType`
   when a loaded exercise no longer has a stored hex.

## Boundaries

- `WorkoutBuildItemRecord` is the only persistence payload changed.
- Catalog download compatibility and timer/shared-state payloads keep their
  existing contracts.
- No test rewrite is included.
