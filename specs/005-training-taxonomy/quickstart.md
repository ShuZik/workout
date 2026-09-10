# Validation Guide: Training Taxonomy Reset

1. Parse `manifest.json` and every referenced JSON record.
2. Confirm the 21 visible tag titles match the approved order.
3. Confirm no manifest path starts with `FunctionalTraining/` or
   `Calisthenics/`.
4. Confirm Core and Strength contain every planned retained key exactly once.
5. Confirm Rest has the four recovery sections after Main and the Other → Rest record
   remains separate with `timerRole: "rest"`.
6. Confirm every manifest JSON/icon pair exists and every included exercise
   has a registry entry.
7. Run `git diff --check`.
