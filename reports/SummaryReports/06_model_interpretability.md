# Phase 6: Model Interpretability and Insights Summary Report
## Project: Red Cross Donor Prediction
## Phase 6 – Model Interpretability & Insights


## Executive Summary
Phase 6 evaluated how the finalized primary model ranks donors, which predictors and historical patterns influence that ranking, how stable those findings are across interpretation methods, and where the model requires operational caution. The analysis also examined outreach cohorts, threshold errors, subgroup disparities, possible proxy relationships, representative local explanations, and the separate historical donor-status benchmark.

The primary model provides a limited but useful signal for relative donor prioritization. Its strongest defensible signal is historical donation behavior considered collectively across amount, frequency, and recency. Individual feature rankings varied across random forest importance, held-out permutation importance, and SHAP, especially for donor age and the highly correlated donation amount measures. The model should therefore be used as a ranking aid rather than as a probability model or definitive donor classifier.

The clearest concentration of observed donors occurred in the top 1% of the final-test ranking. This group had an observed donor rate of 18.84%, or 3.40 times the overall final-test rate. The top 10% captured 12.07% of observed donors and was characterized by more recent and repeated historical donation activity. These results support capacity-based outreach prioritization, but they do not establish that the selected donor characteristics cause future donation behavior.


## Model and Evaluation Context
The primary model is the finalized random forest selected in Phase 5 to predict current fiscal year donor status. It uses eight source predictors that expand into 14 transformed model-input columns.

| Evaluation Item | Result |
| --- | ---: |
| Development records | 27,522 |
| Development positive targets | 1,523 |
| Final-test records | 6,881 |
| Final-test positive targets | 381 |
| Final-test positive rate | 5.54% |
| Source predictors | 8 |
| Transformed features | 14 |
| Fixed operating threshold | 0.065424 |
| Final-test PR AUC | 0.0704 |

The saved model output is uncalibrated. It represents a relative donor-prioritization score and should be used to order donors from higher to lower priority. A score such as 0.1000 must not be interpreted as a literal 10% probability of donating.

## Interpretation Methods
The primary model was evaluated using complementary global, local, stability, and subgroup methods:
- Random forest feature importance described how the fitted model used predictors across its tree structure.
- Held-out permutation importance measured the change in final-test PR AUC after each source predictor was shuffled.
- SHAP quantified global and record-level contributions to the model output and was validated for additivity.
- SHAP dependence and ALE analyses examined nonlinear fitted response patterns.
- SHAP interaction analysis identified candidate joint patterns between predictors.
- Correlation and grouped permutation analyses evaluated shared information among historical donation measures.
- Development-fold and bootstrap analyses measured ranking stability.
- Score, rank, capacity, cohort, and threshold analyses connected model behavior to outreach use.
- Subgroup, disparity, and proxy reviews evaluated demographic and address-related concerns.
- The historical donor-status benchmark was interpreted separately because it predicts a different target using a restricted predictor set.

These methods answer different questions. Their raw values and rankings should not be treated as interchangeable.

## What Most Strongly Influences Donor Ranking
Historical donation behavior is the strongest defensible collective influence on donor ranking. Jointly permuting all historical donation measures decreased final-test PR AUC by 0.0148, with a positive decrease in all repeats. Donation amount measures produced a grouped decrease of 0.0123, while donation timing and frequency produced a grouped decrease of 0.0045.

The individual feature rankings varied by interpretation method:

| Interpretation Method | Strongest Findings |
| --- | --- |
| Random forest importance | Donor Age, Total Donation Amount, Average Donation Amount |
| Held-out permutation importance | Gender Identity, Years Since Most Recent Donation, Donation Frequency Rate |
| Mean absolute SHAP | Donor Age, Gender Identity, Preferred Address Type |

Donor age ranked first in both random forest importance and SHAP but showed slightly negative held-out permutation importance. Gender identity ranked lower in the random forest structure but first in permutation importance and second in SHAP. Years since the most recent donation had stronger held-out permutation evidence despite ranking lower in the other methods.

These differences indicate that feature importance is method dependent. Random forest importance reflects how often and how effectively a feature was used in the fitted tree structure. Permutation importance measures whether the feature adds independent held-out predictive information. SHAP measures how much a feature contributed to individual model outputs. Correlation and substitutable information can cause a predictor to appear important under one method but weak under another.

## Correlated Historical Donation Measures
The historical donation predictors were extremely correlated. Total and average donation amounts had a Spearman correlation of 1.0000, while total, average, and maximum amount measures all had correlations near 1.00. Donation frequency was also strongly related to the amount measures and inversely related to years since the most recent donation.

Because these predictors describe overlapping historical behavior, the model can divide or redistribute importance across them. This was visible in the grouped permutation analysis. Jointly permuting the donation amount measures caused a substantially larger performance decrease than the sum of their individual decreases.

