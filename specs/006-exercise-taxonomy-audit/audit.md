# Exercise Taxonomy Audit

**Baseline**: 21 tags, 520 exercises.

| Tag | Verdict | Decision |
|---|---|---|
| Custom | Keep | Intentionally empty. |
| Warm-up | Keep | Cardio and sport-specific flows are preparation variants. |
| Boxing through Judo | Keep | Repeated movements are sport-specific technical variants. |
| Core | Keep | Balance holds retain their Core placement: both require stacked trunk and pelvis control. |
| Cardio | Keep | Modes and jump-rope techniques have distinct sustained-cardio purposes. |
| Rest | Keep | `Breathing` is clear from its Rest context. |
| Breathwork | Keep | Named breathing patterns, distinct from recovery breathing. |
| Strength | Retire one record | Remove the vague `Functional Strength Training` session. |
| HIIT | Keep | Battle ropes, bootcamp and agility are method-led interval content. |
| Tabata | Move one record | `Plank Jacks` moves from Upper Body to Conditioning. |
| CrossFit | Remove one section | All former Gymnastics records belong in Bodyweight. |
| TRX | Keep | Every record requires suspension equipment. |
| Meditation | Keep | Attention and mindfulness practices remain distinct from Breathwork. |
| Other | Keep | Timer controls only. |

## Duplicate Verdict

- Combat techniques repeated by Boxing, Kickboxing, Muay Thai, MMA and BJJ are
  intentional sport-context variants.
- A movement repeated by Strength and HIIT, Tabata or CrossFit is retained when
  the latter provides an interval-method or equipment context.
- Warm-up cardio duplicates are retained because their purpose is preparation,
  not a main cardio session.
- `Functional Strength Training` is the only accidental generic duplicate:
  Strength Main and Circuit Training cover its timer purpose.
