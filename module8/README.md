# BIOT 6900 · Module 8 — Single-Cell & Spatial Omics

**Name:** Krupa Patel
**Course:** BIOT 6900 · AI-Powered Biotech Workflows (Fall 2026)
**Disease:** Breast cancer
**Target gene:** ERBB2 (HER2)

## Files

| File | Contents |
|---|---|
| `BIOT6900_Module8_Assignment_Starter.ipynb` | Completed notebook: Part 1 (single-cell, CZ CELLxGENE Census, scVI + Harmony) and Part 2 (Visium spatial, squidpy), all TODOs filled, run top to bottom |
| `Module8_Report.pdf` | 5-page report connecting which cells express ERBB2 (single-cell) to where they sit in the tumor (spatial), with limitations |
| `README.md` | This file |

## How it was run

- **Environment:** Google Colab, T4 GPU, Python 3.13 (scanpy 1.13.0, squidpy 1.8.4, scvi-tools, harmonypy, cellxgene-census). Run with *Restart session → Run all*.
- **Single-cell data:** CZ CELLxGENE Census, `stable` release (2025-11-08), query `disease == 'breast cancer' and is_primary_data == True`; 10 whole donors × 2,000 cells = 20,000 cells (18,388 after QC).
- **Spatial data:** 10x Visium `V1_Breast_Cancer_Block_A_Section_1` (3,798 spots), loaded with `sc.datasets.visium_sge`.

## Anything that did not run as written

1. **Environment.** The notebook could not be run in the planned setup. On Windows, `cellxgene-census` (tiledbsoma) and `harmonypy` failed to build (no C++ compiler). On Explorer Open OnDemand, no course kernel was available and the package install did not complete. The full notebook was therefore run on Google Colab (T4 GPU), with a guarded install cell at the top.
2. **Cell 1.2 (batch-aware HVG selection).** `flavor="seurat_v3"` raised a loess numerical error (`reciprocal condition number`) on one donor. I set `span=0.5`, with a fallback to batch-aware `flavor="seurat"` if it still fails. Selection remains batch-aware (`batch_key="donor_id"`).
3. **Thread limit.** Colab exposes only 2 CPU threads, and the neighbor-graph step failed when numba requested more. I added `numba.set_num_threads(1)` and `sc.settings.n_jobs = 1` to the import cell.
4. **Cell 2.1 (spots over H&E).** `total_counts` is not computed until cell 2.2, so the H&E overlay is colored by UMIs per spot, computed directly from the count matrix.
5. **Harmony** stopped after its default 10 iterations before formal convergence. It still ran and was plotted and scored. This is noted in the report's limitations.

## Additions beyond the starter

- Printed cells, donors, and **assays** (required by the assignment sheet) and inspected the contents of the "Other" label.
- Plotted the **Harmony UMAP**, so both scVI and Harmony are run and plotted.
- Marker dot plots and a per-cell-type ERBB2 summary (1.7–1.8), and a domain marker panel (EPCAM, KRT19, ERBB2, COL1A1, PTPRC, CD3D, CD68) to support naming the spatial domains (2.3).
- ERBB2 shown side by side with the spatial domains, with mean expression per domain (2.6).
