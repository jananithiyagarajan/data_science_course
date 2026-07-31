# Advanced Data Handling with Pandas

## Overview

After mastering data exploration and cleaning, the next step is **transforming and combining data** to extract meaningful insights.
This module covers: transformations, grouping/aggregation, merging, time series, and file I/O.

---

## 1. Data Transformation

### `apply()` — Apply Any Function

`apply()` applies a function to each element, row, or column.

```python
import pandas as pd

df = pd.DataFrame({
    "Name": ["Arun", "Priya", "Kumar"],
    "Score": [88, 95, 72],
    "Salary": [50000, 65000, 45000]
})

# Apply to a column (Series)
df["Score_Grade"] = df["Score"].apply(lambda x: "Pass" if x >= 75 else "Fail")

# Apply custom function
def salary_level(sal):
    if sal > 60000:
        return "High"
    elif sal > 50000:
        return "Medium"
    else:
        return "Low"

df["Salary_Level"] = df["Salary"].apply(salary_level)

# Apply to rows (axis=1)
df["Summary"] = df.apply(
    lambda row: f"{row['Name']} scored {row['Score']}", axis=1
)
print(df)
```

### `map()` — Element-wise Transformation on Series

```python
# Map values using a dictionary
status_map = {"Pass": 1, "Fail": 0}
df["Status_Code"] = df["Score_Grade"].map(status_map)

# Map with a function
df["Score_Doubled"] = df["Score"].map(lambda x: x * 2)
```

### `applymap()` / `map()` — Apply to Every Cell in DataFrame

```python
# (Pandas >= 2.1: use df.map() instead of applymap)
df_numeric = df[["Score", "Salary"]]
df_numeric = df_numeric.map(lambda x: round(x, 2))
```

### `transform()` — Apply and Keep Original Shape

```python
# Normalize scores within each group
df["Score_Normalized"] = df.groupby("Dept")["Score"].transform(
    lambda x: (x - x.mean()) / x.std()
)
```

### `assign()` — Add New Columns Fluently

```python
df = df.assign(
    Tax=df["Salary"] * 0.1,
    Net_Salary=df["Salary"] * 0.9,
    Grade=df["Score"].apply(lambda x: "A" if x >= 90 else "B" if x >= 75 else "C")
)
```

---

## 2. Grouping and Aggregation

**GroupBy** splits data into groups, applies a function, and combines results.

### Basic GroupBy

```python
df = pd.read_csv("employees.csv")

# Group by a single column
grouped = df.groupby("Department")

# Apply aggregation
print(grouped["Salary"].mean())     # Average salary per department
print(grouped["Salary"].sum())      # Total salary per department
print(grouped["Salary"].max())      # Max salary per department
print(grouped["Salary"].count())    # Count of employees per department
```

### Multiple Aggregations with `agg()`

```python
result = df.groupby("Department").agg(
    Total_Salary=("Salary", "sum"),
    Avg_Salary=("Salary", "mean"),
    Max_Salary=("Salary", "max"),
    Min_Salary=("Salary", "min"),
    Employee_Count=("Name", "count")
)
print(result)
```

### Group by Multiple Columns

```python
result = df.groupby(["Department", "City"])["Salary"].mean()
print(result)
```

### `value_counts()` — Frequency Count

```python
print(df["Department"].value_counts())
print(df["City"].value_counts(normalize=True))  # As percentages
```

### Pivot Tables

```python
pivot = df.pivot_table(
    values="Salary",
    index="Department",
    columns="City",
    aggfunc="mean",
    fill_value=0
)
print(pivot)
```

### `crosstab()` — Cross-Tabulation

```python
ct = pd.crosstab(df["Department"], df["City"])
print(ct)
```

---

## 3. Merging, Joining, and Concatenating

### `concat()` — Stack DataFrames

