# Data Science Introduction

## What is Data Science?

**Data Science** is an interdisciplinary field that uses scientific methods, processes, algorithms, and systems to extract knowledge and insights from structured and unstructured data.

A Data Scientist combines skills from:
- **Statistics** — analyze and interpret data
- **Computer Science** — build systems and algorithms
- **Domain Knowledge** — understand the business context

---

## The Data Science Lifecycle

```
1. Define Problem
      ↓
2. Collect Data
      ↓
3. Clean & Prepare Data
      ↓
4. Explore Data (EDA)
      ↓
5. Build Models
      ↓
6. Evaluate & Interpret
      ↓
7. Deploy & Monitor
```

---

## 1. Data Wrangling

**Data Wrangling** (also called data munging) is the process of transforming raw, messy data into a clean, usable format.

### Common Wrangling Tasks

| Task                     | What It Means                                   |
| ------------------------ | ----------------------------------------------- |
| Handling Missing Values  | Fill or drop NaN/null entries                   |
| Removing Duplicates      | Keep unique records only                        |
| Fixing Data Types        | Convert strings to numbers, dates, etc.         |
| Standardizing Formats    | Make "CHENNAI", "chennai", "Chennai" consistent |
| Outlier Detection        | Find extreme values that skew analysis          |
| Feature Engineering      | Create new useful columns from existing ones    |

```python
import pandas as pd
import numpy as np

df = pd.read_csv("raw_data.csv")

# Check issues
print(df.isnull().sum())
print(df.duplicated().sum())
print(df.dtypes)

# Fix
df.drop_duplicates(inplace=True)
df["Age"].fillna(df["Age"].median(), inplace=True)
df["Salary"] = pd.to_numeric(df["Salary"], errors="coerce")
df["City"] = df["City"].str.strip().str.title()
df.reset_index(drop=True, inplace=True)
```

---

## 2. Exploratory Data Analysis (EDA)

**EDA** is the process of analyzing and visualizing data to discover patterns, spot anomalies, test hypotheses, and check assumptions.

### Statistical EDA

```python
import pandas as pd
import numpy as np

df = pd.read_csv("data.csv")

# Shape and columns
print(df.shape)
print(df.columns.tolist())

# Summary statistics
print(df.describe())

# Value counts for categorical
print(df["Department"].value_counts())

# Correlation matrix
print(df.corr(numeric_only=True))

# Null analysis
print((df.isnull().sum() / len(df) * 100).sort_values(ascending=False))
```

### Visualization with Matplotlib

```python
import matplotlib.pyplot as plt

# Histogram — distribution
df["Age"].hist(bins=20, color="steelblue", edgecolor="white")
plt.title("Age Distribution")
plt.xlabel("Age")
plt.ylabel("Frequency")
plt.show()

# Bar chart — categories
dept_count = df["Department"].value_counts()
dept_count.plot(kind="bar", color="coral")
plt.title("Employees per Department")
plt.xticks(rotation=45)
plt.show()

# Scatter plot — relationship
plt.scatter(df["Experience"], df["Salary"], alpha=0.5)
plt.xlabel("Experience (years)")
plt.ylabel("Salary")
plt.title("Experience vs Salary")
plt.show()

# Box plot — outliers
df.boxplot(column="Salary", by="Department")
plt.title("Salary by Department")
plt.show()
```

### Visualization with Seaborn

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Distribution plot
sns.histplot(df["Salary"], kde=True, color="purple")
plt.title("Salary Distribution with KDE")
plt.show()

# Heatmap — correlations
sns.heatmap(df.corr(numeric_only=True), annot=True, cmap="coolwarm", fmt=".2f")
plt.title("Correlation Heatmap")
plt.show()

# Pairplot — multiple relationships
sns.pairplot(df[["Age", "Salary", "Experience"]])
plt.show()

# Boxplot by category
sns.boxplot(x="Department", y="Salary", data=df, palette="Set2")
plt.xticks(rotation=30)
plt.show()

# Count plot
sns.countplot(x="City", data=df, order=df["City"].value_counts().index[:5])
plt.show()
```

---

## 3. Pandas for Data Science

Pandas is the backbone of data manipulation in data science workflows.

### Key Pandas Operations for Data Science

```python
import pandas as pd

# Load data
df = pd.read_csv("sales.csv", parse_dates=["date"])

# Quick overview
df.info()
df.describe()
df.head()

# GroupBy analysis
dept_stats = df.groupby("department").agg(
    total_sales=("sales", "sum"),
    avg_sales=("sales", "mean"),
    count=("sales", "count")
).reset_index()

# Pivot table
pivot = df.pivot_table(values="sales", index="region", columns="product", aggfunc="sum")

# Rolling average (time series)
df.set_index("date", inplace=True)
df["7_day_MA"] = df["sales"].rolling(7).mean()
```

---

## 4. Feature Engineering

**Feature Engineering** is the process of using domain knowledge to create, transform, or select features (columns) that improve model performance.

### Types of Feature Engineering

#### 4.1 Creating New Features
```python
# Combine columns
df["full_name"] = df["first_name"] + " " + df["last_name"]

