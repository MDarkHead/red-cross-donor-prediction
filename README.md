# Red Cross Donor Prediction
## Overview
This project develops an end-to-end machine learning pipeline to predict donor likelihood for the DFW Red Cross Chapter. By analyzing historical donor and engagement data, the project aims to improve the effectiveness of outreach efforts by helping teams identify likely donors and optimize the allocation of organizational resources.

## Roadmap
- Phase 0 — Project Framing ✅
- Phase 1 — Data Cleaning ✅
- Phase 2 — EDA + Insights ✅
- Phase 3 — Dython Analysis ✅
- Phase 4 — Feature Engineering ✅
- Phase 5 — Classification Modeling ✅
- Phase 6 — Model Interpretability & Insights
- Phase 7 — Tableau Dashboard
- Phase 8 — Portfolio Packaging & Final Deliverables

## Tools & Technologies
### Languages
- Python 3.14

### Libraries
- Pandas
- NumPy
- Scikit-learn
- imbalanced-learn
- Matplotlib
- Seaborn
- Dython
- PyArrow
- Joblib

### Environment
- Jupyter Notebook
- Git/GitHub

## Repository Structure
```
red-cross-donor-prediction/
│
├── README.md
├── requirements.txt
├── LICENSE
│
├── data/
│   ├── raw/
│   │   └── .gitkeep
│   ├── processed/
│   │   └── .gitkeep
│   └── external/
│       └── .gitkeep
│
├── notebooks/
│   ├── 00_project_framing.ipynb
│   ├── 01_data_cleaning.ipynb
│   ├── 02_eda.ipynb
│   ├── 03_dython_analysis.ipynb
│   ├── 04_feature_engineering.ipynb
│   ├── 05_modeling.ipynb
│   ├── 06_model_interpretability.ipynb
│   └── 07_business_recommendations_report.ipynb
│
├── tableau/
│   └── donor_dashboard.twbx
│
├── reports/
│   ├── 00_project_framing.md
│   ├── 01_data_cleaning.md
│   ├── 02_exploratory_data_analysis.md
│   ├── 03_dython_analysis.md
│   ├── 04_feature_engineering.md
│   └── feature_dictionary.csv
│
└── src/
    ├── preprocessing.py
    ├── feature_engineering.py
    ├── modeling.py
    ├── utils.py
    └── evaluation.py
```


## Modeling Strategy
The primary modeling target predicts whether an individual donates during the dataset's current fiscal year using information from the five completed historical fiscal years. This creates a forward-looking classification problem with a positive-class rate of 5.53%.

The original `donor_indicator_flag` will also be evaluated as a separate historical donor-status benchmark during Phase 5. Because the two targets represent different objectives and class distributions, their results will be modeled and reported separately.

Phase 4 produced a leakage-safe modeling dataset containing 34,403 records, 53 safe predictors, one tracking identifier, and one primary binary target. A 77-field feature dictionary documents each field's source, calculation, timing, leakage status, and business meaning.


## Expected Business Impact
A successful predictive system could help the Red Cross:
  - Prioritize outreach lists
  - Improve campaign efficiency
  - Increase donor conversion
  - Better allocate volunteer and marketing resources

## Future Improvements
- Deploy the final predictive model through an interactive dashboard.
- Incorporate geographic visualization into donor outreach analysis.
- Explore donor segmentation using unsupervised learning techniques (e.g., clustering).

## Dataset Notice
The dataset used in this project was provided for educational and analytical purposes through DDSA and the DFW Red Cross Chapter.

Due to privacy, confidentiality, and data usage considerations, the raw, cleaned, engineered, and prediction-level donor datasets are not included in this repository. These files remain stored locally and are excluded through `.gitignore`.

This repository contains only:
- Project code
- Analysis workflows
- Visualizations
- Documentation
- Reproducible pipeline structure

Any references to donor data are for modeling and educational demonstration purposes only.