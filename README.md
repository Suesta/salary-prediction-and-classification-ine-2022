# Salary Prediction & Classification (INE 2022)

Reproducible R workflow to analyze wage determinants and the gender wage gap using the **INE 2022 salary sample**. The project combines:

- **Multiple Linear Regression** to model and predict the continuous salary outcome (`RETRINOIN`)
- **Binary Logistic Regression** to classify employees into **High salary** vs **Non-high salary** (median-based threshold)
- Model evaluation with **RMSE**, **confusion matrix**, and **ROC/AUC**
- An **executive summary** and an **ethical reflection** focused on limitations and responsible interpretation

> **Interpretation note:** Results are **associational**, not causal. The variable `SEXO` (registered male/female) is used as a **limited proxy** for gender, as required by the assignment context.

---

## Repository Structure

- `Suesta_preproceso.Rmd` — Main RMarkdown report (end-to-end analysis: preprocessing, modeling, evaluation, conclusions).
- `salarios_INE_2022_sample.csv` — Input dataset (INE 2022 sample).
- `dr_EES_2022.xlsx` — Data dictionary / coding reference used to label categorical variables.
- `A3_20251_enunciado_es.pdf` — Assignment statement (requirements and instructions).
- `README.md` — Project overview (this file).
- `LICENSE` — MIT License.

---

## Analysis Overview

### 1) Data Preparation
- Load `salarios_INE_2022_sample.csv` and inspect structure.
- Convert coded categorical variables into factors (e.g., `SEXO`, `TIPOPAIS`, `ESTU`, `ANOS2`, `TIPOJOR`, `TIPOCON`, plus sector/occupation codes).
- Create grouped variables for interpretability:
  - `CNACE_grp` (sector grouping)
  - `CNO1_grp` (occupation grouping)
- Train/test split with a fixed seed for reproducibility (80/20).

### 2) Linear Regression (Continuous Salary)
- Target: `RETRINOIN` (gross annual salary excluding incapacity-related components).
- Models:
  - Simple linear models to quantify **raw** differences (e.g., by `SEXO`, `TIPOPAIS`).
  - Multiple linear regression with quantitative and categorical predictors (e.g., `ANOANTI`, `JAP`, `ESTU`, `TIPOJOR`, `CNACE_grp`, `CNO1_grp`).
- Diagnostics:
  - Residuals vs fitted
  - Q–Q plot
- Predictive evaluation:
  - Out-of-sample prediction on the test set
  - **RMSE** as the main error metric

### 3) Logistic Regression (High Salary Classification)
- Target: `SalarioAlto`, defined using the **global median** of `RETRINOIN`:
  - `"Alto"` if `RETRINOIN` is above the median
  - `"No Alto"` otherwise
- Model: logistic regression using predictors required by the assignment (e.g., `SEXO`, `ANOANTI`, `JAP`, `ESTU`, `TIPOCON`, `CNACE_grp`, `CNO1_grp`, `TIPOPAIS`).
- Evaluation on the test set:
  - **Confusion matrix** at probability threshold **0.5**
  - **Accuracy**, **Sensitivity**, **Specificity**
  - ROC curve and **AUC** (via `pROC`)

---

## Key Outputs (What to Expect)

The knitted report (`Suesta_preproceso.Rmd`) includes:

- Exploratory summaries and visual comparisons (salary by sex and nationality)
- Regression estimates for raw vs adjusted differences
- Final multiple regression model and interpretation of key coefficients
- Model diagnostics and RMSE-based evaluation on test data
- Logistic regression estimates, odds ratios (OR), confusion matrix metrics, and ROC/AUC
- A compact summary table of research questions, key statistics, and conclusions
- A brief ethical reflection discussing limitations, proxy use (`SEXO`), and interpretation boundaries

---

## Reproducibility

### Requirements
- R (>= 4.1 recommended)
- Packages used in the report typically include:
  - `readr`, `dplyr`, `stringr`, `tidyr`, `ggplot2`
  - `car` (VIF / multicollinearity checks)
  - `pROC` (ROC/AUC)

### Run the Analysis
1. Open `Suesta_preproceso.Rmd` in RStudio.
2. Ensure all repository files remain in the same folder (root).
3. Knit to HTML:
   - **Knit → Knit to HTML**

If package installation is needed, install them from CRAN before knitting.

---

## Data Notes & Scope

- The project relies on the provided **INE 2022 sample** and its coding dictionary.
- Salary data typically exhibit heavy right tails and unobserved heterogeneity; predictive errors should be interpreted accordingly.
- The models are designed for **analysis and reporting** (understanding patterns and associations), not for decision-making about individuals.

---

## Ethical Considerations (Summary)

- `SEXO` is an administrative variable and only a partial proxy for gender-related dynamics.
- Important confounders may be missing (e.g., job grade, firm effects, negotiation, performance, career interruptions).
- Observational results should not be interpreted as causal.
- Model outputs can support diagnostics and auditing, but must be complemented with contextual and qualitative analyses for responsible use.

---

## License

This repository is distributed under the **MIT License** (see `LICENSE`).
