# Data Cleaning Summary Report
## Project: Red Cross Donor Prediction
## Phase 1 - Data Cleaning

## Dataset Overview
### Initial Dataset Shape
- Rows: 34,508
- Columns: 23
### Final Dataset Shape
- Rows: 34,403
- Columns: 19

### Dataset Size
- ~7.2 MB

A total of 105 records were removed during data cleaning due to the underage donor filtering decision.

The following columns were removed due to excessive missingness:
- `wealth_rating`
- `academic_degree_level`
- `marital_status`
- `donor_date_of_birth`


## Duplicate Analysis
- Exact duplicate rows removed: 0
- Duplicate donor identifiers removed: 0

The original dataset contained 34,508 unique donor records, corresponding to 34,508 unique donor identifiers.


## Missing Value Handling
A universal missing value standard, `np.nan`, was adopted throughout the dataset.

### Missing Value Summary
| Column | Missing Count | Missing % |
|:----|:---:|:---:|
| `WealthRating` | 31,799 | 92.15 |
| `AcademicDegreeLevel` | 26,902 | 77.96 |
| `MaritalStatus` | 24,568 | 71.20 |
| `DonorDateOfBirth` | 21,190 | 61.41 |
| `PreferredAddressType` | 4,043 | 11.72 |
| `GenderIdentity` | 493 | 1.43 |
| `DonorPostalCode` | 91 | 0.26 |

Missing value treatment decisions were deferred to later phases where appropriate.

Columns exceeding the project missingness threshold were removed:
- `wealth_rating` (92.15%)
- `academic_degree_level` (77.96%)
- `marital_status` (71.20%)
- `donor_date_of_birth` (61.41%)

## Column Transformations
### Column Naming
All column names were standardized to:
- Lowercase
- Naming convention: snake_case 
- No special characters
- Pattern matching to keep multi-digit numbers intact
  - Example: grouping 25 as a single unit rather than splitting it into 2_5

Example: `CurrentFiscalYearDonation` -> `current_fiscal_year_donation`


### Datatype Corrections
#### Binary Variables
Columns:
- `is_member_flag`
- `is_alumnus_flag`
- `is_parent_flag`
- `has_involvement_flag`
- `has_email_flag`
- `donor_indicator_flag`

Converted from:
- `"Y"`
- `"N"`

Converted to:
- `1`
- `0`

#### Currency Variables
Columns: 
- `last_fiscal_year_donation`
- `donation_2_fiscal_years_ago`
- `donation_3_fiscal_years_ago`
- `donation_4_fiscal_years_ago`
- `donation_5_fiscal_years_ago`
- `current_fiscal_year_donation`

Removed:
- `"$"`
- commas
- whitespace

Converted from string format to numeric datatype

#### Date Variable
Column:
- donor_date_of_birth

Converted from string format to datetime format during validation and quality assessment. The column was subsequently removed due to excessive missingness.


## Date Validation
### Validation Checks
- Future Birth Dates: 0
- Donors older than 110: 0
- Underage Donors (<18): 157

### Underage Donor Decision
Records with `donor_age` < 16 were removed from the dataset because:
- Individuals younger than 16 are below the minimum Red Cross donation age requirements and were therefore removed from the dataset.
- The affected records represented only 0.304% of the dataset and had a minimal impact on overall sample size.

Impact:
- Records removed: 105
- Remaining records: 34,403

### Age Consistency Review
A comparison of `donor_age` and `donor_date_of_birth` revealed a consistent age difference of 8 years across all valid records. This suggests that `donor_age` was calculated at a fixed point in time and was not subsequently updated. This appears to be a dataset timing artifact rather than a data quality issue.


## Categorical Standardization
### Gender Identity
`gender_identity` values were standardized:
- `"Uknown"` -> `"Unknown"`
- `"U"` -> `"Unknown"`

### Preferred Address Type
`preferred_address_type` values were standardized:
- `"HOME"` -> `"Home"`
- `"BUSN"` -> `"Business"`
- `"CAMP"` -> `"Campus"`
- `"OTR"` -> `"Other"`


## Suspicious Data Review
### Donation Validation
- No negative donation values were identified
- No text contamination remained after currency cleaning


### Cumulative Donation Validation
- Three records were identified where cumulative donation totals were lower than the sum of observed annual donations.
  - All discrepancies were exactly $1
  - Because these differences affected fewer than 0.01% of records and were likely made due to rounding or accounting adjustments, no modifications were made


## Outlier Analysis
Donation variables exhibit significant positive skew.
For most annual donation fields: 
- Median Donation Amount: $0
- 75th Percentile: $0
- 90th Percentile: $0

Meaningful donation amounts do not begin appearing until approximately the 95th percentile.

Several legitimate donation values exceeded: 
- $500,000
- $1,000,000
- $10,000,000

These records were investigated and retained; no outliers were removed.


## Leakage Assessment
A validation test compared `donor_indicator_flag` against `cumulative_donation_amount`.

Results:
- `donor_indicator_flag` = `1` whenever `cumulative_donation_amount` > `0`
- `donor_indicator_flag` = `0` whenever `cumulative_donation_amount` = `0`

This result held for all 34,403 remaining records and is expected because `cumulative_donation_amount` represents the total amount donated by a constituent throughout their history with the organization. Any cumulative donation amount greater than zero indicates prior donation activity and therefore corresponds to `donor_indicator_flag` = `1`.


### Annual Donation Leakage Follow-Through
Additional validation was performed on the annual donation variables:
- `current_fiscal_year_donation`
- `last_fiscal_year_donation`
- `donation_2_fiscal_years_ago`
- `donation_3_fiscal_years_ago`
- `donation_4_fiscal_years_ago`
- `donation_5_fiscal_years_ago`

Results showed that none of these variables perfectly determined `donor_indicator_flag`. Records with positive annual donation amounts were always associated with donor status. However, many donors had zero donations within a specific fiscal year while still maintaining donor status overall.

Therefore:
- `cumulative_donation_amount` was identified as a confirmed leakage feature.
- Annual donation variables were not identified as direct leakage features.
- Annual donation variables remain strong indicators of prior donor behavior and will be evaluated further during feature engineering and model development.


## Feature Classification & Encoding Planning
Feature type classifications were created for:
- Identifier features
- Numeric features
- Binary features
- Categorical features
- High-cardinality categorical features
- Target feature

`donor_postal_code` was reclassified from a numeric feature to a high-cardinality categorical feature because postal codes represent geographic categories rather than measurable numeric quantities.

A preliminary encoding strategy was also documented; however, no feature encoding was performed during Phase 1. Encoding decisions were deferred to Phase 5: Feature Engineering.

## Phase 1 Outcome
The dataset has been cleaned, standardized, validated, and documented.

Key implementations:
- Missing value standardization
- Datatype correction
- Currency cleaning
- Date validation
- Categorical standardization
- Outlier assessment
- Leakage assessment
- Feature classification
- Encoding strategy planning

The resulting dataset contains 34,403 records and 19 features and is prepared for Phase 2: Exploratory Data Analysis (EDA).