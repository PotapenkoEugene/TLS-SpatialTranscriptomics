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

## Project Overview

Tertiary Lymphoid Structures are critical immune hubs within tumors, and their presence is a strong predictor of favorable patient prognosis and response to immunotherapy. This project aims to leverage public spatial transcriptomics (ST) datasets to develop and benchmark computational methods for accurately identifying and characterizing TLS in solid tumors. The primary goal is to build a robust pipeline for processing ST data, applying both signature-based scoring and machine learning models to detect TLS regions. By analyzing the gene expression profiles in their native spatial context, students will gain hands-on experience with cutting-edge bioinformatics techniques and contribute to a clinically relevant challenge in immuno-oncology. The project will focus on exploring the biological heterogeneity of TLS and validating findings using computational deconvolution to confirm the underlying immune cell composition.

## Project Tasks

1. Identify, download, and preprocess public 10x Visium spatial transcriptomics datasets of solid tumors (e.g., lung, kidney, breast cancer) from repositories like GEO and Zenodo. Perform quality control, normalization, and data structuring.

2. Implement and score published TLS-associated gene signatures (e.g., 12-chemokine signature) on a per-spot basis. Visualize the resulting TLS scores as spatial heatmaps overlaid on the corresponding H&E tissue images.

3. Using datasets with expert annotations (e.g., Zenodo: 10.5281/zenodo.14620362 ), train and validate a supervised machine learning model (e.g., Support Vector Classifier) to predict TLS-positive spots from their gene expression profiles.

4. Use computational deconvolution tools (e.g., Kassandra) to estimate the cellular composition of predicted TLS regions. Validate that these regions are enriched in B-cells, T-cells, and dendritic cells, confirming their biological identity as TLS.

---

# Project info 

## Model Development Process

Given the limited number of **samples** available (n = 8), training a deep learning model entirely from scratch was not feasible.  
We initially evaluated several **off-the-shelf pre-trained solutions** for TLS detection, but all yielded suboptimal results, failing to generalize to our dataset.

---

### Baseline: TLS Signature Analysis
To establish a baseline, we performed **TLS signature scoring** using 18 published gene sets, ranging in size from **4 to ~2000 genes**.  
While this approach achieved moderate performance, it was unable to reliably distinguish TLS from other immune cell aggregates, highlighting the need for a spatially resolved method.

---

### Step 1 – Gene Expression–Only Models
We adopted a **spot-level classification approach**, leveraging the thousands of spots available per sample.  
Using gene expression features alone, we trained three supervised machine learning models:

- **Linear Regression** (baseline)
- **Random Forest**
- **Support Vector Machine (SVM)**

The **SVM** outperformed other models in overall accuracy.  
However, **recall** for TLS spots remained too low for practical use, indicating that many TLS regions were being missed.

---

### Step 2 – Image-Only Models
To incorporate histological context, we fine-tuned a **ResNet** architecture on H&E images.  
While this approach captured some morphological features, it exhibited a significant drop in **precision**, resulting in many false positives.

To address the lack of contextual information, we modified the training pipeline to include **spatial context windows** around each spot.  
This improved performance, but the extreme **class imbalance** (TLS spots ≈ 4% of all spots) continued to limit results.

Attempts to artificially balance the dataset (e.g., oversampling TLS spots or undersampling non-TLS spots) degraded generalization.  
We then implemented a **curriculum learning–style strategy**, starting with a more balanced dataset and **gradually reducing the TLS proportion** to match the true class distribution across epochs.  
This yielded a modest improvement but was still insufficient.

---

### Step 3 – Multi-Modal Models
We explored **multi-modal fusion** of gene expression and image features:

- **Simple concatenation (early fusion)** did not improve results and, in some cases, degraded performance.
- An **attention-based fusion architecture** enabled the model to dynamically weight gene expression and image features per sample.

The **attention-based model** significantly outperformed all previous approaches.  
Further fine-tuning of **feature extraction layers** and **fusion parameters** produced a notable increase in both recall and precision.

---

### Final Model
The final architecture integrates:
- **Gene expression features** (top 2000 highly variable genes)
- **Image embeddings** from ResNet with context padding
- **Attention-based modality fusion**
- **Class imbalance mitigation** via curriculum-style sampling

---

Detailed implementation, training scripts, and evaluation metrics are available in this repository.


---

## Datasets

**Key repositories:** GEO (Gene Expression Omnibus), Zenodo, Human Tumor Atlas Network

### Expert annotated datasets:
- **[Kidney (3) and Lung (5) Cancer with Tertiary Lymphoid Structures](https://zenodo.org/records/14620362)**

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


Composition of TLSs and detection of markers from [Munoz-Erazo, 2020](https://pmc.ncbi.nlm.nih.gov/articles/PMC7264315/)
| |Structure/Cell                      | Corresponding Marker                             |
|------------------|------------------|--------------------------------------------------|
| **Most common TLS content**        | |                                                  |
| | High endothelial venules           | No specific marker, MECA-79                      |
| | Dendritic cells                    | DC-LAMP                                          |
| | B cells                            | CD20                                             |
| | T cells                            | CD3                                              |
| **Possible components of TLSs**    |                                                  |
| | Germinal centers                   | Ki67                                             |
| | Proliferating B cells              | Ki67                                             |
| | T follicular helper cells          | CD4, CXCR5, CD40L, IL21, IL6                     |
| **Other markers associated with TLSs** |                                              |
| | Chemokines                         | CCL-2,3,4,5,8,18,19,21; CXCL-9,10,11,13          |
| | Cytotoxic T cells                  | CD8                                              |
| | T regulatory cells                 | FOXP3                                            |
| | Endothelial cells                  | CD31                                             |
| | Follicular DCs, mature B cells     | CD23                                             |


### 3. TLS Signature Scoring

The **12-chemokine signature** includes:

- **CCL2, CCL3, CCL4, CCL5, CCL8, CCL18, CCL19, CCL21, CXCL9, CXCL10, CXCL11 and CXCL13**

[TLS signature associated with good prognosis in ovarian cancer](https://pmc.ncbi.nlm.nih.gov/articles/PMC9871364/)

- **CETP, CCR7, SELL, LAMP3, CCL19, CXCL9, CXCL10, CXCL11, and CXCL13**

[9gene set](https://pubmed.ncbi.nlm.nih.gov/31942071/)
- **CD79B, CD1D, CCR6, LAT, SKAP1, CETP, EIF1AY, RBP5, and PTGDS**

[hallmark genes](https://pubmed.ncbi.nlm.nih.gov/33568675/)
- **CCL19, CCL21, CXCL13, CCR7, CXCR5, SELL, and LAMP3**
---

## Technical Aspects

**Platform:** 10x Visium

---



# Tutorials
## 1. [Seurat Visium 10X](https://satijalab.org/seurat/articles/spatial_vignette#identification-of-spatially-variable-features)
## 2. [Paper with similar goals](https://www.nature.com/articles/s41467-024-52153-4?utm_source=chatgpt.com)
## 3. [Another paper with THE SAME goal](https://pmc.ncbi.nlm.nih.gov/articles/PMC11011734)

