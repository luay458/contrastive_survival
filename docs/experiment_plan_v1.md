# Experiment Plan V1
Multimodal Survival Framework

This document defines the first experimental plan for the flagship project.  
The objective is to establish strong baselines and test the first version of the contrastive survival model using VA structured clinical data.

---

# 1. Dataset Definition

## Data Source
VA structured clinical dataset.

## Unit of Analysis
Patient-level survival prediction.

## Outcome
Incident dementia.

## Time-to-event definition

Start time:
index_date

Event time:
dementia_diagnosis_date

Censoring time:
last_contact_date

Event indicator:
1 = dementia diagnosis occurred  
0 = censored

---

# 2. Feature Set (Version 1)

Initial feature groups:

## Demographics
- age
- sex
- race
- ethnicity
- marital_status

## Social Determinants
- URBAN_RURAL
- DISTRESS_SCORE_BIN

## Comorbidities
- diabetes
- cardiovascular_disease
- stroke
- mdd
- schizophrenia
- tbi
- hearing_impairment

## Substance Use
- tobacco
- alcohol
- drugs

## Healthcare Utilization
- avg_pc_visits_per_year
- cci_score

---

# 3. Dataset Splitting Strategy

Use **patient-level splitting** to avoid leakage.

Train: 70%  
Validation: 15%  
Test: 15%

Alternative future split:

Temporal split  
(train on earlier cohorts, test on later cohorts)

---

# 4. Baseline Models

The first stage establishes benchmark performance.

## Model 1
Cox Proportional Hazards

## Model 2
Regularized Cox Model

## Model 3
Random Survival Forest

## Model 4
Deep Survival Network (DeepSurv)

These models define the baseline performance for the dataset.

---

# 5. Baseline Evaluation Metrics

Evaluate all models using:

## Discrimination
- Concordance Index (C-index)
- Time-dependent AUC

## Calibration
- Brier score
- Calibration plots

## Robustness
- performance across age groups
- performance across sex and race

---

# 6. Contrastive Representation Model

After baselines are established, the first contrastive model will be implemented.

## Encoder

MLP encoder applied to structured clinical features.

Example architecture:

input layer  
hidden layer (128)  
hidden layer (64)  
embedding layer (32)

This produces a patient representation vector.

---

## Patient Views for Contrastive Learning

Two views will be created for each patient.

### View A
- demographics
- comorbidities

### View B
- social determinants
- utilization features

These two views represent the same patient and form positive contrastive pairs.

---

# 7. Training Objective

Joint loss function:

Total Loss =

Survival Loss  
(Cox partial likelihood)

+

Contrastive Loss  
(InfoNCE)

L_total = L_survival + λ * L_contrastive

Where λ controls the contrastive weight.

---

# 8. Initial Experiments

Experiment 1  
Baseline Cox model

Experiment 2  
Deep survival model

Experiment 3  
Contrastive encoder + Cox survival head

Experiment 4  
Contrastive encoder + neural survival head

---

# 9. Ablation Studies

After initial experiments:

Test the effect of:

- different embedding sizes
- different λ values
- different view construction strategies
- feature masking augmentations

---

# 10. Result Reporting

Each experiment should report:

| Model | C-index | Time-AUC | Brier Score |
|------|------|------|------|

Results will be stored in:

results/experiment_logs/

Plots will include:

- survival calibration curves
- C-index comparison plots
- subgroup performance charts

---

# 11. Milestones

Milestone 1  
Clean dataset + baseline Cox results

Milestone 2  
Deep survival baseline results

Milestone 3  
First contrastive survival model

Milestone 4  
Ablation experiments

Milestone 5  
Draft benchmark results for paper

---

# 12. Success Criteria

The first milestone of the flagship project is achieved when:

1. The contrastive survival model matches or improves baseline C-index.
2. The experiment pipeline is reproducible.
3. Results can be regenerated using configuration files.
