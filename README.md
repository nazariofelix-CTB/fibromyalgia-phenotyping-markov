# A Unified Computational Framework for Modelling Longitudinal Trajectories and Recurrent Interventions in Real-World Clinical Data: The Case of Chronic Pain Phenotypes

This repository provides the official source code and reproducible research artifacts supporting the manuscript submitted to the **Journal of Biomedical Informatics (JBI)**.

The framework introduces a rigorous, data-driven methodology to decouple continuous underlying clinical state transitions from exogenous, recurrent clinical interventions using heterogeneous Real-World Data (RWD). It implements automated cross-cohort transactional validation, unsupervised longitudinal phenotype clustering, Continuous-Time Multi-State Models (MSM), and Andersen-Gill intensity formulations for recurrent events.

---

## Repository Structure

The repository is structured to separate raw data, processing code, and frozen clinical insights to comply with JBI's guidelines for reproducible biomedical research:

* data/raw/: De-identified raw baseline registry (Dataset_Bioinformatics_Completo.csv).
* data/results/: Frozen validation artifacts (JBI Reproducibility Checklist).
* notebooks/: Sequential analytical executable pipelines (01_Data_Curation_Longitudinal.ipynb and 02_Pipeline_Longitudinal.ipynb).

---

## Requirements & Installation

This framework is built on Python 3.8+. To ensure strict mathematical reproducibility, it is highly recommended to install the dependencies inside a dedicated virtual environment.

git clone [https://github.com/your-username/longitudinal-trajectories-framework.git](https://github.com/your-username/longitudinal-trajectories-framework.git)
cd longitudinal-trajectories-framework

python -m venv venv
source venv/bin/activate

pip install -r requirements.txt

### Core Dependencies
* pandas & numpy: Multi-dimensional transactional curation and array operations.
* scikit-learn: Scaled geometric k-means clustering and validation metrics.
* scipy: Continuous intensity matrix exponentiation.
* lifelines: Semi-parametric recurrent event intensity estimation (Andersen-Gill model).
* matplotlib & seaborn: Multi-panel phenotyping visualizations and Markov state-occupancy curves.

---

## Execution Workflow (Step-by-Step)

The analysis is broken down into two decoupled notebooks that must be executed sequentially:

### Step 1: Flexible Data Curation & Session Reconstruction
File: notebooks/01_Data_Curation_Longitudinal.ipynb
* Inputs: data/raw/Dataset_Bioinformatics_Completo.csv
* Operations: Implements clinical record parsing, handles highly irregular tracking timeframes, and maps complex longitudinal visit sequences (Initial phase, Intermediate, and Recurrent evaluation sessions).
* Outputs: Generates an intermediate transactional dataset structured chronologically by days from baseline per patient identifier.

### Step 2: Multi-Center Stratification, Unsupervised Clustering, and Survival Overlays
File: notebooks/02_Pipeline_Longitudinal.ipynb
* Inputs: Cured longitudinal clinical dataset.
* Operations:
  1. Splits patient populations into Centre A (Derivation Cohort) and Centre B (External Validation Cohort).
  2. Executes k-means trajectory clustering on Centre A. To prevent data leakage, normalization scales and geometrical centroids are frozen and deterministically projected onto Centre B.
  3. Fits an analytical continuous-time Markov Multi-State Model (MSM) bounded by three clinical severity states (Mild/Maintenance, Severe, Extreme) to compute instantaneous transition rates (Q matrices).
  4. Estimates an independent, exogenous recurrent point process via the Andersen-Gill (AG) intensity formulation to evaluate the hazard ratios of medical re-treatment events overlaid onto the state space.
* Outputs: Computes the overall Model Validation Concordance Index (C-index: 0.742) and generates the 7 mathematical validation artifacts.

---

## Frozen Reproducibility Artifacts (data/results/)

In compliance with the JBI Reproducibility Guidelines, we provide the exact pre-calculated outputs corresponding to the results, tables, and figures reported in the manuscript:

1. center_b_transitions_survival.csv: True transition frequencies and observation windows for the external validation cohort.
2. q_matrices_analytical.csv: The continuous-time transition intensity rates defining state-to-state velocity.
3. temporal_occupancy_curves.csv: Expected probability trajectories spanning 0–360 days (populates Figure 6).
4. andersen_gill_hazard_ratios.csv: Covariate impact and confidence intervals for recurrent re-treatment actions.
5. center_a_frozen_parameters.csv: The geometric anchor-points and scaling coefficients extracted from the derivation cohort.
6. kmeans_performance_metrics.csv: Mathematical cluster validity metrics (Silhouette, Davies-Bouldin, Calinski-Harabasz).
7. strobe_exclusion_registry.csv: Granular population filtering logs used to populate the STROBE Flow Diagram in the paper.

---

## Data Privacy and De-Identification

The patient records contained within data/raw/Dataset_Bioinformatics_Completo.csv have been strictly de-identified in adherence to HIPAA and European General Data Protection Regulation (GDPR) standards for clinical research. All explicit institutional markers, calendar dates, and primary personal keys have been removed or replaced with cryptographically secure generic tokens (PATIENT_XXXX), ensuring no re-identification risk exists while fully preserving longitudinal data variance.

---

## Citation & License

This project is licensed under the MIT License - see the LICENSE file for details.

If you utilize this computational framework, code architecture, or results in your research, please cite our JBI manuscript:

González, N. F., et al. "A Unified Computational Framework for Modelling Longitudinal Trajectories and Recurrent Interventions in Real-World Clinical Data: The Case of Chronic Pain Phenotypes." Journal of Biomedical Informatics (2026). DOI: [Insert Zenodo/Publisher DOI Here]