```python
df1 = pd.DataFrame({"Name": ["Arun", "Priya"], "Score": [88, 95]})
df2 = pd.DataFrame({"Name": ["Kumar", "Kavya"], "Score": [72, 80]})

# Vertical stack (add rows)
combined = pd.concat([df1, df2], ignore_index=True)
print(combined)

# Horizontal stack (add columns)
df3 = pd.DataFrame({"Grade": ["B", "A"]})
combined_h = pd.concat([df1, df3], axis=1)
print(combined_h)
```

### `merge()` — Join on Common Columns (like SQL JOIN)

```python
students = pd.DataFrame({
    "student_id": [1, 2, 3, 4],
    "name": ["Arun", "Priya", "Kumar", "Kavya"],
    "dept_id": [101, 102, 101, 103]
})

departments = pd.DataFrame({
    "dept_id": [101, 102, 103],
    "dept_name": ["CS", "IT", "Data Science"]
})

# INNER JOIN (only matching rows)
result = pd.merge(students, departments, on="dept_id")

# LEFT JOIN (all rows from left, matching from right)
result = pd.merge(students, departments, on="dept_id", how="left")

# RIGHT JOIN
result = pd.merge(students, departments, on="dept_id", how="right")

# OUTER JOIN (all rows from both)
result = pd.merge(students, departments, on="dept_id", how="outer")

print(result)
```

### `join()` — Join on Index

```python
df1 = pd.DataFrame({"Score": [88, 95, 72]}, index=["Arun", "Priya", "Kumar"])
df2 = pd.DataFrame({"City": ["Chennai", "Coimbatore", "Madurai"]}, index=["Arun", "Priya", "Kumar"])

joined = df1.join(df2)
print(joined)
```

### Merge vs Join vs Concat

| Method    | Use Case                                      |
| --------- | --------------------------------------------- |
| `concat`  | Stack DataFrames vertically or horizontally   |
| `merge`   | Join on shared columns (like SQL JOIN)        |
| `join`    | Join on index                                 |

---

## 4. Working with Time Series

### Creating Date Ranges

```python
dates = pd.date_range(start="2024-01-01", end="2024-12-31", freq="D")   # daily
dates_m = pd.date_range(start="2024-01-01", periods=12, freq="MS")      # monthly

df = pd.DataFrame({"Date": dates, "Sales": range(len(dates))})
```

### Parsing Dates

```python
df = pd.read_csv("sales.csv", parse_dates=["Date"])
df["Date"] = pd.to_datetime(df["Date"])
```

### Date Accessor (`.dt`)

```python
df["Year"]    = df["Date"].dt.year
df["Month"]   = df["Date"].dt.month
df["Day"]     = df["Date"].dt.day
df["Weekday"] = df["Date"].dt.day_name()
df["Quarter"] = df["Date"].dt.quarter
df["Week"]    = df["Date"].dt.isocalendar().week
```

### Setting Date as Index

```python
df.set_index("Date", inplace=True)

# Slice by date
print(df["2024-06"])                          # June 2024
print(df["2024-01-01":"2024-03-31"])          # Q1 2024
```

### Resampling (Aggregating by Time Period)

```python
# Monthly sum
monthly = df["Sales"].resample("MS").sum()

# Quarterly average
quarterly = df["Sales"].resample("QS").mean()

# Weekly max
weekly = df["Sales"].resample("W").max()

print(monthly)
```

### Rolling Window (Moving Average)

```python
df["7_day_MA"]  = df["Sales"].rolling(window=7).mean()
df["30_day_MA"] = df["Sales"].rolling(window=30).mean()
```

### Shift (Lag)

```python
df["Prev_Day_Sales"] = df["Sales"].shift(1)    # Lag by 1
df["Next_Day_Sales"] = df["Sales"].shift(-1)   # Lead by 1
df["Daily_Change"]   = df["Sales"].diff(1)     # Day-over-day change
```

---

## 5. Input/Output (I/O) Operations

### Reading Files

