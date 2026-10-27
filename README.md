# ==========================================
# Project:
From Metastatic Progression to Prognosis: Multi-Cohort Transcriptomic Replication, Multi-Omics Prioritization, Single-Cell Localization, and Pre-Specified External Validation of a Machine-Learning Model in Cutaneous Melanoma

# Task:

T5.4.1 - Develop and Internally Evaluate the Baseline Clinical Cox Model

# Input Files:

- `261017_T5.1.1_Survival_SAP_Lock_Results_Ameneh_Hashemzehi_Human.rds`
- `261017_T5.1.2_TCGA_Development_Data_Results_Ameneh_Hashemzehi_Human.rds`
- `261021_T5.2.3_Missing_Data_Model_Ameneh_Hashemzehi_Human.rds`
- `261021_T5.2.3_Missing_Data_Clinical_Data_Ameneh_Hashemzehi_Human.rds`
- `261021_T5.3.1_Nested_CV_Lock_Results_Ameneh_Hashemzehi_Human.rds`

# Output Files:

- `261023_T5.4.1_Clinical_Model_Results_Narges_Omidian_Human.csv`
- `261023_T5.4.1_Clinical_Model_Results_Narges_Omidian_Human.rds`
- `261023_T5.4.1_Clinical_Model_Table_OOF_Predictions_Narges_Omidian_Human.csv`
- `261023_T5.4.1_Clinical_Model_Table_Fold_Performance_Narges_Omidian_Human.csv`
- `261023_T5.4.1_Clinical_Model_Table_Cox_Diagnostics_Narges_Omidian_Human.csv`
- `261023_T5.4.1_Clinical_Model_Script_Narges_Omidian_Human.R`
- `261023_T5.4.1_Clinical_Model_README_Narges_Omidian_Human.md`
- `261023_T5.4.1_Clinical_Model_Session_Info_Narges_Omidian_Human.txt`
- `261023_T5.4.1_Clinical_Model_Execution_Log_Narges_Omidian_Human.txt`

# File Storage and Availability:

**Milestone folder:** `05_Survival_Data_Preparation_Feature_Selection_and_Prognostic_Model_Development`  
**Zenodo:** [Open milestone folder](https://zenodo.org/uploads/21855729)

# Author:

Narges Omidian

# Date:

20261023
# ==========================================
## Overview

A baseline clinical Cox model using age and sex was evaluated in 337 TCGA-SKCM metastatic melanoma patients (179 events; 158 censored) using the five locked outer folds from T5.3.1.

## Methods

The model `Surv(OBS_Days, Event) ~ Age_at_Diagnosis_Years + Sex` was fitted independently in each outer training fold using Efron ties. Stage was excluded according to the locked clinical design.

Censoring distributions were estimated from training data for IPCW evaluation. Each patient received one out-of-fold (OOF) prediction. Performance was summarized as the equal-weight mean of fold-specific Uno C-index at three years and time-dependent AUC and Brier scores at one, three, and five years.

Percentile 95% confidence intervals were calculated from 1,000 fixed-OOF bootstrap resamples, requiring at least 950 valid resamples.

## Results

All 337 patients were retained, with no missing age or sex values. All five Cox models converged.

| Metric | Horizon | Estimate | 95% CI |
|---|---|---:|---|
| Uno C-index | 3 years | 0.575 | 0.521–0.622 |
| AUC | 1 year | 0.579 | 0.507–0.651 |
| Brier score | 1 year | 0.199 | 0.177–0.222 |
| AUC | 3 years | 0.594 | 0.511–0.665 |
| Brier score | 3 years | 0.244 | 0.223–0.267 |
| AUC | 5 years | 0.633 | 0.532–0.732 |
| Brier score | 5 years | 0.230 | 0.194–0.268 |

## Quality Control

Input hashes, patient-to-sample mappings, and all five outer-fold assignments matched the locked records. All 337 OOF predictions were valid, with no duplicated patients.

Independent recalculation confirmed all 35 fold-specific performance estimates. Bootstrap confidence intervals were verified against the stored resamples. Execution completed successfully without recorded errors or warnings.

## Limitations

The proportional-hazards test flagged age in outer Fold 5 (p = 0.046), although the global test in that fold was not significant (p = 0.097).

Five-year performance estimates had limited numbers of controls (8–13 per fold). Bootstrap intervals were based on fixed OOF predictions and therefore did not capture model-refitting uncertainty.

## Reproducibility

The analysis used R 4.6.0, `survival` 3.8-6, and random seed 20260802. Input hashes, package versions, `renv` status, and execution details are recorded in the Session Info and Execution Log.