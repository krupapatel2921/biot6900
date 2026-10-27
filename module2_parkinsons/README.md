# BIOT 6900 — Module 2 Assignment 2: Multi-Omics Target Discovery in Parkinson's Disease

**Name:** Krupa Patel
**Disease:** Parkinson's disease (PD), substantia nigra
**Integration level:** gene-level (unmatched: the three layers come from different cohorts with no shared samples)

## What I did
Reduced three public PD datasets (genomics, transcriptomics, proteomics) to one row per gene, joined them on gene symbol, computed RNA-vs-protein concordance, scored each gene with an equal-weight multi-evidence score (min-max normalized |log2FC| for RNA and protein, -log10 p for GWAS; 1/3 each), and exported the top 15 targets.

## Files
- `Parkinsons Multi Omics Report.pdf` — written report (original, dated Sept 24, 2026)
- `Module2_Report_Addendum.pdf` — addendum to the report (discordant gene and corrections), added in response to the rubric-check announcement
- `parkinsons.ipynb` — full pipeline (Kernel → Restart & Run All)
- `targets_parkinsons.csv` — top 15 ranked targets
- `sources.txt` — data sourcing, filtering decisions and reasoning
- `data/` — gene-level tables produced by the notebook (`pd_gwas.tsv`, `pd_rna.tsv`, `pd_protein.tsv`)
- `data/raw/` — original downloaded files (`pd_gwas_association.tsv`, `gse7621_geo2r.tsv`, `licker2014_supp.csv`)

## Pipeline
Load → harmonize gene symbols → concordance (RNA vs protein direction) → multi-evidence score (equal weights) → rank → export.

## Environment
Python 3.11 (conda environment `biot6900`), pandas, numpy.

## Data sources

| Layer | Dataset | Link | Access | Samples |
|---|---|---|---|---|
| Genomics | NHGRI-EBI GWAS Catalog, Parkinson disease (MONDO_0005180) | https://www.ebi.ac.uk/gwas/efotraits/MONDO_0005180 | Open | 698 PD-risk associations (of 905) |
| Transcriptomics | GEO GSE7621 (Lesnick et al. 2007), Affymetrix GPL570, analyzed with GEO2R | https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE7621 | Open | 16 PD, 9 control |
| Proteomics | Licker et al. 2014, *Proteomics* 14(6):784–794, PRIDE PXD000427; supplementary table pmic7662-sup-0002 | https://www.ebi.ac.uk/pride/archive/projects/PXD000427 (paper: https://doi.org/10.1002/pmic.201300342) | Open | 5 PD, 8 control |

Gated option considered but not used: AMP-PD Knowledge Platform (matched WGS, RNA and proteomics) requires a data-use agreement, which was not feasible within the assignment timeline.

## Key numbers
- Gene-level tables: GWAS 393 genes, RNA 20,799 genes, protein 1,779 genes
- RNA ∩ protein: 1,684 genes; all three layers (inner join): **34 genes**
- Concordant (RNA and protein same direction): 16 of 34 (47.1%); Spearman RNA vs protein log2FC = −0.136

## What didn't work / notes for the grader
- **Proteomics has no numeric p-values.** The Licker supplement gives only a significance flag (`*`), kept as the `significant` column. The score uses |log2FC| magnitude, so p-values were not needed for scoring.
- **Proteomics direction was inferred.** The supplement labels channel groups X/Y without saying which is PD. I set it as PD/control because TH is down and FTL, GFAP and NEBL are up, matching the paper's validated findings.
- **12 proteins lost in extraction.** The paper reports 1,795 quantified proteins; 1,783 were recovered from the PDF-to-Word supplement.
- **RNA sign flipped.** The GEO2R contrast came out as control vs PD (TH, SLC6A3 and DDC were positive), so log2FC was multiplied by −1 to make all values PD vs control.
- **GWAS gene-splitting bug.** The regex that splits `MAPPED_GENE` breaks on every hyphen, so hyphenated symbols were fragmented (e.g., HLA-DRB1 → "HLA" + "DRB1"; SNCA-AS1 → "SNCA" + "AS1"). I re-checked after submission: the GWAS values for all 15 exported targets are identical with a corrected split, so the ranked table is unaffected, but HLA class II genes could not join the other layers.
- **Proteomics depth limits recovery.** Known PD genes LRRK2 and GBA1 are in the GWAS layer but absent from the ~1,800-protein proteomics set, so they are removed by the three-way inner join.

This README was updated on October 27, 2026 in response to the rubric-check announcement. The notebook, target table and original report have not been changed.
