# Feature Engineering Summary Report
## Project: Red Cross Donor Prediction
## Phase 4 – Feature Engineering


## Feature Engineering Overview
Feature engineering was performed on the finalized cleaned analytical dataset produced during Phase 1. The objective of this phase was to define a leakage-safe prediction target, establish the historical observation window, create meaningful predictor variables, document redundant feature representations, and prepare a validated dataset for classification modeling.

The engineered features describe historical donation value, frequency, recency, streaks, trends, engagement, age, categorical characteristics, and meaningful missingness patterns. Model-specific preprocessing was intentionally deferred to Phase 5.


## Prediction Target and Time Window
The binary prediction target was defined as:

```python
target_current_fiscal_year_donor_flag = (
    current_fiscal_year_donation > 0
).astype(int)
```

The target identifies whether an individual donated during the dataset's current fiscal year using information available before that outcome period.

### Target Distribution
| Target | Meaning | Records | Percentage |
|------:|:--------|--------:|-----------:|
| 0 | Did not donate | 32,499 | 94.47% |
| 1 | Donated | 1,904 | 5.53% |

### Key Findings
- The target is highly imbalanced, with donors representing 5.53% of the dataset.
- The historical observation period consists of the five completed fiscal years before the current fiscal year.
- The prediction baseline is the beginning of the dataset's current fiscal year.
- Phase 5 must use evaluation metrics and training strategies appropriate for imbalanced classification.


## Information Leakage Assessment
The following fields were excluded from the modeling predictor list:
- `current_fiscal_year_donation`
- `cumulative_donation_amount`
- `target_current_fiscal_year_donor_flag`
- `donor_unique_id`

`current_fiscal_year_donation` directly defines the prediction target. `cumulative_donation_amount` may contain current-period donations and was replaced with a historical five-year total calculated only from completed fiscal years. `donor_unique_id` was retained only for record tracking.

### Features Requiring Validation
The timing of the following source fields was not confirmed:
- `has_involvement_flag`
- `has_email_flag`
- `consecutive_donor_years`
- `donor_indicator_flag`

Features derived from these fields were also marked `Requires validation` and excluded from the leakage-safe export.

### Key Findings
- Direct target and current-period leakage fields were removed from the predictor list.
- Historical replacements were created when a safe alternative was available.
- Twelve features were marked `Requires validation` because their pre-cutoff availability could not be confirmed.
- Only features classified as `Safe` were included in the exported modeling dataset.


## Historical Donation Feature Engineering
The five completed historical donation columns were used to create features representing donation value, frequency, consistency, recency, trends, and volatility.

### Donation Value Features
Created features include:

- `feature_past_5yr_total_donation`
- `feature_past_5yr_average_donation`
- `feature_past_5yr_median_donation`
- `feature_past_5yr_max_donation`
- `feature_past_5yr_min_donation`
- `feature_past_5yr_donation_std`
- `feature_past_5yr_active_average_donation`

### Five-Year Donation Summary
| Metric | Value |
|:-------|------:|
| Records with historical donations | 9,087 |
| Records without historical donations | 25,316 |
| Mean five-year total | $723.44 |
| Median five-year total | $0.00 |
| Maximum five-year total | $10,200,274.00 |

### Key Findings
- Historical donation values are highly right-skewed.
- Most records contain no donation during the five-year historical window.
- `feature_past_5yr_total_donation` provides a leakage-safe replacement for the original cumulative donation field.
- The engineered total represents five observed historical years rather than complete lifetime giving.


## Donation Frequency and Streak Analysis
Frequency and streak features were created to measure how often and how consistently an individual donated.

Created features include:
- `feature_years_donated_past_5yr`
- `feature_past_5yr_donation_frequency_rate`
- `feature_any_past_donation_flag`
- `feature_multiple_year_donor_flag`
- `feature_consistent_donor_flag`
- `feature_max_donation_streak_past_5yr`
- `feature_intermittent_donor_flag`

### Historical Donation Frequency
| Years Donated | Records | Percentage |
|-------------:|--------:|-----------:|
| 0 | 25,316 | 73.59% |
| 1 | 7,193 | 20.91% |
| 2 | 1,487 | 4.32% |
| 3 | 347 | 1.01% |
| 4 | 50 | 0.15% |
| 5 | 10 | 0.03% |

### Key Findings
- Most records contain zero or one active donation year.
- Streak features distinguish continuous giving from intermittent giving.
- Donors with the same number of active years may still have different historical giving patterns.
- Target rates for donors active in four or five years should be interpreted cautiously because those groups contain very few records.


## Donation Recency Analysis
Recency was measured at fiscal-year resolution because transaction-level dates were unavailable.

