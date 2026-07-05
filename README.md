# Heart Disease Prediction — Exploratory Data Analysis & Machine Learning

This repository contains a reproducible analysis and prediction workflow for heart disease classification. The project is implemented as a Jupyter Notebook and focuses on data inspection, preprocessing, exploratory analysis, model training, and performance evaluation using the provided heart disease dataset.

---

**Project At a Glance**

- **Overview:** End-to-end notebook workflow for heart disease prediction, including data cleaning, exploratory analysis, feature preparation, model development, cross-validation, and hyperparameter tuning.
- **Data:** `heart.csv` — 302 unique patients (after deduplication) with 13 clinical and demographic features used to predict the presence of heart disease.
- **Notebook (local):** `Heart_Disease_Prediction.ipynb`
- **Primary libraries:** pandas, numpy, matplotlib, seaborn, scikit-learn

---

**Contents of this repository**

- `heart.csv` — source dataset for analysis and model training
- `Heart_Disease_Prediction.ipynb` — main Notebook (load, clean, EDA, model training, evaluation, tuning)
- `README.md` — project documentation
- `LICENSE` — license terms

---

**Dataset & Class Balance**

- **Initial Shape:** 1025 rows, 14 columns (including target).
- **Cleaned Shape:** 302 rows (after dropping duplicates).
- **Class Balance:** The target class is balanced (~51.3% positive cases, 48.7% negative cases). This roughly 50/50 split means that Accuracy is a reasonably trustworthy baseline metric. However, because this is a medical diagnostic task, we still prioritize Recall and ROC-AUC. 

---

**Modeling & Performance**

We evaluated Logistic Regression, Random Forest, and K-Nearest Neighbors (KNN). To ensure stability on a relatively small dataset (302 rows), models were evaluated using 5-Fold Stratified Cross-Validation on the training set, followed by a final evaluation on the 20% holdout test set using models optimized via `GridSearchCV`.

### Why Recall and ROC-AUC Matter
In the context of heart disease prediction, **Recall** is often more critical than Precision. Missing an actual case of heart disease (a False Negative) carries potentially fatal consequences, whereas a false alarm (a False Positive) simply leads to further testing. Thus, models with higher recall and a strong **ROC-AUC** (which measures the model's ability to discriminate between positive and negative classes across all thresholds) are prioritized over raw accuracy.

### Tuned Model Evaluation Metrics

| Model | CV F1 (Mean ± Std) | Test Accuracy | Test Precision | Test Recall | Test F1 | Test ROC-AUC |
|---|---|---|---|---|---|---|
| **Random Forest** | **0.867 ± 0.033** | 0.770 | 0.788 | 0.788 | 0.788 | 0.859 |
| **KNN** | 0.840 ± 0.029 | 0.803 | 0.784 | 0.879 | 0.829 | 0.863 |
| **Logistic Regression** | 0.866 ± 0.071 | 0.787 | 0.812 | 0.788 | 0.800 | 0.860 |

### Final Model Selection: Random Forest
While the tuned KNN achieved higher single-test-set scores (Test F1 of 0.829 and Test ROC-AUC of 0.863) on our specific 20% holdout (~60 rows), a test set of this size is susceptible to high variance and noise. The **Random Forest** model is our selected final model because it demonstrated superior stability and a higher mean score during 5-fold cross-validation (`0.867 ± 0.033`), making it the most robust and defensible choice for generalization.

---

**Feature Importance & Domain Relevance**

Our Random Forest model identified the following top predictive features:
1. **`thalach`** (Maximum heart rate achieved)
2. **`cp`** (Chest pain type)
3. **`oldpeak`** (ST depression induced by exercise)

**Interpretation:** This strongly aligns with known clinical risk indicators. Stress-related features and symptomatic markers (like chest pain and exercise-induced ST depression) are classic hallmarks in diagnosing heart disease, giving us confidence that the model is learning medically sound patterns.

---

**Quick start — setup & run**

1. Create & activate a Python environment:
```bash
python -m venv .venv
# Windows
.\.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate
```

2. Install dependencies:
```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

3. Launch Jupyter and run the notebook:
```bash
jupyter notebook
```

Open the Notebook locally: `Heart_Disease_Prediction.ipynb`  
Or run on Colab: [Heart Disease Prediction Notebook](https://colab.research.google.com/drive/17ugahX7KvITt1UUKDr5SobMiRgyoDhAP?usp=sharing)

---

**What I'd try next**

- **XGBoost / LightGBM:** Test gradient boosting frameworks to see if they can capture more complex non-linear relationships and raise the performance ceiling.
- **SHAP Values:** Implement SHAP (SHapley Additive exPlanations) for deeper, instance-level explainability to see exactly how features interact for individual patients.
- **Feature Engineering:** Create interaction terms (e.g., combining age and max heart rate).

---

## License

This project is licensed under the MIT License — see the `LICENSE` file for full terms.

Copyright (c) 2026 Manan
