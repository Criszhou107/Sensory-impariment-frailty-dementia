# ELSA Data Extraction Pipeline

**English Longitudinal Study of Ageing (ELSA) — Longitudinal Data Extraction and Harmonisation**

This repository contains the complete R code used to extract, clean, harmonise, and merge ELSA data across Waves 2–9 (2004–2018) into a longitudinal panel dataset. The pipeline is provided to support reproducibility of published analyses and to assist other researchers working with ELSA.

---

## Data Access

The ELSA dataset is **not included** in this repository. ELSA is managed by the UK Data Service (UKDA Study Number 5050) and is available under an End User Licence that prohibits redistribution.

To access the data:

1. Register at [UK Data Service](https://ukdataservice.ac.uk/)
2. Apply for access to **UKDA-5050** (English Longitudinal Study of Ageing)
3. Download the Stata format (`stata/stata13_se` or `stata15_se`) release
4. Set the `ELSA_DATA_DIR` environment variable as described below

See `codebook.md` for a full description of all variables in the derived longitudinal dataset.

---

## Repository Structure

```
ELSA-data/
├── ELSA_extraction.Rmd   # Main extraction and harmonisation pipeline
├── codebook.md            # Variable dictionary for the derived dataset
├── .Renviron.example      # Template for setting the data directory
├── .gitignore
└── README.md
```

---

## Setup

### 1. R Packages

Install required packages before running:

```r
install.packages(c(
  "haven", "dplyr", "labelled", "tidyverse", "stringr",
  "mice", "lubridate", "msm", "survival", "survminer", "scales"
))
```

### 2. Set Your Data Directory

The pipeline reads ELSA Stata files from a single directory. **Do not hard-code your local path** — instead, set an environment variable.

**Option A — `.Renviron` file (recommended):**

Copy `.Renviron.example` to `.Renviron` in your R project root and fill in your path:

```
ELSA_DATA_DIR=/path/to/UKDA-5050-stata/stata/stata13_se
```

**Option B — in your R session before running:**

```r
Sys.setenv(ELSA_DATA_DIR = "/path/to/UKDA-5050-stata/stata/stata13_se")
```

---

## Pipeline Overview

The `ELSA_extraction.Rmd` script processes ELSA in the following stages:

| Stage | Description |
|---|---|
| **Death tracking** | Integrates the index file, End-of-Life archives, and harmonised EOL file to construct a unified mortality record |
| **Wave-level extraction** | Extracts covariates, sensory variables, dementia classification, and a 49-item Frailty Index for each of Waves 2–9 |
| **Frailty Index** | Computed across mobility, ADL/IADL, diagnosed conditions, CES-D depression, and general health items; requires ≥ 80% non-missing (≥ 40/49 items); binary threshold at FI ≥ 0.25 |
| **Dementia** | Defined as physician-diagnosed dementia/Alzheimer's *or* IQCODE mean ≥ 3.4 |
| **Multiple imputation** | MICE (m = 20, maxit = 10) at each wave using `pmm` for continuous, `polr` for ordered, and `logreg` for binary variables |
| **Longitudinal merge** | All wave-level datasets are bound into a single long-format panel (`elsa_long`) |

### Waves Covered

| Wave | Year | Core File |
|---|---|---|
| 2 | 2004 | `wave_2_core_data_v4.dta` |
| 3 | 2006 | `wave_3_elsa_data_v4.dta` |
| 4 | 2008 | `wave_4_elsa_data_v3.dta` |
| 5 | 2010 | `wave_5_elsa_data_v4.dta` |
| 6 | 2012 | `wave_6_elsa_data_v2.dta` |
| 7 | 2014 | `wave_7_elsa_data.dta` |
| 8 | 2016 | `wave_8_elsa_data_eul_v2.dta` |
| 9 | 2018 | `wave_9_elsa_data_eul_v2.dta` |

---

## Output

Running the full pipeline produces `elsa_long.csv` — a longitudinal panel in long format with one row per participant per wave. See `codebook.md` for a complete variable dictionary.

---

## Citation

If you use this pipeline, please cite the original ELSA study and this repository:

**ELSA:**
> Marmot M, et al. *English Longitudinal Study of Ageing: Waves 0–9, 2001–2019*. [data collection]. UK Data Service. SN: 5050.

**This repository:**
> [Author(s)]. ELSA Data Extraction Pipeline [source code]. GitHub. Available at: https://github.com/Criszhou107/ELSA-data

---

## Licence

The code in this repository is released under the [MIT Licence](https://opensource.org/licenses/MIT). The ELSA data itself is subject to the [UK Data Service End User Licence](https://ukdataservice.ac.uk/app/uploads/cd137-enduserlicence.pdf) and must be accessed directly from UKDA.
