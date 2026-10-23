# Roadmap

In order. Each step ends in something you can open, play or diff.

1. **Make the story a real ASAPS project folder.** Import `story/slopling-odyssey.asaps-generated.json` into ASAPS, fix the generator's 3 warnings (`connection` on two `randomTarget` beats, `axis` on beat 79), and save as an **ASML 2.0 project folder** into `story/project/` (one beat per file, deterministic JSON, so git diffs are readable). From then on that folder is the source of truth, and the generated JSON stays as the archived first draft.
2. **Decide about the AI beats.** The prompt asked for none, but the generator added an `aiConversation` (Karpathi), an `aiInfoText` and an `aiSummary`. Keep them with fallbacks, or replace them with authored text.
3. **Watch the orchard in real time.** Every review so far used stills. Have someone watch the 68-second passage with sound and judge its timing and holds, then repair.
4. **Export an instantiation from ASAPS.** Record the visited beats, choices and state over time as JSON, and have the film read it instead of its hard-coded scene list (see CONTEXT §4).
5. **Animate the missing islands:** Karpathi, Brookes's Ledger, Carmick's workshop, Hassaby's Charter Room (gated on leverage ≥ 5), Knuthe's cave and the Flat Island. Then the other three endings (Telemachus, Argos, Ajax's silence).
6. **The Road and Mau Piailug.** Design a separate, non-comic visual treatment, ideally reviewed with someone from the Pacific voyaging community, before animating it.
7. **Spin out to its own repository** when it outgrows `1000-medical` (see below).

## Standalone repository (proposed)

`slopling-odyssey/` is already laid out to be lifted out as-is:

```
slopling-odyssey/            → repo root
  README.md  CONTEXT.md
  ideation/  story/project/  film/  rig/  docs/
  LICENSE                    ← to choose; ASAPS itself is PolyForm Noncommercial, but this content is yours
```

Keep ASAPS out of the repo. Link it, and record the ASAPS version used (v0.9.133) in `story/project/.asaps/format.json`.
