# Data Exploration with Pandas

## What is Data Exploration?

Data exploration (also called **Exploratory Data Analysis** or EDA) is the first step after loading a dataset.
The goal is to **understand the structure, content, and quality** of your data before doing any analysis or modeling.

---

## 1. Viewing Data

### `head()` and `tail()`

```python
import pandas as pd

df = pd.read_csv("students.csv")

print(df.head())     # First 5 rows (default)
print(df.head(10))   # First 10 rows
print(df.tail())     # Last 5 rows
print(df.tail(3))    # Last 3 rows
```

### `info()` — Column Types & Non-null Counts

```python
df.info()
# <class 'pandas.core.frame.DataFrame'>
# RangeIndex: 100 entries, 0 to 99
# Data columns (total 5 columns):
#  #   Column      Non-Null Count  Dtype
# ---  ------      --------------  -----
#  0   Name        100 non-null    object
#  1   Age         98 non-null     float64
#  2   Department  100 non-null    object
#  3   Score       95 non-null     float64
#  4   City        100 non-null    object
```

### `describe()` — Statistical Summary

```python
df.describe()
# Shows: count, mean, std, min, 25%, 50%, 75%, max
# Only for numeric columns by default

df.describe(include="all")     # Include non-numeric columns too
df.describe(include="object")  # Only string/object columns
```

### `sample()` — Random Rows

```python
print(df.sample(5))     # 5 random rows
print(df.sample(frac=0.1))  # 10% of the data randomly
```

---

## 2. Shape and Indexing

### Shape
```python
print(df.shape)          # (rows, columns) → (100, 5)
print(df.shape[0])       # Number of rows
print(df.shape[1])       # Number of columns
```

### Index
```python
print(df.index)          # RangeIndex(start=0, stop=100, step=1)
df.index = range(1, 101) # Set custom index starting from 1
print(df.index)          # RangeIndex(start=1, stop=101, step=1)
```

### Columns
```python
print(df.columns)            # Column names as Index object
print(df.columns.tolist())   # As a Python list
print(len(df.columns))       # Number of columns
```

### Other Useful Attributes
```python
print(df.dtypes)    # Data type of each column
print(df.size)      # Total elements (rows × cols)
print(df.ndim)      # Number of dimensions (2 for DataFrame)
print(df.values)    # Underlying NumPy array
```

---

## 3. Selecting Data

### Select a Single Column
```python
print(df["Name"])          # Returns a Series
print(df.Name)             # Same result (dot notation — avoid for col names with spaces)
```

### Select Multiple Columns
```python
print(df[["Name", "Age", "Score"]])   # Returns a DataFrame
```

### Select Rows by Index (Position) — `iloc`

`iloc` uses integer position (0-based).

```python
print(df.iloc[0])          # First row as Series
print(df.iloc[0:5])        # First 5 rows
print(df.iloc[-1])         # Last row
print(df.iloc[2, 3])       # Row 2, Column 3 (single value)
print(df.iloc[:, 1])       # All rows, column 1
print(df.iloc[0:3, 0:2])   # Rows 0-2, columns 0-1
```

### Select Rows by Label — `loc`

`loc` uses labels (index labels and column names).

```python
print(df.loc[0])                     # Row with label/index 0
print(df.loc[0:4])                   # Rows 0 to 4 (inclusive)
print(df.loc[0, "Name"])             # Value at row 0, column 'Name'
print(df.loc[:, "Name"])             # All rows, 'Name' column
print(df.loc[0:3, ["Name", "Age"]]) # Rows 0-3, selected columns
```

### `iloc` vs `loc` Summary

| Feature    | `iloc`                    | `loc`                          |
| ---------- | ------------------------- | ------------------------------ |
| Based on   | Integer position (0-based)| Labels (index labels, col names)|
| End index  | Exclusive                 | Inclusive                      |
| Usage      | `df.iloc[0:3, 1:3]`       | `df.loc[0:3, ["Name", "Age"]]` |

---

## 4. Column and Row Selection

### Add a New Column
```python
df["Full_Marks"] = 100
df["Percentage"] = (df["Score"] / df["Full_Marks"]) * 100
```

### Drop a Column
```python
df = df.drop(columns=["Full_Marks"])           # Remove one column
df = df.drop(columns=["Col1", "Col2"])         # Remove multiple
```

### Drop a Row
```python
df = df.drop(index=0)          # Remove row with index 0
df = df.drop(index=[1, 3, 5])  # Remove multiple rows
```

### Reorder Columns
```python
df = df[["Name", "Score", "Age", "City"]]  # New column order
```

