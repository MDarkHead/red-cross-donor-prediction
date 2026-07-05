# Red Cross Donor Prediction
## Overview
This project develops an end-to-end machine learning pipeline to predict donor likelihood for the DFW Red Cross Chapter. By analyzing historical donor and engagement data, the project aims to improve the effectiveness of outreach efforts by helping teams identify likely donors and optimize the allocation of organizational resources.

## Roadmap
- Phase 0: Project Framing ✅
- Phase 1: Data Cleaning ✅
- Phase 2: Exploratory Data Analysis ✅
- Phase 3: Dython Analysis
- Phase 4: Tableau Dashboard
- Phase 5: Feature Engineering
- Phase 6: Classification Modeling
- Phase 7: Model Interpretability & Insights
- Phase 8: Business Recommendations 
- Phase 9: Portfolio Packaging & Final Deliverables

## Tools & Technologies
### Languages
- Python

### Libraries
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Seaborn
- Dython
- XGBoost

### Environment
- Jupyter Notebook
- Git/GitHub

## Repository Structure
```
red-cross-donor-prediction/
│
├── README.md
├── requirements.txt
│
├── data/
│   ├── raw/
│   │   └── donor_data.csv
│   ├── processed/
│   │   └── cleaned_donor_data.csv
│   └── external/
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
│   └── 02_exploratory_data_analysis.md
│
└── src/
    ├── preprocessing.py
    ├── feature_engineering.py
    ├── modeling.py
	├── utils.py
    └── evaluation.py
```

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

Due to privacy, confidentiality, and data usage considerations, the raw dataset is not included in this repository.

This repository contains only:
- Project code
- Analysis workflows
- Visualizations
- Documentation
- Reproducible pipeline structure

Any references to donor data are for modeling and educational demonstration purposes only.