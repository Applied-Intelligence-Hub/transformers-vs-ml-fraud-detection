# transformers-vs-trees--fraud-detection

This repository contains the evaluation suite for comparing tree ensembles (LightGBM, CatBoost, Random Forest) and tabular transformers (FT-Transformer) in financial fraud detection under distribution shifts and representational changes. It builds upon baseline operating points to test model robustness and the limits of explainability.

**Key Features**

* **Controlled Distribution Transfer:** Evaluates Base-trained models on five Bank Account Fraud (BAF) Variant cohorts, strictly excluding exact common-predictor matches to development profiles to test true transferability.


* **Imbalance & Representation Sensitivity:** Examines how models respond to seven data-level interventions, highlighting the representation-sensitive performance drops observed when applying SMOTE to transformers versus boosted trees.


* **Model Interpretation:** Implements compatible original-feature SHAP set comparisons for classical models and separate final-layer, head-averaged attention diagnostics for the FT-Transformer.


* **Performance Tracking:** Tracks average precision (PR-AUC), ROC-AUC, and fixed-threshold $F_{2}$ scores across all tested cohorts and sensitivity controls.



This codebase provides an evidence-bounded evaluation of tabular architectures, demonstrating that ranking performance, distribution robustness, and explanation stability are distinct dimensions of a fraud model's viability.
