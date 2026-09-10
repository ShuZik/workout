# Catalog Data Model: Exercise Taxonomy Audit

- `key` remains the stable saved-workout identity for every retained record.
- `section` is the only display-grouping field changed by this feature.
- Retiring a record removes its manifest JSON/icon pair and matching registry
  key together.
- A title may clarify recovery purpose without changing identity or timer data.
