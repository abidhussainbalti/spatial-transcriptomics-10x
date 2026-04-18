# 🧬 Spatial Transcriptomics with 10x Genomics

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue)](https://www.python.org/)
[![Scanpy](https://img.shields.io/badge/Scanpy-1.9%2B-green)](https://scanpy.readthedocs.io/)
[![Squidpy](https://img.shields.io/badge/Squidpy-1.2%2B-orange)](https://squidpy.readthedocs.io/)
[![Platform](https://img.shields.io/badge/Platform-Google%20Colab-yellow)](https://colab.research.google.com/)
[![License](https://img.shields.io/badge/License-MIT-red)](LICENSE)

A complete spatial transcriptomics analysis project following the 10x Genomics pipeline tutorials. This project covers the full spectrum — from basic single-cell analysis tools to cutting-edge single-cell spatial resolution — using Scanpy and Squidpy on publicly available datasets.

---

## 📖 What Is Spatial Transcriptomics?

Spatial transcriptomics answers a fundamental question in biology: **not just which genes are expressed, but *where* they are expressed inside a tissue.** Traditional single-cell RNA sequencing (scRNA-seq) tells us the transcriptional identity of each cell but destroys spatial information during dissociation. Spatial transcriptomics preserves the physical location of each measurement, allowing us to map gene expression onto tissue architecture.

This matters because:
- Cells behave differently depending on their neighbors and microenvironment
- Disease (e.g. tumors) creates spatially structured gene expression patterns
- Brain regions, lymph node zones, and organ compartments have distinct molecular identities that are invisible without spatial context

---

## 🗂️ Project Structure

```
spatial-transcriptomics-10x/
│
├── notebooks/
│   ├── 01_scanpy_basics.ipynb          # Foundation: core analysis toolkit
│   ├── 02_visium_fluorescence.ipynb    # Image features + fluorescence channels
│   ├── 03_visium_hne.ipynb             # Spatial statistics + molecular interactions
│   └── 04_xenium_analysis.ipynb        # Single-cell resolution analysis
│
├── figures/
│   ├── 01_scanpy/                      # Output plots from notebook 01
│   ├── 02_visium_fluorescence/         # Output plots from notebook 02
│   ├── 03_visium_hne/                  # Output plots from notebook 03
│   └── 04_xenium/                      # Output plots from notebook 04
│
├── data/
│   └── README.md                       # Dataset download instructions
│
├── docs/
│   └── methodology.md                  # Detailed methodology and pipeline explanation
│
├── requirements.txt                    # Python dependencies
├── .gitignore                          # Files excluded from version control
└── README.md                           # This file
```

---

## 🔗 How the 4 Notebooks Connect

The notebooks form a **progressive learning pyramid** — each one builds on the skills established in the previous one:

```
[01] Basic Scanpy
      ↓ Core toolkit: QC → normalize → cluster → UMAP → spatial plot
      ↓
[02] Visium Fluorescence          [03] Visium H&E
     Image segmentation                Spatial graph analysis
     Multi-channel features            Neighborhood enrichment
     Image-space clustering            Ligand-receptor interactions
     Compare vs gene clusters          Moran's I spatial genes
              ↘                       ↙
               [04] Xenium (Single-Cell Resolution)
                    Single-cell spatial data
                    Delaunay spatial graph
                    Centrality scores
                    Co-occurrence at cell scale
                    Neighborhood enrichment
                    Spatially variable genes
```

| Notebook | Platform | Resolution | Dataset | Key Addition |
|---|---|---|---|---|
| 01 — Scanpy basics | Visium | ~55 µm spots | Human Lymph Node | Core pipeline |
| 02 — Visium Fluorescence | Visium | ~55 µm spots | Mouse Brain | Image segmentation, DAPI/NEUN/GFAP features |
| 03 — Visium H&E | Visium | ~55 µm spots | Mouse Brain | Spatial graphs, ligand-receptor, Moran's I |
| 04 — Xenium | Xenium | Single cell | Synthetic Lung Cancer | Cell-level spatial resolution |

---

## 📓 Notebook Summaries

### 01 — Basic Scanpy (`01_scanpy_basics.ipynb`)
**Dataset:** 10x Genomics Visium Human Lymph Node (public)

The foundational notebook establishing the Scanpy analysis toolkit used across all subsequent notebooks.

**Pipeline:**
- Quality control: total counts, unique genes, mitochondrial percentage
- Filter low-quality spots (min/max counts, MT% threshold)
- Normalize (CPM) → log-transform → select 2,000 highly variable genes
- PCA → KNN graph → UMAP → Leiden clustering
- Spatial overlay of clusters on tissue image
- Marker gene analysis (t-test, rank_genes_groups)
- Gene expression visualization (CR2, COL1A2, SYPL1)
- Synthetic MERFISH-style data demo

**Key figures:**

| Figure | Description |
|---|---|
| `scanpy_qc_distributions.png` | QC histograms — total counts and unique genes per spot |
| `scanpy_umap_clusters.png` | UMAP colored by QC metrics and Leiden clusters |
| `scanpy_spatial_clusters.png` | Leiden clusters overlaid on H&E tissue image |
| `scanpy_marker_genes_heatmap.png` | Top 10 marker genes for cluster 9 |
| `scanpy_spatial_cr2.png` | CR2 (B cell marker) spatial expression |

---

### 02 — Visium Fluorescence (`02_visium_fluorescence.ipynb`)
**Dataset:** Squidpy built-in — Mouse brain coronal section (fluorescence)

Extends the Scanpy pipeline with image analysis. The fluorescence image has three protein-specific channels (DAPI, NEUN, GFAP), enabling cell-type inference from morphology alone.

**Pipeline:**
- Visualize pre-annotated gene-expression clusters spatially
- Display individual fluorescence channels
- Pre-process image (smoothing) and segment nuclei (watershed on DAPI)
- Extract segmentation features: cell count per spot, channel intensities
- Extract multi-scale summary, histogram, and texture features
- Leiden clustering of image features
- Compare image-space vs gene-space clusters

**Key figures:**

| Figure | Description |
|---|---|
| `visium_fluo_cluster_spatial.png` | Gene-space clusters on tissue |
| `visium_fluo_channels.png` | Three fluorescence channels (DAPI, NEUN, GFAP) |
| `visium_fluo_segmentation.png` | DAPI raw vs watershed-segmented |
| `visium_fluo_segmentation_features.png` | Cell counts and channel intensities per region |
| `visium_fluo_image_vs_gene_clusters.png` | Image clusters vs gene clusters comparison |

---

### 03 — Visium H&E (`03_visium_hne.ipynb`)
**Dataset:** Squidpy built-in — Mouse brain coronal section (H&E)

The most analytically complete Visium notebook. H&E (Hematoxylin & Eosin) is the gold standard clinical staining. This notebook adds graph-level spatial statistics and molecular interaction analysis.

**Pipeline:**
- Multi-scale summary feature extraction
- Image-space Leiden clustering
- Spatial connectivity graph construction
- **Neighborhood enrichment**: which clusters are spatially adjacent?
- **Co-occurrence analysis**: spatial co-occurrence across increasing radii
- **Ligand-receptor interaction** (CellPhoneDB/OmniPath): molecular drivers of cell communication
- **Moran's I**: spatially variable gene detection

**Key figures:**

| Figure | Description |
|---|---|
| `visium_hne_cluster_spatial.png` | Gene clusters on H&E tissue |
| `visium_hne_nhood_enrichment.png` | Neighborhood enrichment heatmap |
| `visium_hne_co_occurrence.png` | Hippocampus co-occurrence score curves |
| `visium_hne_ligrec.png` | Ligand-receptor dotplot (Hippocampus → Pyramidal) |
| `visium_hne_spatially_variable_genes.png` | Top Moran's I genes on tissue |

---

### 04 — Xenium Analysis (`04_xenium_analysis.ipynb`)
**Dataset:** Synthetic Xenium-like data (8,000 cells × 100 genes) — mimics 10x Xenium Human Lung Cancer structure

The most advanced notebook. Xenium provides single-cell resolution spatial data — each measurement is one cell, not a spot containing multiple cells.

**Pipeline:**
- Generate synthetic Xenium-like data (all real Xenium metadata columns)
- QC: transcripts per cell, cell area, nucleus ratio, control probe rates
- Filter → normalize → log → PCA → UMAP → Leiden
- Spatial scatter at single-cell resolution
- Spatial graph via Delaunay triangulation
- Centrality scores (closeness, degree, clustering coefficient)
- Co-occurrence on 50% subsample
- Neighborhood enrichment
- Moran's I on full gene panel

**Key figures:**

| Figure | Description |
|---|---|
| `xenium_qc_distributions.png` | QC metrics for all 8,000 cells |
| `xenium_umap.png` | UMAP of single cells |
| `xenium_spatial_leiden.png` | Leiden clusters on tissue at cell resolution |
| `xenium_centrality_scores.png` | Spatial network centrality per cluster |
| `xenium_nhood_enrichment.png` | Neighborhood enrichment + spatial scatter |
| `xenium_spatially_variable_genes.png` | Top Moran's I genes spatially |

---

## 🛠️ Setup & Running

### Option 1: Google Colab (Recommended)
Each notebook installs its own dependencies in the first cell. Simply open in Colab and run all cells.

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

### Option 2: Local Setup

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/spatial-transcriptomics-10x.git
cd spatial-transcriptomics-10x

# Create and activate conda environment
conda create -n spatial python=3.9
conda activate spatial

# Install dependencies
pip install -r requirements.txt

# Launch Jupyter
jupyter notebook notebooks/
```

### Running Order
Run notebooks in order: `01` → `02` → `03` → `04`. Each notebook is self-contained but the skills build progressively.

---

## 📦 Dependencies

| Package | Purpose |
|---|---|
| `scanpy` | Core single-cell analysis framework |
| `squidpy` | Spatial transcriptomics analysis and visualization |
| `anndata` | Annotated data matrix format |
| `pandas` | Data manipulation |
| `numpy` | Numerical computing |
| `matplotlib` | Plotting |
| `seaborn` | Statistical visualization |
| `igraph` | Graph analysis (Leiden clustering) |
| `leidenalg` | Leiden community detection algorithm |

See `requirements.txt` for exact versions.

---

## 📊 Datasets

| Dataset | Source | How to Access |
|---|---|---|
| Human Lymph Node (Visium) | 10x Genomics | `sc.datasets.visium_sge(sample_id="V1_Human_Lymph_Node")` |
| Mouse Brain Fluorescence | Squidpy | `sq.datasets.visium_fluo_image_crop()` |
| Mouse Brain H&E | Squidpy | `sq.datasets.visium_hne_image()` |
| Xenium Lung Cancer | Synthetic (this repo) | Generated in notebook 04 |

See `data/README.md` for details on downloading the real Xenium dataset.

---

## 🔬 Key Concepts Covered

- **Visium spot-level analysis**: gene expression measured per ~55µm tissue spot
- **Image feature extraction**: morphological information from tissue histology images
- **Spatial graph construction**: connecting neighboring spots/cells for graph analysis
- **Neighborhood enrichment**: statistical test for spatial cluster co-adjacency
- **Co-occurrence analysis**: cluster proximity across spatial scales
- **Ligand-receptor interactions**: molecular communication between spatially adjacent clusters
- **Moran's I**: spatial autocorrelation statistic for identifying spatially variable genes
- **Delaunay triangulation**: geometry-based spatial graph for single-cell data
- **Centrality scores**: spatial network topology per cell cluster

---
---

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgements

- [Scanpy](https://scanpy.readthedocs.io/) — Wolf et al., Genome Biology 2018
- [Squidpy](https://squidpy.readthedocs.io/) — Palla et al., Nature Methods 2022
- [10x Genomics](https://www.10xgenomics.com/) — Visium and Xenium platform datasets
- [AnnData](https://anndata.readthedocs.io/) — Annotated data format
