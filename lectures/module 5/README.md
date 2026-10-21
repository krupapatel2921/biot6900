# BIOT 6900 · Module 5 Assignment: Variant Interpretation on CYP2C19

**Student:** Krupa Patel
**Gene:** CYP2C19 (assigned)
**AlphaGenome Atlas:** run live (62 of 78 filtered variants scored)

## Files
- `BIOT6900_Module5_Part3_Assignment.ipynb`: analysis notebook (runs top to bottom)
- `pgx_classified_CYP2C19.csv`: 33 VUS with evidence and provisional calls
- `Report_CYP2C19.pdf`: written report
- `data/` is not committed (listed in `.gitignore`); it is recreated on the first run

## How to run
1. Requires internet on the first run (MyVariant.info fetch, cached to `data/CYP2C19.tsv`).
2. The Atlas cell needs an AlphaGenome API key. A `getpass` cell prompts for it at runtime,
   so no key is stored in the notebook. To skip it, set `SKIP_ATLAS = True`.
3. Kernel → Restart & Run All.

## Key choices
- MAX_AF = 0.35, chosen to retain CYP2C19*2 and *17, which are common functional alleles
  (justification in the report).
- Calibration predictor: CADD (scores every consequence class; REVEL is missense-only).
- Calibration declared NOT possible: 0 P/LP and 5 B/LB labelled variants, so all 33 VUS
  were left unchanged rather than moved on artefactual likelihood ratios.

## What didn't work, and how it was handled
- **Environment:** the notebook ran in the base Anaconda kernel, not `biot6900`.
  `alphagenome` was installed into that kernel with `%pip install "alphagenome>=0.9.0"`.
- **Calibration band edge:** the lowest CADD band starts at -0.01, so a score of exactly 0
  is not dropped by `pd.cut`.
- **Atlas merge:** added `avi.dropna(subset=["rsid"]).drop_duplicates("rsid")` before the
  merge, to prevent row duplication.
- **Empty `top_feature` column:** caused by rerunning the Atlas cell after an initial failed
  run (column name conflict). Fixed with a cell that reloads the saved Atlas results from
  `data/avi_CYP2C19.tsv` and re-merges cleanly.
- **Duplicate rsid:** rs4244285 (CYP2C19*2) has two alternate-allele rows. Because Atlas
  results merge on rsid, the second row carries a copied AVI score (a known limitation).
- **Atlas coverage:** 16 of 78 variants were not scored, likely because they are not
  single-base substitutions.
  
