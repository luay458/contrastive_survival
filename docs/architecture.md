# Architecture Overview

## Purpose

This document defines the initial technical architecture for the multimodal survival framework. The framework is designed to support clinical time-to-event prediction using modular data adapters, representation encoders, survival heads, and evaluation pipelines.

Version 1 focuses on VA structured clinical data. Future versions will extend to clinical notes, imaging, and other modalities.

---

## High-Level Design

The framework is organized into five main layers:

1. Data Layer
2. Representation Layer
3. Learning Objective Layer
4. Prediction Layer
5. Evaluation Layer

The design goal is to keep each layer modular so that new modalities, losses, and downstream tasks can be added with minimal code changes.

---

## 1. Data Layer

### Role
The data layer standardizes raw clinical data into a common format for training and evaluation.

### Initial input
Version 1 uses structured VA clinical data with:
- patient identifier
- observation start date
- event date
- censor date
- event indicator
- baseline covariates
- utilization and risk features

### Data adapter responsibilities
Each adapter should:
- load raw data
- validate required columns
- handle missing values
- encode categorical variables
- normalize continuous variables
- generate train/validation/test splits
- construct multiple patient views for contrastive learning

### Initial adapter
`StructuredVAAdapter`

### Planned future adapters
- `ClinicalNotesAdapter`
- `ImagingAdapter`
- `MultimodalFusionAdapter`

---

## 2. Representation Layer

### Role
The representation layer transforms patient data into dense embeddings that can be used for downstream survival prediction.

### Version 1 encoder
The first encoder will be a multilayer perceptron (MLP) applied to structured clinical features.

Example flow:

Structured feature vector  
→ encoder network  
→ latent embedding  
→ projection head

### Components

#### Encoder
Maps input features into a latent patient representation.

Initial model:
- input layer
- hidden layer 1
- hidden layer 2
- embedding layer

#### Projection head
Maps the latent embedding into a contrastive space used by the InfoNCE loss.

This separation allows:
- one embedding for downstream prediction
- one projection for contrastive training

### Future encoders
- Transformer-based encoder for longitudinal structured data
- Clinical language model encoder for notes
- CNN or vision transformer encoder for imaging

---

## 3. Learning Objective Layer

### Role
This layer defines how the model is trained.

The framework combines:
- survival prediction loss
- contrastive representation loss

### Joint objective

Total loss:

L_total = L_survival + λ * L_contrastive

Where:
- `L_survival` is the time-to-event prediction loss
- `L_contrastive` is the InfoNCE loss
- `λ` controls the strength of the contrastive term

---

### 3.1 Survival Loss

Initial implementation:
- Cox partial likelihood loss

Possible future implementations:
- discrete-time hazard loss
- DeepHit-style loss
- competing risks objective

### 3.2 Contrastive Loss

Initial implementation:
- InfoNCE loss

Positive pairs:
- two views of the same patient

Negative pairs:
- views from different patients in the same batch

### 3.3 Survival-Aware Contrastive Extension

This is the main methodological direction of the flagship project.

The contrastive term will be modified to account for survival information such as:
- event indicator
- censoring status
- follow-up duration
- time proximity

Possible strategies:
- weight contrastive loss more heavily for observed events
- assign pair weights based on event-time similarity
- reduce contribution of weakly informative censored pairs

This creates a survival-aware representation learning objective.

---

## 4. Prediction Layer

### Role
This layer takes learned patient embeddings and produces downstream predictions.

### Initial task
Incident dementia time-to-event prediction.

### Prediction heads

#### Survival head
Uses patient embeddings to estimate survival risk.

Initial version:
- linear Cox head on top of learned embedding

Alternative future heads:
- neural Cox head
- discrete hazard prediction head
- multitask head for multiple related outcomes

### Future downstream tasks
- dementia + MCI prediction
- transfer learning across related outcomes
- horizon-specific risk prediction
- subgroup-specific calibration

---

## 5. Evaluation Layer

### Role
This layer measures model performance, robustness, and clinical usefulness.

### Core metrics

#### Discrimination
- Concordance index
- Time-dependent AUC

#### Calibration
- Brier score
- Expected calibration error
- calibration plots

#### Robustness
- subgroup performance by age, sex, race, and ethnicity
- sensitivity to missing features
- ablation across feature views

### Benchmarking strategy
Each experimental run should compare:
- baseline survival models
- deep survival models
- contrastive + survival models

This ensures the new method is evaluated against strong baselines.

---

## Patient View Construction for Contrastive Learning

Contrastive learning requires at least two views of the same patient.

### Version 1 approach
Views will be constructed from structured data by partitioning or augmenting feature subsets.

Possible examples:

### View A
- demographics
- comorbidities

### View B
- utilization
- social determinants
- summary risk variables

Alternative augmentations:
- feature masking
- stochastic dropout of variables
- noise injection for continuous features

The objective is to preserve patient identity while providing distinct but related views.

---

## Training Workflow

### Phase 1: Baseline modeling
- prepare clean dataset
- fit Cox and other baseline survival models
- record benchmark results

### Phase 2: Representation learning
- generate patient views
- train encoder with contrastive + survival loss
- evaluate learned embeddings

### Phase 3: Ablation studies
- compare survival-only vs joint training
- compare multiple λ values
- compare view construction strategies
- test survival-aware weighting methods

### Phase 4: Extension
- add notes and imaging encoders
- evaluate multimodal fusion approaches

---

## Modularity Principles

The framework should follow these design rules:

1. Data adapters are independent of model code.
2. Encoders can be swapped without changing the training loop.
3. Loss functions are modular and configurable.
4. Prediction heads are task-specific but reusable.
5. Evaluation scripts are standardized across experiments.

This structure is essential if the framework is going to become a reusable research platform.

---

## Initial File Mapping

Suggested implementation structure:

- `src/data/adapters/va_structured.py`
- `src/models/encoders/mlp.py`
- `src/models/heads/survival_cox.py`
- `src/losses/survival_contrastive.py`
- `src/training/train_baseline.py`
- `src/training/train_contrastive_survival.py`
- `src/evaluation/survival_metrics.py`

---

## Near-Term Technical Priorities

1. Finalize structured VA data schema
2. Implement baseline survival models
3. Implement MLP encoder
4. Implement InfoNCE loss
5. Integrate Cox survival loss
6. Test first joint training pipeline
7. Produce first benchmark table

---

## Long-Term Vision

The long-term architecture will support:
- structured data
- clinical notes
- medical imaging
- multimodal fusion
- transferable patient representations
- survival and risk prediction across multiple clinical outcomes

The end goal is a generalizable clinical AI framework for longitudinal prediction that is technically rigorous, modular, and reusable across healthcare applications.
