# Codebook — `elsa_long.csv`

Derived longitudinal panel dataset produced by `ELSA_extraction.Rmd`.  
**Format:** long (one row per participant per wave).  
**Waves:** 2–9 (2004–2018).  
**Source:** English Longitudinal Study of Ageing, UKDA-5050.

> The raw data underlying this codebook are available from the UK Data Service under a restricted End User Licence: https://ukdataservice.ac.uk/

---

## Identifiers and Wave Metadata

| Variable | Type | Description |
|---|---|---|
| `source` | character | Wave label (`elsa04`, `elsa06`, …, `elsa18`) |
| `idauniq` | character | ELSA unique participant identifier |
| `year` | integer | Survey year |

---

## Demographics and Covariates

| Variable | Type | Coding | Source variable(s) |
|---|---|---|---|
| `birth_year` | numeric | Year of birth | `indobyr` |
| `age` | numeric | Age at interview (years) | `indager` (wave-specific) |
| `Gender` | integer | 1 = Male, 2 = Female | `indsex` |
| `Education` | integer | 0 = No qualification; 1 = O-Level/CSE/Foreign; 2 = A-Level or above | `w2edqual` / `w3edqual` (wave-specific) |
| `Marital_status` | integer | 1 = Married/partnered; 0 = Other | `DiMar` |
| `Employment` | integer | 1 = Employed/self-employed; 0 = Other | `wpdes` |
| `Smoking` | integer | 1 = Current smoker; 0 = Non-smoker | `HESka` |
| `Alcohol` | integer | 1 = Drank in last 7 days; 0 = Did not | `scal7a` |
| `Physical_activity` | integer | 1 = Vigorous activity ≥ monthly; 0 = Rarely/never | `HeActa` |
| `Social_activity` | integer | 1 = Member of ≥ 1 organisation; 0 = No memberships | `scorg01`–`scorg08` |

---

## Sensory Variables

| Variable | Type | Coding | Source variable(s) |
|---|---|---|---|
| `hearing_loss` | integer | 1 = Fair/poor self-rated hearing; 0 = Good or better | `Hehear` |
| `vision_overall` | integer | 1 = Fair/poor/blind self-rated vision; 0 = Good or better | `Heeye` |
| `cataract_surgery` | integer | 1 = Ever reported cataract; 0 = No | `heoptca` |
| `glaucoma_treatment` | integer | 1 = Ever reported glaucoma; 0 = No | `heoptgl` |
| `impaired_vision` | integer | 1 = Cataract or glaucoma reported; 0 = Neither | Derived |

---

## Frailty Index

Based on a 49-item deficit accumulation index across five domains (mobility, ADL/IADL, diagnosed conditions, depression, general health).

| Variable | Type | Description |
|---|---|---|
| `fi_n_valid` | integer | Number of non-missing FI items (max 49) |
| `fi_n_deficit` | integer | Number of deficits present |
| `fi_score` | numeric | FI = `fi_n_deficit / fi_n_valid`; `NA` if < 80% of items valid |
| `frailty` | integer | 1 = Frail (FI ≥ 0.25); 0 = Non-frail (FI < 0.25) |

### FI Item Domains

| Domain | Items (n) | ELSA prefix |
|---|---|---|
| Mobility difficulties | 10 | `hemob*` |
| ADL/IADL difficulties | 13 | `headl*` |
| Diagnosed conditions | 13 | `hedim*`, `hedib*` |
| CES-D depression | 8 | `psced*` |
| General health | 5 | `hehelf`, `hefla`, `hefrac`, `heji`, `mmpain` |

---

## Cognitive / Dementia Variables

| Variable | Type | Coding | Description |
|---|---|---|---|
| `diagnosed_dementia` | integer | 1/0/NA | Physician-diagnosed dementia or Alzheimer's (`hedibde` or `hedibad` = 1 or 2) |
| `iqcode_mean` | numeric | 1–5 | Mean of 16-item IQCODE (Heiqb–Heiqq); 3 = no change, >3 = decline |
| `iqcode_dementia` | integer | 1/0/NA | 1 if `iqcode_mean` ≥ 3.4 |
| `dementia` | integer | 1/0/NA | **Primary outcome.** 1 if `diagnosed_dementia == 1` OR `iqcode_dementia == 1` |

---

## Missing Data

Multiple imputation (MICE, m = 20, maxit = 10) was applied at each wave to the following variables before wave-level merging:

- `Education` (polr), `age` (pmm)
- `Marital_status`, `Employment`, `Smoking`, `Alcohol`, `Physical_activity`, `Social_activity`, `frailty` (logreg)

`Gender` and `dementia` were not imputed.  
The `elsa_long.csv` output contains values from imputation run 1 (i.e., a single complete dataset). For full multiple-imputation inference, run the complete pipeline and pool across all 20 imputed datasets.
