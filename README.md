# Global meta-analysis shows that immunisation reduces amphibian susceptibility to the chytrid fungus

**Authors:** Meng-Han Joseph Chung, Richard P. Duncan, Daniel W. A. Noble, Benjamin C. Scheele, Simon Clulow

**Corresponding author:** chungmenghan@gmail.com

**Zenodo repository:** https://doi.org/10.5281/zenodo.20278226

---

## Contents

| File | Description |
|------|-------------|
| `Meta_analysis.rmd` | Primary statistical analysis code (all meta-analytic models) |
| `Plot.rmd` | Figure generation code (all manuscript figures) |
| `func.R` | Helper functions for the missing-cases method (lnRR variance estimation) |
| `metadata.csv` | Main dataset: 208 effect sizes from 38 studies |
| `study_info_map.csv` | Study location data for Figure 1 (geographic map) |
| `tree_for_imputation.tre` | Amphibian phylogenetic tree (from Pottier et al. 2024, *Nature*) |
| `AmphibiaWeb_species_list.csv` | Updated species name based on AmphibiaWeb (checked on 2026-July-01) |
---

## System Requirements

### Operating system
Compatible with **Windows**, **macOS**, and **Linux**.

### Software
- **R** version ≥ 4.4.0 (tested on R 4.6.0)
- **RStudio** (recommended, any recent version)

### R packages

**Analysis packages** (install via CRAN):
```r
install.packages(c(
  "metafor",    # multilevel meta-analysis
  "corrplot",   # variance-covariance visualisation
  "ape",        # phylogenetic tree manipulation
  "stringr",    # string operations
  "minqa",      # optimiser (dependency)
  "dplyr",      # data manipulation
  "tidyr",      # data reshaping
  "readr"       # data import
))
```

**Visualisation packages** (install via CRAN):
```r
install.packages(c(
  "ggplot2",
  "sf",
  "rnaturalearth",
  "forcats",
  "scales",
  "ggnewscale"
))
```

**Bioconductor packages** (for phylogenetic tree plots):
```r
if (!requireNamespace("BiocManager", quietly = TRUE)) install.packages("BiocManager")
BiocManager::install(c("ggtree", "treeio", "tidytree", "ggtreeExtra"))
```

**GitHub packages**:
```r
install.packages("remotes")
remotes::install_github("daniel1noble/orchaRd")   # orchaRd: heterogeneity & forest plots
```

### Package versions used
> Please run `sessionInfo()` in R after loading packages to record exact versions.  
> Key packages: metafor 5.0-1, orchaRd 2.2.0, ape 5.8-1 — fill in before depositing.

---

## Installation Guide

1. Install R (https://cran.r-project.org/) and RStudio (https://posit.co/download/rstudio-desktop/).
2. Run the package installation commands above (one-time setup; ~5–15 minutes).
3. Download or clone this repository from Zenodo.
4. Open `Meta_analysis.rmd` in RStudio and set the working directory to the folder containing all data files (`Session → Set Working Directory → To Source File Location`).

---

## Demo / How to Run

### Reproduce all statistical results

1. Open **`Meta_analysis.rmd`** in RStudio.
2. Set the working directory to the folder containing `metadata.csv` and `tree_for_imputation.tre`.
3. Run code chunks sequentially from top to bottom (or use `Knit` to render the full document).
4. The script proceeds through three outcome analyses in order:
   - **Prevalence** (log risk ratio): overall effect → procedural covariates → immunisation type → life stage → host origin → host taxonomy → publication bias
   - **Infection intensity** (log response ratio): same moderator sequence with additional sensitivity analyses
   - **Mortality** (log risk ratio): same moderator sequence

### Reproduce all figures

1. First run `Meta_analysis.rmd` to generate all model objects in the R environment.
2. Open **`Plot.rmd`** and run all chunks sequentially.

### Expected outputs

All model outputs correspond to the results reported in the main text and Supplementary Tables S1–S7, including:
- Overall effect size estimates with 95% confidence intervals and prediction intervals
- Moderator estimates by immunisation type, life stage, host origin, and host taxonomy
- Heterogeneity statistics (I², CVH², M²)
- Publication bias regression coefficients

### Expected run time

| Script | Approximate run time |
|--------|----------------------|
| `Meta_analysis.rmd` (prevalence + intensity models) | ~5–15 minutes |
| `Meta_analysis.rmd` (mortality models, BFGS optimiser) | ~10–20 minutes |
| `Plot.rmd` | ~2–5 minutes |

Times are estimates for a standard desktop computer (8 GB RAM, quad-core processor). Mortality models use the BFGS optimiser and may be slower on some systems.

---

## Data Description

**`metadata.csv`** contains one row per effect size with the following key columns:

| Column | Description |
|--------|-------------|
| `study_id` | Unique study identifier |
| `study_exp_id` | Unique experiment identifier |
| `trait` | Outcome type: `prevalence`, `intensity`, or `mortality` |
| `treatment` | Immunisation type: `live_pathogen`, `dead_pathogen`, `probiotic`, `natural_chem`, `synthetic_antiparastics`, `other` |
| `host_species` | Original amphibian species name |
| `AmphibiaWeb_species` | Updated amphibian species name based on AmphibiaWeb |
| `family` | Taxonomic family |
| `life_stage_tested` | Life stage at Bd challenge: `adult`, `juvenile`, `larva` |
| `host_origin` | `captive` or `wild` |
| `mean_T_trait` | Mean trait value in treatment group |
| `mean_C_trait` | Mean trait value in control group |
| `n_T_animals` | Sample size, treatment group |
| `n_C_animals` | Sample size, control group |
| `SD_T_initial` / `SD_C_initial` | Standard deviations (where available) |
| `T_ID` / `C_ID` | Cohort identifiers (for shared-control correction) |
| `publication_year` | Year of publication |
| `time_interval` | Days between immunisation and Bd challenge |
| `T_time_post` / `C_time_post` | Days between Bd challenge and outcome measurement |

---

## License

This code is released under the **MIT License**.

---

## Contact

For questions about the code or data, please contact **chungmenghan@gmail.com**.
