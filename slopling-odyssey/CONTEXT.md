# Context: ASAPS, and how this project comes out of it

## 1. ASAPS in one paragraph

**ASAPS (Advanced Stories Authoring and Presentation System)** is Hartmut Koenitz's tool for authoring and playing interactive digital narratives (IDN). He started it in ActionScript at Georgia Tech and developed it from 2005 to 2017, alongside **ASML** (Advanced Stories Markup Language), the XML-based format it reads and writes. The current version, *ASAPS Modern* (https://github.com/sumo961/ASAPS_New, v0.9.133, beta), is a TypeScript monorepo (`packages/core`, `renderer`, `player`, `builder`; desktop and mobile apps). It adds a visual flowchart builder, rich characters, AI-assisted authoring, and a git-friendly project format. It is licensed **PolyForm Noncommercial 1.0.0**, so this project references ASAPS but does not vendor its code.

## 2. The theory ASAPS embodies

ASAPS is built on Koenitz's account of IDN (*Understanding Interactive Digital Narrative*, Routledge 2023), and its vocabulary shapes this whole project:

| Concept | Meaning | Where it shows up here |
|---|---|---|
| **System builder** (not author) | You define what *can* happen, not what does. | The odyssey is a possibility space: 4 answers at the Call, islands in varying order, 4 endings. |
| **Protostory** | The authored system holding all potential narratives. | `story/slopling-odyssey.asaps-generated.json` is the protostory. |
| **SPP model: System → Process → Product** | The system is instantiated through the interactor's process; each playthrough produces one product (a story). | The film is one *product*: a single instantiation (the Decline/Renegotiate/… branch + the Laertes ending) performed in animation. |
| **Interactor** | A participant who plans, acts and shapes, rather than a passive viewer. | The choice at the Call is handed to the viewer even inside the film. |
| **Instantiation** | A specific narrative emerging from play. | Each run of the film or the story is one. |
| **IDN for complexity** | IDN as a way to represent complex issues: plural perspectives, systemic causes, consequence chains, replay. | The Ideator interview drew out exactly these five dimensions (below). |

## 3. The ASAPS pipeline this project went through

ASAPS Modern has an AI-assisted front end. This project used all three stages in order:

