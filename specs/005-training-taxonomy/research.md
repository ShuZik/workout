# Research: Training Taxonomy Reset

## Decision: Keep post-workout Rest separate from Other → Rest

**Rationale**: Other → Rest is an in-workout interval control. The top-level
Rest tag is a recovery flow that includes low-intensity movement, breathing,
mobility, stretching and release work. Their contexts differ even if both use
the word “Rest”.

**Alternatives considered**: Keeping the visible title Cool-down was rejected
by the approved product naming. Using a different title for Other → Rest was
rejected because it is an existing stable timer-control title.

## Decision: Merge Functional Training and Calisthenics by primary purpose

**Rationale**: Core owns trunk control, bracing, rotation and balance work;
Strength owns general resistance and bodyweight strength. Exact duplicates are
retired so users do not see the same movement in several tags.

**Alternatives considered**: Keeping either source tag as a parallel category
would preserve vague classification and duplicate choices.

## Decision: Preserve legacy wire compatibility

**Rationale**: The released 1.1.3 client still consumes the legacy color and
`cooldown` workout wire type. Visible names and semantic colors can change
without breaking that client when the legacy fields remain stable.

**Alternatives considered**: Publishing `workoutType: "rest"` or replacing
the legacy blue color now would require a client compatibility change and is
outside this catalog-only task.