### Rename Columns
```python
df = df.rename(columns={"Score": "Marks", "City": "Location"})
```

---

## 5. Filtering with Conditions

### Single Condition
```python
# Students who scored above 80
high_scorers = df[df["Score"] > 80]
print(high_scorers)

# Students from Chennai
chennai = df[df["City"] == "Chennai"]
```

### Multiple Conditions
```python
# Use & (and), | (or), ~ (not) — always wrap conditions in ()
top_students = df[(df["Score"] > 80) & (df["Age"] < 22)]
print(top_students)

# Students from Chennai OR Coimbatore
south = df[(df["City"] == "Chennai") | (df["City"] == "Coimbatore")]
```

### `isin()` — Filter by List of Values
```python
cities = ["Chennai", "Coimbatore", "Madurai"]
south_students = df[df["City"].isin(cities)]
```

### `between()` — Range Filter
```python
young_adults = df[df["Age"].between(18, 25)]
```

### `str.contains()` — Partial String Match
```python
# Students whose name starts with 'A'
a_students = df[df["Name"].str.startswith("A")]

# Students with 'kumar' in name (case-insensitive)
kumar = df[df["Name"].str.contains("kumar", case=False)]
```

### `query()` — SQL-like Filtering
```python
result = df.query("Score > 80 and Age < 25")
result = df.query("City == 'Chennai'")
```

### `notna()` and `isna()` — Filter by Null Values
```python
# Rows where Score is NOT null
valid = df[df["Score"].notna()]

# Rows where Score IS null
missing = df[df["Score"].isna()]
```

---

## 6. Sorting Data

```python
# Sort by single column (ascending by default)
df.sort_values("Score", ascending=False)

# Sort by multiple columns
df.sort_values(["Department", "Score"], ascending=[True, False])

# Sort by index
df.sort_index()
```

---

## 7. Value Counts

```python
# Count of each unique value in a column
print(df["City"].value_counts())
print(df["Department"].value_counts(normalize=True))  # As percentages
```

---

## 8. Unique Values

```python
print(df["City"].unique())      # Array of unique values
print(df["City"].nunique())     # Count of unique values
```

---

## 🎯 Student Tasks – Module 07: Data Exploration with Pandas

### Task 1: Dataset Quick Explorer (Easy)
**Objective**: Use head, tail, info, describe on a real dataset.

**Instructions**:
1. Create a CSV file `employees.csv` with columns: `emp_id`, `name`, `department`, `salary`, `experience`, `city`.
   Add 15 rows of data.
2. Load it with Pandas.
3. Print:
   - Shape
   - `head(5)` and `tail(3)`
   - `info()` output
   - `describe()` for numeric columns
   - Number of unique departments

**Expected Output**:
```
Shape: (15, 6)
Unique Departments: 4
Average Salary: 58000.0
```

---

### Task 2: loc vs iloc Data Retrieval (Medium)
**Objective**: Practice iloc and loc for data access.

**Instructions**:
Using the same `employees.csv`:
1. Use `iloc` to get:
   - The first 3 rows
   - Rows 5–9
   - The 3rd column only
2. Use `loc` to get:
   - Rows where index is 0–4 and columns `name`, `salary`
   - A single cell value (row 2, column `department`)
3. Filter employees with salary > 50000 AND experience > 3 years.
4. Get all employees from IT department using `isin()`.

**Expected Output**:
```
First 3 rows via iloc:
[shows rows]

Filtered (salary > 50000 and experience > 3):
[shows matching rows]

IT Department employees:
[shows IT employees]
```

---

### Task 3: Comprehensive EDA on Real Dataset (Challenge)
**Objective**: Perform full exploratory analysis on a dataset.

**Instructions**:
Download or create a dataset `sales_data.csv` with: `date`, `product`, `region`, `quantity`, `unit_price`, `total_sale`.

Perform:
1. Load and display shape, dtypes, first 5 rows.
2. Find:
   - Product with highest total sales
   - Region with most transactions
   - Average quantity per product
3. Filter transactions where `total_sale` > average total sale.
4. Sort by `total_sale` descending and display top 10.
5. Count unique products per region using `value_counts()`.
6. Print a summary report.

**Expected Output**:
```
=== Sales Data EDA Report ===
Shape: (200, 6)
Unique Products: 12
Unique Regions: 4

Top Product: Laptop (Total Sales: ₹450,000)
Best Region: South (150 transactions)
Average Quantity: 5.3

Top 10 Transactions:
[sorted by total_sale descending]
```

---
