# Data Model: Persist Semantic Colors

| Entity | Stored color after migration | Legacy handling |
| --- | --- | --- |
| Exercise payload | `colorType` | Read old `colorHex`, map it, then remove it on rewrite. |
| Catalog download | `color` + `colorType` | Unchanged for older app compatibility. |
| Timer runtime state | Concrete color when required | Derived from `colorType` if the item has no hex. |

The approved type set is the 15 bare palette names defined by the design
system. Unknown historical hex values resolve to `red`.
