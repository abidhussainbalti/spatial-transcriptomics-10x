# Data

Raw data files are **not stored in this repository** because they are large binary files (50MB–5GB). All datasets are publicly available and can be downloaded from the sources listed below.

---

## Datasets Used in This Project

### Notebook 01 — Human Lymph Node (Visium)
**Downloaded automatically by Scanpy:**
```python
import scanpy as sc
adata = sc.datasets.visium_sge(sample_id="V1_Human_Lymph_Node")
```
No manual download needed. Scanpy downloads (~100MB) on first run and caches locally.

---

### Notebooks 02 & 03 — Mouse Brain (Visium Fluorescence + H&E)
**Downloaded automatically by Squidpy:**
```python
import squidpy as sq

# Notebook 02 — Fluorescence
img   = sq.datasets.visium_fluo_image_crop()   # ~303 MB
adata = sq.datasets.visium_fluo_adata_crop()   # ~65 MB

# Notebook 03 — H&E
img   = sq.datasets.visium_hne_image()         # ~314 MB
adata = sq.datasets.visium_hne_adata()
```
No manual download needed. Squidpy downloads and caches automatically.

**Original source:** [10x Genomics Dataset Portal](https://support.10xgenomics.com/spatial-gene-expression/datasets)

---

### Notebook 04 — Xenium (Single-cell, Lung Cancer)
**Notebook 04 uses a synthetic dataset generated entirely in Python** — no download required. The synthetic data replicates the exact column structure of the real 10x Genomics Xenium Human Lung Cancer dataset.

**If you want to run on the real Xenium dataset:**

1. Download from [10x Genomics Xenium Preview Dataset](https://www.10xgenomics.com/products/xenium-in-situ/preview-dataset-human-lung)
2. Extract the archive into a folder named `Xenium/` in the root of this repository
3. Replace the data generation section in notebook 04 with:

```python
from spatialdata_io import xenium
import spatialdata as sd

xenium_path = "../Xenium"
zarr_path   = "../Xenium.zarr"

sdata = xenium(xenium_path)
sdata.write(zarr_path)
sdata = sd.read_zarr(zarr_path)

adata = sdata.tables["table"]
```

> ⚠️ The real Xenium dataset is approximately 5GB. It is listed in `.gitignore` and must never be committed to the repository.

---

## Cache Locations

Scanpy and Squidpy cache downloaded datasets at:
- **Linux/Mac:** `~/.cache/squidpy/` and `~/.cache/scanpy/`
- **Google Colab:** `/root/.cache/`

Once cached, re-running the notebooks does not re-download the data.
