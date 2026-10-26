# Python Pandas Data Cleaning

## Project Overview

## This project demonstrates practical data cleaning using Pandas.

## Tools Used

- Pandas
- Python
- Jupyter Notebook
- Excel
- Git and GitHub

## Tasks Completed

# 🟢 LEVEL 1 — Basic Structural Cleaning

## Task 1: Inspect the Dataset

### Inspected the dataset to understand its structure and memory usage before starting the cleaning process.

**Checks performed:**
- Found the number of rows and columns using `df.shape`.
- Retrieved column names using `df.columns`.
- Checked data types using `df.dtypes`.
- Inspected dataset information and default memory usage using `df.info()`.
- Checked detailed memory usage, including text data, using `df.info(memory_usage="deep")`.
- Displayed the first five rows using `df.head(5)`.
- Displayed the last five rows using `df.tail(5)`.

🟢 LEVEL 1 — Basic Structural Cleaning
## Task 2: Clean Column Names

### Standardized column names to make them consistent and easier to use in data analysis.

**Cleaning performed:**
- Removed leading and trailing spaces using `.str.strip()`.
- Converted column names to lowercase using `.str.lower()`.
- Replaced whitespace characters with underscores using `.str.replace(r"\s+", "_", regex=True)`.

**Example:**

| Original Column Name | Cleaned Column Name |
|---|---|
| ` Employee ID ` | `employee_id` |
| `NAME` | `name` |
| `Department` | `department` |
| `Join Date` | `join_date` |

**Result:** All column names follow a consistent lowercase naming convention with underscores.

🟢 LEVEL 1 — Basic Structural Cleaning
## Task 3: Remove Leading and Trailing Whitespace

### Cleaned whitespace from all string columns to ensure consistent text formatting.

**Cleaning performed:**
- Selected string-type columns using `df.select_dtypes(include="str")`.
- Removed leading and trailing whitespace using `.str.strip()`.
- Replaced consecutive whitespace characters with a single space using `.str.replace(r"\s+", " ", regex=True)`.

**Example:**

| Before | After |
|---|---|
| `"  John  "` | `"John"` |
| `"Mary   Smith"` | `"Mary Smith"` |
| `"  Sales    Team  "` | `"Sales Team"` |

**Result:** Text values have consistent spacing, making the dataset cleaner for further analysis.

🟢 LEVEL 1 — Basic Structural Cleaning
## Task 4: Find Completely Empty Strings

### Identified empty strings and whitespace-only strings across all columns and replaced them with `NaN`.

**Cleaning performed:**
- Iterated through all columns using `df.columns`.
- Used the regular expression `r"^\s*$"` to identify empty strings and strings containing only whitespace.
- Replaced matching values with `np.nan`.
- Displayed the first 10 rows using `df.head(10)` to inspect the result.

**Examples:**

| Before | After |
|---|---|
| `""` | `NaN` |
| `"   "` | `NaN` |
| `"  "` | `NaN` |

**Result:** Empty text values are represented as missing values, making missing-data detection and handling easier.

# 🟢 LEVEL 2 — Missing Values
## Task 5: Identify Missing Values

Identified and counted missing values in each column using Pandas.

**Code:**

```python
missing_values = df.isna().sum()
missing_values
```

**Results:**

| Column | Missing Values |
|---|---:|
| employee_id | 0 |
| name | 1 |
| department | 1 |
| age | 1 |
| salary | 1 |
| bonus | 37 |
| join_date | 7 |
| email | 8 |
| phone | 4 |
| gender | 27 |
| status | 26 |

**Key observations:**
- `employee_id` contains no missing values.
- `bonus` has the highest number of missing values, with 37.
- `gender` has 27 missing values, while `status` has 26.
- Other columns contain between 1 and 8 missing values.

**Result:** Identified the extent of missing data in each column before deciding how to handle it.

# 🟢 LEVEL 2 — Missing Values
## Task 6: Calculate Missing-Value Percentages

### Calculated the number and percentage of missing values in each column to assess data completeness.

**Code:**

```python
missing_percentage = df.isna().mean() * 100.0

result = pd.DataFrame({
    "missing_count": missing_values,
    "missing_percentage": missing_percentage
})

result = result.reset_index(names="Column")
result
```
**Results:**

