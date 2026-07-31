# Pandas Introduction

## What is Pandas?

**Pandas** is an open-source Python library used for **data manipulation and analysis**.
It provides two key data structures:
- **Series** — 1-dimensional labeled array
- **DataFrame** — 2-dimensional labeled table (like a spreadsheet)

Pandas is built on top of NumPy and is the most widely used tool in Data Science for working with structured data.

---

## 1. Installing and Importing Pandas

### Installation

```bash
pip install pandas
```

### Importing

```python
import pandas as pd    # 'pd' is the standard alias
print(pd.__version__)  # Check version
```

---

## 2. Series

A **Series** is a one-dimensional labeled array that can hold any data type (int, float, str, etc.).

Think of it as a single column in a spreadsheet.

### Creating a Series

```python
import pandas as pd

# From a list
s = pd.Series([10, 20, 30, 40, 50])
print(s)
# 0    10
# 1    20
# 2    30
# 3    40
# 4    50
# dtype: int64

# With custom index
marks = pd.Series([85, 90, 78, 92], index=["Math", "Science", "English", "History"])
print(marks)
# Math       85
# Science    90
# English    78
# History    92
```

### Series Attributes

```python
print(marks.values)   # array([85, 90, 78, 92])
print(marks.index)    # Index(['Math', 'Science', 'English', 'History'])
print(marks.dtype)    # int64
print(marks.shape)    # (4,)
print(marks.size)     # 4
```

### Series Operations

```python
s = pd.Series([10, 20, 30, 40])

print(s + 5)           # Adds 5 to each element
print(s * 2)           # Multiplies each by 2
print(s[s > 15])       # Filter elements > 15
print(s.mean())        # 25.0
print(s.max())         # 40
print(s.describe())    # Statistical summary
```

### Accessing Elements

```python
marks = pd.Series([85, 90, 78], index=["Math", "Science", "English"])

print(marks["Math"])        # 85 — label-based
print(marks[0])             # 85 — position-based
print(marks[["Math", "English"]])  # Multiple items
```

---

## 3. DataFrame

A **DataFrame** is a 2-dimensional labeled data structure — like a table with rows and columns.
Each column is a Series.

### Creating a DataFrame

#### From a Dictionary

```python
data = {
    "Name": ["Arun", "Priya", "Kumar", "Kavya"],
    "Age":  [20, 21, 19, 22],
    "City": ["Chennai", "Coimbatore", "Madurai", "Salem"],
    "Score": [88, 95, 72, 80]
}

df = pd.DataFrame(data)
print(df)
```

Output:
```
    Name  Age        City  Score
0   Arun   20     Chennai     88
1  Priya   21  Coimbatore     95
2  Kumar   19     Madurai     72
3  Kavya   22       Salem     80
```

#### From a List of Dictionaries

```python
records = [
    {"Name": "Ravi", "Marks": 75},
    {"Name": "Meena", "Marks": 88},
    {"Name": "Siva", "Marks": 91},
]

df2 = pd.DataFrame(records)
print(df2)
```

#### From a CSV File

```python
df = pd.read_csv("students.csv")
```

#### From a List of Lists

```python
data = [["Alice", 25], ["Bob", 30], ["Charlie", 22]]
df = pd.DataFrame(data, columns=["Name", "Age"])
```

---

## 4. Exploring a DataFrame

Once you load a DataFrame, use these methods to explore it:

```python
df = pd.read_csv("data.csv")

# Shape: (rows, columns)
print(df.shape)         # (1000, 8)

# First 5 rows
print(df.head())        # default 5
print(df.head(10))      # first 10

# Last 5 rows
print(df.tail())

# Column names
print(df.columns)

# Data types of each column
print(df.dtypes)

# Quick summary: non-null count, dtype per column
print(df.info())

# Statistical summary (mean, std, min, max, etc.)
print(df.describe())

# Number of rows and columns
print(f"Rows: {df.shape[0]}, Columns: {df.shape[1]}")
```

---

## 5. DataFrame Attributes

| Attribute     | Description                                    |
| ------------- | ---------------------------------------------- |
| `df.shape`    | (rows, columns) as a tuple                     |
| `df.columns`  | Column names (Index object)                    |
| `df.index`    | Row index                                      |
| `df.dtypes`   | Data type of each column                       |
| `df.values`   | NumPy array of all values                      |
| `df.size`     | Total number of elements (rows × columns)      |
| `df.ndim`     | Number of dimensions (always 2 for DataFrame)  |

---

## 6. Saving a DataFrame

```python
# Save to CSV
df.to_csv("output.csv", index=False)

# Save to Excel
df.to_excel("output.xlsx", index=False)

# Save to JSON
df.to_json("output.json")
```

---

## 7. Reading Various File Types

```python
# CSV
df = pd.read_csv("data.csv")

# Excel
df = pd.read_excel("data.xlsx", sheet_name="Sheet1")

# JSON
df = pd.read_json("data.json")

# HTML table
df = pd.read_html("https://example.com/table")[0]

# SQL (with SQLAlchemy)
import sqlalchemy
engine = sqlalchemy.create_engine("sqlite:///mydb.db")
df = pd.read_sql("SELECT * FROM students", engine)
```

---

## 🎯 Student Tasks – Module 06: Pandas Introduction

### Task 1: Create Your First DataFrame (Easy)
**Objective**: Practice creating Series and DataFrames.

**Instructions**:
1. Create a `Series` for student scores in 5 subjects with subject names as the index.
2. Create a `DataFrame` with information about 5 students: name, age, city, and scores in 3 subjects.
3. Print:
   - The shape
   - The first 3 rows with `head(3)`
   - Column names
   - Data types with `dtypes`

**Expected Output**:
```
--- Marks Series ---
Maths      88
Science    75
English    90
History    82
Tamil      95
dtype: int64

--- Student DataFrame ---
     Name  Age      City  Maths  Science  English
0    Arun   20   Chennai     88       75       90
1   Priya   21    Madurai    95       88       78
...

Shape: (5, 6)
Columns: Index(['Name', 'Age', 'City', 'Maths', 'Science', 'English'])
```

---

### Task 2: Read & Explore a CSV File (Medium)
**Objective**: Load a real dataset and explore it with Pandas.

**Instructions**:
1. Create a CSV file `employees.csv` with columns: `emp_id`, `name`, `department`, `salary`, `years_exp`.
2. Add at least 10 rows of sample data.
3. Load it with `pd.read_csv()`.
4. Print:
   - Shape
   - `head()` and `tail(3)`
   - `info()` output
   - `describe()` for numerical columns
   - Average salary using `.mean()`

**Expected Output**:
```
Shape: (10, 5)
Average Salary: 55000.0

       salary  years_exp
count    10.0       10.0
mean  55000.0        5.2
std   12500.0        2.1
...
```

---

### Task 3: Series Operations & DataFrame Builder (Challenge)
**Objective**: Combine Series operations with DataFrame construction.

**Instructions**:
1. Create a Series of monthly sales (Jan–Dec) in ₹.
2. Calculate: total annual sales, monthly average, best month, worst month.
3. Create a DataFrame with 2 columns: `Month` and `Sales`.
4. Add a new column `Above_Average` — `True` if that month's sales > average.
5. Save the final DataFrame to `monthly_sales.csv`.

**Expected Output**:
```
Total Sales: ₹1,240,000
Average Monthly Sales: ₹103,333
Best Month: October (₹145,000)
Worst Month: February (₹72,000)

       Month   Sales  Above_Average
0      Jan    95000          False
1      Feb    72000          False
...
9      Oct   145000           True
...

Saved to monthly_sales.csv
```

---
