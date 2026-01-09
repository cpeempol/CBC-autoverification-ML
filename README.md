# CBC Autoverification Using Machine Learning

This repository contains the machine learning code used in the study:

**"Optimizing Laboratory Autoverification in Complete Blood Count Testing Using Machine Learning: A Performance Evaluation Study"**

## Overview
The code implements Random Forest, Decision Tree, and XGBoost models for classifying complete blood count (CBC) results as verifiable or non-verifiable.

## Contents
- Data preprocessing scripts
- Model training and evaluation
- Performance metric calculation (sensitivity, specificity, PPV, NPV)

## Sysmex CSV Column Name Standardization

CBC result files exported from **Sysmex hematology analyzers** contain column names with spaces, special characters, units, and symbols (e.g., parentheses, brackets, slashes, percent signs) that are **not compatible with Python variable naming conventions** and may cause parsing or execution errors in machine learning pipelines.

To ensure reproducibility and consistency across models, **all column names are standardized before analysis**.


### General Naming Rules

The following rules were applied to all Sysmex-exported CSV files:

- Spaces and special characters are replaced with underscores (`_`)
- Measurement units (e.g., `(10^3/uL)`, `(%)`) are removed from column names
- Percent-based variables are explicitly encoded using the suffix **`per`**, where  
  **`per` = percentage (%)**
- Column names are restricted to alphanumeric characters and underscores only
- Naming is harmonized to match the feature names expected by the ML scripts


## Mapping of Sysmex Columns to Model Features

Below is the explicit mapping between representative Sysmex column headers and the standardized feature names used in this repository.


### Core CBC Parameters

| Sysmex column (original) | Standardized name | Description |
|--------------------------|------------------|-------------|
| `WBC(10^3/uL)` | `swbc` | White blood cell count |
| `RBC(10^6/uL)` | `srbc` | Red blood cell count |
| `HGB(g/dL)` | `shgb` | Hemoglobin |
| `HCT(%)` | `shct` | Hematocrit |
| `MCV(fL)` | `smcv` | Mean corpuscular volume |
| `MCH(pg)` | `smch` | Mean corpuscular hemoglobin |
| `MCHC(g/dL)` | `smchc` | Mean corpuscular hemoglobin concentration |
| `PLT(10^3/uL)` | `splt` | Platelet count |
| `RDW-SD(fL)` | `srdw` | Red cell distribution width (SD) |
| `RDW-CV(%)` | `srdw_1` | Red cell distribution width (CV, percent) |
| `PDW(fL)` | `spdw` | Platelet distribution width |
| `MPV(fL)` | `smpv` | Mean platelet volume |
| `P-LCR(%)` | `slcr` | Platelet large cell ratio (percent) |
| `PCT(%)` | `spct` | Plateletcrit (percent) |

**Note:** Variables ending with `per` or representing ratios (e.g., `slcr`, `spct`) correspond to percentage values (%).


### Differential Counts

| Sysmex column | Standardized name | Unit |
|---------------|------------------|------|
| `NRBC#(10^3/uL)` | `snrbc` | absolute count |
| `NRBC%(%)` | `snrbcpercent` | percentage |
| `NEUT#(10^3/uL)` | `sn` | absolute count |
| `LYMPH#(10^3/uL)` | `sl` | absolute count |
| `MONO#(10^3/uL)` | `smo` | absolute count |
| `EO#(10^3/uL)` | `seo` | absolute count |
| `BASO#(10^3/uL)` | `sbaso` | absolute count |
| `NEUT%(%)` | `snper` | percentage |
| `LYMPH%(%)` | `slper` | percentage |
| `MONO%(%)` | `smoper` | percentage |
| `EO%(%)` | `seoper` | percentage |
| `BASO%(%)` | `sbaper` | percentage |

Here, the suffix **`per` explicitly denotes percentage (%)**.


### Sysmex IP ABN / IP SUS Flags

Sysmex interpretive flags are converted to binary indicators  
(`0 = absent`, `1 = present`) and renamed as follows:

| Sysmex column | Standardized name |
|---------------|------------------|
| `IP ABN(WBC)IG Present` | `IP_ABN_WBC_IG_Present` |
| `IP ABN(WBC)NRBC Present` | `IP_ABN_WBC_NRBC_Present` |
| `IP ABN(RBC)Anisocytosis` | `IP_ABN_RBC_Anisocytosis` |
| `IP ABN(RBC)Microcytosis` | `IP_ABN_RBC_Microcytosis` |
| `IP ABN(RBC)Macrocytosis` | `IP_ABN_RBC_Macrocytosis` |
| `IP ABN(RBC)Hypochromia` | `IP_ABN_RBC_Hypochromia` |
| `IP ABN(RBC)Anemia` | `IP_ABN_RBC_Anemia` |
| `IP ABN(PLT)Thrombocytopenia` | `IP_ABN_PLT_Thrombocytopenia` |
| `IP ABN(PLT)Thrombocytosis` | `IP_ABN_PLT_Thrombocytosis` |
| `IP SUS(WBC)Blasts/Abn Lympho?` | `IP_SUS_WBC_Blasts_Abn_Lympho` |
| `IP SUS(WBC)Left Shift?` | `IP_SUS_WBC_Left_Shift` |
| `IP SUS(RBC)Fragments?` | `IP_SUS_RBC_Fragments` |
| `IP SUS(PLT)PLT Clumps?` | `IP_SUS_PLT_PLT_Clumps` |

All question marks, slashes, and parentheses are removed to ensure Python compatibility.


## Required Derived Variables

In addition to analyzer-generated fields, the following variables are required and derived prior to model execution:

- **`age`**: calculated in years from `Birth` date and sample collection `Date`
- **`sex`**: binary encoding  
  - `0` = female  
  - `1` = male
- **`WARD`**: patient location encoding  
  - `0` = outpatient (OPD)  
  - `1` = inpatient (IPD)

These variables are mandatory inputs for all machine learning models in this repository.


## Reproducibility Note

Because Sysmex export formats and column naming conventions may vary by analyzer model, software version, and local configuration, minor adjustments to column mapping may be required. This repository provides a transparent and reproducible preprocessing framework, rather than a plug-and-play solution for all Sysmex configurations.


## Data availability
Due to institutional and ethical restrictions, patient-level data are not publicly available.
The code is provided to support reproducibility of the analytical framework.

## Environment
- Python 3
- scikit-learn
- xgboost
- pandas
- numpy

## Disclaimer
This code is intended for research and reproducibility purposes only and is not a clinical decision support system.
# CBC-autoverification-ML
Machine learning models for CBC autoverification (Random Forest, Decision Tree, XGBoost)