| Column | Missing Count | Missing Percentage |
|---|---:|---:|
| employee_id | 0 | 0.0% |
| name | 1 | 0.2% |
| department | 1 | 0.2% |
| age | 1 | 0.2% |
| salary | 1 | 0.2% |
| bonus | 37 | 7.4% |
| join_date | 7 | 1.4% |
| email | 8 | 1.6% |
| phone | 4 | 0.8% |
| gender | 27 | 5.4% |
| status | 26 | 5.2% |

**Key observations:**
- `bonus` has the highest missing percentage at **7.4%**.
- `gender` has **5.4%** missing values, while `status` has **5.2%**.
- `employee_id` has **no missing values**.
- The remaining columns have missing percentages ranging from **0.2% to 1.6%**.

**Result:** Measured data completeness for each column using both missing counts and percentages.

**Checks performed:**
- Calculated missing-value percentages using `df.isna().mean() * 100.0`.
- Combined missing counts and percentages into a single DataFrame.
- Converted the column-name index into a regular `Column` column.

**Result:** Created a summary table showing the missing-value count and missing percentage for each column, helping identify columns with incomplete data.

# 🟢 LEVEL 2 — Missing Values
## Task 7: Replace Fake Missing Values

### Replaced predefined text markers representing missing data with actual `NaN` values.

**Code:**

```python
fake_missing = ["N/A", "NULL", "unknown", "Unknown", "", "null"]

for cl in df.columns:
    df[cl] = df[cl].replace(fake_missing, np.nan)

df.head(10)
```

**Cleaning performed:**
- Defined common fake missing-value markers in a list.
- Replaced matching values across all columns with `np.nan`.
- Included different capitalization variants such as `"NULL"` and `"null"`.
- Displayed the first 10 rows to inspect the cleaned data.

**Result:** Standardized the specified fake missing-value markers into actual missing values, making them easier to identify and analyze using Pandas.

# 🟢 LEVEL 2 — Missing Values
## Task 8: Decide How to Handle Missing Values

### Reviewed the missing-value handling strategy and postponed filling or removing missing values until the other cleaning tasks are completed.

**Reason:** Filling missing values too early could affect missing-value indicators and subsequent data-cleaning steps.

**Planned approach:** Decide whether to fill, retain, or remove missing values based on the meaning of each column and the analysis requirements.

# 🟢 LEVEL 2 — Missing Values
## Task 9: Create Missing Indicators

### Created binary indicator columns to identify missing values in selected fields.

**Code:**

```python
missing_columns = ['age', 'salary', 'email', 'phone']

for cl in missing_columns:
    df[cl + "_missing"] = df[cl].isna().astype("int")
```

**Indicator columns created:**
- `age_missing`
- `salary_missing`
- `email_missing`
- `phone_missing`

**Example:**

| age | age_missing |
|---:|---:|
| 25 | 0 |
| NaN | 1 |
| 32 | 0 |

**Result:** Missing values are represented by `1`, while non-missing values are represented by `0`. These indicators make missing-data patterns easier to analyze.
# 🟡 LEVEL 3 — Data Types
## Task 10: Convert Age to Numeric

### Converted the `age` column to a numeric data type and handled invalid values.

**Code:**

```python
df["age"] = pd.to_numeric(df["age"], errors="coerce")
```

**Cleaning performed:**
- Converted valid numeric strings into numeric values.
- Used `errors="coerce"` to replace invalid values with `NaN`.
- Displayed the first 10 rows to inspect the results.

**Examples:**

| Before | After |
|---|---:|
| `"25"` | `25` |
| `" 30 "` | `30` |
| `"35"` | `35` |
| `"twenty-eight"` | `NaN` |
| `"N/A"` | `NaN` |

**Result:** The `age` column contains numeric values where conversion is possible, while invalid values are represented as missing data.

# 🟡 LEVEL 3 — Data Types
## Task 11: Convert Salary to Numeric

### Cleaned the `salary` column by removing currency symbols and thousands separators, then converted the values to a numeric data type.

**Code:**

```python
df["salary"] = df["salary"].str.replace('$', "")
df["salary"] = df["salary"].str.replace(',', "")
df["salary"] = pd.to_numeric(df["salary"], errors="coerce")
```



| Original Value | Converted Value |
|---|---:|
| `"$5,000"` | 5000 |
| `"6000"` | 6000 |
| `"7,500.50"` | 7500.50 |
| `"8,000"` | 8000 |

