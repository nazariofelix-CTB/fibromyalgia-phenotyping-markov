# A Longitudinal Computational Framework for Phenotypic Clustering and Stochastic Trajectory Modeling in Chronic Pain

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)

This repository contains the official, reproducible Python implementation of the computational framework designed to model longitudinal symptom trajectories and recurrent clinical interventions. The architecture integrates unsupervised machine learning, multi-state Markov chains, and recurrent-event survival analysis to transition from static clinical assessments to dynamic, time-decoupled prognostic models.

---

## 1. Methodological Architecture

The core framework is structured into three decoupled, sequential analytical layers to guarantee mathematical isolation and eliminate data leakage:

### Layer 1: Data Curation & Multi-Center Ingestion (`src/data_curation.py`)
Processes the raw empirical registry (`Dataset_Bioinformatics_Completo.csv`) by auditing longitudinal sampling frequencies, executing multi-dimensional data cleaning, and managing missingness distributions through automated imputation mechanisms. It handles the cohort assignment across independent operational hubs:
* **Derivation Cohort (Center A):** $846$ unique patients, utilized to establish and freeze baseline geometric profiles and distribution parameters.
* **Validation Cohort (Center B):** $581$ unique patients, reserved exclusively for blind out-of-sample validation.

### Layer 2: Phenotypic Stratification & Centroid Freezing (`src/clustering.py`)
Applies partitioning algorithms to map multi-dimensional clinical severity indicators into three distinct, cross-sectional phenotypes:
* **State 1 ($S_1$):** Mild / Maintenance Phenotype.
* **State 2 ($S_2$):** Severe Symptom Phenotype.
* **State 3 ($S_3$):** Extreme Symptom Phenotype.

*Note on Transferability:* To prevent data leakage, the normalization coordinates and geometric centroids calculated from the Derivation Cohort are frozen and deterministically applied to the Validation Cohort.

### Layer 3: Multi-State Stochastic Modeling (`src/markov_engine.py`)
Models continuous-time/discrete-state pathways representing longitudinal patient transitions among the severity states ($S_1 \leftrightarrow S_2 \leftrightarrow S_3$). The engine overlays an exogenous **State 4 (Recall/Healthcare Resource Event)** on the state-space, mathematically isolating active clinical re-treatments from passive, underlying symptom fluctuations.

### Layer 4: Recurrent Time-to-Event Intensities (`src/survival_analysis.py`)
Quantifies cumulative transition hazards and interventional risks using an **Andersen-Gill recurrent-event intensity formulation**. The pipeline structures the longitudinal data into chronological counting-process intervals (**Start-Stop formats**), evaluating predictive gains over traditional stratification strategies.

---

## 2. Repository Structure

```text
├── data/
│   └── Dataset_Bioinformatics_Completo.csv  # Raw longitudinal clinical registry
├── src/
│   ├── data_curation.py                      # Multi-center ingestion & imputation
│   ├── clustering.py                         # Phenotypic partitioning & centroid freezing
│   ├── markov_engine.py                      # Stochastic transition matrices & State 4 logic
│   └── survival_analysis.py                  # Andersen-Gill counting process & hazards
├── notebooks/
│   ├── 01_data_curation_and_clustering.ipynb
│   ├── 02_markovian_trajectories.ipynb
│   └── 03_recurrent_survival_analysis.ipynb
├── requirements.txt                          # Pinned reproducible scientific dependencies
└── LICENSE                                   # MIT License
