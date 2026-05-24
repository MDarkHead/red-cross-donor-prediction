# Red Cross Donor Prediction
## Overview
This project develops an end to end machine learning pipeline to predict donor likelihood for the DFW Red Cross Chapter. The goal is to improve the effectiveness of outreach teams by helping them focus on likely donors and optimize the allocation of organizational resources.
By analyzing historical donor and engagement data, the project aims to improve outreach efforts and increase donor participation. 

## Business Problem
The Red Cross operates with limited outreach resources, including volunteer availability, communication capacity, and campaign funding. Without a data driven prioritization approach, outreach efforts may focus on low likelihood prospect and reduce operational efficiency. This project aims to identify individuals who are most likely to donate so outreach efforts can focus on those prospects.  

## Objective
Develop a machine learning solution capable of predicting donor likelihood using demographic characteristics, behavioral patterns, and engagement metrics. 

The resulting model should help:
  - Optimize resource allocation across donor outreach campaigns 
  - Increase donor conversion rates
  - Minimize time and budget spent on low probability prospects
  - Facilitate strategic planning through predictive analytics

## Dataset
### Dataset Source
- DFW Red Cross Donor Dataset provided through DDSA

### Expected Feature Categories
- Donor History
- Engagement Metrics
- Demographic Information
- Geographic Information
- Communication & Contact Information
- Financial Capacity Indicators
- Administrative Fields

### Target Variable
- Donor Indicator (binary classification)

## Machine Learning Framing
### Problem Type
Supervised Binary Classification

### Prediction Goal 
Predict the likelihood that an individual will donate

### Potential Models
This project will assess multiple supervised machine learning algorithms to identify the most effective method for donor prediction. 

Potential approaches include: 
  - Logistic Regression
    - Baseline interpretability and probability estimation
  - Random Forest
    - Capturing nonlinear relationships and feature interactions 
  - XGBoost
    - High performance gradient boosting classification
  - LightGBM
    - Scalable and efficient boosting framework

## Metrics
### Technical Metrics
Model performance will be evaluated using classification metrics that measure donor prediction accuracy while optimizing outreach efficiency 

These metrics are:
  - Precision
    - To measure how many predicted donors are actually likely to donate
  - Recall
    - To measure how effectively the model identifies potential donors
  - F1-Score
    - To balance precision and recall
  - ROC-AUC
    - To evaluate the model’s ability to distinguish between donor and non-donor classes

### Business Metrics
- Improved outreach efficiency
- Better donor prioritization
- Reduced wasted contact attempts
- Higher donor conversion rates

## Project Workflow
- 0. Project Framing
- 1. Data Cleaning
- 2. Exploratory Data Analysis
- 3. Feature Engineering
- 4. Baseline Modeling
- 5. Model Evaluation
- 6. Model Optimization
- 7. Business Recommendations

## Roadmap
Phase 0 - Project Framing
Phase 1 - Data Cleaning
Phase 2 - EDA + Insights
Phase 3 - Dython Analysis
Phase 4 - Tableau Dashboard
Phase 5 - Feature Engineering
Phase 6 - Classification Modeling
Phase 7 - Model Interpretability & Insights
Phase 8 - Business Recommendations 
Phase 9 - Portfolio Packaging & Final Deliverables

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
    └── evaluation.py
```

## Initial Hypothesis
- Individuals with a prior donation history or recent engagement are more likely to make future donations
- The frequency of communication may significantly influence the likelihood of donor contributions
- Geographic and demographic factors may have an impact on donor behavior

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