**Result:** Converted salary values into numeric data suitable for calculations and analysis. Invalid values are converted to `NaN`.

# 🟡 LEVEL 3 — Data Types
### Task 12 — Convert Bonus to Numeric

Clean the `bonus` column by removing currency symbols (`$`) and commas, then convert the values to a numeric data type using `pd.to_numeric()`.

Invalid values such as `"unknown"` are converted to `NaN` using `errors="coerce"`.

**Examples:**

| **Original Value** | **Converted Value** |
| ------------------ | ------------------: |
| `"$750"`           | 750                 |
| `"1,000"`          | 1000                |
| `"-200"`           | -200                |
| `"unknown"`        | `NaN`               |

🟡 LEVEL 3 — Data Types
### Task 13 — Convert Join Date

Convert valid date values into Pandas datetime format using `pd.to_datetime()`.

Invalid values are converted to `NaT` using `errors="coerce"`.

| **Original Value** | **Converted Value** |
|---|---|
| `"2020-01-15"` | `2020-01-15` |
| `"15/02/2021"` | `2021-02-15` |
| `"invalid"` | `NaT` |
| `"2020/13/01"` | `NaT` |
| `"N/A"` | `NaT` |

**Methods Used:**
- `pd.to_datetime()` — Converts valid date strings into datetime values.
- `errors="coerce"` — Converts invalid dates into `NaT`.
- `format="mixed"` — Allows Pandas to infer different date formats.
- `dayfirst=True` — Interprets dates with the day before the month when applicable.

**Result:** The `join_date` column is converted to Pandas datetime format, ready for date-based analysis.

## 🟡 LEVEL 4 — Text Cleaning

### Task 14 — Standardize Names

Convert names into proper case and remove multiple spaces between words.

| **Original Value** | **Cleaned Value** |
|---|---|
| `"JOHN SMITH"` | `"John Smith"` |
| `"john smith"` | `"John Smith"` |
| `"John Smith"` | `"John Smith"` |
| `"mohammed  hassan"` | `"Mohammed Hassan"` |
| `"Lisa   Thomas"` | `"Lisa Thomas"` |

**Methods Used:**
- `.str.title()` — Converts the first letter of each word to uppercase.
- `.str.replace()` — Uses a regular expression to clean whitespace.

**Result:** Names are standardized into proper case with consistent spacing.

**Note:** To remove multiple spaces between words and leading/trailing spaces, use:

```python
df["name"] = df["name"].str.title()
df["name"] = df["name"].str.replace(r"\s+", " ", regex=True).str.strip()
```
## 🟡 LEVEL 4 — Text Cleaning
## Task 15 — Clean Department

### Standardize department names by converting inconsistent capitalization and naming variations into consistent department names.

**Expected Output:**

| **Original Value** | **Cleaned Value** |
|---|---|
| `IT` | `IT` |
| `it` | `IT` |
| `IT Department` | `IT` |
| `HR` | `HR` |
| `Human Resources` | `HR` |
| `Human Resource` | `HR` |
| `sales` | `Sales` |
| `Sales` | `Sales` |
| `SALES` | `Sales` |
| `Finance` | `Finance` |
| `FINANCE` | `Finance` |
| `finance` | `Finance` |
| `Operations` | `Operations` |
| `operations` | `Operations` |
| `Marketing` | `Marketing` |
| `marketing` | `Marketing` |

**Methods Used:**
- `.str.lower()` — Converts department names to lowercase before mapping.
- `.map()` — Maps inconsistent department names to standardized values.

**Result:** Department names are standardized into consistent categories, ready for grouping and analysis.

## 🟡 LEVEL 4 — Text Cleaning
## Task 16 — Clean Gender

### Standardize gender values by converting inconsistent capitalization and abbreviations into consistent categories.

**Expected result:**

- **`"Male"` → `"Male"`**
- **`"MALE"` → `"Male"`**
- **`"M"` → `"Male"`**
- **`"m"` → `"Male"`**
- **`"Female"` → `"Female"`**
- **`"F"` → `"Female"`**
- **`"female"` → `"Female"`**
- **`"other"` → `"Other"`**

**Methods used:**
- `.str.lower()` — Converts gender values to lowercase.
- `.map()` — Maps gender variations and abbreviations to standardized values.

