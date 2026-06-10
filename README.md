# Red Cross Donor Prediction
## Overview
This project develops an end to end machine learning pipeline to predict donor likelihood for the DFW Red Cross Chapter. The goal is to improve the effectiveness of outreach teams by helping them focus on likely donors and optimize the allocation of organizational resources.
By analyzing historical donor and engagement data, the project aims to improve outreach efforts and increase donor participation. 

## Roadmap
- Phase 0: Project Framing ✅
- Phase 1: Data Cleaning ✅
- Phase 2: EDA + Insights
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
├── insights.md
├── business_recommendations.md
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
│   ├── project_slides.pdf
│   └── final_report.pdf
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
- Deploy model through an interactive dashboard
- Add geographic visualization
- Perform donor segmentation using clustering techniques

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