Created features include:
- `feature_years_since_last_donation_past_5yr`
- `feature_donated_last_year_flag`
- `feature_donated_within_2_years_flag`
- `feature_lapsed_donor_flag`
- `feature_never_donated_past_5yr_flag`
- `feature_most_recent_positive_donation`

`feature_years_since_last_donation_past_5yr` uses the following values:

- `0`: donated in the most recent completed historical year
- `1–4`: most recent donation occurred progressively earlier
- `6`: no donation during the five-year historical window

### Key Findings
- Recency was limited to the precision supported by the source data.
- Day-level or month-level recency was not created because donation dates were unavailable.
- Numeric recency and recency indicators were retained as alternative representations for Phase 5 comparison.
- The value `6` clearly separates records with no historical donation from records with an observed donation year.


## Donation Trend and Trajectory Analysis
Trend and trajectory features were created to capture increases, decreases, and volatility in historical giving.

Created features include:
- `feature_recent_vs_older_donation_difference`
- `feature_recent_vs_older_donation_ratio`
- `feature_past_5yr_donation_trend_slope`
- `feature_last_year_donation_amount_change`
- `feature_past_5yr_donation_coefficient_of_variation`
- `feature_last_vs_previous_donation_ratio`
- `feature_previous_year_donation_zero_flag`
- `feature_log1p_last_vs_previous_donation_ratio`

### Key Findings
- Negative difference, change, and slope values represent declining giving and are valid.
- Ratio features can become extreme when the earlier donation amount is zero or very small.
- A denominator-zero flag and log-transformed ratio were retained to support more stable modeling comparisons.
- `feature_recent_vs_older_donation_ratio` contains intentional missing values where the older-period average is zero.


## Skew Transformation Analysis
Log-transformed versions were created for highly skewed donation features:
- `feature_log_past_5yr_total_donation`
- `feature_log_past_5yr_average_donation`
- `feature_log_past_5yr_max_donation`
- `feature_log_most_recent_positive_donation`
- `feature_log1p_last_vs_previous_donation_ratio`

### Key Findings
- Log transformations compress extreme donation values while preserving record ordering.
- Original and transformed features were both retained for model comparison.
- Linear models may benefit from transformed values, while tree-based models may handle original skewed values differently.
- Final representation choices should be based on Phase 5 performance and interpretability.


## Engagement Feature Engineering
Engagement features were created from available relationship and contact indicators.

Created features include:
- `feature_engagement_score`
- `feature_core_engagement_score`
- `feature_has_any_engagement_flag`
- `feature_high_engagement_flag`
- `feature_core_high_engagement_flag`
- `feature_alumnus_involvement_interaction`
- `feature_email_involvement_interaction`

### Key Findings
- Engagement scores summarize multiple relationship indicators.
- Individual engagement flags were retained alongside the combined features.
- The core score excludes parental status and provides an alternative engagement definition.
- Features using `has_involvement_flag` or `has_email_flag` remain timing-sensitive and were excluded from the leakage-safe export.


## Age Feature Engineering
Created age features include:
- `feature_age_group`
- `feature_age_decade`
- `feature_age_squared`
- `feature_age_missing_flag`
- `feature_donor_years_age_ratio`

### Age Group Distribution
| Age Group | Records | Percentage |
|:----------|--------:|-----------:|
| 16–17 | 52 | 0.15% |
| 18–24 | 1,100 | 3.20% |
| 25–34 | 3,278 | 9.53% |
| 35–44 | 23,863 | 69.36% |
| 45–54 | 2,115 | 6.15% |
| 55–64 | 1,680 | 4.88% |
| 65–74 | 1,263 | 3.67% |
| 75+ | 1,052 | 3.06% |

### Key Findings
- The age distribution is heavily concentrated in the 35–44 group because many records have age 42.
- `feature_age_squared` allows models to capture nonlinear age effects.
- `feature_age_missing_flag` is constant at zero and was excluded from the safe predictor export.
- `feature_donor_years_age_ratio` remains experimental because the timing of `consecutive_donor_years` is unverified.


## Categorical Feature Preparation
### Gender Identity
Gender values were standardized into:

| Category | Records | Percentage |
|:---------|--------:|-----------:|
| Female | 16,631 | 48.34% |
| Male | 16,178 | 47.02% |
| Unknown | 1,594 | 4.63% |

Created features include:
- `feature_gender_identity`
- `feature_gender_missing_flag`
- `feature_gender_unknown_flag`

### Preferred Address Type
Preferred address values were standardized into:

| Category | Records | Percentage |
|:---------|--------:|-----------:|
| Home | 28,677 | 83.36% |
| Missing | 4,033 | 11.72% |
| Business | 975 | 2.83% |
| Campus | 647 | 1.88% |
| Other | 71 | 0.21% |

