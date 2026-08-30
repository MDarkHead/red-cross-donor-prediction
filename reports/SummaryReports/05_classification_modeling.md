# Classification Modeling Summary Report
## Project: Red Cross Donor Prediction
## Phase 5 – Classification Modeling


## Classification Modeling Overview
Phase 5 developed and evaluated classification models using the leakage safe feature dataset created during Phase 4. The primary objective was to predict whether an individual would donate during the dataset's current fiscal year and use the resulting probabilities to prioritize donor outreach.

A separate benchmark was also created using `donor_indicator_flag` to classify historical donor status. The benchmark was evaluated independently and is not interpreted as a future donation forecast.


## Modeling Targets
### Primary Target
The primary target was:

`target_current_fiscal_year_donor_flag`

| Target | Meaning | Records | Percentage |
|------:|:--------|--------:|-----------:|
| 0 | Did not donate | 32,499 | 94.47% |
| 1 | Donated | 1,904 | 5.53% |

The 5.53% positive rate created a highly imbalanced classification problem. PR AUC was therefore used as the primary model selection metric.

### Historical Donor Status Benchmark
The secondary target was:

`donor_indicator_flag`

| Target | Records | Percentage |
|------:|--------:|-----------:|
| 0 | 13,034 | 37.89% |
| 1 | 21,369 | 62.11% |

The benchmark was used only for historical cohort comparison.


## Leakage Controls
Only predictors classified as `Safe` during Phase 4 were eligible for the primary workflow. The following remained excluded from primary predictors:

- `current_fiscal_year_donation`
- `cumulative_donation_amount`
- `target_current_fiscal_year_donor_flag`
- `donor_unique_id`
- timing sensitive variables whose availability before the prediction cutoff could not be confirmed

`donor_unique_id` was retained only for tracking.

The benchmark target was confirmed to follow:

`donor_indicator_flag == cumulative_donation_amount > 0`

Historical donation variables and their engineered derivatives were therefore excluded from the benchmark predictor set to prevent circular classification.

### Key Findings
- The primary workflow used only leakage safe predictors.
- Target and identifier fields remained outside all predictor lists.
- The benchmark used a separate restricted predictor set.
- Final validation confirmed that no excluded features entered either workflow.


## Data Splitting and Preprocessing
The primary dataset was split using stratification.

| Partition | Records | Positive Records | Positive Rate |
|:----------|--------:|-----------------:|--------------:|
| Development | 27,522 | 1,523 | 5.53% |
| Final Test | 6,881 | 381 | 5.54% |

Five fold stratified cross validation was performed only within the development partition. The final test set remained untouched during model comparison, feature set experiments, imbalance experiments, hyperparameter tuning, calibration review, and threshold selection.

Preprocessing was contained inside model pipelines. Numeric imputation, scaling where appropriate, categorical imputation, and one hot encoding were therefore learned only from training data.

### Key Findings
- Development and final test data remained separate.
- Validation and test records did not influence learned preprocessing.
- The final test set did not participate in model or threshold selection.


## Model Comparison
The following model families were evaluated:

| Model | Mean CV PR AUC |
|:------|---------------:|
| Dummy Prior | 0.0553 |
| Standard Logistic Regression | 0.0731 |
| L1 Logistic Regression | 0.0743 |
| Controlled Decision Tree | 0.0623 |
| Random Forest | 0.0740 |
| HistGradientBoosting | 0.0715 |

Logistic Regression provided a strong linear baseline, while Random Forest became the strongest overall model after feature representation and hyperparameter experiments.


## Feature Set Experiments
Four leakage safe feature sets were compared.

| Feature Set | Feature Count | L1 Logistic CV PR AUC | Random Forest CV PR AUC |
|:------------|--------------:|----------------------:|----------------------------:|
| Baseline Historical | 10 | 0.0743 | 0.0740 |
| Aggregate RFM | 8 | 0.0758 | 0.0782 |
| Trend Enhanced | 15 | 0.0735 | 0.0770 |
| Full Leakage Safe | 53 | 0.0715 | 0.0766 |

Original and transformed donation amount representations were also compared. L1 Logistic Regression performed best with original donation amounts, while Random Forest performed similarly with original and log transformed values.

Near constant and redundant feature removals did not produce a consistent improvement, so no universal deletion rule was applied.

### Key Findings
- Aggregate RFM produced the strongest results for both model families.
- The compact eight feature set outperformed the full 53 feature set.
- Additional trend features did not improve performance.
- More predictors did not automatically improve generalization.


## Class Imbalance Experiments
The following strategies were evaluated using training data only:

