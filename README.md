# SMLM glycosylation analysis workflow

This repository contains the Jupyter notebook used for the DNA-PAINT SMLM analysis associated with the manuscript's WT vs 3NQ glycosylation comparison. The notebook documents the exact analysis sequence used in the study rather than providing a general-purpose SMLM software package.

## What the notebook does

The notebook is organized into five analysis stages:

1. **Data loading and filtering**
   - loads localization tables exported from **Zeiss ELYRA** or **ThunderSTORM** via `locan`
   - pools localization metrics across all files
   - suggests percentile-based defaults for filtering
   - applies the same user-confirmed thresholds to both conditions

2. **Manual visual QC**
   - displays filtered localization heatmaps
   - lets the user keep or reject datasets for downstream analysis

3. **ROI selection and Ripley's H analysis**
   - selects non-overlapping square ROIs from approved datasets
   - computes Ripley's H(r) per ROI
   - generates publication-style WT vs 3NQ plots

4. **DBSCAN clustering**
   - applies DBSCAN to selected ROIs
   - calculates ROI-level clustering metrics
   - saves a summary table for downstream statistics

5. **WT vs 3NQ comparison plots**
   - creates publication-style summary plots for clustering metrics
   - performs Mann–Whitney U tests
   - saves figures and CSV summary tables

## Expected inputs

The notebook expects localization tables from:

- **Zeiss ELYRA**
- **ThunderSTORM**

The files are loaded through `locan`, and the downstream analysis expects the imported data to contain the following columns:

- `position_x`
- `position_y`
- `uncertainty`
- `intensity`
- `frame`
- `psf_sigma` **or** `psf_half_width`

If your exported files differ from these assumptions, adapt the loader step before running the rest of the notebook.

## Interactive steps

This notebook is intentionally interactive because it reproduces the analysis decisions used in the manuscript.

During execution, the user will be asked to:

- select files for the `WT` condition
- specify whether the selected files are `ELYRA` or `THUNDERSTORM`
- select files for the `3NQ` condition
- confirm or edit suggested filtering thresholds
- approve or reject filtered datasets during manual QC
- accept ROI selections
- enter DBSCAN parameters (`eps` and `min_samples`)

## Installation

Create a fresh environment and install the required packages.

### Option 1: pip

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Option 2: conda

```bash
conda create -n smlm-glyco python=3.11
conda activate smlm-glyco
pip install -r requirements.txt
```

`locan` can be installed from PyPI with `pip install locan`, and the project also documents installation through conda-forge. 


## Output files

The current notebook writes outputs to a dated folder on the user's Desktop. Example outputs include:

- ROI summaries
- Ripley's H metadata CSVs
- DBSCAN summary CSVs
- publication-style PNG figures

```

