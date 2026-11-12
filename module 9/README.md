# Module 9 — Clinical NLP on Parkinson's Disease (Synthetic Cohort)

**Course:** BIOT 6900 · Computational Lab 9 · Fall 2026
**Disease:** Parkinson's disease — hospitalized PD patients; outcome = in-hospital mortality
**Data:** 400 synthetic patients, 3 notes each (admission, day 2, final), generated inside the notebook. No real patient data.
**Seed:** 2476153 (student ID 002476153; see note below)

## Files

| File | Purpose |
|---|---|
| `BIOT6900_Module9_Notebook3_Graded.ipynb` | Graded assignment: configuration, extraction, model, D1–D4 answers |
| `BIOT6900_Module9_Toy_ClinicalNLP.ipynb` | Warm-up: negation, abbreviation, temporality on 6 toy notes (ungraded) |
| `BIOT6900_Module9_Notebook2_RealWorkflow.ipynb` | Full pipeline walkthrough (ungraded) |

## Configuration

| Concept | Role | Rationale |
|---|---|---|
| aspiration_pneumonia | signal | Present on admission; a leading cause of death in PD |
| dysphagia | signal | Pre-existing PD swallowing impairment; upstream of aspiration |
| hip_fracture | signal | Fall-related injury that is the reason for admission, preceding decline |
| intubation | consequence (leakage) | Rescue intervention performed because of respiratory failure |
| comfort_care | consequence (leakage) | End-of-life decision made once death is expected |
| dnr_dni | consequence (leakage) | Code-status change made as prognosis worsens |

Each concept has 3–5 synonyms/abbreviations (e.g. "asp pna", "CMO", "DNR/DNI") to exercise normalization.

## How to run

1. Activate the `biot6900` environment (pandas + scikit-learn + matplotlib only; no clinical-NLP installs).
2. Open `BIOT6900_Module9_Notebook3_Graded.ipynb`.
3. Kernel → Restart & Run All. The fixed seed reproduces every number below.

## Key results

- Naive vs assertion-aware count (aspiration_pneumonia): **170 vs 106**
- CV AUROC, all features: **0.929**
- CV AUROC, declared leakage removed: **0.872**
- CV AUROC, all leakage removed: **0.818**
- Final leak set: intubation, comfort_care, dnr_dni, renal_replacement
- Wildcard kept as signal: lactate_not_clearing

## What did not work / notes

- **Seed:** `SEED = 002476153` raised a `SyntaxError` (Python integers cannot have leading zeros). Fixed by using `2476153`.
- **Synonym choices:** avoided "PD" (also means peritoneal dialysis, which would collide with the `renal_replacement` wildcard) and words that appear in the generator's template text (e.g. "ICU", "resuscitation").
- **Optional real-tool previews** (scispaCy, negspaCy, Med7) were not installed; those cells skip by design and the notebooks still finish.
- **Engine limitations noticed:** the teaching assertion engine has no "rule out" trigger, so "rule out X" would be counted as present; and "dialysis … since stopped" in survivors' notes is also counted as present.
- All results are illustrative and come from one synthetic cohort; they are not a deployable model.
