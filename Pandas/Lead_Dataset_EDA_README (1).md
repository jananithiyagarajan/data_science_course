# Lead Dataset EDA - Practice


## Task 1

### Objective
Explain and execute the following code.

### Code
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

### Explanation

- This task executes: `import pandas as pd`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 2

### Objective
Explain and execute the following code.

### Code
```python
df = pd.read_csv("clicks_export.csv", encoding="latin1")
```

### Explanation

- This task executes: `df = pd.read_csv("clicks_export.csv", encoding="latin1")`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 3

### Objective
Explain and execute the following code.

### Code
```python
df.head()
```

### Explanation

- This task executes: `df.head()`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 4

### Objective
Explain and execute the following code.

### Code
```python
print(df.tail())
```

### Explanation

- This task executes: `print(df.tail())`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 5

### Objective
Explain and execute the following code.

### Code
```python
print(df.sample(5))
```

### Explanation

- This task executes: `print(df.sample(5))`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 6

### Objective
Explain and execute the following code.

### Code
```python
print(df.shape)
```

### Explanation

- This task executes: `print(df.shape)`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 7

### Objective
Explain and execute the following code.

### Code
```python
print(df.columns)
```

### Explanation

- This task executes: `print(df.columns)`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 8

### Objective
Explain and execute the following code.

### Code
```python
print(df.info())
```

### Explanation

- This task executes: `print(df.info())`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 9

### Objective
Explain and execute the following code.

### Code
```python
df.describe(include='all')
```

### Explanation

- This task executes: `df.describe(include='all')`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 10

### Objective
Explain and execute the following code.

### Code
```python
print(df.isna().sum())
```

### Explanation

- This task executes: `print(df.isna().sum())`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 11

### Objective
Explain and execute the following code.

### Code
```python
print((df.isna().sum()/len(df))*100)
```

### Explanation

- This task executes: `print((df.isna().sum()/len(df))*100)`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 12

### Objective
Explain and execute the following code.

### Code
```python
print(df.duplicated().sum())
```

### Explanation

- This task executes: `print(df.duplicated().sum())`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 13

### Objective
Explain and execute the following code.

### Code
```python
for col in df.columns:

    print("="*50)

    print(col)

    print(df[col].unique())
```

### Explanation

- This task executes: `for col in df.columns:`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 14

### Objective
Explain and execute the following code.

### Code
```python
for col in df.columns:

    print("="*50)

    print(col)

    print(df[col].value_counts(dropna=False))
```

### Explanation

- This task executes: `for col in df.columns:`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 15

### Objective
Explain and execute the following code.

### Code
```python
numeric = df.select_dtypes(include="number")

print(numeric.head())
```

### Explanation

- This task executes: `numeric = df.select_dtypes(include="number")`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 16

### Objective
Explain and execute the following code.

### Code
```python
numeric.hist(figsize=(15,10))

plt.tight_layout()

plt.show()
```

### Explanation

- This task executes: `numeric.hist(figsize=(15,10))`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 17

### Objective
Explain and execute the following code.

### Code
```python
numeric.boxplot(figsize=(15,6))

plt.xticks(rotation=45)

plt.show()
```

### Explanation

- This task executes: `numeric.boxplot(figsize=(15,6))`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 18

### Objective
Explain and execute the following code.

### Code
```python
corr = numeric.corr()

print(corr)
```

### Explanation

- This task executes: `corr = numeric.corr()`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 19

### Objective
Explain and execute the following code.

### Code
```python
print(df["utm_source"].value_counts(dropna=False))
```

### Explanation

- This task executes: `print(df["utm_source"].value_counts(dropna=False))`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 20

### Objective
Explain and execute the following code.

### Code
```python
df["utm_source"].value_counts().plot(
    kind="bar",
    figsize=(8,5),
    title="Visitors by Source"
)

plt.ylabel("Visitors")

plt.show()
```

### Explanation

- This task executes: `df["utm_source"].value_counts().plot(`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 21

### Objective
Explain and execute the following code.

### Code
```python
print(df["utm_campaign"].value_counts(dropna=False))
```

### Explanation

- This task executes: `print(df["utm_campaign"].value_counts(dropna=False))`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 22

### Objective
Explain and execute the following code.

### Code
```python
df["utm_campaign"].value_counts().plot(
    kind="bar",
    figsize=(10,5)
)

plt.xticks(rotation=90)

plt.show()
```

### Explanation

- This task executes: `df["utm_campaign"].value_counts().plot(`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 23

### Objective
Explain and execute the following code.

### Code
```python
print(df["utm_medium"].value_counts(dropna=False))
```

### Explanation

- This task executes: `print(df["utm_medium"].value_counts(dropna=False))`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 24

### Objective
Explain and execute the following code.

### Code
```python
print(df["referrer"].value_counts(dropna=False))
```

### Explanation

- This task executes: `print(df["referrer"].value_counts(dropna=False))`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 25

### Objective
Explain and execute the following code.

### Code
```python
print(df["referrer"].value_counts(dropna=False))
```

### Explanation

- This task executes: `print(df["referrer"].value_counts(dropna=False))`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 26

### Objective
Explain and execute the following code.

