# R Survival Analysis of Stroke Data

This repository contains R code for performing survival analysis on a stroke dataset. The analysis aims to understand factors influencing the time to death in stroke patients using Kaplan-Meier estimation and Cox Proportional Hazards models.

## Table of Contents

- [Introduction](#introduction)
- [Dataset](#dataset)
- [Analysis Steps](#analysis-steps)
  - [Setup and Data Loading](#setup-and-data-loading)
  - [Data Cleaning and Summary](#data-cleaning-and-summary)
  - [Kaplan-Meier Survival Analysis](#kaplan-meier-survival-analysis)
  - [Log-Rank and Peto-Peto Tests](#log-rank-and-peto-peto-tests)
  - [Cox Proportional Hazards (PH) Regression](#cox-proportional-hazards-ph-regression)
  - [Model Diagnostics](#model-diagnostics)
  - [Model Prediction and Residual Analysis](#model-prediction-and-residual-analysis)
  - [Adjusted Survival Curves](#adjusted-survival-curves)
- [Requirements](#requirements)
- [Usage](#usage)
- [Author](#author)


## Introduction

This project conducts a comprehensive survival analysis to explore the mortality patterns among stroke patients. It leverages various techniques, including Kaplan-Meier curves for visualizing survival probabilities and Cox Proportional Hazards models to identify covariates associated with the risk of death.

## Dataset

The analysis uses a dataset named `stroke_data.csv`, which is expected to contain information on stroke patients, including:

- `time2`: Time to event (e.g., in days).
- `status`: Event indicator (e.g., "dead" or "alive").
- `stroke_type`: Type of stroke (e.g., Ischemic Stroke, Hemorrhagic Stroke).
- `gcs`: Glasgow Coma Scale score.
- `dm`: Diabetes Mellitus status (Yes/No).
- `doa`: Date of admission.
- `dob`: Date of death.
- `referral_from`: Source of referral (if used in the full model).

## Analysis Steps

The R script performs the following key analysis steps:

### Setup and Data Loading

- Installs and loads necessary R packages for data manipulation (`tidyverse`, `here`), survival analysis (`survival`, `survminer`), table creation (`gtsummary`), and model tidying (`broom`).
- Sets the working directory and loads the `stroke_data.csv` file.

### Data Cleaning and Summary

- Converts date columns (`doa`, `dob`) into proper R date formats using `dmy()`.
- Provides a structural summary (`str()`) and statistical summary (`summary()`) of the dataset.
- Generates a descriptive table of patient characteristics grouped by `status` (dead/alive) using `tbl_summary()` from `gtsummary`.

### Kaplan-Meier Survival Analysis

- Calculates and visualizes the overall Kaplan-Meier survival probability for all patients.
- Calculates and visualizes Kaplan-Meier survival probabilities for different `stroke_type` and `dm` (diabetes mellitus) groups.
- `ggsurvplot()` is used to create publication-quality survival plots with risk tables, confidence intervals, and p-values.

### Log-Rank and Peto-Peto Tests

- Performs Log-rank tests (`survdiff(rho = 0)`) to compare survival curves between `stroke_type` and `dm` groups.
- Also includes Peto-Peto tests (`survdiff(rho = 1)`) for comparison, which gives more weight to earlier events.
- Interprets the p-values from these tests to determine if there are significant differences in survival across groups.

### Cox Proportional Hazards (PH) Regression

- **Simple Cox Models:**
    - Fits a Cox PH model with `stroke_type` as a single covariate to assess its crude effect on the log hazard for death.
    - Fits another simple Cox PH model with `gcs` (Glasgow Coma Scale) to evaluate its crude effect.
    - Uses `tidy()` from the `broom` package to extract and present model coefficients, hazard ratios (exponentiated coefficients), and confidence intervals.
- **Multivariable Cox Model:**
    - Builds a more comprehensive Cox PH model (`cox_model_full`) including `stroke_type`, `gcs`, and `referral_from` (if available in the dataset) to assess their adjusted effects.
    - Presents the hazard ratios and confidence intervals using `tbl_regression()` for a nicely formatted table.
- **Model Comparison:**
    - Compares two nested Cox models using `anova()` to determine if adding `referral_from` significantly improves the model fit.
- **Visualizing Coefficients:**
    - Generates a forest plot (`ggforest()`) to visually represent the hazard ratios and their confidence intervals from the full Cox model.

### Model Diagnostics

- **Proportional Hazards Assumption Check:**
    - Uses `cox.zph()` to test the proportional hazards assumption for the full Cox model, with both "Kaplan-Meier" and "rank" transformations.
    - Plots Schoenfeld residuals (`ggcoxdiagnostics(type = "schoenfeld")` and `plot(stroke_zph, var = ...)`) for individual covariates to visually inspect the assumption.
- **Other Residual Plots:**
    - Generates plots of deviance residuals (`ggcoxdiagnostics(type = "deviance")`) to identify outliers.
    - Creates plots of `dfbetas` (`ggcoxdiagnostics(type = "dfbetas")`) to assess the influence of individual observations on model coefficients.

### Model Prediction and Residual Analysis

- Uses `augment()` from `broom` to add predictions (e.g., `.`fitted` log-hazard, `.resid` residuals, `expected` number of events, `risk` scores) back to the original dataset.
- Extracts and displays various types of residuals from the Cox model (Martingale, Deviance, Schoenfeld, dfbeta, scaled Schoenfeld) for further diagnostic checks.

### Adjusted Survival Curves

- Plots adjusted survival curves for the Cox model (`ggadjustedcurves()`), allowing visualization of survival probabilities while accounting for other covariates in the model.

## Requirements

To run this analysis, you will need R installed on your system along with the following R packages:

- `tidyverse`
- `gtsummary`
- `lubridate`
- `survival`
- `survminer`
- `broom`
- `here`
- `ggplot2`

These packages will be automatically installed if they are not already present, as specified in the `setup` chunk of the R Markdown file.

## Usage

1.  **Save the R code:** Copy the provided R code into an `.Rmd` (R Markdown) file or an `.R` script.
2.  **Place Data:** Ensure your `stroke_data.csv` file is located at the specified path (`/Users/careenevansjoseph/Downloads/Stroke Analysis/stroke_data.csv`) or update the `read.csv()` line to reflect its correct location.
3.  **Run the analysis:**
    - If it's an `.Rmd` file, you can knit it to PDF or HTML using RStudio's "Knit" button.
    - If it's an `.R` script, you can run the code line by line or source the entire script.

The output will include statistical summaries, Kaplan-Meier plots, Cox model summaries, diagnostic plots, and adjusted survival curves.

## Author

Careen Evans-Joseph