**Result:** Gender values are standardized into two consistent categories: `Male` and `Female`. Values not found in the mapping dictionary become `NaN`.

## 🟡 LEVEL 4 — Text Cleaning
## Task 17 — Clean Status

**Objective:** Standardize inconsistent status values into consistent categories.

**Values handled:**
- `Active`, `active`, `ACTIVE`, `A` → `Active`
- `Inactive`, `inactive`, `INACTIVE`, `I` → `Inactive`
- `in active` → `Inactive`

**Approach:**
- Converted status values to lowercase for consistent matching.
- Used a dictionary with `map()` to standardize status values.

**Result:** Status values are standardized into two consistent categories: `Active` and `Inactive`.

## 🟡 LEVEL 5 — Text Cleaning
## Task 18 — Clean Email Whitespace

**Objective:** Remove leading and trailing whitespace from email addresses.

**Code:**
```python
df["email"] = df["email"].str.strip()
```

**Example:**

| Before | After |
|---|---|
| `" mary.johnson@gmail.com "` | `"mary.johnson@gmail.com"` |

**Result:** Email addresses are cleaned by removing unnecessary whitespace from the beginning and end.

## 🟡 LEVEL 5 — Text Cleaning
## Task 19 — Validate Email Addresses

**Objective:** Identify email addresses that do not follow the expected format without deleting them.

**Expected format:**
- `something@domain.extension`

**Examples of invalid emails:**
- `sarakhan@gmail`
- `james.miller@gmail`
- `unknown`

**Approach:**
- Used a regular expression (`regex`) to check the email format.
- Created a new `email_valid` column.
- Marked emails that match the pattern as `True` and invalid formats as `False`.
- Retained the original email values for further review.

**Result:** Email addresses with invalid formats can be identified without immediately deleting them.

## 🟡 LEVEL 5 — Text Cleaning
## Task 20 — Create Email Domain

**Objective:** Extract the domain from each email address and store it in a new column named `email_domain`.

**Example:**

| Email | Email Domain |
|---|---|
| `john.smith@gmail.com` | `gmail.com` |
| `mary@yahoo.com` | `yahoo.com` |
| `user@company.com` | `company.com` |

**Approach:**
- Used a regular expression to extract the text after `@`.
- Created a new column named `email_domain`.
- Retained missing email values as missing.

**Result:** Email domains are extracted into a separate column for further analysis.

## 🟠 LEVEL 6 — Phone Cleaning
## Task 21 — Remove Phone Formatting

**Objective:** Standardize phone numbers by removing spaces, hyphens, and other non-digit characters.

**Code:**
```python
df["phone"] = df["phone"].str.replace(r"\D", "", regex=True)
```

**Example:**

| Before | After |
|---|---|
| `+974 5555 1111` | `97455551111` |
| `974-5555-2222` | `97455552222` |
| `974 5555 5555` | `97455555555` |
| `+974-5555-8888` | `97455558888` |

**Approach:**
- Used a regular expression (`\D`) to identify non-digit characters.
- Removed all non-digit characters using `.str.replace()`.
- Preserved the country code digits.

**Result:** Phone numbers are standardized into digits-only format.

## 🟠 LEVEL 6 — Phone Cleaning
## Task 22 — Standardize Qatar Phone Numbers

**Objective:** Standardize Qatar phone numbers by adding the country code `974` to 8-digit phone numbers and validating the expected format.

**Approach:**
- Identified phone numbers containing exactly 8 digits.
- Added the country code `974` to those numbers.
- Used `str.fullmatch()` with a regular expression to validate phone numbers.
- Created a new column named `phone_valid` to identify valid and invalid numbers.

**Code:**
```python
df.loc[df["phone"].str.len() == 8, "phone"] = (
    '974' + df.loc[df["phone"].str.len() == 8, "phone"]
)

df["phone_valid"] = df["phone"].str.fullmatch(
    r"974\d{8}", na=False
)
```

**Expected Results:**

| Original Phone | Standardized Phone | `phone_valid` |
|---|---|---|
| `55551111` | `97455551111` | `True` |
| `97455551111` | `97455551111` | `True` |
| `9745555222` | `9745555222` | `False` |
| `12345678901` | `12345678901` | `False` |

**Conclusion:** Eight-digit phone numbers were standardized by adding the Qatar country code. The `phone_valid` column identifies numbers that match the expected format. Invalid phone numbers were retained for further review rather than deleted.

