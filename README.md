# multimodal-survival-framework

> **Generalizable clinical AI framework for survival prediction using contrastive representation learning.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/python-3.9%2B-blue.svg)](https://www.python.org/)

---

## Project Overview

`multimodal-survival-framework` is a research-grade machine learning project focused on building a generalizable clinical AI framework for **survival prediction** using **contrastive representation learning**. The framework is designed with modularity and reproducibility at its core, enabling rapid iteration across datasets, model architectures, and loss functions.

The first version targets **dementia incidence prediction** using VA (Veterans Affairs) structured clinical data. The architecture is intentionally designed to accommodate future multimodal inputs — including clinical notes (NLP) and medical imaging — without requiring a full rewrite.

---

## Research Objective

Develop and validate a contrastive survival model that:

1. Learns clinically meaningful patient representations from structured EHR data.
2. Outperforms classical survival baselines (Cox PH, RSF, DeepSurv) on time-to-dementia prediction.
3. Generalizes across patient subgroups with varying follow-up durations and censoring rates.
4. Provides a modular backbone ready for multimodal fusion in future work.

---

## Repository Structure

```
multimodal-survival-framework/
├── data/               # Raw and processed data (not committed; see .gitignore)
├── notebooks/          # Exploratory analysis and result visualization notebooks
├── configs/            # YAML experiment configuration files
├── src/
│   ├── data/           # Dataset loaders, preprocessing pipelines
│   ├── models/         # Model architectures (baselines + contrastive-survival)
│   ├── losses/         # Survival and contrastive loss functions
│   ├── training/       # Training loops, optimizers, schedulers
│   └── evaluation/     # Metrics (C-index, Brier score, calibration)
├── scripts/            # Entry-point scripts for training and evaluation
├── results/            # Saved metrics, plots, model checkpoints (not committed)
├── docs/               # Extended documentation and design notes
├── .gitignore
├── LICENSE
└── README.md
```

---

## Setup Instructions

> _Full setup guide coming soon. Placeholder below._

```bash
# 1. Clone the repository
git clone https://github.com/<your-org>/multimodal-survival-framework.git
cd multimodal-survival-framework

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Linux / macOS
# venv\Scripts\activate         # Windows

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure your experiment
cp configs/example_config.yaml configs/my_experiment.yaml
# Edit configs/my_experiment.yaml as needed

# 5. Run training
python scripts/train.py --config configs/my_experiment.yaml
```

---

## Experiment Roadmap

> _Detailed roadmap coming soon. High-level milestones below._

| Phase | Description | Status |
|-------|-------------|--------|
| 1 | Data preprocessing & EDA on VA structured data | 🔲 Planned |
| 2 | Baseline survival models (Cox PH, RSF, DeepSurv) | 🔲 Planned |
| 3 | Contrastive survival model — initial prototype | 🔲 Planned |
| 4 | Hyperparameter tuning & ablation studies | 🔲 Planned |
| 5 | Subgroup analysis & fairness evaluation | 🔲 Planned |
| 6 | Multimodal extension (clinical notes) | 🔲 Future |
| 7 | Multimodal extension (medical imaging) | 🔲 Future |

---

## Future Multimodal Extension

The framework is architected to support additional input modalities beyond structured tabular data:

- **Clinical notes**: A text encoder (e.g., ClinicalBERT, BioClinicalBERT) can be plugged into `src/models/` and fused with the structured encoder via a cross-modal contrastive objective defined in `src/losses/`.
- **Medical imaging**: CNN or Vision Transformer encoders can be integrated following the same modular fusion interface.
- **Multimodal contrastive pretraining**: Future experiments will explore cross-modal contrastive pretraining to align patient representations across modalities before fine-tuning on the survival objective.

---

## License

This project is licensed under the [MIT License](LICENSE).
