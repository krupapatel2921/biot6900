# BIOT 6900 · Module 3 Assignment: Target Triage with Protein Language Models

**Name:** Krupa Patel
**Disease:** Parkinson's disease (PD)
**Course:** BIOT 6900 AI-Powered Biotech Workflows, Fall 2026, Northeastern University

## What this does
Adds a sequence-based tractability prior to my top 15 Parkinson's targets from Module 2.
Using pre-computed ESM C 300M embeddings and Open Targets clinical-precedence labels, the notebook
validates the prior with family-aware cross-validation, fits separate small-molecule and antibody
priors, lists the three nearest drugged proteins for each target, scores the course anchor variants
(APOE ε2 and ε4, TREM2 R47H, TP53 R175H), and re-ranks the targets for Week 4.

## Files
- `BIOT6900_Module3_Assignment.ipynb`: full analysis (run with Kernel → Restart & Run All)
- `targets_parkinsons_w3.csv`: re-ranked targets, the hand-off to Week 4 (structure prediction)
- `assessment report.pdf`: interpretation report (original)
- `Module3_Report_Addendum.pdf`: report addendum, added in response to the rubric-check announcement
- `assignments/targets_parkinsons.csv`: input, my Module 2 top 15 targets (same file as `module2_parkinsons/targets_parkinsons.csv`)

## How to rerun
1. Activate the environment: `conda activate biot6900` (Python, pandas, numpy, scikit-learn, matplotlib).
2. Unzip the Module 3 data pack (Canvas) into `data/` next to the notebook.
   The data pack is not committed to this repo (about 75 MB; listed in `.gitignore`).
3. Open the notebook and run Kernel → Restart & Run All. No model downloads are needed.

## Notes for the grader
- **Input location:** the notebook reads my Module 2 file from `assignments/targets_parkinsons.csv` (set as `TARGETS_FILE` in the first cell).
- **Column rename:** my Module 2 file has no `score` column. The ranking column is `multi_evidence_score`
  (equal-weight RNA + protein + GWAS score), set as `SCORE_COL`. It appears as `week2_score` in the output CSV.
- **Mapping:** 15 of 15 targets mapped to `proteome.tsv`; 0 unmapped.
- **Leakage guard:** targets removed from the training panel before fitting: 2 (SH3GL2, SNCA).
- **Validation (family split):** AUROC 0.754, average precision 0.555
  (random split: 0.848 / 0.731; chance AP 0.334).
- **Blending weight:** w = 0.6 (Module 2 evidence vs. best-modality tractability); justification in the addendum.
- **Variant numbering:** APOE variants use UniProt precursor numbering (R176C = mature R158C, ε2; C130R = mature C112R, ε4).
- **Data pack:** real data; main model esmc_300m, baseline facebook/esm2_t12_35M_UR50D.

## What didn't work / issues
- **Input file was not committed at submission.** The notebook ran cleanly on my machine, but `assignments/targets_parkinsons.csv` was missing from the repo, so a grader could not rerun the notebook. It was added on October 28, 2026; the notebook itself is unchanged.
- **README pointed to the wrong report file name and to a weight justification the report did not contain.** Both are corrected here; the justification and other missing items are in `Module3_Report_Addendum.pdf`.

This README was updated on October 28, 2026 in response to the rubric-check announcement. The notebook, output CSV and original report have not been changed.
