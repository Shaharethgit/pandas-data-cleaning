# Pandas Data Cleaning Project

## Project Overview

This project demonstrates practical data cleaning using Pandas.

## Tools Used

- Pandas
- Python
- Jupyter Notebook
- Excel
- Git and GitHub

## Tasks Completed

# Level 1 — Basic Structural Cleaning

## **Task 1: Inspect the Dataset**

Inspected the dataset to understand its structure and memory usage before starting the cleaning process.

**Checks performed:**
- Found the number of rows and columns using `df.shape`.
- Retrieved column names using `df.columns`.
- Checked data types using `df.dtypes`.
- Inspected dataset information and default memory usage using `df.info()`.
- Checked detailed memory usage, including text data, using `df.info(memory_usage="deep")`.
- Displayed the first five rows using `df.head(5)`.
- Displayed the last five rows using `df.tail(5)`.

## Task 2: Clean Column Names

Standardized column names to make them consistent and easier to use in data analysis.

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

Cleaned whitespace from all string columns to ensure consistent text formatting.

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

Identified empty strings and whitespace-only strings across all columns and replaced them with `NaN`.

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

**Checks performed:**
- Used `df.isna()` to identify missing values.
- Used `.sum()` to calculate the number of missing values in each column.
- Stored the results in `missing_values` for inspection.

**Code:**

```python
missing_values = df.isna().sum()
missing_values
```

**Result:** Obtained the missing-value count for every column to understand the extent of missing data before handling it.