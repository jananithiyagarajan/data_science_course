# Data Cleaning using Pandas

## What is Data Cleaning?

**Data Cleaning** (also called data wrangling or data munging) is the process of identifying and fixing problems in a dataset — such as missing values, duplicates, incorrect data types, and inconsistent formatting.

> Real-world data is never perfect. A data scientist typically spends **60–80% of their time cleaning data**.

---

## 1. Handling Missing Data

### Detecting Missing Values

```python
import pandas as pd
import numpy as np

df = pd.read_csv("students.csv")

# Check for missing values — returns True/False
print(df.isnull())

# Count missing values per column
print(df.isnull().sum())

# Total missing values in the entire DataFrame
print(df.isnull().sum().sum())

# Percentage of missing values per column
print((df.isnull().sum() / len(df)) * 100)

# Rows with at least one missing value
print(df[df.isnull().any(axis=1)])
```

### Dropping Missing Values

```python
# Drop rows with ANY missing values
df_clean = df.dropna()

# Drop rows where ALL values are missing
df_clean = df.dropna(how="all")

# Drop rows only if missing in specific columns
df_clean = df.dropna(subset=["Score", "Name"])

# Drop columns with missing values
df_clean = df.dropna(axis=1)

# Drop columns where more than 50% values are missing
threshold = len(df) * 0.5
df_clean = df.dropna(thresh=threshold, axis=1)
```

### Filling Missing Values

```python
# Fill all NaN with a constant value
df["Score"].fillna(0, inplace=True)

# Fill with mean (good for numeric columns)
mean_score = df["Score"].mean()
df["Score"].fillna(mean_score, inplace=True)

# Fill with median (better if outliers exist)
df["Salary"].fillna(df["Salary"].median(), inplace=True)

# Fill with mode (for categorical columns)
df["City"].fillna(df["City"].mode()[0], inplace=True)

# Forward fill — carry forward the last valid value
df["Score"].fillna(method="ffill", inplace=True)

# Backward fill — carry backward the next valid value
df["Score"].fillna(method="bfill", inplace=True)

# Fill with different values per column
df.fillna({"Score": 0, "City": "Unknown", "Age": df["Age"].mean()}, inplace=True)
```

### Replace NaN with `np.nan`

```python
import numpy as np
# Sometimes data has "NA", "null", "None" as strings
df.replace("NA", np.nan, inplace=True)
df.replace(["None", "null", "N/A"], np.nan, inplace=True)
```

---

## 2. Handling Duplicates

### Detecting Duplicates

```python
# Check which rows are duplicated (True = duplicate)
print(df.duplicated())

# Count duplicated rows
print(df.duplicated().sum())

# Show all duplicate rows
print(df[df.duplicated()])

# Show duplicates based on specific columns
print(df.duplicated(subset=["Name", "Email"]))
```

### Removing Duplicates

```python
# Remove duplicate rows — keep first occurrence
df_unique = df.drop_duplicates()

# Keep last occurrence instead of first
df_unique = df.drop_duplicates(keep="last")

# Remove duplicates based on specific columns
df_unique = df.drop_duplicates(subset=["Email"])

# In-place
df.drop_duplicates(inplace=True)
```

---

## 3. Renaming Columns and Index

### Rename Specific Columns

```python
# Rename using a dictionary {old_name: new_name}
df.rename(columns={"Dept": "Department", "Sal": "Salary"}, inplace=True)

# Rename using a function (e.g., lowercase all column names)
df.rename(columns=str.lower, inplace=True)
df.rename(columns=str.upper, inplace=True)
df.rename(columns=lambda x: x.strip().replace(" ", "_"), inplace=True)
```

### Rename All Columns at Once

```python
df.columns = ["emp_id", "emp_name", "dept", "salary", "city"]
```

### Rename Index

```python
df.index.name = "Row_Number"
df.rename(index={0: "first", 1: "second"}, inplace=True)

# Reset index (after filtering)
df.reset_index(drop=True, inplace=True)

# Set a column as the index
df.set_index("emp_id", inplace=True)
```

---

## 4. Changing Data Types

### Check Data Types

```python
print(df.dtypes)
print(df["Salary"].dtype)
```

### Convert Data Types with `astype()`

```python
# String to Integer
df["Age"] = df["Age"].astype(int)

# String to Float
df["Score"] = df["Score"].astype(float)

# Integer to String
df["emp_id"] = df["emp_id"].astype(str)

# Integer to Boolean
df["is_active"] = df["is_active"].astype(bool)

# Category type (memory-efficient for repeated strings)
df["Department"] = df["Department"].astype("category")
```

### Convert Dates with `pd.to_datetime()`

```python
df["JoinDate"] = pd.to_datetime(df["JoinDate"])
df["JoinDate"] = pd.to_datetime(df["JoinDate"], format="%d/%m/%Y")

# Extract date parts
df["Year"] = df["JoinDate"].dt.year
df["Month"] = df["JoinDate"].dt.month
df["Day"] = df["JoinDate"].dt.day
df["DayName"] = df["JoinDate"].dt.day_name()
```

### Convert Numeric with `pd.to_numeric()`

```python
# Coerce errors to NaN (useful when column has non-numeric strings)
df["Salary"] = pd.to_numeric(df["Salary"], errors="coerce")
```

---

## 5. Replacing Values

### `replace()` — Simple Value Replacement

```python
# Replace a single value
df["Status"].replace("Y", "Yes", inplace=True)

# Replace multiple values at once
df["Grade"].replace({"A": 4.0, "B": 3.0, "C": 2.0, "F": 0.0}, inplace=True)

# Replace in entire DataFrame
df.replace(0, np.nan, inplace=True)

# Replace a list of values with a single value
df["City"].replace(["Trichy", "Tiruchirapalli"], "Tiruchirappalli", inplace=True)
```

