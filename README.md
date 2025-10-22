# 1000-medical

**Tagline:** *An AI patient thinks out loud. The clinic answers.*
**Core line:** **1000-small-futures → medical fork** — a legible, consequence-driven simulation engine for emergency-care training.

---

## Overview

`thousand-medical.html` is a **single-file**, browser-run simulation built on the **LEGOS multi-channel engine**. It renders **real-time, dialog-driven medical scenarios** with:

* **Time pressure:** explicit critical windows (e.g., 10-minute resuscitation).
* **Dynamic physiology:** vitals & labs evolve with learner actions.
* **Transparent state:** on-screen telemetry of vitals, actions, and outcomes.
* **Consequence learning:** correct, delayed, and harmful pathways all modeled.
* **Debriefability:** checklists, timelines, and prompts for post-scenario teaching.

> **Purpose:** Education and rehearsal. **Not for clinical use.**
> **Audience:** Medical students, residents, simulation faculty.

---

## Quick Start

1. **Clone** this repo and open `thousand-medical.html` in a modern browser (desktop recommended).
2. Optional: set `OPENAI_API_KEY` in your environment and launch via a local server if you’ll use the AI co-pilot features (see *AI Integration*).
3. On load you’ll see **5 channels** pre-configured:

   * **GI BLEED**, **COPD**, **COCAINE**, **ORGANO**, **BLANK SIM**
4. **Type actions** (e.g., “two large-bore IVs, 1L NS wide open; NPO; labs: CBC, BMP, coags; GI consult”), hit **Enter**, and follow the evolving case.
5. Use the **scenario selector** to switch or **spawn** new channels for parallel practice.

---

## Implemented Scenarios (v1.0)

All are consequence-based, time-sensitive, and **fully runnable**.

* **Upper GI Bleed (`gi_bleed`)** — fluids within 10 min stabilize; omission → shock.
* **Acute Migraine (`migraine`)** — normal neuro exam = key teaching; typical imaging **normal**.
* **Cocaine Intoxication (`cocaine_intox`)** — **benzodiazepines first-line**; **beta-blockers contraindicated**.
* **Organophosphate Poisoning (`organophosphate`)** — **clinical diagnosis**; aggressive **atropine**; **2-PAM** for nicotinic effects; **airway** priority.
* **COPD Exacerbation (`copd`)** — **O2 + nebulized β-agonist + antimuscarinic + steroids**; inhalers fail when dyspneic.
* **Blank Simulation (`blank`)** — clean template for custom cases.
* **Medical Training General (`medical_general`)** — debrief/teaching channel.

A side-by-side comparison table, critical actions, vitals progressions, and teaching points are embedded in the engine’s help overlay.

---

## Design Thesis

This fork applies the **1000-small-futures** principle of **legibility**—exposing an agent’s drives, beliefs, and prediction errors—to **clinical training**:

* **Legible autonomy:** Patients, nurses, and consultants “think in the open” via structured replies.
* **Slow story under pressure:** Realistic deterioration or stabilization unfolds over minutes.
* **No script:** Multiple plausible futures; your choices steer the outcome.

> “If clinical AIs are to share our spaces, their reasons should be inspectable.”
> This engine prototypes **inspectable** decision-making in medical practice scenarios.

---

## Educational Framework

**Objectives per scenario**

1. **Knowledge:** core pathophysiology & differential.
2. **Skills:** focused H&P, procedures, interpretation (ECG, ABG, CXR).
3. **Clinical reasoning:** prioritization & time management.
4. **Communication:** patient lay dialogue, team/consultant handoffs.

**Assessment captured automatically**

* Critical actions checklist (per case).
* Time-to-intervention clocks.
* Decision & vitals timeline (ring memory).
* Outcome grade (stabilize, decompensate, harm event).

**Debrief**

* Built-in prompts: *What went well? Where did time slip? Alternate branches?*
* Exportable transcript + action log (copy-paste).

---

## Scenario Authoring (API)

Open `thousand-medical.html`, find the `scenarios` object (≈ lines 1980-2112), and add:

```js
your_scenario_id: {
  id: 'your_scenario_id',
  name: 'Scenario Display Name',
  role: 'Learner Role (e.g., EM Resident)',
  goal: 'Primary learning objective',
  obstacle: 'Key challenges/complications',
  intro: 'Brief presentation with initial vitals',
  context: [
    'Critical actions to track',
    'Time constraints',
    'Decision points',
    'Key physical findings',
    'Teaching focus'
  ],
  initialPrompt: 'Opening scene text / EMS handoff / patient quote',
  systemInstruction:
    'You simulate [SCENARIO]. PATIENT uses lay terms. NURSE/CONSULTANT use clinical language. Track vitals {…}. Map [intervention] → [vital/lab effect]. Enforce time windows. Mark checklist items. Offer realistic consequences. Educational tone; avoid hallucinated protocols.'
}
```

### Vital/Lab Progressions

Define clear, testable rules (examples):

* `If 1–2 L fluids <10min → HR 110, BP 145/90; else → HR 150, BP 70/40 (shock).`
* `If benzo given in cocaine intoxication → HR ↓, BP ↓; if beta-blocker → paradoxical HTN (teaching).`
* `If atropine repeated until secretions dry → O2 ↑, bradycardia resolves; add 2-PAM → fasciculations stop.`

### Roles & Dialogue

* **Patient:** lay terms, fear, uncertainty.
* **Nurse:** concise SBAR handoff; executes orders; returns vitals.
* **Consultant:** focused recommendations; correct terminology.

