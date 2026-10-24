# Pandas Data Cleaning Project

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
## Task 22 — Validate Qatar Phone Numbers

**Objective:** Identify phone numbers that match the expected Qatar phone number format.

**Expected format:** `974XXXXXXXX` — the country code `974` followed by exactly 8 digits.

**Code:**
```python
df["phone_valid"] = df["phone"].str.fullmatch(r"974\d{8}", na=False)
```

**Approach:**
- Used a regular expression to check the phone number format.
- Used `.str.fullmatch()` to validate the entire phone number.
- Created a new column named `phone_valid`.
- Marked matching phone numbers as `True` and non-matching or missing values as `False`.

**Result:** Phone numbers that do not follow the expected format can be identified without deleting them.

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



