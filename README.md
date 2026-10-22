# Red Cross Donor Prediction
## Overview
This project develops an end-to-end machine learning pipeline to support donor outreach for the DFW Red Cross Chapter. It uses historical donor and engagement data to rank records by relative outreach priority, helping teams focus limited resources on the donors for outreach prioritization.

The final primary model predicts current fiscal year donor status using donor age, gender identity, preferred address type, and aggregate donation behavior from the five completed historical fiscal years. Its output is an uncalibrated prioritization score, not a literal probability of donating.

## Roadmap
- Phase 0: Project Framing ✅
- Phase 1: Data Cleaning ✅
- Phase 2: EDA & Insights ✅
- Phase 3: Dython Analysis ✅
- Phase 4: Feature Engineering ✅
- Phase 5: Classification Modeling ✅
- Phase 6: Model Interpretability & Insights ✅
- Phase 7: Portfolio Packaging & Final Deliverables ✅

## Key Results
The final analytical dataset contains 34,403 donor records. The held out final test set contains 6,881 records, including 381 positive targets for a donor rate of 5.54%.

The selected primary model is a Random Forest classifier developed for relative donor prioritization. Its final test PR AUC was 0.0704. The strongest concentration of observed donors appeared in the narrow highest ranked group:

| Outreach Group | Records | Observed Donor Rate | Donor Capture | Lift |
| --- | ---: | ---: | ---: | ---: |
| Top 1% | 69 | 18.84% | 3.41% | 3.40 |
| Top 5% | 345 | 6.67% | 6.04% | 1.20 |
| Top 10% | 689 | 6.68% | 12.07% | 1.21 |
| Top 20% | 1,377 | 6.10% | 22.05% | 1.10 |

Historical donation amount, frequency, and recency provided the strongest supported collective signal. These measures are highly correlated, so they should be interpreted as one related behavior profile rather than as independent drivers.

Feature rankings differed across random forest importance, permutation importance, and SHAP. This disagreement is documented in the interpretation report and is carried into the business recommendations as a limitation. The historical donor status benchmark also remains separate because it predicts a different target and showed weak test discrimination.

## Tools & Technologies
### Languages
- Python 3.14

### Libraries
- Pandas
- NumPy
- scikit-learn
- imbalanced-learn
- Matplotlib
- Seaborn
- SHAP
- SciPy
- Dython
- PyArrow
- Joblib

### Environment
- Jupyter Notebook
- Git/GitHub

## Repository Structure
```text
red-cross-donor-prediction/
├── LICENSE
├── README.md
│
├── data/
│   ├── external/
│   ├── processed/
│   └── raw/
│
├── models/
│   ├── final_primary_pipeline.joblib
│   └── historical_donor_status_benchmark_pipeline.joblib
│
├── notebooks/
│   ├── 00_project_framing.ipynb
│   ├── 01_data_cleaning.ipynb
│   ├── 02_eda.ipynb
│   ├── 03_dython_analysis.ipynb
│   ├── 04_feature_engineering.ipynb
│   ├── 05_classification_modeling.ipynb
│   └── 06_model_interpretability.ipynb
│
├── outputs/
│   ├── interpretability/
│   │   ├── figures/
│   │   ├── reporting/
│   │   └── tables/
│   ├── modeling/
│   └── private/
│
└── reports/
    ├── feature_dictionary.csv
    └── SummaryReports/
        ├── 01_data_cleaning.md
        ├── 02_exploratory_data_analysis.md
        ├── 03_dython_analysis.md
        ├── 04_feature_engineering.md
        ├── 05_classification_modeling.md
        └── 06_model_interpretability.md
```

The `outputs/interpretability/` directory contains the saved tables, figures, reporting manifests, dashboard catalog, and validation results produced during Phase 6. The complete asset manifest records repository relative paths and metadata for later reporting or dashboard use.

## Modeling Strategy
The primary target identifies whether a donor contributed during the dataset's current fiscal year. Predictors were limited to information available from the five completed historical fiscal years and selected donor characteristics. This creates a forward-looking classification problem with a low positive target rate.

The final Random Forest uses eight source predictors that expand into 14 model input columns. Model development used a stratified development and final test split, five-fold cross validation, PR AUC as the primary evaluation metric, and a fixed operating threshold of 0.065424.

The original `donor_indicator_flag` was modeled separately as a historical donor status benchmark. Historical donation measures were excluded from that benchmark because they define or closely encode its target. The two models predict different outcomes, so their metrics, feature importance, SHAP values, thresholds, and business meaning are not combined.

## Model Interpretation
Phase 6 evaluated the primary model using:
- Random forest feature importance
- Permutation importance using final test PR AUC
- Global and local SHAP explanations
- SHAP dependence and interaction analysis
- Accumulated Local Effects
- Correlation and grouped permutation analysis
- Development fold and bootstrap stability checks
- Score, rank, threshold, and outreach capacity analysis
- Cohort profiling and local donor explanations
- Fairness, subgroup support, disparity, and proxy reviews
- Separate interpretation of the historical benchmark

Final validation confirmed donor alignment, model outputs, SHAP calculations, the fixed threshold, outreach capacity groups, and exported artifacts.

## Responsible Use
The model is intended to support outreach prioritization. It is not a probability model and should not be used as a definitive decision about whether an individual will donate.
- Use score ordering, observed donor capture, and lift when evaluating outreach plans.
- Interpret historical donation measures collectively because their information overlaps.
- Do not treat feature importance, SHAP, ALE, or subgroup differences as causal evidence.
- Do not use age, gender identity, or preferred address type as standalone outreach rules.
- Use demographic variables for fairness monitoring and disparity review.
- Keep unsupported subgroup metrics suppressed when records or positive outcomes are limited.
- Validate performance on later fiscal periods before broader operational use.

## Expected Business Use
The final model can support the Red Cross by helping teams:
- Rank donor records within a defined outreach capacity
- Focus limited outreach resources on higher priority groups
- Use recent and repeated giving as supporting stewardship context
- Review contact readiness and address data quality before outreach
- Track donor capture, lift, campaign cost, response outcomes, and subgroup results

## Future Improvements
- Validate ranking performance on later fiscal periods.
- Monitor score, feature, and subgroup drift over time.
- Reevaluate the operating threshold and outreach capacities when campaign costs or target prevalence change.
- Calibrate model scores before making any probability claims.
- Deploy selected reporting assets through an interactive dashboard.
- Explore geographic analysis and donor segmentation when suitable data is available.

## Reports
Detailed phase summaries are available in [`reports/SummaryReports/`](reports/SummaryReports/). The Phase 6 report documents the final interpretation findings, outreach cohort results, threshold errors, local explanations, fairness review, proxy concerns, benchmark comparison, business recommendations, and limitations.

## Dataset Notice
The dataset used in this project was provided for educational and analytical purposes through DDSA and the DFW Red Cross Chapter. Due to privacy, confidentiality, and data usage considerations, raw, cleaned, engineered, and prediction level donor datasets are not included in this repository. These files remain stored locally and are excluded through `.gitignore`.

This repository contains only:
- Project code
- Analysis workflows
- Derived reporting tables and visualizations
- Documentation
- Reproducible pipeline structure

Any references to donor data are for modeling and educational demonstration purposes only.
