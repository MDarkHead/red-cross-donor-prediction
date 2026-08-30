# Exploratory Data Analysis Summary Report
## Project: Red Cross Donor Prediction
## Phase 2 - Exploratory Data Analysis

## Dataset Overview
### Historical Donor Distribution
| Historical Donor Status | Count | Percentage |
|:-------------------------|------:|-----------:|
| Historical Donor | 21,369 | 62.11% |
| Historical Non-Donor | 13,034 | 37.89% |

The exploratory data analysis was performed on the cleaned dataset produced during Phase 1. The objective of this phase was to understand the characteristics of the data, identify important relationships, and document potential opportunities for feature engineering and predictive modeling.


## Numerical Variable Analysis
### Summary Statistics
Numerical variables were evaluated using:
- Summary statistics
- Histograms
- Kernel density estimates (KDE)
- Boxplots
- Skewness analysis

### Key Findings
- Donation variables exhibit substantial positive skew.
- Most annual donation variables have a median value of $0.
- Donation-related variables exhibit substantial variability characterized by numerous extreme, high-value outliers; these were verified as legitimate and retained for subsequent phases of the project.


## Donation Behavior Analysis
### Key Findings
- Annual donor participation ranged from 5.53% to 7.36%, indicating that only a small proportion of constituents donated in any given fiscal year.
- Donation amounts exhibit a highly right-skewed distribution.

### Major Donor Concentration
- Top 1% of historical donors contribute **88.67%** of all cumulative donations.
- Top 10 historical donors contribute **60.27%** of all cumulative donations.

These findings indicate that a relatively small number of major donors account for the majority of historical donation revenue.


## Demographic Analysis
### Age
- The majority of constituents fall within the 40–45 year age group.
- Historical donor rates remain relatively consistent across age groups.
- Age alone does not strongly differentiate historical donor status.

### Gender
- The dataset is nearly evenly split between female and male constituents.
- Historical donor rates are similar across gender groups.


## Categorical & Binary Indicator Analysis
Variables analyzed:
- `gender_identity`
- `preferred_address_type`
- `is_alumnus_flag`
- `is_parent_flag`
- `has_involvement_flag`
- `has_email_flag`

### Key Findings
- Institutional involvement exhibits the largest difference in historical donor rates.
- Alumni demonstrate slightly higher historical donor rates than non-alumni.
- Parent status, email availability, gender identity, and preferred address type exhibit comparatively small differences.


## Historical Donor vs. Non-Donor Comparison
### Key Findings
- Historical donors have substantially more consecutive donor years.
- Historical donors exhibit substantially higher historical donation amounts across every donation metric.
- Historical donors have an average cumulative donation amount of **$3,814.88**, while the median is **$100**, confirming a highly right-skewed distribution.
- Average age is nearly identical between historical donors and historical non-donors.


## Correlation Analysis
### Key Findings
- Donation history variables exhibit the strongest positive correlations.
- The strongest observed correlation is between `donation_3_fiscal_years_ago` and `cumulative_donation_amount` (r = 0.88).
- Several donation history variables exhibit correlations greater than 0.80, indicating potential multicollinearity.
- `donor_age` and `consecutive_donor_years` exhibit relatively weak correlations with the remaining numerical variables.

These findings suggest that feature selection should be considered during the feature engineering phase.


## Overall Findings
The exploratory data analysis identified several important characteristics of the dataset:
- Historical donation history exhibits the strongest relationships with historical donor status and is likely to provide the most informative features during model development.
- Donation amounts are highly concentrated among a small number of major donors.
- Institutional involvement is associated with higher historical donor rates.
- Demographic characteristics exhibit comparatively weak relationships with historical donor status.
- Several donation variables exhibit strong correlations and should be evaluated for redundancy during feature engineering.


## Limitations
Several limitations were identified during exploratory data analysis:
- Historical donor status reflects past behavior and does not directly represent future donation behavior.
- Several potentially informative variables were removed during Phase 1 because of excessive missingness.
- Strong correlations among donation variables may introduce multicollinearity.


## Transition to Feature Engineering
The exploratory data analysis identified several opportunities for feature engineering, including:
- Donation frequency
- Donation consistency score
- Donation trend
- Number of fiscal years donated
- Major donor indicators
- Age groups
- Log-transformed donation variables
- Encoded categorical variables

These ideas will be evaluated further during Phase 5: Feature Engineering.


## Phase 2 Outcome
The exploratory data analysis successfully characterized the cleaned donor dataset and identified key patterns, relationships, and potential predictive signals.

Key implementations:
- Numerical variable analysis
- Donation behavior analysis
- Demographic analysis
- Categorical and binary indicator analysis
- Historical donor comparison
- Correlation analysis
- Feature engineering planning

The findings from this phase provide the foundation for Phase 5: Feature Engineering, where candidate features identified during exploratory data analysis will be engineered, evaluated, and prepared for predictive modeling.