# Date features
df["year"] = df["order_date"].dt.year
df["month"] = df["order_date"].dt.month
df["day_of_week"] = df["order_date"].dt.dayofweek
df["is_weekend"] = df["day_of_week"].isin([5, 6]).astype(int)

# Ratio feature
df["revenue_per_unit"] = df["revenue"] / df["quantity"]

# Binning (Age groups)
df["age_group"] = pd.cut(df["age"],
                          bins=[0, 18, 35, 60, 100],
                          labels=["Teen", "Young", "Mid", "Senior"])
```

#### 4.2 Encoding Categorical Variables
```python
# Label Encoding (for ordinal data)
from sklearn.preprocessing import LabelEncoder
le = LabelEncoder()
df["grade_encoded"] = le.fit_transform(df["grade"])

# One-Hot Encoding (for nominal data)
df = pd.get_dummies(df, columns=["city", "department"])

# Manual mapping
df["gender"] = df["gender"].map({"Male": 1, "Female": 0})
```

#### 4.3 Feature Scaling
```python
from sklearn.preprocessing import StandardScaler, MinMaxScaler

# StandardScaler: mean=0, std=1 (for algorithms like SVM, LogReg)
scaler = StandardScaler()
df[["age", "salary"]] = scaler.fit_transform(df[["age", "salary"]])

# MinMaxScaler: scale to [0, 1] (for neural networks)
minmax = MinMaxScaler()
df[["score"]] = minmax.fit_transform(df[["score"]])
```

---

## 5. Databases & SQL in Data Science

Data scientists regularly query databases to extract data for analysis.

```python
import pandas as pd
import sqlalchemy

# Connect to database
engine = sqlalchemy.create_engine("mysql+pymysql://user:pass@localhost/mydb")

# Query to DataFrame
query = """
SELECT department, AVG(salary) as avg_salary, COUNT(*) as count
FROM employees
WHERE join_date >= '2022-01-01'
GROUP BY department
HAVING COUNT(*) > 3
ORDER BY avg_salary DESC;
"""

df = pd.read_sql(query, engine)
print(df)

# Write DataFrame back to SQL
df.to_sql("dept_summary", engine, if_exists="replace", index=False)
```

---

## 🎯 Student Tasks – Module 17: Data Science Introduction

### Task 1: End-to-End Data Wrangling (Easy)
**Objective**: Load, clean, and explore a messy dataset.

**Instructions**:
1. Create or download a CSV with intentional issues (missing values, duplicates, wrong types).
2. Perform:
   - Load and display basic info
   - Count missing values per column
   - Fill numeric nulls with mean, categorical with mode
   - Remove duplicates
   - Fix data types
   - Standardize string columns
3. Print a before/after report.

**Expected Output**:
```
=== Before Cleaning ===
Shape: (120, 7) | Missing: 18 | Duplicates: 5

=== After Cleaning ===
Shape: (115, 7) | Missing: 0 | Duplicates: 0
Data types: all correct
```

---

### Task 2: Full EDA with Visualization (Medium)
**Objective**: Perform EDA and create 5 different visualizations.

**Instructions**:
Using any dataset (employees, sales, or student data):
1. Print shape, dtypes, describe(), value_counts for 2 columns.
2. Create:
   - Histogram of a numeric column
   - Bar chart of a categorical column
   - Scatter plot showing correlation between 2 columns
   - Box plot for detecting outliers
   - Correlation heatmap using Seaborn
3. Write 5 observations from your EDA.

**Expected Output**:
```
5 charts displayed.

Observations:
1. Salary is positively skewed (median < mean).
2. IT department has the highest average salary.
3. Strong positive correlation (r=0.78) between Experience and Salary.
4. 3 outliers detected in Salary column (> Q3 + 1.5*IQR).
5. Most employees are from Chennai (32%).
```

---

### Task 3: Feature Engineering Pipeline (Challenge)
**Objective**: Build a complete feature engineering pipeline.

**Instructions**:
Using an employee dataset with: `name`, `dob`, `salary`, `department`, `city`, `join_date`:

1. Create features:
   - `age` from `dob`
   - `experience_years` from `join_date`
   - `age_group` (Teen/Young/Mid/Senior)
   - `salary_band` (Low/Medium/High/Very High)
   - `is_senior` (True if experience > 5 years)
2. Encode: `department` (one-hot), `city` (label encode), `age_group` (ordinal encode).
3. Scale: `salary` and `age` using StandardScaler.
4. Print the final feature-engineered DataFrame.
5. Save to `features.csv`.

**Expected Output**:
```
Original columns: 6
After feature engineering: 18 columns

New features: age, experience_years, age_group, salary_band, is_senior
Encoded: department (4 dummy cols), city (label 0-5), age_group (0-3)
Scaled: salary_scaled, age_scaled

Final shape: (100, 18)
Saved to features.csv
```

---
