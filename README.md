# Childhood Anemia Screening and Severity Classification using Machine Learning
<img width="1693" height="929" alt="image" src="https://github.com/user-attachments/assets/9a00ba4e-67a0-4934-beb4-9ded20130203" />

## Overview

Childhood anemia remains one of India's most significant public health challenges, affecting nearly 68% of children aged 6–59 months. Early identification of anemia and its severity is essential for timely intervention and effective public health planning.

This project presents a **two-stage ensemble machine learning framework** for childhood anemia screening and severity classification using data from the **National Family Health Survey (NFHS-5)**, India's largest nationally representative health survey.

Unlike traditional approaches that only predict whether a child is anemic, this framework additionally identifies the severity level, enabling more targeted healthcare interventions.

---

## Research Contribution

<img width="542" height="587" alt="stages drawio" src="https://github.com/user-attachments/assets/f6d35283-8d5a-48b0-8b7f-c087cb780265" />

This work introduces a novel two-stage architecture designed to mimic real-world public health workflows.

### Stage 1 — Anemia Screening

Binary classification:

* Anemic
* Non-Anemic

### Stage 2 — Severity Classification

For children predicted as anemic:

* Mild Anemia
* Moderate Anemia
* Severe Anemia

This design aligns closely with the intervention framework used under India's **Anemia Mukt Bharat (AMB)** initiative.

---

## Dataset

**Source:** National Family Health Survey (NFHS-5), India (2019–21)

**Population:** Children aged 6–59 months

**Sample Size:** 183,816 children

### Predictor Categories

The framework utilizes 24 predictors covering:

* Child characteristics
* Maternal characteristics
* Household socioeconomic indicators
* Geographic factors
* Environmental and sanitation conditions

Examples include:

* Child age
* Birth interval
* Mother's anemia status
* Maternal education
* Wealth index
* State of residence
* Toilet facilities
* Drinking water source
* Housing quality indicators

---

## Machine Learning Pipeline

```text
NFHS-5 Dataset
       │
       ▼
Data Cleaning & Feature Engineering
       │
       ▼
SMOTE-NC
(Class Imbalance Handling)
       │
       ▼
Stage 1: Binary Screening
       │
       ├── Non-Anemic
       │
       └── Anemic
                 │
                 ▼
Stage 2: Severity Classification
                 │
                 ├── Mild
                 ├── Moderate
                 └── Severe
```

---

## Models Evaluated

The following machine learning models were benchmarked:

* LightGBM
* XGBoost
* CatBoost
* Random Forest
* Logistic Regression

Among these, **LightGBM** consistently achieved the strongest performance across both stages.

---

## Explainable AI

To improve transparency and interpretability, SHAP (SHapley Additive exPlanations) was used to identify key predictors.

### Major Predictors of Childhood Anemia

* Child age
* Mother's anemia status
* Birth interval
* Maternal education
* Household wealth
* Geographic location
* Housing quality indicators

### Severity-Specific Insights

Different predictors influenced different severity levels:

| Severity Level | Key Drivers               |
| -------------- | ------------------------- |
| Mild           | Maternal anemia           |
| Moderate       | Birth interval            |
| Severe         | Environmental deprivation |

These insights would not be visible through traditional binary classification alone.

---

## Results

### Stage 1 — Binary Anemia Screening

**Best Model:** LightGBM

| Metric         | Score  |
| -------------- | ------ |
| AUC-ROC        | 0.6785 |
| Macro F1 Score | 0.6185 |
| Cohen's Kappa  | 0.2398 |

---

### Stage 2 — Severity Classification

**Best Model:** LightGBM

| Metric            | Score  |
| ----------------- | ------ |
| Accuracy          | 0.5777 |
| Weighted F1 Score | 0.5671 |
| Weighted AUC      | 0.6133 |

---

## Performance Interpretation

At first glance, the predictive performance may appear lower than many benchmark machine learning projects. However, several characteristics of the problem make childhood anemia prediction substantially more challenging than traditional structured-data classification tasks.

### Why are the scores moderate?

