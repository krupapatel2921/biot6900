# Module 4 Assignment: Structure-Based Druggability (Parkinson's disease)

**Name:** Krupa Patel  
**Course:** BIOT 6900 · AI-Powered Biotech Workflows · Fall 2026  
**Disease:** Parkinson's disease  
**Targets:**
- **MAP4K4** (UniProt O95819), Module 2 rank 4, Module 3 rank 1: highest small-molecule tractability among my top targets (p_sm = 0.665) and an inhibitor-bound experimental structure (PDB 4U43) to check the prediction against.
- **SNCA / α-synuclein** (UniProt P37840), Module 2 rank 3: the strongest GWAS signal in my data (−log10 p = 169.4) and the central protein of Lewy body pathology, with a micelle-bound NMR structure (PDB 1XQ8).

**Note on target choice:** my Module 3 report recommended MAP4K4 and PBXIP1 for Week 4. I analysed SNCA instead of PBXIP1; the reason and its trade-off are explained in `Module3_Report_Addendum.pdf` (Section G) at the top level of this repo.

## Files
- `Parkinsons_Module4_Assignment.ipynb` — analysis notebook (Steps 1–7)
- `structure_assessment_parkinsons.csv` — summary table (Step 7)
- `report_parkinsons_structure assessment.pdf` — report (Steps 8–9), original
- `Module4_Report_Addendum.pdf` — addendum added in response to the rubric-check announcement (druggability calls and corrections)
- `structures/fpocket_out.zip` — precomputed fpocket output from Google Colab, read by the Step 6 cell

## Experimental structures used
- **MAP4K4:** PDB 4U43, chain A (kinase domain with bound inhibitor 3D8, X-ray, 2.18 Å)
- **SNCA:** PDB 1XQ8, chain A (micelle-bound α-synuclein, deposited average NMR structure)

## What I changed or worked around
1. **fpocket on Windows:** fpocket cannot be installed on Windows. I ran it in Google Colab (installed with micromamba from conda-forge), zipped the output, and extracted it into `structures/`. The Step 6 cell was edited to read this precomputed output instead of stopping with an error.
2. **Sequence alignment:** I replaced the provided global alignment with a semi-global alignment, because 4U43 covers only the MAP4K4 kinase domain. The global version paired some crystal residues with the C-terminal CNH domain (RMSD 20.99 Å before the fix, 1.19 Å after).
3. **Guard in Step 5:** superposition is skipped if fewer than 10 identical, confident residue pairs are available.
4. **Extra analysis cells:**
   - pairwise PAE between all MAP4K4 domain pairs (28.1, 25.4 and 28.7 Å)
   - per-helix RMSD for SNCA (helix ranges 3–37 and 45–92, from Ulmer et al., 2005, J Biol Chem 280:9595–9603)
   - pocket inspection (lining residues and per-residue pLDDT) and inhibitor core-site confidence for MAP4K4

## How to rerun
The `structures/` folder is not committed (see `.gitignore`), except `structures/fpocket_out.zip`. The notebook downloads the AlphaFold models, PAE files and PDB structures automatically, and the Step 6 cell extracts the fpocket output from the zip, so the notebook reruns without installing fpocket.

## Anything that didn't work
The workarounds above (fpocket via Colab, semi-global alignment) are documented in the notebook's markdown cells. Two problems were found after submission:
- **Druggability calls were never filled in.** The `CALLS` cell in Step 7 still contains the template placeholder, so the `druggability_call` column of `structure_assessment_parkinsons.csv` reads `<one of the three phrases>` for both targets, and the report discusses the evidence without stating one of the three calls. The calls and their justification are given in `Module4_Report_Addendum.pdf`. The notebook and CSV are unchanged.
- **Wrong numbers in the report's core-site comparison.** Two sentences list residue deviations and top-five pocket values instead of the inhibitor-contact residue confidence. The notebook output gives the correct values (15 residues, mean pLDDT 87.1, minimum 62, mean PAE among them 2.9 Å); the addendum corrects this.
- **`structures/fpocket_out.zip` was not committed at submission**, so Step 6 could not be rerun without repeating the Colab step. It was added on October 28, 2026.

This README was updated on October 28, 2026 in response to the rubric-check announcement.