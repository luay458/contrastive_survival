# Flagship Project: Multimodal Survival Framework

## Project Goal
Develop a generalizable machine learning framework for clinical risk prediction using contrastive representation learning combined with survival modeling. The framework will learn robust patient representations from clinical data and improve time-to-event prediction performance compared to traditional survival models.

The long-term goal is to support multimodal inputs including structured clinical data, clinical notes, and medical imaging.

---

## Initial Dataset (Version 1)

**Source:** VA structured clinical dataset

**Data Type:** Tabular longitudinal clinical data

**Core elements:**
- Patient identifiers
- Observation start date
- Event date
- Censor date
- Event indicator

**Initial covariates include:**

Demographics
- age
- sex
- race
- ethnicity
- marital_status

Social determinants
- URBAN_RURAL
- DISTRESS_SCORE_BIN

Clinical comorbidities
- diabetes
- cardiovascular_disease
- stroke
- mdd
- schizophrenia
- tbi
- hearing_impairment

Substance use
- tobacco
- alcohol
- drugs

Healthcare utilization
- avg_pc_visits_per_year
- cci_score

---

## Prediction Task

Primary outcome (v1):

**Incident dementia**

Task type:
Time-to-event prediction using survival modeling.

Prediction horizons:
- 1 year
- 3 years
- 5 years

---

## Baseline Models

Initial models used for benchmarking:

- Cox Proportional Hazards
- Regularized Cox Model
- Random Survival Forest
- Deep Survival Network (DeepSurv or equivalent)

These models establish baseline performance for comparison.

---

## Proposed Method

The proposed approach combines **contrastive representation learning** with **survival prediction**.

Key idea:
Learn patient representations using contrastive learning between multiple views of the same patient.

Examples of views:
- demographic + comorbidity features
- utilization + social determinant features
- augmented feature subsets

The learned representation is then used for survival prediction.

Training objective:

Total loss =

Survival loss  
(Cox partial likelihood or discrete-time survival loss)

+

Contrastive loss  
(InfoNCE objective applied to patient representation pairs)

The contrastive component encourages meaningful patient embeddings that improve downstream survival prediction.

---

## Evaluation Metrics

Model performance will be evaluated using:

Discrimination
- Concordance index (C-index)
- Time-dependent AUC

Calibration
- Brier score
- Expected calibration error (ECE)

Robustness
- performance across demographic subgroups
- sensitivity to missing features

---

## Expected Contributions

1. A modular machine learning framework for survival prediction using contrastive learning.

2. A survival-aware representation learning approach for clinical data.

3. Empirical evaluation demonstrating improved risk prediction compared to standard survival models.

4. A reusable open-source pipeline supporting extension to multimodal clinical data.

---

## Future Extensions

The framework will later incorporate additional modalities:

Clinical notes
- embeddings from clinical language models

Medical imaging
- ophthalmic imaging
- multimodal fusion

Genomic or biomarker data

The final system will support **multimodal clinical representation learning for longitudinal risk prediction**.

---

## Immediate Development Milestones

Phase 1
- Build clean VA structured dataset
- Implement baseline survival models

Phase 2
- Implement contrastive representation encoder
- train survival + contrastive objective

Phase 3
- Evaluate representation transfer across prediction tasks

Phase 4
- Extend framework to multimodal inputs
