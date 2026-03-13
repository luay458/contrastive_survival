# Dataset Schema V1
Multimodal Survival Framework

This document defines the canonical dataset schema for Version 1 of the flagship project.

The purpose of this schema is to:
- standardize the first experimental dataset
- document required and optional variables
- prevent data drift during model development
- support reproducible preprocessing and benchmarking

---

# 1. Dataset Overview

## Dataset Version
`va_survival_dataset_v1`

## Data Source
VA structured clinical dataset

## Unit of Analysis
One row per patient

## Primary Task
Time-to-event prediction for incident dementia

---

# 2. Required Columns

The following columns are required for all Version 1 experiments.

## 2.1 Identifier

| Column Name | Type | Required | Description |
|------------|------|----------|-------------|
| PatientICN | string | Yes | Unique patient identifier |

---

## 2.2 Time-to-Event Fields

| Column Name | Type | Required | Description |
|------------|------|----------|-------------|
| index_date | date | Yes | Start of follow-up |
| dementia_diagnosis_date | date | No | Date of dementia diagnosis if event occurred |
| last_contact_date | date | Yes | Last observed follow-up date |
| dementia | integer | Yes | Event indicator: 1 = dementia occurred, 0 = censored |

### Time-to-event derivation rules

- If `dementia = 1`, then `dementia_diagnosis_date` must be present.
- If `dementia = 0`, then `dementia_diagnosis_date` should be null.
- Survival time is calculated from `index_date` to:
  - `dementia_diagnosis_date` if event occurred
  - `last_contact_date` if censored

---

# 3. Predictor Variables

## 3.1 Demographics

| Column Name | Type | Required | Description |
|------------|------|----------|-------------|
| age | numeric | Yes | Age at index date |
| sex | categorical | Yes | Biological sex |
| race | categorical | Yes | Race category |
| ethnicity | categorical | Yes | Ethnicity category |
| marital_status | categorical | No | Marital status at baseline |

---

## 3.2 Social Determinants

| Column Name | Type | Required | Description |
|------------|------|----------|-------------|
| URBAN_RURAL | categorical | No | Rural/urban classification |
| DISTRESS_SCORE_BIN | categorical | No | Distressed Communities Index bin |

---

## 3.3 Comorbidities

| Column Name | Type | Required | Description |
|------------|------|----------|-------------|
| diabetes | integer | Yes | 0/1 indicator |
| cardiovascular_disease | integer | Yes | 0/1 indicator |
| stroke | integer | Yes | 0/1 indicator |
| mdd | integer | Yes | 0/1 indicator |
| schizophrenia | integer | Yes | 0/1 indicator |
| tbi | integer | Yes | 0/1 indicator |
| hearing_impairment | integer | Yes | 0/1 indicator |

---

## 3.4 Substance Use

| Column Name | Type | Required | Description |
|------------|------|----------|-------------|
| tobacco | integer | Yes | 0/1 indicator |
| alcohol | integer | Yes | 0/1 indicator |
| drugs | integer | Yes | 0/1 indicator |

---

## 3.5 Healthcare Utilization

| Column Name | Type | Required | Description |
|------------|------|----------|-------------|
| avg_pc_visits_per_year | numeric | Yes | Average primary care visits per year |
| cci_score | numeric | Yes | Charlson Comorbidity Index or equivalent summary score |

---

# 4. Optional Columns

These columns may be present in the source dataset but are not required for Version 1 benchmarking.

| Column Name | Type | Description |
|------------|------|-------------|
| start_date | date | Alternate name for index_date |
| end_date | date | Alternate name for last_contact_date |
| dementia_mci | integer | Optional future outcome |
| dementia_mci_diagnosis_date | date | Optional future outcome date |
| mci | integer | Optional secondary outcome |
| mci_diagnosis_date | date | Optional secondary outcome date |
| cataract | integer | Not used in Version 1 |
| cataract surgery fields | mixed | Not used in Version 1 |

---

# 5. Derived Fields for Modeling

The following fields should be created during preprocessing.

| Derived Column | Type | Description |
|---------------|------|-------------|
| event_time_days | numeric | Days from index_date to event or censoring |
| event_time_years | numeric | Years from index_date to event or censoring |
| event_observed | integer | Copy of dementia event indicator |
| split | categorical | train / val / test assignment |

---

# 6. Data Validation Rules

The preprocessing pipeline must validate the following:

1. `PatientICN` must be unique.
2. `index_date` must be present.
3. `last_contact_date` must be present.
4. `last_contact_date >= index_date`.
5. If `dementia = 1`, then `dementia_diagnosis_date >= index_date`.
6. If `dementia = 1`, then `dementia_diagnosis_date <= last_contact_date`.
7. Binary indicator fields should contain only 0 or 1.
8. Continuous variables should be numeric and within valid ranges.

---

# 7. Missing Data Policy

Initial Version 1 policy:

- Required time-to-event fields: no missing values allowed
- Binary comorbidity indicators: missing values should be investigated before imputation
- Categorical predictors: encode missing as explicit category if needed
- Continuous predictors: use a documented imputation strategy or complete-case filtering

The missing-data strategy must be consistent across all baseline and contrastive experiments.

---

# 8. Canonical Version 1 Modeling Dataset

The final modeling dataset for Version 1 should contain:

- one row per patient
- required survival columns
- fixed baseline predictor set
- derived event-time fields
- no ambiguous column naming

Recommended export name:

`va_survival_dataset_v1.csv`

---

# 9. Notes

This schema defines the initial structured-data version of the flagship project.

Future versions may extend this schema to include:
- longitudinal repeated visits
- clinical note embeddings
- imaging features
- multimodal fusion metadata
