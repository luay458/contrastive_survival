# Dataset Mapping V1

## Canonical dataset choice
- Outcome: dementia
- Lookback period: 1 year
- Unit of analysis: one row per patient

---

## Identifier

| Source Column | Canonical Name | Use in V1 | Notes |
|---|---|---:|---|
| patienticn | PatientICN | Yes | rename to standard casing |

---

## Time-to-event fields

| Source Column | Canonical Name | Use in V1 | Notes |
|---|---|---:|---|
| index_date | index_date | Yes | follow-up start |
| dementia_diagnosis_date | dementia_diagnosis_date | Yes | event date if dementia=1 |
| study_end_date | last_contact_date | Yes | use as censoring/end-of-follow-up field for v1 |
| dementia | dementia | Yes | event indicator |
| time_to_event | event_time_years or event_time_days | Optional | verify units before using directly |
| follow_up_years | follow_up_years | Optional | keep for QA only |

---

## Demographics

| Source Column | Canonical Name | Use in V1 | Notes |
|---|---|---:|---|
| age | age | Yes | |
| sex | sex | Yes | |
| race | race | Yes | |
| ethnicity | ethnicity | Yes | |
| marital_status | marital_status | Yes | |

---

## Social determinants

| Source Column | Canonical Name | Use in V1 | Notes |
|---|---|---:|---|
| URBAN_RURAL | URBAN_RURAL | Yes | |
| DISTRESS_SCORE | DISTRESS_SCORE | Optional | raw continuous value |
| median_income | median_income | Optional | not in core v1 |
| mean_income | mean_income | Optional | not in core v1 |
| AdjustedIncome2023 | AdjustedIncome2023 | Optional | not in core v1 |
| occupation | occupation | Optional | not in core v1 |
| household_size | household_size | Optional | not in core v1 |
| Education | Education | Optional | not in core v1 |
| Zip | Zip | No | avoid in v1 |
| zip_not_found_flag | zip_not_found_flag | No | QA only |

---

## Comorbidities / clinical variables

| Source Column | Canonical Name | Use in V1 | Notes |
|---|---|---:|---|
| tbi | tbi | Yes | |
| stroke | stroke | Yes | |
| schizophrenia | schizophrenia | Yes | |
| mdd | mdd | Yes | |
| diabetes | diabetes | Yes | |
| cardiovascular_disease | cardiovascular_disease | Yes | |
| hearing_impairment | hearing_impairment | Yes | |
| cci_score | cci_score | Yes | |
| fi | fi | Optional | leave out unless clearly defined in project docs |

---

## Substance use

| Source Column | Canonical Name | Use in V1 | Notes |
|---|---|---:|---|
| alcohol | alcohol | Yes | |
| tobacco | tobacco | Yes | |
| drugs | drugs | Yes | |

---

## Healthcare utilization

| Source Column | Canonical Name | Use in V1 | Notes |
|---|---|---:|---|
| total_pc_visits | total_pc_visits | Optional | QA / secondary |
| avg_pc_visits_per_year | avg_pc_visits_per_year | Yes | |
| latest_pc_date | latest_pc_date | Optional | QA only |

---

## Cataract-related columns

| Source Column | Canonical Name | Use in V1 | Notes |
|---|---|---:|---|
| cataract_dx | cataract_dx | No | exclude from flagship v1 |
| first_cataract_dx_date | first_cataract_dx_date | No | exclude |
| cataract | cataract | No | exclude |
| first_cataract_surgery | first_cataract_surgery | No | exclude |
| second_cataract_surgery | second_cataract_surgery | No | exclude |
| all_catarct_dates | all_catarct_dates | No | exclude |

---

## Administrative / QA fields

| Source Column | Canonical Name | Use in V1 | Notes |
|---|---|---:|---|
| study_start_date | study_start_date | Optional | QA only |
| rn | rn | No | drop |
| dob | dob | No | drop from modeling file if age already exists |