The amount, frequency, and recency measures should therefore be interpreted as a related historical behavior profile. No single measure should be presented as independently determining donor priority.


## Stability of the Findings
The consolidated evidence review classified two findings as stable, seven as moderately supported, five as method dependent, and two as unstable.
- The collective contribution of all historical donation measures was stable.
- Gender identity was a stable model association, but it is appropriate only for fairness evaluation and monitoring.
- Preferred address type, donation recency, donation amount measures as a group, and donation timing and frequency as a group were moderately supported.
- Donor age and the individual historical donation measures were method dependent.
- The leading SHAP interactions were candidate findings rather than confirmed outreach relationships.

Bootstrap analysis showed that SHAP ranks were stable for the highest-ranked features. Donor age, gender identity, preferred address type, and maximum annual donation amount generally remained near the top of the SHAP ranking. Stability within one interpretation method does not eliminate disagreement across different methods, so the business conclusions remain based on combined evidence rather than one ranking.

## Score and Outreach-Capacity Results
Model scores were concentrated near the middle of the observed score range, with a small number of much higher scores. The score ordering was more informative than the raw values.

| Outreach Capacity | Records | Positive Targets | Observed Donor Rate | Donor Capture | Lift |
| --- | ---: | ---: | ---: | ---: | ---: |
| Top 1% | 69 | 13 | 18.84% | 3.41% | 3.40 |
| Top 5% | 345 | 23 | 6.67% | 6.04% | 1.20 |
| Top 10% | 689 | 46 | 6.68% | 12.07% | 1.21 |
| Top 20% | 1,377 | 84 | 6.10% | 22.05% | 1.10 |

The strongest donor concentration occurred in the top 1%. The broader capacity groups produced more modest lift, and the 1% to 5% rank band performed below the overall donor rate. This nonuniform ranking performance means outreach capacity should be selected using observed campaign results rather than assuming that each additional rank band contributes equal value.


## Highest-Priority Outreach Cohort
The top-ranked donors had a distinct historical profile. The top 10% showed more recent and repeated donation activity than the remaining 90%:
- A donation in the most recent year appeared in 19.45% of the top 10%, compared with 5.31% of the remaining population.
- One donation year appeared in 42.67% of the top 10%, compared with 18.67% of the remaining population.
- Three to five donation years were 5.72 times as common in the top 10% as in the remaining population.
- No donation in the past five years appeared in 45.72% of the top 10%, compared with 76.31% of the remaining population.

Historical donation amounts were also higher in the selected cohort, but their distributions were strongly influenced by a small number of unusually large donations. Amount comparisons should include medians, zero percentages, and distribution shape rather than relying only on means.

The profile supports using recent and repeated giving as context for stewardship and reengagement planning after donors have been ranked. It does not support selecting donors with one historical feature alone.


## Threshold Errors
At the fixed 0.065424 threshold, the model produced:

| Threshold Outcome | Records |
| --- | ---: |
| True positives | 38 |
| False positives | 526 |
| False negatives | 343 |
| True negatives | 5,974 |

The resulting recall was 9.97%, and precision was 6.74%. False negatives substantially outnumbered true positives, meaning most observed donors were below the threshold. False positives also substantially outnumbered true positives among selected records.

The threshold should be treated as an operating choice rather than a statement of certainty. A donor below the threshold may still donate, and a donor above the threshold should not be assumed to donate. The model is more appropriate for prioritizing limited outreach capacity than for making definitive individual classifications.


## Local Explanations
Representative local SHAP explanations showed how the same source predictors could increase or decrease scores for different donors. The explanations were generated for selected high-ranked donors, true positives, false positives, and false negatives.

These explanations are useful for reviewing why the model ranked an individual record as it did. They should not be interpreted as causal explanations for the donor’s actual behavior, and they should not be used to justify contacting or excluding an individual without the broader operational and fairness safeguards.

## Fairness, Subgroup, and Proxy Review
Selection and performance differences were observed across age, gender, and preferred address groups. The largest threshold-selection gap occurred across age groups. The selection rate was 3.85% for donors ages 30 to 39 and 37.54% for donors ages 70 and older, a difference of 33.69 percentage points.

Gender and preferred address groups also showed differences in selection, recall, precision, PR AUC, and top-10% donor rates. Some smaller groups had limited outcome support:
- Business Address had nine positive targets.
- Campus Address had three positive targets.
- Other Address Type had only 19 records and one positive target.
- The Under 30, 60 to 69, and 70 and Older age groups had limited positive-outcome support.

Six reviewed subgroups had limited or insufficient support. Unsupported performance metrics should remain suppressed, and reported differences should not be generalized beyond the evaluated final-test records.