#### 1. National-Scale Heterogeneity

Unlike many previous studies that focused on individual states or regions, this framework was trained using data from **183,816 children across all Indian states and union territories**.

The resulting population exhibits enormous geographic, socioeconomic, environmental, and cultural diversity, making generalization significantly more difficult than localized prediction tasks.

---

#### 2. Anemia is a Multifactorial Condition

Childhood anemia is influenced by numerous factors that are not fully captured within NFHS-5, including:

* Dietary diversity
* Micronutrient intake
* Genetic conditions
* Parasitic infections
* Healthcare accessibility
* Environmental exposures

Consequently, perfect prediction is not possible using survey variables alone.

---

#### 3. Severe Class Imbalance

Severe anemia cases represent only a small proportion of the population.

Despite applying SMOTE-NC, the model had substantially fewer severe examples than mild or moderate cases, making severity classification particularly challenging.

This is a common issue in healthcare machine learning where clinically critical outcomes are often the rarest.

---

#### 4. Real-World Survey Data

NFHS-5 is a large-scale public health survey rather than a controlled clinical dataset.

Compared to curated benchmark datasets, survey data contains:

* Measurement variability
* Missing information
* Self-reported variables
* Population-level noise

While this generally lowers predictive performance, it often improves real-world applicability.

---

### Why the Results are Still Valuable

The primary contribution of this work is not maximizing predictive accuracy, but demonstrating that:

* Severity-specific prediction is feasible at national scale.
* Different anemia severity levels exhibit distinct risk profiles.
* Explainable AI can uncover severity-specific drivers.
* Geographic severity analysis provides insights beyond prevalence estimates.

The framework is therefore intended as a **population-level decision-support system** rather than an individual clinical diagnostic tool.

---
<img width="2090" height="1053" alt="stage2_statewise_severity_map" src="https://github.com/user-attachments/assets/79d9ab70-2b71-4469-b350-7c27eb859cfe" />

## Geographic Analysis

State-level analysis revealed an important finding:

> High anemia prevalence does not necessarily imply a high severe-anemia burden.

For example:

* Punjab exhibited one of the highest severe anemia rates.
* Delhi showed severe burden above the national average.
* Gujarat displayed high overall prevalence but comparatively lower severe burden.

These findings suggest that public health interventions should consider severity distribution in addition to prevalence.

---
<img width="772" height="940" alt="stage2_shapbeeswarm_sever" src="https://github.com/user-attachments/assets/eed838a8-05d6-4dcf-83c0-48211f17f6f3" />

## Technology Stack

### Data Processing

* Python
* Pandas
* NumPy

### Machine Learning

* Scikit-Learn
* LightGBM
* XGBoost
* CatBoost
* Imbalanced-Learn (SMOTE-NC)

### Explainability

* SHAP

### Visualization

* Matplotlib
* Seaborn
* GeoPandas

---

## Repository Structure

```text
├── notebooks/
│   ├── Stage1_Anemia_Screening.ipynb
│   └── Stage2_Severity_Classification.ipynb
│
├── figures/
│   ├── pipeline.png
│   ├── shap_analysis.png
│   ├── geographic_analysis.png
│   └── model_comparison.png
│
├── requirements.txt
├── README.md
├── LICENSE
└── .gitignore
```

---

## Dataset Availability

The NFHS-5 dataset is governed by DHS Program data-use restrictions and therefore cannot be redistributed through this repository.

Researchers wishing to reproduce the work should request access directly through the DHS Program.

---

## Research Status

**Manuscript currently under peer review.**

Submitted to:

**Clinical Epidemiology and Global Health (Elsevier)**

### Manuscript Title

*A Two-Stage Ensemble Machine Learning Framework for Childhood Anemia Screening and Severity Classification in India: Evidence from the National Family Health Survey-5*

---

## Author

Valencia Dmonte

B.Tech Computer Engineering
Sardar Patel Institute of Technology

Research Interests:

* Machine Learning
* Healthcare AI
* Explainable AI (XAI)
* Public Health Analytics
