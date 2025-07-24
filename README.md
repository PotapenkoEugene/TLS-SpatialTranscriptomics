# Spatial TLS Detection

## 🔬 Overview

This repository provides a comprehensive bioinformatics pipeline for identifying and characterizing Tertiary Lymphoid Structures (TLS) in tumor tissue using spatial transcriptomics. TLS are critical immune hubs that serve as strong predictors of favorable patient prognosis and immunotherapy response.

---

# Initial Info

**Title:** Computational Detection of Tertiary Lymphoid Structures (TLS) in Solid Tumors using Spatial Transcriptomics

**Principal Investigator:** Vladimir Kushnarev | BostonGene

## Team
- Matvei Belyakov
- Bibimariyam Djakupova
- Kate Petrenko
- Evgenii Potapenko
- Veronika Samusik
- Diana Sharysh

## Project Overview

Tertiary Lymphoid Structures are critical immune hubs within tumors, and their presence is a strong predictor of favorable patient prognosis and response to immunotherapy. This project aims to leverage public spatial transcriptomics (ST) datasets to develop and benchmark computational methods for accurately identifying and characterizing TLS in solid tumors. The primary goal is to build a robust pipeline for processing ST data, applying both signature-based scoring and machine learning models to detect TLS regions. By analyzing the gene expression profiles in their native spatial context, students will gain hands-on experience with cutting-edge bioinformatics techniques and contribute to a clinically relevant challenge in immuno-oncology. The project will focus on exploring the biological heterogeneity of TLS and validating findings using computational deconvolution to confirm the underlying immune cell composition.

## Project Tasks

1. Identify, download, and preprocess public 10x Visium spatial transcriptomics datasets of solid tumors (e.g., lung, kidney, breast cancer) from repositories like GEO and Zenodo. Perform quality control, normalization, and data structuring.

2. Apply unsupervised clustering algorithms to identify spatially distinct tissue domains. Annotate clusters based on the expression of canonical marker genes for immune (e.g., PTPRC, MS4A1, CD3D) and stromal cells to locate potential lymphoid aggregates.

3. Implement and score published TLS-associated gene signatures (e.g., 12-chemokine signature) on a per-spot basis. Visualize the resulting TLS scores as spatial heatmaps overlaid on the corresponding H&E tissue images.

4. Using datasets with expert annotations (e.g., Zenodo: 10.5281/zenodo.14620362 ), train and validate a supervised machine learning model (e.g., Support Vector Classifier) to predict TLS-positive spots from their gene expression profiles.

5. Use computational deconvolution tools (e.g., Kassandra) to estimate the cellular composition of predicted TLS regions. Validate that these regions are enriched in B-cells, T-cells, and dendritic cells, confirming their biological identity as TLS.

---

# Project info 

## Tasks detailed

1. **Find datasets** (GEO, Zenodo, **❓**)
2. **Load datasets**
3. **QC** (Quality Control)
4. **Normalization**
5. **Data structuring**
6. **Clustering** (UMAP)
7. **Annotation** (based on canonical immune markers: PTPRC, MS4A1, CD3D, **❓** and stromal cells markers)
8. **Locate lymphoid aggregates** based on annotation
9. **Score TLS-associated gene signatures** (12-chemokine signatures, **❓**) on per-spot basis
10. **Visualize TLS-score per spot** (spatial heatmap) as overlay on tissue image
11. **Train supervised model**: Based on datasets with "expert annotations" learn SVC for predicting TLS-positive spots from their gene expression profiles (probably using TLS-associated gene signature gene subset**❓**)
12. **Deconvolution analysis**: Use deconvolution tools (Kassandra, **❓**) for estimating cellular composition of predicted TLS regions and proof the TLS nature of these regions by enrichment of B-cells, T-cells and dendritic cells (also High endothelial venules (HEVs)**❓**)

---

## Datasets

**Key repositories:** GEO (Gene Expression Omnibus), Zenodo, Human Tumor Atlas Network

### Expert annotated datasets:
- **[Kidney (3) and Lung (5) Cancer with Tertiary Lymphoid Structures](https://zenodo.org/records/14620362)**

---

## Biology Basis

### 1. Tertiary Lymphoid Structures (TLS) Overview

Tertiary Lymphoid Structures (TLS) are ectopic lymphoid organs that form in non-lymphoid tissues, particularly in chronic inflammatory conditions and tumors. They're essentially "mini lymph nodes" that develop within the tumor microenvironment and contain:

- **B-cell follicles** with germinal centers
- **T-cell zones**
- **Dendritic cells**
- **High endothelial venules (HEVs)**

### 2. Cellular Markers

#### Pan-immune markers:
- **PTPRC** (CD45)

#### B-cell markers:
- **MS4A1** (CD20)
- **CD79A**
- **IGHM**

#### T-cell markers:
- **CD3D**
- **CD3E**
- **CD8A**
- **CD4**

#### Dendritic cell markers:
- **CLEC9A**
- **FCER1A**

#### Plasma cell markers:
- **IGHG1**
- **MZB1**

### 3. TLS Signature Scoring

The **12-chemokine signature** typically includes:

- **CCL2, CCL3, CCL4, CCL5, CCL8, CCL18, CCL19, CCL21**
- **CXCL9, CXCL10, CXCL11, CXCL13**

---

## Technical Aspects

**Platform:** 10x Visium

---

## Expected Challenges

### 1. Dataset heterogeneity across cancer types
**Solution:** Batch correction methods, cancer-type specific models

### 2. Class imbalance (TLS regions are rare)
**Solution:** SMOTE, weighted loss functions, ensemble methods


# Tutorials
## 1. [Seurat Visium 10X](https://satijalab.org/seurat/articles/spatial_vignette#identification-of-spatially-variable-features)
