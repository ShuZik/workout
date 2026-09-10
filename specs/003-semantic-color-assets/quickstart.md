# Validation Guide: Semantic Catalog Color Assets

1. Inspect `manifest.json` and every exercise JSON: `color` remains unchanged
   and `colorType` is one approved bare palette name.
2. Inspect the application color assets: every approved name has a standard
   and dark-appearance value.
3. Open a catalog tag and exercise in light and dark appearance. They use the
   same named family and change with appearance.
4. Load a record with no `colorType`. It stays visible using its stored
   `color`.
5. Load the expanded catalog in 1.1.2 and 1.1.3. They continue to use `color`
   and do not show a catalog-unavailable state.

Build and automated-test commands are intentionally not included because this
task did not request them.
