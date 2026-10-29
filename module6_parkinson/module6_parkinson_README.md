# BIOT 6900 · Module 6 Assignment: Polygenic Risk Score Portability (Parkinson's disease)

**Name:** Krupa Patel
**Course:** BIOT 6900 · AI-Powered Biotech Workflows · Fall 2026
**Disease:** Parkinson's disease (my own Part I disease, as confirmed with Prof. Prabakaran)
**Score:** PGS Catalog **PGS000902** (PRS90_PD; Nalls et al., *Lancet Neurol* 2019; 90 genome-wide significant variants)

## What I did
Built my own Parkinson's dataset in the format of the course data pack, then used the graded starter notebook as a guide to score 2,336 people from 1000 Genomes phase 3 with a published Parkinson's PRS. I compared the score across five ancestry groups: distributions and a pooled top-10% cutoff, AUC per ancestry, calibration of a EUR-fitted model, and cross-validated AUC for age alone vs age + PRS.

## Files
- `Parkinsons_Module6_PRS.ipynb`: graded analysis (Kernel → Restart & Run All, `USE_DEMO = False`)
- `build_pd_data.ipynb`: Google Colab notebook that built the four data files (genotype extraction and phenotype simulation)
- `data/`: `genotypes.csv`, `variants.csv`, `score.csv`, `meta.csv`
- `fig_prs_by_ancestry.png`: PRS distributions by ancestry
- `fig_calibration.png`: calibration of the EUR-fitted model by ancestry
- `Module6_Report_Parkinsons.pdf`: written report

## Data sources (all open access)
| Item | Source | Access |
|---|---|---|
| Score | PGS Catalog PGS000902, harmonized GRCh37 file (`PGS000902_hmPOS_GRCh37.txt.gz`); other allele from `hm_inferOtherAllele` | Open |
| Genotypes | 1000 Genomes phase 3 (release 20130502, GRCh37), extracted remotely with bcftools; biallelic SNPs only | Open |
| Samples, ancestry, age | The same 2,336 people as the course Module 6 data pack (AFR 500, EAS 500, EUR 500, SAS 489, AMR 347) | Course data pack |

**Variant matching:** 90 of 90 score variants found and allele-matched; 0 missing; 0 unexpected genotype calls. Notebook harmonization: 90 used, 38 flipped, 0 dropped.

## The phenotype is simulated
1000 Genomes has no disease labels, so the Parkinson's phenotype was simulated in `build_pd_data.ipynb`:
- Liability = PD genetic score from the 90 score variants (scaled to SD 1) + 0.35 × (age − 60)/8 + Normal(0, 2.0) noise
- Top 30% of liability across the whole cohort = cases; the same rule for every ancestry; seed 6900
- Noise SD 2.0 was chosen from 1.5–3.5 because it gave a EUR AUC (0.641) closest to the course target of about 0.67
- **Design difference from the course AD pack:** the score variants are themselves the causal variants (no LD-proxy step), so LD mismatch cannot reduce portability in this dataset. This is discussed in the report.

Simulated case rates: EUR 32.4%, AFR 12.4%, EAS 39.0%, SAS 36.4%, AMR 30.0% (overall 30%).

## Key results
- AUC: EUR 0.641, AFR 0.617, EAS 0.705, SAS 0.664, AMR 0.704, overall 0.715
- Pooled top-10% cutoff 1.227; % above it: EUR 9.8, AFR 0.0, EAS 17.2, SAS 14.9, AMR 7.5
- Mean PRS: EUR +0.25, AFR −1.34, EAS +0.64, SAS +0.42, AMR +0.06
- Cross-validated AUC: age only 0.602, age + PRS 0.750
- Proof of data (notebook Section 2) checksums: genotypes.csv `df5d53ce`, variants.csv `87fe0b48`, score.csv `ed2a80d0`, meta.csv `a4e494a2`

## Notes / anything that didn't work
- The course data pack contains only the 22 variants of an Alzheimer's score, so I built my own Parkinson's data files instead (`build_pd_data.ipynb`). Its first Colab run failed because bcftools was not installed; reinstalling fixed it.
- No published `other_allele` in PGS000902; the PGS Catalog harmonizer's inferred other allele was used, choosing the one matching the 1000 Genomes REF/ALT.
- Added to the starter notebook: a count of score variants not found in the genotype file, `plt.savefig` lines for both figures, and a cell printing predicted vs observed risk per ancestry for the calibration analysis.
- The largest-weight variant (LRRK2 G2019S, rs34637584, weight 2.43) is carried by only one person in this cohort, so it contributes almost nothing to the score here.

## How to rerun
1. `conda activate biot6900` (pandas, numpy, scikit-learn, matplotlib).
2. Open `Parkinsons_Module6_PRS.ipynb` in this folder and run Kernel → Restart & Run All. The data files are committed in `data/`.
3. To rebuild the data from scratch, run `build_pd_data.ipynb` in Google Colab (needs internet; installs bcftools).