The proxy review found weak indirect associations rather than strong proxy evidence. The strongest reviewed indirect association was between preferred address type and donor age, with an association value of 0.1688. However, donor age, gender identity, and preferred address type are direct model predictors. Their use and resulting disparities therefore require continued monitoring even without strong indirect proxy evidence.

Demographic variables should not be used as standalone outreach eligibility rules, to alter donor rank, or to assign solicitation treatment. Preferred address information should be limited to contact-readiness and data-quality review.


## Historical Donor-Status Benchmark
The historical donor-status benchmark predicts a different target from the primary model. It uses restricted non-historical donor characteristics and derived indicators. Historical donation amount, frequency, and recency measures were excluded because they define or closely encode historical donor status and would introduce target leakage.

| Benchmark Metric | Result |
| --- | ---: |
| Accuracy | 0.6205 |
| Balanced accuracy | 0.5000 |
| Precision | 0.6211 |
| Recall | 0.9977 |
| F1 score | 0.7656 |
| ROC AUC | 0.5247 |
| PR AUC | 0.6370 |

The benchmark’s accuracy did not improve on the 0.6211 majority-class reference, balanced accuracy was at the no-skill level, and ROC AUC was only slightly above 0.50. Its high recall resulted from classifying nearly every record as positive rather than from meaningful class separation.

The benchmark should remain a separate diagnostic comparison. Its target, predictor construction, feature importance, performance metrics, thresholds, SHAP values, and business meaning must not be combined with those of the primary model.


## Business Recommendations Supported by the Evidence
The following findings are sufficiently supported for cautious business use:
- Use the complete uncalibrated model score to rank donors within a defined outreach capacity.
- Treat historical donation amount, frequency, and recency as a collective behavior profile.
- Use recent and repeated giving as supporting stewardship and reengagement context for donors already ranked highly.
- Interpret total, average, and maximum donation amounts together rather than assigning independent meaning to one measure.
- Use preferred address information for contact-readiness and data-quality review.
- Use demographic variables only for fairness monitoring and investigation of disparities.
- Evaluate operational use through donor capture, lift, campaign cost, response outcomes, and subgroup results.
- Maintain human review and validate performance on future fiscal periods before expanding use.

The following interpretations are not supported:
- Treating the uncalibrated score as a literal probability of donating.
- Claiming that a feature importance or SHAP value establishes a causal effect.
- Using age, gender identity, or preferred address type as standalone outreach rules.
- Treating one correlated historical donation measure as an independent driver.
- Converting exploratory interaction findings into operational rules.
- Assuming current ranking performance or subgroup rates will remain unchanged in future periods.
- Using the historical benchmark as a substitute for the primary current-year model.


## Limitations
The interpretation results are subject to several important limitations:
- The positive target rate was only 5.54%, so accuracy alone is not informative and ranking results must be interpreted relative to class imbalance.
- Historical donation features were highly correlated, which divided and redistributed importance across individual predictors.
- Scores were not calibrated and cannot support literal probability claims.
- Several subgroup comparisons had limited records or positive outcomes.
- Subgroup findings were observational and do not explain why disparities occurred.
- SHAP, ALE, feature importance, permutation importance, and interaction analyses describe fitted model behavior rather than causal effects.
- Evaluation used a held-out partition from the available analytical dataset rather than a separate future fiscal period.
- Outreach lift, feature relationships, and subgroup rates may change as donor behavior, data collection, and campaign strategy change.

Future validation should use later fiscal periods, monitor score and feature drift, compare realized outreach outcomes with expected lift, and reevaluate the threshold and outreach capacities when costs or target prevalence change.

## Saved Interpretation Assets
Phase 6 produced structured tables and figures covering global interpretation, nonlinear patterns, interactions, correlated features, stability, score distributions, cohort profiles, local explanations, threshold errors, fairness, proxy review, limitations, the historical benchmark, and final conclusions.

The reporting catalog and manifests provide repository-relative paths, file metadata, table schemas, and dashboard-readiness information for later reporting and visualization work. Final consistency checks confirmed donor alignment, model outputs, SHAP calculations, thresholds, rank groups, and the previously cataloged exported artifacts.

The conclusion tables and figure should be included by rerunning the reporting-catalog and final-validation cells after the Phase 6 notebook is finalized.


## Final Conclusion
The primary model should be retained as a relative donor-prioritization aid rather than a probability model or definitive classifier. Its strongest supported value is the ability to concentrate observed donors within narrow high-ranked groups while summarizing historical donation behavior for outreach planning.

Operational use should remain capacity based, monitored, and subject to human review. Historical donation measures should be interpreted collectively, demographic and address-related disparities should be reviewed regularly, unsupported subgroup conclusions should be avoided, and performance should be validated on future donor periods before the model is used more broadly.