1. **Ideator** (AI → *Ideate with Ideator*): a "thesis advisor" interview, one question per turn, optionally doing live web research. It is steered by the IDN-for-complexity framing and draws out five dimensions:
   - **Plurality of perspectives:** manager, colleagues, father, apprentice, and eleven figures who lived different answers.
   - **Systemic causation:** ladders reward what organizations can see (Scott's *seeing like a state*). It isn't a single villain.
   - **Agency → consequence chains:** Accept / Decline / Renegotiate / Stall, each changing later islands and who takes the role.
   - **Variability and replay:** random landfall, an island hub, a leverage gate on the Charter Room, four endings.
   - **Audience as proto-citizen:** the five closing questions.

   The full transcript is in `ideation/ideator-session-2026-10-21.md`. Seventeen web searches grounded it (Hassabis's 2026 role change, Carmack's resignation, Torvalds, Thompson, Valve, Shannon, Sutskever, Naur, Graeber, the Compagnons, Ise, Mau Piailug, Ajax and others).
2. **Synthesized prompt → Story Generator.** The Ideator wrote a single generation prompt (end of the transcript) with knobs: *tragicomedy, long, complex branching, rich affect, no AI beats*. The Story Generator produced **97 beats** across six clusters (*I. The Call, II. The Voyage, II. Islands, II. The Underworld, III. The Return, Ithaca*) and 17 characters.
3. **ASAPS beat system.** The generated story uses ASAPS's own machinery:
   - **Visible beats:** `titleScreen`, `infoText` (44), `dialogTree` (16), `multiChoice` (8), `pickProp` (the 1 a.m. desk: journal, calipers, offer letter), `durScreen`, `endScreen` (3).
   - **Logic beats:** `conditionBeat` (16), `randomTarget` (Slack-poll stall, first landfall), `updateAffect` (act-break bookmarks).
   - **AI runtime beats:** an `aiConversation` with Karpathi, plus `aiInfoText` and `aiSummary`. The prompt asked for none, so these need a decision (see ROADMAP).
   - **Rich characters:** counters for **Craft, Standing, Cunning, Meaning, Leverage**. The father has sentiments (*pride*, *recognition*), emotions, reflections ("My son asked when I stopped building things. Nobody has asked me that in thirty years."), character variants (`odie_lead`, `odie_bench`, `lars_orchard`) and goals (`son_succeeds`, `be_seen`, `keep_craft`).
   - **Affect bookmarks:** `act1_end` is compared at the end to ask whether the father's recognition grew over the voyage.

## 4. How this extends ASAPS

ASAPS stops at a played story: text, choices, images, sound. This project adds a **performance layer** under the same protostory. That's the direction the earlier Threshold Workbench sketches pointed to: ASAPS manages beats and memory, and something else *performs* them.

| Layer | Owner | What it does |
|---|---|---|
| Situation and argument | Ideator transcript | Why the story exists; the five IDN-for-complexity dimensions |
| Protostory | ASAPS (`story/`) | Beats, choices, conditions, counters, affect, endings |
| Instantiation | the interactor | One path: the Call choice, islands, ending |
| **Performance (new)** | `film/` | Brad-jointed cardboard puppets act the instantiated path, with staging, timing and sound |
| **Visual encoding of state (new)** | `film/` | Makes ASAPS's invisible state visible on screen (below) |

The film's formal devices are translations of ASAPS state into image:

| ASAPS state | Film encoding |
|---|---|
| `craft` / Naur's "theory" | A thin line of light from Odie's head to the work. It is visible only to the audience, and it fades when he stops practising. |
| `standing` | A paper tag on a string over his head (TEAM LEAD, LACKS AMBITION?, SENIOR CRAFT LEAD). |
| `callChoice` | The interactive choice at the Call. It changes the branch scene and the suitor at homecoming. |
| `roleHolderName` (suitor) | Chad / Ines / Dougie / nobody standing at Odie's old desk. |
| father's `recognition` sentiment | The orchard: the calipers close "without a hair of light", and his hand lands on the shoulder rather than the title, which he cuts loose. |
| `knowsFatherCraft` (calipers prop) | The calipers handed back in the orchard. The prop picked at 1 a.m. pays off at the end. |

**What ASAPS would need to drive the film directly** (not built yet):

1. An export of one instantiation, meaning the visited beats, choices and state over time, as a JSON timeline the film can read instead of its hard-coded scene list.
2. A `shot` annotation per beat (stage, puppets, pose keys, cue), stored in the beat's `notes` or in a sidecar `film/shots/<beatId>.json`, so the two stay in step through git.
3. Character `variants` mapped to puppet costumes and tags (`odie_lead` → TEAM LEAD tag).

## 5. Lineage: the related projects

This sits inside a wider set of experiments, and the roles carry over:

- **1000-small-futures:** situations, roles and alternative perspectives (here: the ladder dilemma).
- **ASAPS:** encounters, choices, persistent consequences, endings.
- **Metisim:** pressures and symbolic transformation (here: the monster map, where Circe is the craftsperson turned taskmaster and the Cyclops is the ladder's single eye).
- **Halfworld:** performance through bodies and space (here: the puppet rig).
- **ONYX:** inspection (here: the film's production record and the beat-to-shot map).

## 6. Intellectual sources

Homer, *Odyssey* (Books 16 and 24); Sophocles, *Ajax*; Gilbert Ryle, *The Concept of Mind* (knowing-how vs knowing-that); James C. Scott, *Seeing Like a State* (metis); Peter Naur, "Programming as Theory Building" (1985); David Graeber, *Bullshit Jobs*; Detienne and Vernant, *Cunning Intelligence* (polymetis); Lave and Wenger, *Situated Learning*. The comic registers echo *Mythic Quest*, *Silicon Valley* and *The Audacity* by type only.
