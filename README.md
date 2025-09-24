# NoMAISI: Nodule-Oriented Medical AI for Synthetic Imaging

<div align="center">
<p align="center">
  <img src="NoMAISI_logo.png" alt="PiNS Logo" width="500">
</p>

**Nodule-Oriented Medical AI for Synthetic Imaging and Augmentation in Chest CT**

[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/)
[![Docker](https://img.shields.io/badge/Docker-ft42%2Fpins%3Alatest-2496ED?logo=docker)](https://hub.docker.com/r/ft42/pins)
[![Python](https://img.shields.io/badge/Python-3.9+-green.svg)](https://python.org)
[![Medical Imaging](https://img.shields.io/badge/Medical-Imaging-red.svg)](https://simpleitk.org)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.8.0-orange.svg)](https://pytorch.org)
[![MONAI](https://img.shields.io/badge/MONAI-1.4.0-blue.svg)](https://monai.io)

</div>


# Abstract
Medical imaging datasets are increasingly available, yet abnormal and annotation-intensive cases such as lung nodules remain underrepresented. We developed NoMAISI (Nodule-Oriented Medical AI for Synthetic Imaging), a generative framework built on foundational backbones with flow-based diffusion and ControlNet conditioning. Using NoMAISI, we curated a large multi-cohort lung nodule dataset and applied context-aware nodule volume augmentation, including relocation, shrinkage to simulate early-stage disease, and expansion to model progression. Each case was rendered into multiple synthetic variants, producing a diverse and anatomically consistent dataset. Fidelity was evaluated with cross-cohort similarity metrics, and downstream integration into lung nodule detection, and classification tasks demonstrated improved external test performance, particularly in underrepresented lesion categories. These results show that nodule-oriented synthetic imaging and curated augmentation can complement clinical data, reduce annotation demands, and expand the availability of training resources for healthcare AI.

## 🧩 Workflow Overview

The overall pipeline for organ, body, and nodule segmentation with alignment is shown below:

<p align="center">
  <img src="https://github.com/fitushar/NoMAISI/blob/main/doc/images/workflow.png" alt="Segmentation Pipeline"/>
</p>

**Workflow** for constructing the **NoMAISI** development dataset. The pipeline includes **(1)** organ segmentation using AI models, **(2)** body segmentation with algorithmic methods, **(3)** nodule segmentation through AI-assisted and ML-based refinement, and **(4)** segmentation alignment to integrate organs, body, and nodules segmentations into anatomically consistent volumes.

<p align="center">
  <img src="https://github.com/fitushar/NoMAISI/blob/main/doc/images/NoMAISI_train_and_infer.png" alt="NoMAISI_train_and_infer"/>
</p>

**Overview** of our flow-based latent diffusion model with ControlNet conditioning for AI-based CT generation. The pipeline consists of three stages: **(top) Pretrained VAE** for image compression, where CT images are encoded into latent features using a frozen VAE; **(middle)** Model fine-tuning, where a **Rectified Flow ODE sampler**, conditioned on segmentation masks and voxel spacing through a **fine-tuned ControlNet**, predicts velocity fields in latent space and is optimized with a region-specific contrastive loss emphasizing ROI sensitivity and background consistency; and **(bottom) Inference**, where segmentation masks and voxel spacing guide latent sampling along the ODE trajectory to obtain a clean latent representation, which is then decoded by the VAE into full-resolution AI-generated CT images conditioned by body and lesion masks.


## 📊 Dataset Composition

The table below summarizes the datasets included in this project, with their split sizes (Patients, CT scans, and Nodules) and the annotation types available.  

| Dataset          | Patients <br>n (%) | CT Scans <br>n (%) | Nodules <br>n (%) | Organ Seg | Nodule Seg | Nodule CCC | Nodule Box |
|------------------|---------------------|---------------------|-------------------|-----------|------------|------------|------------|
| **LNDbv4**       | 223 (3.17)          | 223 (2.52)          | 1132 (7.84)       | ✗         | ✓          | ✗          | ✓          |
| **NSCLC-R**      | 415 (5.89)          | 415 (4.69)          | 415 (2.87)        | ✗         | ✓          | ✗          | ✓          |
| **LIDC-IDRI**    | 870 (12.35)         | 870 (9.84)          | 2584 (17.89)      | ✗         | ✓          | ✓          | ✓          |
| **DLCS-24**      | 1605 (22.79)        | 1605 (18.15)        | 2478 (17.16)      | ✗         | ✓          | ✗          | ✓          |
| **Intgmultiomics** | 1936 (27.49)       | 1936 (21.90)        | 1936 (13.40)      | ✗         | ✓          | ✗          | ✗          |
| **LUNA-25**      | 1993 (28.30)        | 3792 (42.89)        | 5899 (40.84)      | ✗         | ✓          | ✗          | ✓          |
| **TOTAL**        | 7042 (100)          | 8841 (100)          | 14444 (100)       | —         | —          | —          | —          |

---
**Notes**  
- Percentages indicate proportion relative to the total for each column.  
- ✔︎ = annotation available, ✗ = annotation not available.  
- “Nodule CCC” = nodule center coordinates.  
- “Nodule Box” = bounding-box annotations.

### 📚 Dataset citations References
* LNDbv4 : [https://zenodo.org/records/8348419](https://zenodo.org/records/8348419)
* NSCLC-Radiomics : [https://www.cancerimagingarchive.net/collection/nsclc-radiogenomics/](https://www.cancerimagingarchive.net/collection/nsclc-radiogenomics/)
* LIDC-IDRI: [https://ieee-dataport.org/documents/lung-image-database-consortium-image-collection-lidc-idri](https://ieee-dataport.org/documents/lung-image-database-consortium-image-collection-lidc-idri)
* DLCS24: [https://zenodo.org/records/13799069](https://zenodo.org/records/13799069)
* Intgmultiomics: [M Zhao et. al, Nat.Commun(2025).](https://www.nature.com/articles/s41467-024-55594-z#citeas)
* LUNA25: [https://luna25.grand-challenge.org/](https://luna25.grand-challenge.org/)

# AI-Generated CT Evaluations 

### 📉 Fréchet Inception Distance (FID) Results

Fréchet Inception Distance (FID) of the **MAISI-v2** baseline and **NoMAISI** models with multiple public clinical datasets (test dataset) as the references (Lower is better).

| **FID (Avg.)**    | **LNDbv4** | **NSCLC-R** | **LIDC-IDRI** | **DLCS-24** | **Intgmultiomics** | **LUNA-25** |
|-------------------|------------|-------------|---------------|-------------|--------------------|-------------|
| **Real** LNDbv4        |   —    | 5.13 | 1.49 | 1.05 | 2.40 | 1.98 |
| **Real** NSCLC-R       | 5.13   |   —   | 3.12 | 3.66 | 1.56 | 2.65 |
| **Real** LIDC-IDRI     | 1.49   | 3.12 |   —   | 0.79 | 1.44 | 0.75 |
| **Real** DLCS-24       | 1.05   | 3.66 | 0.79 |   —   | 1.56 | 1.00 |
| **Real** Intgmultiomics| 2.40   | 1.56 | 1.44 | 1.56 |   —   | 1.57 |
| **Real** LUNA-25       | 1.98   | 2.65 | 0.75 | 1.00 | 1.57 |   —   |
| **AI-Generated** MAISI-V2 | 3.15 | 5.21 | 2.70 | 2.32 | 2.82 | 1.69 |
| **AI-Generated** NoMAISI (ours) | 2.99 | 3.05 | 2.31 | 2.27 | 2.62 | 1.18 |


### 📉 FID Parity Plot

<p align="left">
  <img src="https://github.com/fitushar/NoMAISI/blob/main/doc/images/GanAI_fid_scatter_marker_legend.png" alt="Parity comparison of FID for real↔real vs AI-generated CT across datasets" width="500">
</p>

**Comparison of Fréchet Inception Distance (FID) between real↔real and AI-generated CT datasets.**   Each point represents a clinical dataset (**LNDbv4, NSCLC-R, LIDC-IDRI, DLCS24, Intgmultiomics, LUNA25**) under different generative models (**MAISI-V2, NoMAISI**).The x-axis shows the **median FID** computed between real datasets, while the y-axis shows the **FID of AI-generated data** compared to real.  
The dashed diagonal line denotes **parity (y = x)**, where AI-generated fidelity would match real↔real fidelity.

### 🖼️ Example Results
**Comparison of CT generation from anatomical masks.**  
- **Left:** Input organ/body segmentation mask.  
- **Middle:** Generated CT slice using **MAISI-V2**.  
- **Right:** Generated CT slice using **NoMAISI (ours)**.  
- **Yellow boxes** highlight lung nodule regions for comparison.

<p align="center">
  <img src="https://github.com/fitushar/NoMAISI/blob/main/doc/images/DLCS_1419_ann0_slice134_triple.png" alt="Comparison of MAISI-V2 vs NoMAISI on lung CT with input masks" width="1000">
</p>
<p align="center">
  <img src="https://github.com/fitushar/NoMAISI/blob/main/doc/images/DLCS_1508_ann0_slice46_triple.png" alt="Comparison of MAISI-V2 vs NoMAISI on lung CT with input masks" width="1000">
</p>
<p align="center">
  <img src="https://github.com/fitushar/NoMAISI/blob/main/doc/images/DLCS_1453_ann0_slice204_triple.png" alt="Comparison of MAISI-V2 vs NoMAISI on lung CT with input masks" width="1000">
</p>

# Acknowledgements

We gratefully acknowledge the open-source projects that directly informed this repository: the [MAISI tutorial](https://github.com/Project-MONAI/tutorials/tree/main/generation/maisi) from the Project MONAI tutorials, the broader [Project MONAI ecosystem](https://github.com/Project-MONAI),
our related benchmark repo [AI in Lung Health – Benchmarking](https://github.com/fitushar/AI-in-Lung-Health-Benchmarking-Detection-and-Diagnostic-Models-Across-Multiple-CT-Scan-Datasets), 
and our companion toolkits [PiNS – Point-driven Nodule Segmentation](https://github.com/fitushar/PiNS)
 and [CaNA – Context-Aware Nodule Augmentation](https://github.com/fitushar/CaNA). We thank these communities and contributors for their exceptional open-source efforts. If you use our models or code, please also consider citing these works (alongside this repository) to acknowledge their contributions.