- No adjustment
- Class weighting
- Random oversampling
- Random undersampling
- SMOTENC

Neither L1 Logistic Regression nor Random Forest improved when imbalance treatments were applied.

### Key Findings
- The natural class distribution produced the strongest PR AUC.
- Class weighting substantially worsened probability quality.
- Sampling strategies also reduced performance.
- The final workflow handled imbalance through ranking, outreach capacity analysis, and threshold selection rather than resampling.


## Final Model Selection
The two finalists were Random Forest and L1 Logistic Regression. After hyperparameter tuning:

| Model | Mean CV PR AUC |
|:------|---------------:|
| Random Forest | 0.0786 |
| L1 Logistic Regression | 0.0766 |

Random Forest was selected because it produced stronger ranking performance and better outreach lift across most tested campaign capacities.

The final model configuration was:

| Property | Final Selection |
|:---------|:----------------|
| Model Family | Random Forest |
| Configuration | `rf_depth_14` |
| Feature Set | Aggregate RFM |
| Source Features | 8 |
| Probability Version | Uncalibrated |
| Random State | 121 |
| Operating Threshold | 0.065424 |

### Final Source Features
1. `feature_past_5yr_total_donation`
2. `feature_past_5yr_average_donation`
3. `feature_past_5yr_donation_frequency_rate`
4. `feature_years_since_last_donation_past_5yr`
5. `feature_past_5yr_max_donation`
6. `donor_age`
7. `feature_gender_identity`
8. `feature_preferred_address_type`

Calibration diagnostics did not justify adding a separate calibration layer, so the final Random Forest retained its original probability output.


## Operating Threshold
The default 0.50 threshold was not suitable for the rare positive class. The operating objective was:

> Capture the maximum number of actual donors without contacting more than 10% of the development population.

The selected threshold was:

`0.065424`

At this threshold, the development results were:

| Metric | Result |
|:-------|-------:|
| Contact Rate | 9.95% |
| Donors Captured | 208 |
| Recall | 13.66% |
| Precision | 7.60% |
| Lift Over Random | 1.37× |

The threshold was selected using development data only.

### Key Findings
- Threshold selection was based on outreach capacity rather than default classification behavior.
- Maximum F1 was rejected because it required contacting approximately 69% of the population.
- The final test set did not influence the selected threshold.


## Final Test Performance
The finalized Random Forest was evaluated once on the untouched test set.

| Metric | Final Test Result |
|:-------|------------------:|
| Test Records | 6,881 |
| Actual Donors | 381 |
| Positive Rate | 5.54% |
| No Skill PR AUC | 0.0554 |
| PR AUC | 0.0704 |
| ROC AUC | 0.5236 |
| Recall | 9.97% |
| Precision | 6.74% |
| F1 Score | 0.0804 |
| Specificity | 91.91% |
| Balanced Accuracy | 50.94% |
| Brier Score | 0.0522 |
| Predicted Positive Rate | 8.20% |
| Operating Threshold | 0.065424 |

### Key Findings
- PR AUC remained above the no skill baseline.
- Overall class separation was modest.
- The model is more useful as an outreach ranking tool than as a broad binary classifier.
- The final holdout result did not show a severe generalization collapse despite evidence of training overfit.


## Outreach Utility
The final test ranking was evaluated at several campaign sizes.

| Campaign Capacity | Individuals Contacted | Donors Captured | Precision | Lift |
|:------------------|----------------------:|----------------:|----------:|-----:|
| 1% | 69 | 13 | 18.84% | 3.40× |
| 5% | 345 | 23 | 6.67% | 1.20× |
| 10% | 689 | 46 | 6.68% | 1.21× |
| 20% | 1,377 | 84 | 6.10% | 1.10× |

### Key Findings
- The strongest model value occurs among the highest ranked records.
- The top 1% achieved 3.40× lift over random outreach.
- Lift decreased as campaign size increased.
- The model is most useful when outreach capacity is limited.


## Historical Donor Status Benchmark
The historical benchmark used its own stratified split, preprocessing, predictor restrictions, models, and evaluation metrics. Five candidate models were compared, and Decision Tree produced the highest mean CV ROC AUC at `0.5336` and was selected.

### Benchmark Final Test Results
| Metric | Result |
|:-------|-------:|
| Positive Rate | 62.11% |
| No Skill PR AUC | 0.6211 |
| ROC AUC | 0.5247 |
| PR AUC | 0.6370 |
| Accuracy | 62.05% |
| Balanced Accuracy | 0.5000 |
| Recall | 99.77% |
| Specificity | 0.23% |

The benchmark classified almost every record as a historical donor at the 0.50 threshold.

