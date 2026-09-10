# Validation Guide: Exercise Taxonomy Audit

1. Confirm `functional_strength_training` is absent from its folder, manifest
   and icon registry.
2. Confirm the three retained edited records preserve their keys and metadata,
   with only the approved title or section changed.
3. Parse every manifest record; confirm every JSON/icon pair and registry key
   resolve once.
4. Confirm 21 tags remain and the catalog contains 519 exercises.
5. Run `git diff --check`.
