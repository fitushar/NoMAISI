# NoMAISI 

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


# NoMAISI: Nodule-Oriented Medical AI for Synthetic Imaging and Augmentation in Chest CT
NoMAISI (Nodule-Oriented Medical AI for Synthetic Imaging), a generative framework built on foundational backbones with flow-based diffusion and ControlNet conditioning.



# Abstract
Medical imaging datasets are increasingly available, yet abnormal and annotation-intensive cases such as lung nodules remain underrepresented. We developed NoMAISI (Nodule-Oriented Medical AI for Synthetic Imaging), a generative framework built on foundational backbones with flow-based diffusion and ControlNet conditioning. Using NoMAISI, we curated a large multi-cohort lung nodule dataset and applied context-aware nodule volume augmentation, including relocation, shrinkage to simulate early-stage disease, and expansion to model progression. Each case was rendered into multiple synthetic variants, producing a diverse and anatomically consistent dataset. Fidelity was evaluated with cross-cohort similarity metrics, and downstream integration into lung nodule detection, and classification tasks demonstrated improved external test performance, particularly in underrepresented lesion categories. These results show that nodule-oriented synthetic imaging and curated augmentation can complement clinical data, reduce annotation demands, and expand the availability of training resources for healthcare AI.
