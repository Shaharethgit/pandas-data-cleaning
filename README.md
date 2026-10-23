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

## Level 1 — Basic Structural Cleaning

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

# LEVEL 2 — Missing Values
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

## Task 8: Decide How to Handle Missing Values

### Reviewed the missing-value handling strategy and postponed filling or removing missing values until the other cleaning tasks are completed.

**Reason:** Filling missing values too early could affect missing-value indicators and subsequent data-cleaning steps.

**Planned approach:** Decide whether to fill, retain, or remove missing values based on the meaning of each column and the analysis requirements.


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

## Task 11: Convert Salary to Numeric

### Cleaned the `salary` column by removing currency symbols and thousands separators, then converted the values to a numeric data type.

**Code:**

```python
df["salary"] = df["salary"].str.replace('$', "")
df["salary"] = df["salary"].str.replace(',', "")
df["salary"] = pd.to_numeric(df["salary"], errors="coerce")
```

**Examples:**

| Original Value | Converted Value |
|---|---:|
| `"$5,000"` | 5000 |
| `"6000"` | 6000 |
| `"7,500.50"` | 7500.50 |
| `"8,000"` | 8000 |

**Result:** Converted salary values into numeric data suitable for calculations and analysis. Invalid values are converted to `NaN`.

