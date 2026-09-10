# Validation Guide: Persist Semantic Colors

1. Save a custom exercise after selecting a palette color. Its stored exercise
   payload contains `colorType` and has no `colorHex`.
2. Load an old exercise payload containing only a palette hex. It is rewritten
   with the matching type and no hex.
3. Load an old exercise payload with an unknown hex. It is rewritten as `red`.
4. Start a workout from a migrated custom exercise. It receives a valid runtime
   color.

Automated tests are not part of this task unless requested separately.
