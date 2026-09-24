# Module 2 Assignment 2 — Parkinson's Disease Multi-Omics Target Discovery

**Disease:** Parkinson's disease (PD)
**Integration level:** gene-level (layers come from different, unmatched cohorts)

## Datasets

| Layer | Dataset | Source | Access |
|---|---|---|---|
| Genomics | GWAS Catalog, Parkinson disease (MONDO_0005180) | https://www.ebi.ac.uk/gwas/efotraits/MONDO_0005180 | Open |
| Transcriptomics | GSE7621, substantia nigra, PD vs control (analyzed with GEO2R) | https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE7621 | Open |
| Proteomics | Licker et al. 2014, *Proteomics* 14(6):784–794, PRIDE PXD000427 (supplementary table) | https://www.ebi.ac.uk/pride/archive/projects/PXD000427 | Open |

## Files
- `parkinsons.ipynb` — full pipeline (run with Kernel → Restart & Run All)
- `targets_parkinsons.csv` — top 15 ranked targets
- `data/` — gene-level tables (`pd_gwas.tsv`, `pd_rna.tsv`, `pd_protein.tsv`)
- `data/raw/` — original downloaded files
- `sources.txt` — data sourcing, filtering decisions, and reasoning

## Pipeline
Load → harmonize gene symbols → concordance (RNA vs protein direction) → multi-evidence score (equal weights) → rank → export

## Environment
Python 3.11 (conda environment `biot6900`), pandas, numpy