Created address indicators include:

- `feature_is_business_address`
- `feature_is_home_address`
- `feature_is_campus_address`
- `feature_address_type_other`
- `feature_preferred_address_missing_flag`

### Postal Code
Postal codes were preserved as strings to retain leading zeros.

| Metric | Value |
|:-------|------:|
| Nonmissing postal codes | 34,312 |
| Missing postal codes | 91 |
| Unique full postal codes | 20,944 |
| Full postal-code cardinality | 60.88% |
| Singleton postal-code levels | 14,868 |
| Unique three-digit prefixes | 941 |

### Key Findings
- Gender and preferred address categories were standardized before modeling.
- Missing and unknown gender information were preserved separately.
- The business-address flag describes address preference only and does not imply corporate-giving eligibility.
- Full postal code is too high-cardinality for the baseline model.
- The grouped three-digit prefix was discarded because it mainly separated prefix `902` from all other values.
- Geographic features were excluded from the baseline export and may be evaluated separately.


## Missingness Feature Analysis
Missingness indicators were created only where missing or unknown values could reflect a meaningful characteristic or data-collection pattern.

| Feature | Flagged Records | Percentage |
|:--------|----------------:|-----------:|
| Gender unknown | 1,594 | 4.63% |
| Gender originally missing | 491 | 1.43% |
| Preferred address missing | 4,033 | 11.72% |
| Postal code missing | 91 | 0.26% |
| Age missing | 0 | 0.00% |

### Key Findings
- Missingness flags were not created automatically for every column.
- Gender unknown and gender missing represent different conditions.
- Preferred address missingness is the most common selected missingness pattern.
- The age-missing flag has no variance in the current dataset.


## Feature Redundancy Analysis
Several engineered features intentionally represent the same underlying behavior in different forms.

Confirmed deterministic relationships include:
- Five-year average donation equals five-year total divided by five.
- Donation frequency rate equals years donated divided by five.
- Engagement scores equal the sum of their component flags.
- Log-transformed features equal `log1p` of their original values.

### Key Findings
- Total and average donation features contain the same information on different scales.
- Years donated and frequency rate are perfectly correlated.
- Recency indicators overlap with numeric recency but provide more interpretable groups.
- Log-transformed donation features preserve the rank ordering of their original values.
- No correlated features were automatically removed because their impact depends on the model type.


## Feature-Set Variant Analysis
Four feature-set variants were defined for comparison during Phase 5.

| Feature Set | Feature Count | Timing-Sensitive Features |
|:------------|--------------:|--------------------------:|
| Baseline Historical Set | 12 | 2 |
| Aggregate RFM Set | 9 | 1 |
| Trend-Enhanced Set | 16 | 1 |
| Full Candidate Set | 63 | 10 |

### Feature-Set Definitions
- Baseline Historical Set: individual historical donation-year values, original engagement indicators, donor age, standardized gender, and standardized preferred address type.
- Aggregate RFM Set: historical donation total, average, frequency, recency, maximum donation, engagement score, and basic demographic features.
- Trend-Enhanced Set: Aggregate RFM features plus donation differences, ratios, trend slope, year-over-year change, and volatility.
- Full Candidate Set: original historical values and the broad set of engineered donation, engagement, demographic, categorical, address, and missingness features.

### Key Findings
- All four feature sets passed validation with no missing, duplicate, or excluded features.
- Correlated features were intentionally retained for model-specific comparison.
- Timing-sensitive features must be verified or removed before final model training.
- The feature-set variants support comparisons of predictive performance, stability, and interpretability.


## Engineered Dataset Validation
The engineered dataset passed all required quality checks.

### Validation Results
- Row count remained 34,403.
- Donor identifiers contained no missing or duplicate values.
- The target contained only 0 and 1.
- No infinite ratio or rate values were found.
- No invalid negative donation amounts were found.
- Recency, frequency, and streak values remained within expected ranges.
- Binary features contained only 0 and 1.
- Engineered feature names followed snake_case.
- No direct leakage columns appeared in the defined predictor sets.

### Constant and Near-Constant Features
| Feature | Status | Dominant Percentage |
|:--------|:-------|--------------------:|
| `feature_age_missing_flag` | Constant | 100.00% |
| `feature_past_5yr_min_donation` | Near constant | 99.97% |
| `feature_address_type_other` | Near constant | 99.79% |
| `feature_postal_code_missing_flag` | Near constant | 99.74% |

### Key Findings
- The final engineered dataset passed all structural and leakage checks.
- Constant and near-constant features were documented rather than automatically removed.
- Individual numeric features showed weak standalone relationships with the target.
- Weak individual correlations do not rule out nonlinear or interaction-based predictive value.


