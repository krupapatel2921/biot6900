# Module 4 Assignment: Structure-Based Druggability (Parkinson's disease)

**Name:** Krupa Patel  
**Course:** BIOT 6900 · AI-Powered Biotech Workflows · Fall 2026  
**Disease:** Parkinson's disease  
**Targets:** MAP4K4 (UniProt O95819), SNCA / α-synuclein (UniProt P37840)

## Files
- `Parkinsons_Module4_Assignment.ipynb` — analysis notebook (Steps 1–7)
- `structure_assessment_parkinsons.csv` — summary table (Step 7)
- `report_parkinsons_structure assessment.pdf` — report (Steps 8–9)

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
The `structures/` folder is not committed (see `.gitignore`). The notebook downloads the AlphaFold models, PAE files and PDB structures automatically. Step 6 needs fpocket output in `structures/AF-<accession>_out/`: run fpocket directly on Mac/Linux, or in Google Colab as described above.

## Anything that didn't work
No unresolved issues. The workarounds above (fpocket via Colab, semi-global alignment) are documented in the notebook's markdown cells.