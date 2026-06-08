# Data Cleaning Summary Report
## Project: Red Cross Donor Prediction
## Phase 1 - Data Cleaning

## Dataset Overview
### Initial Dataset Shape
- Rows: 34,508
- Columns: 23
### Final Dataset Shape
- Rows: 34,508
- Columns: 23
### Dataset Size
- ~7.2 MB

No records were removed during data cleaning


## Duplicate Analysis
- Exact duplicate rows removed: 0
- Duplicate donor identifiers removed: 0

The dataset contained 34,508 unique donor records


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

Missing value treatment decisions were deferred to later phase where appropriate

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
- `”N"`

Converted to:
- `"1"`
- `"0"`

#### Currency Variables
Columns: 
- `last_fiscal_year_donation`
- `donation_2_fiscal_years_ago`
- `donation_3_fiscal_years_ago`
- `donation_4_fiscal_years_ago`
- `donation_5_fiscal_years_ago`
- `current_fiscal_year_donation`

Removed:
- “`$`"
- commas
- whitespace

Converted from string format to numeric datatype

#### Date Variable
Column:
- `donor_date_of_birth`

Converted from string format to datetime format


## Date Validation
### Validation Checks
- Future Birth Dates: 0
- Donors older than 110: 0
- Underage Donors: 157

### Age Consistency Review
A comparison of `donor_age` and `donor_date_of_birth` revealed a consistent age difference of 8 years across all valid records. This suggests that `donor_age` was calculated at a fixed point in time and was not subsequently updated. This appears to be a dataset timing artifact rather than a data quality issue.



## Categorical Standardization
### Gender Identity
`gender_identity` values were standardized:
- `"Uknown"` -> `"Unknown"`
- `"U"` -> `"Unknown"`

### Marital Status
`marital_status` values were standardized:
- `"Never Married"` -> `"Single"`

### Preferred Address Type
`preferred_address_type` values were standardized:
- `"HOME"` -> `"Home"`
- `"BUSN"` -> `"Business"`
- `"CAMP"` -> `"Campus"`
- `"OTR"` -> `"Other"`


## 