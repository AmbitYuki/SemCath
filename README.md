<div align="center">

# SemCath

### **Sem**antic-Guided **Cath**eterization Reconstruction

**Semantic-Guided 3D Reconstruction from 2D Fluoroscopy via Multimodal Language Models for Interventional Navigation**

---

[![Paper](https://img.shields.io/badge/Paper-Medical%20Image%20Analysis-blue?style=flat-square&logo=elsevier)](https://www.sciencedirect.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.8+-yellow?style=flat-square&logo=python)](https://www.python.org)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-red?style=flat-square&logo=pytorch)](https://pytorch.org)
[![SOFA](https://img.shields.io/badge/Simulation-SOFA%20Framework-orange?style=flat-square)](https://www.sofa-framework.org)

</div>

---

## 📖 Overview

**SemCath** reconceptualizes interventional 3D reconstruction as a **medical reasoning problem** rather than a geometric inverse problem. By combining domain-adapted multimodal large language models (MLLMs) with physics-informed neural rendering, SemCath bridges the semantic gap between 2D fluoroscopic observations and 3D anatomical understanding — enabling real-time, anatomically plausible coronary reconstruction during percutaneous cardiovascular intervention.

<div align="center">

```
2D Fluoroscopic Sequence
         │
         ▼
┌─────────────────────────┐
│  Medical Scene          │  ← Domain-adapted MLLM
│  Understanding (MSU)    │    + Medical Knowledge Graph
└────────────┬────────────┘
             │  Semantic Description
             ▼
┌─────────────────────────┐
│  Semantic-to-Geometry   │  ← Knowledge Graph-Constrained
│  Translation Engine     │    Neural Field Synthesis
└────────────┬────────────┘
             │  3D Geometric Primitives
             ▼
┌─────────────────────────┐
│  Adaptive Neural        │  ← Semantic-Weighted Rendering
│  Rendering System       │    + Uncertainty Quantification
└────────────┬────────────┘
             │
             ▼
   3D Anatomical Reconstruction
```

</div>

---

## ✨ Key Contributions

- **Semantic Primacy**: Medical reasoning drives geometric reconstruction — not the reverse
- **Semantic-to-Geometry Translation**: Novel mechanism mapping clinical natural language concepts (e.g., *"moderate eccentric stenosis in mid-LAD"*) to parameterized 3D geometric primitives via a medical knowledge graph
- **Anatomical Plausibility Constraints**: Differentiable constraints enforcing Murray's law, vessel topology, and morphological consistency during neural field synthesis
- **Uncertainty-Aware Rendering**: Variational inference models both aleatoric and epistemic uncertainty, providing confidence-weighted reconstructions for clinical decision support
- **Real-Time Performance**: 278 ms per frame on NVIDIA RTX A6000 — clinically feasible for intraoperative guidance

---

## 📊 Results

Evaluated against 9 baselines on a SOFA-simulated cardiovascular intervention dataset with 5-fold cross-validation:

| Metric | Best Baseline | **SemCath** | Improvement |
|--------|:-------------:|:-----------:|:-----------:|
| Anatomical Consistency (ACS) ↑ | 0.812 | **0.887** | +9.2% |
| Centerline Deviation (CDE) ↓ | 2.28 mm | **1.94 mm** | −14.9% |
| Volumetric Overlap (VOC) ↑ | 0.854 | **0.896** | +4.9% |
| Temporal Consistency (TCM) ↑ | 0.837 | **0.891** | +6.5% |
| Pathology Recognition (PCRA) ↑ | 0.623 | **0.791** | +27.0% |
| Anatomical Plausibility (API) ↑ | 0.784 | **0.876** | +11.7% |
| Inference Time ↓ | 287.9 ms | **278.3 ms** | ✅ Real-time |

> All improvements statistically significant: paired Wilcoxon signed-rank test with Bonferroni correction, *p* < 0.01.

---

## 🗂️ Repository Structure

```
SemCath/
├── networks/           # Model architectures (MLLM encoder, neural field, renderer)
├── function/           # Core framework functions and utilities
├── baseline/           # Baseline method implementations for comparison
├── intervention/       # Interventional procedure simulation and environment
├── observation/        # Fluoroscopic observation processing modules
├── reward/             # Training objective and loss components
├── start/              # Entry points and experiment launchers
├── training_scripts/   # Training, validation, and evaluation scripts
├── util/               # Utility functions, metrics, and visualization tools
└── setup.py            # Installation and dependency configuration
```

---

## 🚀 Getting Started

### Prerequisites

```bash
Python >= 3.8
PyTorch >= 2.0
CUDA >= 11.7
SOFA Framework (for simulation environment)
```

### Installation

```bash
git clone https://github.com/AmbitYuki/SemCath.git
cd SemCath
pip install -e .
```

### Training

```bash
# Stage 1: Pre-train Medical Scene Understanding Module
python training_scripts/train_msu.py --config configs/stage1.yaml

# Stage 2: Train Semantic-to-Geometry Translation Engine
python training_scripts/train_s2g.py --config configs/stage2.yaml

# Stage 3: End-to-end fine-tuning with adaptive rendering
python training_scripts/train_full.py --config configs/stage3.yaml
```

### Evaluation

```bash
python training_scripts/evaluate.py --checkpoint checkpoints/semcath_final.pth
```

---

## 🏥 Simulation Environment

SemCath is trained and evaluated on a high-fidelity simulation platform built upon [SOFA Framework](https://www.sofa-framework.org), featuring:

- **Patient-specific vascular geometries** reconstructed from clinical CT angiography
- **Physiologically calibrated biomechanics**: Young's modulus 1.0–8.0 MPa, cardiac displacement 3–8 mm
- **Pathological diversity**: stenosis severity 30–90%, calcification density 800–2000 HU, curvature 0.1–0.6 mm⁻¹
- **Realistic imaging chain**: polychromatic X-ray spectra, scatter modeling, contrast agent dynamics

---

## 📋 Citation

If you find SemCath useful in your research, please cite:

[Under Review....]
---

## 👥 Authors

| Name | Affiliation |
|------|-------------|
| Xihe Qiu | Shanghai University of Engineering Science; National University of Singapore |
| Haoyu Wang | Shanghai University of Engineering Science; Tongji University |
| Xiaoyu Tan | National University of Singapore; Tencent Youtu Lab |
| Lizhi Yang | Preclinic Medtech (Shanghai) Co., Ltd. |
| Chong Wang | Preclinic Medtech (Shanghai) Co., Ltd. |
| Liang Liu* | Zhongshan Hospital, Fudan University |
| Xiaofei Zhang* | Zhongshan Hospital, Fudan University |
| Lu Gan* | Zhongshan Hospital, Fudan University |

*Corresponding authors

---

## 🙏 Acknowledgements

This work is supported by the **Shanghai Municipal Natural Science Foundation** (Grant No. 23ZR1425400). We thank the SOFA Framework community for their excellent open-source simulation tools.

---

<div align="center">

**Key Laboratory of Carcinogenesis and Cancer Invasion, Ministry of Education**
Zhongshan Hospital, Fudan University · Shanghai University of Engineering Science · Tongji University · National University of Singapore · Tencent Youtu Lab · Preclinic Medtech

<br>

⭐ If you find this work helpful, please consider starring the repository.

</div>