## Feature Dictionary
A feature dictionary was created for all 77 fields in the engineered dataset.

Each entry documents:
- Feature name
- Feature type
- Source columns
- Exact engineering logic
- Timing classification
- Leakage status
- Business meaning

### Leakage Status Summary
| Leakage Status | Feature Count |
|:---------------|--------------:|
| Safe | 53 |
| Requires validation | 12 |
| Excluded | 12 |

### Key Findings
- Every dataset field was documented exactly once.
- The dictionary includes original fields, engineered predictors, the target, identifiers, excluded variables, and timing-sensitive features.
- The dictionary was exported to `reports/feature_dictionary.csv` for use during modeling, interpretability, and business recommendation development.


## Exported Feature Dataset
The leakage-safe feature dataset contains only fields marked `Safe`, plus the target and tracking identifier.

| Export Component | Count |
|:-----------------|------:|
| Records | 34,403 |
| Safe predictors | 53 |
| Tracking identifiers | 1 |
| Target columns | 1 |
| Total exported columns | 55 |

Exported files:
- `data/processed/donor_features.csv`
- `data/processed/donor_features.parquet`
- `reports/feature_dictionary.csv`
- `notebooks/04_feature_engineering.ipynb`

The donor-level CSV and Parquet files remain excluded from the public Git repository. The feature dictionary and notebook can be committed because they contain documentation and project logic rather than the full donor-level modeling data.


## Model Preprocessing Boundary
Phase 4 created deterministic and interpretable variables but did not perform model-specific preprocessing.

The following steps remain in the Phase 5 pipeline:

- Train, validation, and test splitting
- One-hot encoding
- Numeric scaling
- Missing-value imputation fitted only on training data
- SMOTE, class weighting, or other class-balancing methods
- Feature selection
- Hyperparameter tuning

### Key Findings
- Learned preprocessing must be fitted only on the training data.
- Validation and test data must not influence encoding, scaling, imputation, balancing, or feature selection.
- The Phase 4 dataset retains original numeric scales, standardized categorical labels, intentional missing values, and alternative feature representations.


## Overall Findings
The feature engineering phase established the following conclusions:

- Historical donation behavior remains the primary source of engineered predictive information.
- Donation value, frequency, recency, consistency, trends, and volatility were represented through multiple interpretable features.
- Direct leakage variables were excluded and replaced with historical alternatives where possible.
- Timing-sensitive engagement and loyalty features were documented but excluded from the leakage-safe export.
- Categorical values were standardized while model-specific encoding was deferred to Phase 5.
- Redundant representations were retained for model comparison rather than removed automatically.
- The final dataset contains 53 safe predictors and passed all required validation checks.


## Limitations
Several limitations should be considered when using the engineered dataset:
- The exact prediction baseline date and fiscal-year boundaries were not confirmed in the source documentation.
- The timing of involvement, email availability, consecutive donor years, and donor-status fields remains unverified.
- Recency is available only at fiscal-year resolution because transaction dates are absent.
- Historical donation value covers five observed years and should not be described as complete lifetime value.
- Donation variables contain major outliers and heavy skew.
- Geographic variables require separate evaluation because of high cardinality, privacy, and fairness concerns.
- The target is highly imbalanced.
- Weak individual feature relationships do not determine final model usefulness.


## Transition to Classification Modeling
Phase 4 produced several inputs for Phase 5 Classification Modeling, including:
- A leakage-safe feature dataset
- Four feature-set variants
- Original and log-transformed donation representations
- Documented constant and near-constant features
- A complete feature dictionary
- A list of timing-sensitive features requiring confirmation
- Clear separation between feature creation and learned preprocessing

Phase 5 will split the data before fitting preprocessing, compare feature-set variants, evaluate multiple model types, address class imbalance using training-only methods, and select models using appropriate classification metrics.


## Phase 4 Outcome
The feature engineering phase successfully transformed the cleaned donor dataset into a validated and leakage-controlled modeling dataset.

Key implementations:
- Prediction target and time-window definition
- Formal leakage audit
- Historical donation aggregation
- Frequency, streak, and recency engineering
- Donation trend and trajectory engineering
- Skew-resistant transformations
- Engagement and age feature engineering
- Categorical standardization
- Address and missingness indicators
- Redundancy review
- Feature-set variant definition
- Engineered dataset validation
- Feature dictionary creation
- Leakage-safe CSV and Parquet exports

The final output contains 34,403 records, 53 safe predictor features, one tracking identifier, and one binary prediction target. These results establish the foundation for Phase 5 Classification Modeling.