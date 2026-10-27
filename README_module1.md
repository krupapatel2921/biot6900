\# BIOT 6900 — Module 1: Environment Setup \& Database Queries



\*\*Name:\*\* Krupa Patel

\*\*Notebook:\*\* `module1\_setup.ipynb`



\## What I did

Set up the `biot6900` conda environment on Windows, verified tool versions,

ran the BioPython translation example (Part B), and queried four biomedical

databases through their APIs (Part C).



\## Environment

Python 3.11.16, conda 26. 7 .2 , Git 2.55.0, BioPython 1.88,

pandas 3.0.5, numpy 2.4.6, requests 2.34.2. Environment name: biot6900.



\## Data sources (all open access, no application needed)

\- PubMed — NCBI E-utilities via Bio.Entrez; search term "AlphaFold protein structure"

\- UniProt — UniProt REST API; accession P69905 (I substituted this for the default P04637)

\- RCSB PDB — RCSB REST API; entry 1TUP

\- GWAS Catalog — EBI GWAS REST API; variant rs7412



\## What didn't work / notes for the grader

\- \*\*Part D not completed.\*\* It was made optional in the Sept 14 Canvas

&#x20; announcement. The placeholder cell was left in the notebook, so it raises

&#x20; FileNotFoundError when the notebook is run top to bottom. This is expected;

&#x20; every other cell runs.

\- \*\*conda version not printed in Part A.\*\* The notebook shows the active

&#x20; environment name (biot6900) but not the conda version. I checked it from

&#x20; Anaconda Prompt with `conda --version`: conda 26.7.2.

\- \*\*C2 (UniProt) print lines.\*\* The query ran and stored the protein name in

&#x20; the variable `name`, but my print statements show typed text instead of

&#x20; printing the variables. The correct lines would be `print("Accession:", acc)`

&#x20; and `print("Protein:", name)`. The value returned is Hemoglobin subunit alpha.

\- \*\*C1 email typo.\*\* `Entrez.email` is set to "pate.krupav@..." instead of

&#x20; "patel.krupav@northeastern.edu". The query still returned results.



This README was added on 10/27/2026 in response to the rubric-check announcement.

The notebook itself has not been changed since the original submission.

