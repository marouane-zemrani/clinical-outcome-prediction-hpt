# Predicting Postoperative Biochemical Outcomes in Primary Hyperparathyroidism

**Research internship · CHRU Nancy, France · 2026**  
**Marouane Zemrani** — Engineering student, EMINES, Mohammed VI Polytechnic University (UM6P)

> **Project summary only.** This public repository describes the scientific question, methods, and aggregate findings of an internship project. It intentionally contains **no clinical data, source notebooks, trained models, or internship report**. The study is retrospective, has not been externally validated, and **must not be used for clinical decision-making**.

## Research question

Primary hyperparathyroidism is commonly treated by parathyroidectomy. Although surgery is often successful, biochemical recovery is not universal. This project investigated whether routinely recorded clinical and biological information, together with available pathology information, could help **predict biochemical recovery six months after surgery**.

The work had two complementary objectives:

1. **Prediction:** assess whether a supervised learning model could distinguish patients with and without six-month biochemical recovery.
2. **Interpretation:** examine which variables were associated with the observed outcome and how stable these findings were under statistical evaluation.

The emphasis was not on maximizing a single performance metric, but on **data integrity, robust validation, uncertainty, and clinically meaningful limitations**.

## Study design

The study used a retrospective clinical cohort from CHRU Nancy. The source dataset comprised **403 records**, of which **276** had sufficient follow-up information to establish the six-month outcome. The modeling dataset included **27 explanatory variables**, covering patient characteristics, laboratory results, and relevant clinical/pathology information.

The six-month endpoint was defined using postoperative **parathyroid hormone (PTH) and serum calcium** measurements according to the study's specified biochemical criteria. Records with insufficient evidence to classify the outcome were not assigned a label.

Only aggregated information is described here. Individual records and potentially identifying clinical examples are excluded.

## Methodological approach

### 1. Clinical data quality and preprocessing

A substantial part of the internship focused on preparing retrospective hospital data for analysis:

- Standardizing inconsistent laboratory measurement units and naming conventions.
- Checking physiological plausibility and identifying erroneous or ambiguous entries.
- Harmonizing categorical clinical and pathology variables.
- Defining outcome labels while accounting for incomplete follow-up measurements.
- Examining missingness and applying numerical/categorical imputation within the modeling workflow.

An important methodological lesson was that **clinically defensible preprocessing decisions matter even when they do not increase predictive performance**.

### 2. Exploratory statistics

Exploratory analysis investigated variable distributions, missing-data patterns, correlations, and univariate associations with recovery. Statistical interpretation considered multiple comparisons and the instability of conclusions based on small subgroups.

### 3. Predictive modeling

Two complementary supervised learning approaches were used:

- **Regularized logistic regression**, as an interpretable statistical baseline.
- **Random forest**, to model potentially nonlinear relationships and interactions.

Model selection and evaluation accounted for the relatively small number of patients in the less frequent outcome class. More complex methods were considered during development but were not retained in the final comparison.

### 4. Validation, uncertainty, and interpretation

The evaluation framework included:

- **Repeated nested cross-validation**: an outer 5-fold evaluation repeated three times, with 5-fold inner validation for model selection.
- Preprocessing and model-selection operations fitted within the appropriate training folds to limit information leakage.
- Assessment of **ROC-AUC**, precision–recall behavior, probability calibration, and decision-threshold trade-offs.
- **Bootstrap-based uncertainty analysis** and paired model comparisons.
- Model interpretation using standardized logistic-regression coefficients and out-of-fold permutation importance.

Decision thresholds were studied separately from ranking performance because the clinical consequences of incorrect predictions are asymmetric. **No threshold was adopted for clinical use.**

## Main results

| Measure | Logistic regression | Random forest |
|---|---:|---:|
| Nested cross-validation ROC-AUC | **0.686** | **0.764** |
| Reported 95% bootstrap interval | [0.577, 0.723] | [0.673, 0.802] |
| Precision–recall AUC for non-recovery | 0.474 | 0.564 |

**Random forest showed better discrimination** in this cohort, with an advantage that persisted under a paired bootstrap comparison. The analysis also found that more extensive hyperparameter tuning provided little measurable benefit, while feature selection did not improve the random-forest model.

Across complementary analyses, **preoperative PTH, age, and renal function** emerged as repeatedly relevant variables. These are exploratory associations, **not evidence of causation**.

## Limitations and interpretation

The study's limitations are central to its interpretation:

- **Limited sample size:** the modeling cohort was small relative to the number of predictors, particularly for the less frequent outcome.
- **Substantial uncertainty:** even the better-performing model had a fairly wide confidence interval.
- **No external validation:** internal cross-validation and bootstrapping cannot establish generalization to other hospitals or patient populations.
- **Clinical trade-offs:** thresholds prioritizing the detection of non-recovery could generate many false alerts.
- **No deployment readiness:** the results are insufficient to support independent clinical decisions.

The main conclusion is methodological as well as predictive: **credible clinical machine learning requires careful data curation, leakage-aware evaluation, explicit uncertainty quantification, and transparent reporting of negative results and limitations.**

## Potential next steps

Subject to appropriate clinical and institutional oversight, promising research directions include evaluating the approach on a larger multicenter cohort, testing external generalization, and studying longer-term postoperative outcomes.

## Availability, ethics, and confidentiality

This repository contains **only a written project overview**. It does not distribute:

- Patient-level data, including de-identified or derived patient tables.
- Jupyter notebooks, data-cleaning scripts, or trained model files.
- The original French internship report or its clinical figures.

The absence of those materials is intentional. The underlying hospital data are not publicly available, and the project **cannot be reproduced from this README alone**. Any public disclosure of internship findings or institution-related materials remains subject to applicable permissions and review by the relevant supervisors or institutional data controller.

## Acknowledgments

This work was completed during an internship at **CHRU Nancy**, with supervision from **Claire Nominé-Criqui (CHRU Nancy)** and **Alexandre Nominé (Mines Nancy)**.