---

## Controls

* **Type** actions and questions; **Enter** to send.
* **Tap/Click** timeline markers to inspect events.
* **R** reseed case; **P** pause/resume; **↑/↓** nudge time (debug).
* **Prefers-reduced-motion** honored; DPR clamped ≤ 2 for legibility.

---

## Architecture

Single file, inline CSS/JS with modular regions:

```
thousand-medical.html (≈ 6,900 lines)
├─ 1–1900: CSS & HUD
├─ 1980–2112: Scenarios (ADD HERE)
├─ 2400–2600: Channel class & init
├─ 2700–4000: State & persistence (ring memory)
├─ 4000–6000: AI integration & message handling
└─ 6000–6927: Rendering & interaction
```

**Engine traits**

* **Multi-channel:** run several cases simultaneously.
* **Ring memory:** compact timeline of interventions & physiology.
* **State persistence:** resume where you left off.
* **Deterministic cores w/ stochastic edges:** repeatable tests + variance.

---

## AI Integration (optional)

* **Model:** GPT-4 class (for clinical coherence).
* **Temperature:** 0.6–0.8 (balance realism/consistency).
* **How:** serve the file via a tiny local server that injects `OPENAI_API_KEY` (or paste into settings pane if enabled in your fork).
* **Guardrails:** system prompts constrain roles, forbid protocol invention, and enforce scenario rules.

> You may run **offline** (no AI): scenarios still function using built-in logic/state machines; AI augments dialogue realism and variation.

---

## Safety & Limits

* **For education only.** Not medical advice. Not a device.
* Scenarios align with public guidelines (ACLS/ATLS/IHS/ACEP) where applicable, but **faculty must validate** content for their institution.
* **No PHI**; simulated patients only.
* Avoid protocol substitution in live care settings.

---

## Roadmap

* [ ] Visual vitals dashboard (strip + trend charts).
* [ ] Checklist & timer overlay with scoring rubric.
* [ ] Multi-learner mode (paired roles).
* [ ] Standardized patient video stubs.
* [ ] Procedure micro-sims (airway, lines).
* [ ] One-click transcript export (JSON/CSV).
* [ ] Additional scenarios: STEMI, Septic Shock, DKA, Anaphylaxis, Stroke.

---

## Contributing

1. Open an issue with **Scenario Proposal** (template includes objectives, actions, time windows, outcomes, references).
2. Submit PRs editing only the **scenario block** + optional CSS tokens.
3. Include **teaching notes** + **reference URLs** (guideline pages; no paywalled text).
4. Add unit checks in the **self-tests** section if you alter engine behavior.

**Coding style:** Bauhaus-minimal, plain JS, no external fonts, semantic CSS vars, comments ≤ 80 cols, no magic numbers without a `// why` note.

---

## License

* **Code:** MIT (suggested)
* **Docs:** CC BY-NC-SA 4.0 (suggested)
  Adjust as required for your institution.

---

## Credits & Basis

* **Engine lineage:** **1000-small-futures** — legible, interpretable agent loops adapted to clinical training.
* **Scenario seeds:** Harvard Medical School **Gilbert Program in Medical Simulation** cases (for educational structure).
* **Standards touchpoints:** ACLS/ATLS, ACEP Clinical Policies, IHS criteria, ACGME CLER.

> Independent educational prototype. No affiliation or endorsement by source institutions.

---

## Appendix A — Example: Acute MI (STEMI)

```js
acute_mi: {
  id: 'acute_mi',
  name: 'Acute MI (STEMI)',
  role: 'EM Resident',
  goal: 'Recognize STEMI; activate reperfusion <90 min',
  obstacle: 'Time-critical cath lab activation; inferior MI pitfalls',
  intro:
    'ED: 62M, crushing chest pain radiating L arm ×2h, diaphoretic, anxious. Vitals: HR 95, BP 160/95, O2 94% RA, RR 18.',
  context: [
    'CRITICAL: O2, IV, monitor, aspirin, ECG <10 min',
    'Door-to-balloon <90 min for PCI',
    'Differentiate STEMI vs NSTEMI; consider dissection if unequal pulses',
    'Decide PCI vs lytics (contraindications checklist)'
  ],
  initialPrompt:
    'Paramedic: “62-year-old male, 10/10 crushing chest pain ×2h. Given aspirin en route. No relief with nitro.” Your first action?',
  systemInstruction:
    'You simulate acute STEMI. PATIENT uses lay terms; NURSE executes orders; CARDIOLOGY advises. If ECG <10 min and interpreted correctly → show ST elevation II, III, aVF (inferior STEMI). Track time to aspirin, ECG, and cath activation. If activation <30 min → stabilization; if >60 min → cardiogenic shock (BP 80/50, HR 115). Educational: enforce contraindication checks; forbid beta-blockers if brady/hypotension; suggest right-sided leads if inferior MI with hypotension.'
}
```

---

## Appendix B — Quality Checklist (Faculty)

* [ ] Critical actions enumerate **do-first** steps.
* [ ] Time windows match **realistic practice**.
* [ ] Vitals/lab effects are **physiologically coherent**.
* [ ] Dialogue separates **lay** vs **clinical** language.
* [ ] Debrief prompts present **alternatives & rationales**.
* [ ] References cited; no proprietary protocols copied.

---

**Version:** 1.0 • **Last Updated:** October 2025
**Maintainer:** *(add name/contact)*
**Project family:** 1000-small-futures → **1000-medical** (LEGOS Medical Training Engine)
