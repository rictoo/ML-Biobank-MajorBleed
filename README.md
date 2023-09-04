# Major Bleeding Risk Prediction Tool

## Introduction

### Background:
- **Definition:** Major bleeding refers to bleeding into critical areas or organs like the stomach, brain, or joints.
- **Case-fatality:** 13.4%
- Anticoagulants, commonly known as "blood thinners", significantly increase the risk of major bleeding.
- Validated Clinical Scoring Systems: Existing tools for determining the risk of major bleeding when on anticoagulants.
  - HAS-BLED
  - VTE-BLEED
  - ORBIT-AF: Current recommendation in the UK and other regions to guide AF anticoagulation.
    - Parameters: Sex, age, major bleeding history, kidney function, antiplatelet treatment

### Aim:
Develop a risk prediction tool with superior predictive performance compared to current validated methods. The tool should utilize predictors that can be easily accessed in a standard clinical setting.

### Study Population:
- **Source:** UK Biobank
- **About:** An extensive, ongoing prospective cohort study encompassing demographic, lifestyle, medical, and genetic information.
- **Participants:** Around 500,000 participants between the ages of 40 and 69.
- **Publications:** Over 6,000 research papers have been based on the UK Biobank data.

## Methods

### Predictors:
- **Selection Criteria:** Based on domain knowledge and literature review.
- **Categories:**
  - Demographic: Age, sex, ethnicity
  - Socio-economic: Highest qualifications, Deprivation index
  - Behavioural: Smoking, alcohol consumption
  - Biological: Blood pressure, Haematology, Liver and Kidney markers, Nutritional indicators (like albumin, lipids, glucose)
  - Medical History: HTN, T2DM, CHF, liver disease, anaemia, cancer, VTE, vascular disease, COPD, previous major bleed
  - Medications: Benzodiazepines, NSAIDs, OCS, PPI, SSRI, St John’s Wart
  - Others: Frequency of falls

### Unsupervised Methods:
- **Clustering:** K-prototypes due to mixed data types.
- **Distance Metrics:** Euclidian distance for numeric data and simple matching dissimilarity for categorical data.
- **Optimal Cluster Count:** Determined using the Silhouette score and WCSS.
- **Repetitions:** Clustering was done 100 times to optimize cluster compactness.
- **Results Interpretation:** 
  - Cluster prototypes
  - Univariate analysis
  - Chi-squared test with outcome

### Model Training and Hyperparameter Tuning:
- **Preprocessing:** 80/20 train-test split (stratified by outcome) and data scaling.
- **Balancing:** SMOTE (up to 50%) and Undersampling (up to 100%), performed within each cross-validation training iteration.
- **Random Forest:** GridSearchCV with repeated stratified 5-fold cross-validation (20 repeats) was used.
  - Parameters tuned: Tree depth, min. samples for leaf and split, split criterion, max. features per split
- **Neural Network (Multilayer Perceptron with 2 hidden layers):** 
  - GridSearchCV + Skorch with repeated stratified 5-fold cross-validation (100 repeats).
  - Parameters tuned: Optimizer, learning rate, neuron count per layer, dropout.
  - Early stopping was determined using a nested validation set (patience = 5).
- **Calibration:** Trained models were calibrated.
- **Evaluation:** AUC evaluated on the test set, and feature importance plots were showcased.
- **Random Seed:** Set to 42 for all randomness-involved methods.