**Note:** This validation checks the number's format only; it does not confirm whether the phone number is active or assigned.

## 🟠 LEVEL 7 — Numeric Validation
## Task 23 — Find invalid ages

**Objective:** Identify employee ages outside the valid age range of 18–65.

**Code:**
```python
df["age_valid"] = df["age"].between(18, 65, inclusive="both")
```

**Examples:**

| Age | age_valid |
|---:|:---|
| 150 | False |
| -5 | False |
| 25 | True |
| 65 | True |

**Approach:**
- Used Pandas `between()` to check whether ages fall within the valid range.
- Created a new column named `age_valid`.
- Marked ages within the range as `True` and ages outside it as `False`.

**Result:** Invalid ages are identified without deleting the original records.

## 🟠 LEVEL 7 — Numeric Validation
## Task 24 — Find Invalid Salaries

**Objective:** Identify salaries that do not meet the valid salary condition.

**Valid salary condition:** Salary must be greater than `0`.

**Code:**
```python
df["salary_valid"] = df["salary"] > 0
```

**Examples:**

| Salary | salary_valid |
|---:|:---|
| -5000 | False |
| 5000 | True |
| 0 | False |

**Approach:**
- Compared each salary with zero.
- Created a new column named `salary_valid`.
- Marked salaries greater than zero as `True` and salaries equal to or below zero as `False`.

**Result:** Invalid salaries are identified without modifying or deleting the original values.

## 🟠 LEVEL 7 — Numeric Validation
## Task 25 — Find Invalid Bonuses

**Objective:** Identify bonus values that do not meet the valid bonus condition.

**Valid bonus condition:** Bonus must be greater than or equal to `0`.

**Code:**
```python
df["bonus_valid"] = df["bonus"] >= 0
```

**Examples:**

| Bonus | bonus_valid |
|---:|:---|
| -200 | False |
| 500 | True |
| 0 | True |

**Approach:**
- Compared each bonus value with zero.
- Created a new column named `bonus_valid`.
- Marked non-negative bonuses as `True` and negative bonuses as `False`.

**Result:** Invalid bonus values are identified without modifying or deleting the original values.

## 🟠 LEVEL 8 — Outlier Detection
## Task 26 — Detect Salary Outliers Using IQR

**Objective:   Identify unusually low or high salaries using the Interquartile Range (IQR) method.**

**Approach:**
- Calculated the first quartile (Q1) using the 25th percentile.
- Calculated the third quartile (Q3) using the 75th percentile.
- Calculated the IQR as the difference between Q3 and Q1.
- Calculated the lower and upper outlier boundaries.
- Created a new column named `salary_outlier` to identify potential outliers.

**Formulas:**
- `IQR = Q3 - Q1`
- `Lower Boundary = Q1 - (1.5 × IQR)`
- `Upper Boundary = Q3 + (1.5 × IQR)`

**Result:**
- `True` — salary is below the lower boundary or above the upper boundary.
- `False` — salary is within the boundaries.

**Potential outliers were identified without immediately deleting or modifying the original salary values.**

## 🟠 LEVEL 8 — Outlier Detection
## Task 27 — Detect Age Outliers Using IQR

**Objective:** Identify unusually low or high employee ages using the Interquartile Range (IQR) method.

**Approach:**
- Calculated the first quartile (Q1) using the 25th percentile.
- Calculated the third quartile (Q3) using the 75th percentile.
- Calculated the IQR as the difference between Q3 and Q1.
- Calculated the lower and upper outlier boundaries.
- Created a new column named `age_outlier` to identify potential outliers.

**Formulas:**
- `IQR = Q3 - Q1`
- `Lower Boundary = Q1 - (1.5 × IQR)`
- `Upper Boundary = Q3 + (1.5 × IQR)`

**Result:**
- `True` — age is below the lower boundary or above the upper boundary.
- `False` — age is within the boundaries.

Potential age outliers were identified without deleting or modifying the original age values.

# 🔴 LEVEL 9 — Duplicate Cleaning
## Task 28 — Find Duplicate Rows

**Objective:** Identify duplicate rows in the dataset.

**Approach:**
- Used `df.duplicated()` to check for duplicate rows.
- Compared each row with the rows that appeared before it.
- Returned `True` for duplicate rows and `False` for non-duplicate rows.

