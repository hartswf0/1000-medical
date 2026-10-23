# Rig: man.body

The puppets are built to the card layout in `reference/` ("paint inside each ghost; keep every brad on its cross"). Scale: **10 cm = 60 px** on the sheet.

| Card | A→B length | Front | Profile |
|---|---|---|---|
| 2 torso | 57 cm | sh.R, sh.L, hipR, hipL brads | sh.L, hipL |
| 4/5 uarm | 31 cm (30 profile) | ✓ | ✓ |
| 6/7 farm | 27 cm | ✓ | ✓ |
| 8/9 hand | 20 cm | ✓ | ✓ |
| 10/11 thigh | 47 cm | ✓ | ✓ |
| 12/13 shin | 43 cm | ✓ | ✓ |
| 14/15 foot | 6 cm front / 30 cm profile | ✓ | ✓ |

In the film (`film/index.html`, `drawPuppet`):

- `K = 2.2` px per cm on screen. Bone lengths come from the table above.
- Odie's thigh, shin, upper arm, forearm, hand, boot and both heads are the painted cardboard cards from the style references, with the white background removed (`film/assets/`). Each sprite's brad pivots are listed in `PIV`.
- Every other character is drawn procedurally in the same material: corrugated card, kraft edge, brass brads.
- A pose is a set of joint angles (0 = hanging down), plus `x`, `y` (the ground line), `s` (scale), `f` (facing), `gy` (0 = `y` is the hip, for seated poses), `hd` (front or profile head), and `hold`/`holdB` (props in the front and back hands).