### Key Findings
- The restricted predictor set provides little separation between historical donors and non donors.
- High recall is caused largely by predicting nearly every record as positive.
- Balanced accuracy of 0.5000 indicates essentially random class separation.
- The benchmark is for cohort comparison only and is not a future donation forecast.


## Primary and Benchmark Results
The two workflows should not be compared directly.

| Comparison | Primary Model | Historical Benchmark |
|:-----------|:--------------|:---------------------|
| Target | Future current fiscal year donation | Historical donor status |
| Positive Rate | 5.53% | 62.11% |
| No Skill PR AUC | ~0.055 | ~0.621 |
| Main Purpose | Future outreach prioritization | Historical cohort comparison |
| Donation History Predictors | Allowed when leakage safe | Excluded to prevent circularity |

Metrics such as PR AUC, precision, recall, F1, and accuracy are affected by target prevalence and should be interpreted within each workflow rather than treated as directly interchangeable.


## Limitations
Important Phase 5 limitations include:

- The primary target is highly imbalanced.
- Overall final test discrimination is modest.
- Donor and non donor probability distributions overlap substantially.
- Random Forest showed meaningful training overfit.
- Most actual donors remain below the selective operating threshold.
- Ranking lift decreases as campaign size expands.
- Contact volume is sensitive to changes in the probability threshold.
- Recency is available only at fiscal year resolution.
- The exact prediction baseline date and fiscal year boundaries were not confirmed.
- Some source fields remain excluded because their timing could not be verified.
- The historical benchmark provides weak discrimination once donation history is removed.


## Saved Artifacts
A donor level prediction file was created at:

`outputs/private/primary_donor_predictions_final_test.csv`

It contains donor IDs, actual targets, probabilities, predictions, donor rankings, outreach flags, and dataset partition. The file remains excluded from Git.

The fitted primary and benchmark pipelines were saved under `models/` and are also excluded from Git.

Aggregate modeling outputs were saved under `outputs/modeling/`, including:

- model configuration
- feature lists
- cross validation results
- hyperparameter results
- final test metrics
- outreach results
- benchmark results
- environment information
- artifact manifest

Aggregate outputs contain no donor identifiers.


## Final Modeling Validation
The notebook kernel was restarted and every cell was executed from top to bottom. All 31 final validation checks passed.

The validation confirmed:

- test information remained outside model selection
- excluded features did not enter final predictor lists
- prediction rows remained aligned with donor IDs
- saved results matched notebook outputs
- saved pipelines reproduced final probabilities
- private donor files remained excluded from Git

The final primary test PR AUC remained `0.0704`, the benchmark test ROC AUC remained `0.5247`, and the operating threshold remained `0.065424`.


## Overall Findings
Phase 5 established the following conclusions:

- Historical donation behavior provides the strongest predictive information.
- Aggregate RFM was the strongest feature representation.
- Random Forest produced the strongest ranking and outreach performance.
- Class imbalance treatments did not improve the primary model.
- The final model provides modest overall discrimination but useful ranking value among the highest ranked records.
- The historical donor status benchmark provides weak discrimination without donation history.
- Primary and benchmark results must remain separately interpreted.


## Questions for Model Interpretability
Phase 6 should investigate:

- Which final features contribute most strongly to donor ranking?
- Are importance results consistent across multiple interpretation methods?
- How should related donation features such as total and average donation be interpreted?
- What nonlinear relationships exist between donation behavior, age, and predicted likelihood?
- What patterns characterize the highest ranked records?
- What distinguishes correct and incorrect predictions?
- Why is lift strongest among the highest ranked donors despite weak overall discrimination?
- Are model explanations stable across the development data?
- Do demographic or address variables raise fairness or proxy concerns?
- Which findings are strong enough to support practical donor outreach insights?


## Transition to Model Interpretability and Insights
Phase 6 should preserve the finalized Random Forest, Aggregate RFM feature set, eight source predictors, uncalibrated probabilities, and operating threshold of `0.065424`. The next phase should focus on explaining how the finalized model produces its rankings rather than revisiting model selection or tuning.


## Phase 5 Outcome
Phase 5 produced a leakage controlled and reproducible classification workflow for donor outreach prioritization. The final primary model is a Random Forest using the eight feature Aggregate RFM representation.

On the untouched final test set:

- PR AUC was `0.0704` compared with a no skill baseline of `0.0554`.
- The top 1% of ranked records achieved `3.40×` lift over random outreach.
- The final operating threshold was `0.065424`.

All 31 final validation checks passed after restarting the kernel and running the complete notebook. Phase 5 is complete and ready for Phase 6 Model Interpretability and Insights.