**Result:**
- `True` — the row duplicates a previous row.
- `False` — the row is not a duplicate.

**Note:** By default, Pandas checks all columns when identifying duplicate rows. The first occurrence is marked as `False`, while subsequent identical rows are marked as `True`.

Duplicate rows were identified without deleting or modifying the original dataset.

# 🔴 LEVEL 9 — Duplicate Cleaning
## Task 29 — Find Duplicate Employee IDs

**Objective:** Identify employee records with duplicate employee IDs.

**Approach:**
- Checked the `employee_id` column for duplicate values.
- Used `duplicated()` with `subset="employee_id"` to check only employee IDs.
- Set `keep=False` to return all occurrences of duplicated IDs.
- Filtered the DataFrame to display the matching records.

**Code:**
```python
duplicate_employee_id = df[df.duplicated(subset="employee_id", keep=False)]
duplicate_employee_id
```

**Result:**

| Employee ID | Number of Records |
|---|---:|
| E015 | 2 |
| E500 | 2 |
| **Total** | **4** |

**Conclusion:** Two employee IDs, `E015` and `E500`, appear more than once. All four corresponding records were identified for further review without deleting any data.

## 🔴 LEVEL 9 — Duplicate Cleaning
## Task 30 — Remove Exact Duplicate Rows

**Objective:** Remove exact duplicate rows while preserving records with duplicate employee IDs that require further investigation.

**Approach:**
- Counted exact duplicate occurrences using `df.duplicated().sum()`.
- Displayed all rows involved in duplication using `df.duplicated(keep=False)`.
- Removed exact duplicate rows using `df.drop_duplicates()`.
- Verified the result by checking for duplicates again.

**Code:**
```python
df = df.drop_duplicates()

number_of_exact_duplicate = df.duplicated().sum()

print("Number of exact duplicate:", number_of_exact_duplicate)
```

**Results:**

| Metric | Result |
|---|---:|
| Duplicate occurrences before cleaning | 1 |
| Rows involved in duplication | 2 |
| Duplicate occurrences after cleaning | 0 |

**Duplicate Employee ID Investigation:**
- **E500:** Two records had identical details. One duplicate copy was removed.
- **E015:** Records had different details. Both were retained for further investigation.

**Conclusion:** Exact duplicate rows were removed successfully. Records sharing an employee ID were not automatically deleted; their details were considered before deciding how to handle them.

## 🔴 LEVEL 10 — Advanced Cleaning
### Task 31 — Clean Invalid Dates

**Objective:** Convert mixed date formats into datetime values and identify invalid dates.

**Approach:**
- Used `pd.to_datetime()` to convert the `join_date` column into datetime format.
- Used `format="mixed"` to handle different date formats.
- Used `dayfirst=True` to interpret dates with the day before the month where applicable.
- Used `errors="coerce"` to convert invalid or unrecognized values into `NaT`.
- Created `join_date_valid` to identify successfully converted dates.

## 🔴 LEVEL 10 — Advanced Cleaning
## Task 33 — Detect Future Join Dates

**Objective:** Identify employee records with join dates later than the current date.

**Approach:**
- Retrieved today's date using `pd.Timestamp.today().normalize()`.
- Compared each employee's `join_date` with today's date.
- Created a new column named `future_join_date` to flag future dates.

**Code:**
```python
today = pd.Timestamp.today().normalize()
print(today)

df["future_join_date"] = df["join_date"] > today

df.head()
```

**Expected Results:**

| Join Date | `future_join_date` |
|---|---|
| A date before today | `False` |
| Today's date | `False` |
| A date after today | `True` |
| Missing date (`NaT`) | `False` |

**Conclusion:** Future join dates were identified using the current date. Records flagged as `True` can be reviewed to determine whether they are valid future hires or data-entry errors.

## 🔴 LEVEL 10 — Advanced Cleaning
## Task 34 — Standardize All Categorical Columns

**Objective:** Standardize categorical columns by retaining approved values and replacing unexpected values with `NaN`.

**Allowed Values:**

| Column | Allowed Values |
|---|---|
| `department` | IT, HR, Sales, Finance, Operations, Marketing |
| `gender` | Male, Female, Other |
| `status` | Active, Inactive |

