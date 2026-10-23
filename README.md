# Pandas Data Cleaning Project

## Project Overview

##This project demonstrates practical data cleaning using Pandas.

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