```python
# CSV
df = pd.read_csv("data.csv")
df = pd.read_csv("data.csv", sep=";", encoding="utf-8", nrows=100)

# Excel
df = pd.read_excel("data.xlsx", sheet_name="Sales")
df = pd.read_excel("data.xlsx", sheet_name=0)  # First sheet

# JSON
df = pd.read_json("data.json")

# Parquet (fast columnar format)
df = pd.read_parquet("data.parquet")

# SQL
import sqlalchemy
engine = sqlalchemy.create_engine("mysql+pymysql://user:pass@localhost/db")
df = pd.read_sql("SELECT * FROM employees", engine)
df = pd.read_sql_query("SELECT * FROM sales WHERE year = 2024", engine)
```

### Writing Files

```python
# CSV
df.to_csv("output.csv", index=False)
df.to_csv("output.csv", sep="|", encoding="utf-8")  # Custom separator

# Excel
df.to_excel("output.xlsx", index=False, sheet_name="Results")

# Multiple sheets
with pd.ExcelWriter("report.xlsx") as writer:
    df1.to_excel(writer, sheet_name="Sales", index=False)
    df2.to_excel(writer, sheet_name="Expenses", index=False)

# JSON
df.to_json("output.json", orient="records", indent=2)

# Parquet
df.to_parquet("output.parquet", index=False)

# SQL
df.to_sql("employees", engine, if_exists="replace", index=False)
```

---

## 🎯 Student Tasks – Module 09: Advanced Data Handling

### Task 1: GroupBy Sales Report (Easy)
**Objective**: Practice groupby and aggregation.

**Instructions**:
1. Create a `sales.csv` with columns: `salesperson`, `region`, `product`, `quantity`, `revenue`.
   Add 20 rows across 4 regions and 3 products.
2. Calculate:
   - Total revenue per region
   - Average quantity per product
   - Top salesperson by total revenue
3. Use `agg()` to get min, max, mean revenue per region in one call.
4. Create a pivot table: rows = region, columns = product, values = revenue (mean).

**Expected Output**:
```
Revenue by Region:
North    ₹125,000
South    ₹148,000
East     ₹98,000
West     ₹112,000

Top Salesperson: Priya (₹52,000)

Pivot Table:
Product    Laptop   Phone   Tablet
Region
North      45000    30000   50000
...
```

---

### Task 2: Merge & Time Series (Medium)
**Objective**: Practice merging DataFrames and time series operations.

**Instructions**:
1. Create two DataFrames:
   - `orders`: `order_id`, `customer_id`, `product`, `amount`, `order_date`
   - `customers`: `customer_id`, `name`, `city`
2. Merge them using LEFT JOIN on `customer_id`.
3. Convert `order_date` to datetime.
4. Extract: year, month, day, weekday name.
5. Resample by month → total monthly revenue.
6. Calculate 3-month rolling average of revenue.

**Expected Output**:
```
Merged DataFrame: (25 rows, 7 columns)

Monthly Revenue:
2024-01   ₹45,000
2024-02   ₹38,000
...

3-Month Rolling Avg:
2024-01       NaN
2024-02       NaN
2024-03   ₹41,000
...
```

---

### Task 3: Full ETL Pipeline (Challenge)
**Objective**: Build a complete Extract-Transform-Load pipeline.

**Instructions**:
1. **Extract**: Read `raw_sales.csv` (create with messy data).
2. **Transform**:
   - Clean and standardize all columns
   - Add calculated columns: `profit`, `profit_margin`, `sales_category`
   - Group by region + product → summary stats
   - Merge with a `products.csv` (containing product details)
   - Create time-based features
3. **Load**: Save:
   - `cleaned_sales.csv` — cleaned data
   - `sales_summary.xlsx` — with 2 sheets (Regional Summary, Monthly Trends)
   - `top_products.json` — top 5 products by revenue

**Expected Output**:
```
=== ETL Pipeline Complete ===
Input: 500 rows
After cleaning: 487 rows
Output files created:
  - cleaned_sales.csv
  - sales_summary.xlsx (2 sheets)
  - top_products.json
```

---