**Approach:**
- Used `isin()` to check whether values belong to the approved categories.
- Used `where()` to retain allowed values.
- Replaced values outside the approved categories with `NaN`.

**Conclusion:** The `department`, `gender`, and `status` columns now follow their defined allowed values. Unexpected categories are marked as missing for further review.


## 🔴 LEVEL 11 — Advanced Conditional Cleaning
## Task 35 — Create Salary Band

**Objective:** Categorize employee salaries into `Low`, `Medium`, and `High` bands.

**Salary Band Rules:**

| Salary Range | Salary Band |
|---|---|
| Below 6000 | Low |
| 6000–7999 | Medium |
| 8000 and above | High |

**Approach:**
- Created conditions using comparison operators and `between()`.
- Used `np.select()` to assign labels based on the conditions.
- Added the results to a new column named `salary_band`.

**Conclusion:** Each valid salary is assigned to its corresponding salary band, making salary groups easier to analyze and compare.

## 🔴 LEVEL 11 — Advanced Conditional Cleaning
## Task 36 — Create Employee Age Group

**Objective:** Categorize employees into age groups to support workforce demographic analysis.

**Age Group Rules:**

| Age Range | Age Group |
|---|---|
| 18–25 | Young |
| 26–35 | Early Career |
| 36–50 | Mid Career |
| 51–65 | Senior |

**Approach:**
- Used `pd.cut()` to divide ages into defined intervals.
- Assigned descriptive labels to each age group.
- Stored the results in a new column named `age_group`.

**Result:** Employees aged 18–65 are assigned to the appropriate age group. Ages below 18, above 65, and missing ages remain `NaN`.

**Conclusion:** Age grouping makes it easier to summarize and compare employee demographics.

## 🔴 LEVEL 11 — Advanced Conditional Cleaning
## Task 37 — Create Employee Review Status

**Objective:** Assign an overall review status to each employee based on data validation results.

**Review Rules:**

| Condition | Review Status |
|---|---|
| All six validation checks pass | Good |
| One or more validation checks fail | Needs Review |

**Validation Columns:**
- `age_valid`
- `salary_valid`
- `bonus_valid`
- `email_valid`
- `phone_valid`
- `join_date_valid`

**Approach:**
- Selected the six validation columns.
- Used `.all(axis=1)` to check whether all six validations pass for each employee.
- Used `np.where()` to assign `Good` when all checks pass and `Needs Review` otherwise.
- Displayed employee values, validation results, and review status together to inspect the results.

**Result:** Employees who pass all six validation checks are marked `Good`. Employees

## 🟣LEVEL 12 — Complete Data Quality Report
## Task 38 — Create a Complete Data Quality Report

**Objective:** Generate a consolidated data quality report for the employee dataset.

**Report Metrics:**

| Metric | Description |
|---|---|
| Missing | Number of missing values in each column |
| Invalid | Number of values failing the available validation checks |
| Duplicates | Number of repeated values within each column |

**Approach:**
- Selected 11 original employee data columns.
- Used `isna().sum()` to calculate missing-value counts.
- Used existing validation flags to calculate invalid counts for age, salary, bonus, email, phone, and join date.
- Used `duplicated().sum()` to count repeated values within each column.
- Combined the metrics into a single summary table.

**Important Note:** The invalid counts for `name`, `department`, `gender`, and `status` are zero because separate validation checks have not been defined for these columns. This does not necessarily mean those columns contain no invalid values.

**Conclusion:** The report provides a consolidated overview of missing values, validated invalid values, and repeated values, helping identify areas that may require further data cleaning.

## 🟢 Bonus Task — Handle Missing Values

**Objective:** Handle missing values using appropriate imputation strategies.

**Approach:**
- Replaced missing bonus values with `0`, based on the assumption that missing means no bonus.
- Filled missing age and salary values using their respective medians.
- Filled missing department, gender, and status values using the mode when available.
- Added a check to avoid errors when a categorical column contains no valid values.

**Note:** Imputation strategies were selected based on the assumed business meaning of the data. In real projects, these assumptions should be verified before replacing missing values.

## 🔵Final Master Task — Export Cleaned Data to Excel

**Objective:** Export the final cleaned DataFrame to Excel.

**Approach:**
- Used Pandas `to_excel()` to export `clean_df`.
- Set `index=False` to exclude the DataFrame index.

**Output:** `cleaned_employee_data.xlsx`