### Code
```python
print(df["landing_path"].value_counts())
```

### Explanation

- This task executes: `print(df["landing_path"].value_counts())`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 27

### Objective
Explain and execute the following code.

### Code
```python
lead_score = df.groupby("utm_source")["max_lead_score"].mean()

print(lead_score)
```

### Explanation

- This task executes: `lead_score = df.groupby("utm_source")["max_lead_score"].mean()`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 28

### Objective
Explain and execute the following code.

### Code
```python
print(df.nlargest(10,"max_lead_score"))
```

### Explanation

- This task executes: `print(df.nlargest(10,"max_lead_score"))`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 29

### Objective
Explain and execute the following code.

### Code
```python
engagement = df.groupby("utm_source")["max_engaged_seconds"].mean()

print(engagement)
```

### Explanation

- This task executes: `engagement = df.groupby("utm_source")["max_engaged_seconds"].mean()`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 30

### Objective
Explain and execute the following code.

### Code
```python
scroll = df.groupby("utm_source")["max_scroll_percent"].mean()

print(scroll)
```

### Explanation

- This task executes: `scroll = df.groupby("utm_source")["max_scroll_percent"].mean()`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 31

### Objective
Explain and execute the following code.

### Code
```python
click = df.groupby("utm_source")["has_clicked_syllabus"].sum()

print(click)
```

### Explanation

- This task executes: `click = df.groupby("utm_source")["has_clicked_syllabus"].sum()`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 32

### Objective
Explain and execute the following code.

### Code
```python
df["timestamp"] = pd.to_datetime(df["timestamp"])
```

### Explanation

- This task executes: `df["timestamp"] = pd.to_datetime(df["timestamp"])`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 33

### Objective
Explain and execute the following code.

### Code
```python
daily = df.groupby(df["timestamp"].dt.date).size()

print(daily)

daily.plot(figsize=(12,5))

plt.title("Daily Visitors")

plt.show()
```

### Explanation

- This task executes: `daily = df.groupby(df["timestamp"].dt.date).size()`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 34

### Objective
Explain and execute the following code.

### Code
```python
hourly = df.groupby(df["timestamp"].dt.hour).size()

hourly.plot(kind="bar")

plt.title("Visitors by Hour")

plt.show()
```

### Explanation

- This task executes: `hourly = df.groupby(df["timestamp"].dt.hour).size()`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 35

### Objective
Explain and execute the following code.

### Code
```python
landing = df["landing_path"].value_counts()

print(landing)

landing.head(10).plot(kind="bar")

plt.show()
```

### Explanation

- This task executes: `landing = df["landing_path"].value_counts()`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 36

### Objective
Explain and execute the following code.

### Code
```python
ref = df["referrer"].value_counts()

print(ref)

ref.head(10).plot(kind="bar")

plt.show()
```

### Explanation

- This task executes: `ref = df["referrer"].value_counts()`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 37

### Objective
Explain and execute the following code.

### Code
```python
high = df[df["max_lead_score"]>70]

print(high)
```

### Explanation

- This task executes: `high = df[df["max_lead_score"]>70]`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 38

### Objective
Explain and execute the following code.

### Code
```python
engaged = df[df["max_engaged_seconds"]>60]

print(engaged)
```

### Explanation

- This task executes: `engaged = df[df["max_engaged_seconds"]>60]`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 39

### Objective
Explain and execute the following code.

### Code
```python
scroll100 = df[df["max_scroll_percent"]==100]

print(scroll100)
```

### Explanation

- This task executes: `scroll100 = df[df["max_scroll_percent"]==100]`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 40

### Objective
Explain and execute the following code.

### Code
```python
clicked = df[df["has_clicked_syllabus"]==1]

print(clicked)
```

### Explanation

- This task executes: `clicked = df[df["has_clicked_syllabus"]==1]`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 41

### Objective
Explain and execute the following code.

### Code
```python
print("="*50)

print("Top Traffic Sources")

print(df["utm_source"].value_counts())

print("="*50)

print("Highest Lead Score Source")

print(
    df.groupby("utm_source")["max_lead_score"]
      .mean()
      .sort_values(ascending=False)
)

print("="*50)

print("Highest Engagement")

print(
    df.groupby("utm_source")["max_engaged_seconds"]
      .mean()
      .sort_values(ascending=False)
)

print("="*50)

print("Highest Scroll")

print(
    df.groupby("utm_source")["max_scroll_percent"]
      .mean()
      .sort_values(ascending=False)
)

print("="*50)

print("Most Syllabus Clicks")

print(
    df.groupby("utm_source")["has_clicked_syllabus"]
      .sum()
      .sort_values(ascending=False)
)
```

### Explanation

- This task executes: `print("="*50)`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.


## Task 42

### Objective
Explain and execute the following code.

### Code
```python
!jupyter nbconvert --to pdf Lead_Dataset_EDA.ipynb
```

### Explanation

- This task executes: `!jupyter nbconvert --to pdf Lead_Dataset_EDA.ipynb`.

- Review the output and understand how it contributes to Exploratory Data Analysis (EDA).

### Interpretation
- Analyze the generated output, table, or visualization before moving to the next task.
