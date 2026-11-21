# Module 10 — Medical Imaging AI: Build It, Then Break It

**BIOT 6900 · AI-Powered Biotech Workflows · Computational Lab 10 (Notebook 3 of 3, graded) · Fall 2026**
Northeastern University · Krupa

## What this is

A transfer-learned tumour-vs-normal classifier for colorectal H&E tissue tiles, and an audit of whether it can be trusted. I fine-tune an ImageNet-pretrained DenseNet121, probe it with Grad-CAM and occlusion sensitivity, then inject my own synthetic confounder (a barcode-style scanner tag on every tumour tile) and test whether a retrained model learns the tag instead of the tissue, using a clean model as the control.

## Files

| File | Contents |
|---|---|
| `Medical Imaging AI assignment.ipynb` | Graded notebook, Parts A–D (executed top to bottom) |
| `Module10_Report.pdf` | 3–4 page written report (Part E) |
| `module10_results.csv` | Summary metrics table produced by the notebook |
| `figures/` | Figures saved by the notebook and used in the report |
| `BIOT6900_Module10_Toy_MedNIST.ipynb` | Notebook 1 (ungraded, course-provided): MedNIST toy model |
| `BIOT6900_Module10_Real_Pathology_executed.ipynb` | Notebook 2 (ungraded, course-provided): pipeline template |

## Data

- **Source:** PathMNIST (MedMNIST v2), derived from the NCT-CRC-HE colorectal histology dataset.
- **Task:** normal colon mucosa (PathMNIST class 6 → label 0) vs. colorectal adenocarcinoma epithelium (class 8 → label 1).
- **Split used:** 600 train / 400 validation / 600 test tiles, balanced, resized to 128 × 128 px.
- **File:** `pathmnist_tumour_vs_normal_subset.npz` (~71 MB, from Canvas). It is **not committed** to this repository because of its size (`data/` is listed in `.gitignore`).

## How to reproduce

1. Use the course `biot6900` conda environment and install the requirements:
   ```
   pip install "monai[pillow]" "medmnist>=3.0" scikit-learn pandas matplotlib
   ```
   Tested with MONAI 1.6.1 and PyTorch 2.15.0 (CPU).
2. Download `pathmnist_tumour_vs_normal_subset.npz` from Canvas and place it here:
   ```
   module 10/
   ├─ Medical Imaging AI assignment.ipynb
   └─ data/
      └─ pathmnist_tumour_vs_normal_subset.npz
   ```
3. Open the notebook and run **Kernel → Restart & Run All**.
   Runtime is about 8–12 minutes on a laptop CPU; no GPU is needed. The seed is fixed (`SEED = 0`), so results should closely match the table below. Small differences across machines or library versions are expected.

## Pipeline

| Part | What the notebook does |
|---|---|
| A | Fine-tunes a pretrained DenseNet121 on clean tiles (3 epochs, Adam, lr 1e-4); reports validation and test accuracy |
| B | Grad-CAM on tumour and normal tiles, target-class switch, occlusion sensitivity, and stain/brightness perturbations; applies the five-point skeptic's checklist |
| C | Injects a synthetic scanner tag (4.3% of the tile, top-left) into every tumour tile in train and validation; retrains the same model |
| D | Measures the share of Grad-CAM and occlusion evidence on the tag, tumour recall with vs. without the tag, and the flip rate on tagged normal tiles, for both the confounded model and the clean control |

## Key results

| Metric | Confounded model | Clean control |
|---|---|---|
| Validation accuracy | 1.000 (tag in val set) | 0.965 |
| Test accuracy, clean tiles | 0.658 | 0.955 |
| Tumour recall, with tag → without tag | 1.000 → 0.317 | 0.980 → 0.943 |
| Normal tiles called tumour after adding the tag | 97.0% (0.0% without) | 12.0% (3.3% without) |
| Mean Grad-CAM share on the tag (tag = 4.3% of tile) | 13% | 1% |
| Mean occlusion share on the tag | 69% | 11% |

The full interpretation, verdict, limitations, and deployment discussion are in `Module10_Report.pdf`.

## Notes

- The scanner tag is a **synthetic** artifact, planted so the ground truth is known; it is not present in the original data.
- Notebooks 1 and 2 are course-provided materials, included for reference only.
