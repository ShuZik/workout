# Implementation Plan: Exercise Taxonomy Audit

**Catalog repository**: `/Users/shuzik/Developer/workout`

## Design

1. Retire only `functional_strength_training`, a generic session with no
   distinct movement or method. Remove its JSON/icon pair, manifest pair and
   icon registry entry.
2. Keep both Core balance holds because their instructions explicitly require
   trunk and pelvis stability; do not move or rename them.
3. Change `tabata_plank_jacks` to `6 Conditioning` and
   `cross_training_box_jump` to `2 Bodyweight`.
4. Keep the Rest record with key `breathing` titled `Breathing`; its parent
   Rest context already conveys recovery purpose.

## Boundaries

- Preserve all remaining keys, ids, defaults, energy profiles, colors, icons,
  availability and descriptions.
- Do not de-duplicate intentional sport, method or warm-up variants.
- Do not modify the app project or commit/push the catalog.