### `map()` — Apply a Mapping to a Column

```python
gender_map = {"M": "Male", "F": "Female"}
df["Gender"] = df["Gender"].map(gender_map)

# If a value has no mapping, it becomes NaN
# Use fillna() to handle it
df["Gender"] = df["Gender"].map(gender_map).fillna("Unknown")
```

### `apply()` — Apply a Function to a Column or Row

```python
# Apply a lambda to a column
df["Salary"] = df["Salary"].apply(lambda x: x * 1.1)   # 10% raise

# Apply a custom function
def categorize_age(age):
    if age < 25:
        return "Young"
    elif age < 40:
        return "Mid"
    else:
        return "Senior"

df["Age_Group"] = df["Age"].apply(categorize_age)
```

---

## 6. Standardizing String Data

```python
# Strip whitespace
df["Name"] = df["Name"].str.strip()

# Convert to lowercase
df["City"] = df["City"].str.lower()

# Convert to title case
df["Name"] = df["Name"].str.title()

# Remove special characters
df["Phone"] = df["Phone"].str.replace(r"[^0-9]", "", regex=True)

# Pad strings (e.g., zero-pad employee IDs)
df["emp_id"] = df["emp_id"].str.zfill(5)
```

---

## 7. Detecting and Handling Outliers

```python
# Using IQR method to detect outliers
Q1 = df["Salary"].quantile(0.25)
Q3 = df["Salary"].quantile(0.75)
IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

# Flag outliers
df["is_outlier"] = (df["Salary"] < lower_bound) | (df["Salary"] > upper_bound)

# Remove outliers
df_clean = df[(df["Salary"] >= lower_bound) & (df["Salary"] <= upper_bound)]

# Cap outliers (Winsorization)
df["Salary"] = df["Salary"].clip(lower=lower_bound, upper=upper_bound)
```

---

## Complete Data Cleaning Workflow

```python
import pandas as pd
import numpy as np

# 1. Load
df = pd.read_csv("messy_data.csv")

# 2. Inspect
print(df.shape)
print(df.dtypes)
print(df.isnull().sum())
print(df.duplicated().sum())

# 3. Remove duplicates
df.drop_duplicates(inplace=True)

# 4. Handle missing values
df["Salary"].fillna(df["Salary"].median(), inplace=True)
df["City"].fillna("Unknown", inplace=True)
df.dropna(subset=["Name"], inplace=True)  # Drop rows without Name

# 5. Fix data types
df["Age"] = pd.to_numeric(df["Age"], errors="coerce").astype(int)
df["JoinDate"] = pd.to_datetime(df["JoinDate"])

# 6. Rename columns
df.rename(columns={"Sal": "Salary", "Dept": "Department"}, inplace=True)

# 7. Clean strings
df["Name"] = df["Name"].str.strip().str.title()
df["City"] = df["City"].str.strip().str.lower()

# 8. Reset index
df.reset_index(drop=True, inplace=True)

# 9. Save cleaned data
df.to_csv("cleaned_data.csv", index=False)
print("Cleaning complete!")
```

---

## 🎯 Student Tasks – Module 08: Data Cleaning using Pandas

### Task 1: Missing Data Handler (Easy)
**Objective**: Detect and handle missing values.

**Instructions**:
1. Create a DataFrame with intentional `None`/`np.nan` values in 3 columns.
2. Print the count of missing values per column.
3. Fill numeric columns with their mean.
4. Fill string columns with mode.
5. Drop any rows that still have missing values.
6. Verify: `df.isnull().sum()` should be all zeros.

**Expected Output**:
```
Missing values before:
Name      0
Age       3
Score     2
City      1

Missing values after cleaning:
Name     0
Age      0
Score    0
City     0
```

---

### Task 2: Duplicate & Type Fixer (Medium)
**Objective**: Remove duplicates and fix data types.

**Instructions**:
1. Create a DataFrame with 15 rows where 5 are duplicates.
2. Detect and print which rows are duplicated.
3. Remove duplicates (keep first).
4. The `Salary` column has some string entries like `"50k"` — clean it to numeric.
5. The `JoinDate` column is a string — convert to `datetime`.
6. Extract `Year` and `Month` from the date.
7. Rename: `"Emp_Name"` → `"Name"`, `"Dept"` → `"Department"`.

**Expected Output**:
```
Duplicates found: 5
After removing: 10 rows

Salary dtype before: object
Salary dtype after: float64

JoinDate dtype: datetime64
Year column added. Month column added.
```

---

### Task 3: Full Data Cleaning Pipeline (Challenge)
**Objective**: Build a complete cleaning pipeline on a messy dataset.

**Instructions**:
1. Download or create `messy_employees.csv` with intentional issues:
   - Missing values in multiple columns
   - Duplicate rows
   - Wrong data types
   - Inconsistent strings (e.g., "  chennai", "CHENNAI", "Chennai")
   - Salary with outliers (e.g., 9999999)
2. Build a step-by-step cleaning function `clean_dataset(filepath)` that:
   - Removes duplicates
   - Fills/drops missing values
   - Fixes data types
   - Standardizes strings
   - Removes outliers using IQR
   - Resets index
   - Saves to `clean_employees.csv`
3. Print a before/after report.

**Expected Output**:
```
=== Data Cleaning Report ===
Before: 150 rows, 8 columns
Duplicates removed: 12
Missing values filled: 23
Outliers removed: 4
After: 134 rows, 8 columns

Saved to: clean_employees.csv
```

---
