# Time Series & Exploratory Data Analysis


## Objectives

-   Load and inspect event data
-   Clean and preprocess timestamps
-   Perform Time Series Analysis
-   Perform Exploratory Data Analysis (EDA)
-   Analyze business metrics
-   Visualize trends and distributions
-   Evaluate data quality

## Table of Contents

1.  Data Loading
2.  Data Preprocessing
3.  Time Series Analysis
4.  Exploratory Data Analysis
5.  Business Analytics
6.  Data Quality Checks

------------------------------------------------------------------------

# Program 1: Data Loading

## Objective

Import required libraries.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Plot style
sns.set_style("whitegrid")
plt.rcParams["figure.figsize"] = (14,6)
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 2: Data Loading

## Objective

Load dataset into a DataFrame.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
df = pd.read_csv("jeevi_events.csv", low_memory=False)
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 3: Data Preprocessing

## Objective

Convert timestamps into datetime objects.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
# Convert to datetime
df["timestamp"] = pd.to_datetime(df["timestamp"], utc=True)

# Convert UTC to IST
df["timestamp"] = df["timestamp"].dt.tz_convert("Asia/Kolkata")
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 4: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
print(df["timestamp"].min())
print(df["timestamp"].max())
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 5: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
df["date"] = df["timestamp"].dt.date
df["month"] = df["timestamp"].dt.to_period("M").astype(str)
df["hour"] = df["timestamp"].dt.hour
df["day_name"] = df["timestamp"].dt.day_name()
df["day_number"] = df["timestamp"].dt.dayofweek
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 6: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
#Daily Event Trends
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 7: EDA

## Objective

Aggregate data for analysis.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
daily = df.groupby("date").size()

plt.figure(figsize=(15,6))

plt.plot(daily.index, daily.values,
         marker='o',
         linewidth=2)

plt.title("Daily Events Trend")
plt.xlabel("Date")
plt.ylabel("Number of Events")
plt.xticks(rotation=45)

plt.tight_layout()
plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 8: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
#Montly Event tredns
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 9: EDA

## Objective

Aggregate data for analysis.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
monthly = df.groupby("month").size()

plt.figure(figsize=(10,5))

sns.barplot(
    x=monthly.index,
    y=monthly.values,
    palette="viridis"
)

plt.title("Monthly Events")
plt.xlabel("Month")
plt.ylabel("Events")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 10: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
# Seven day
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 11: Time Series

## Objective

Calculate moving averages.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
daily = df.groupby("date").size()

rolling = daily.rolling(7).mean()

plt.figure(figsize=(15,6))

plt.plot(daily.index,
         daily.values,
         label="Daily Events")

plt.plot(rolling.index,
         rolling.values,
         linewidth=3,
         label="7-Day Rolling Average")

plt.legend()

plt.title("Daily Events with 7-Day Rolling Average")
plt.xlabel("Date")
plt.ylabel("Events")

plt.xticks(rotation=45)

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 12: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
#Events by hour
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 13: EDA

## Objective

Aggregate data for analysis.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
hourly = df.groupby("hour").size()

plt.figure(figsize=(12,5))

sns.barplot(
    x=hourly.index,
    y=hourly.values,
    palette="crest"
)

plt.title("Events by Hour (IST)")
plt.xlabel("Hour")
plt.ylabel("Events")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 14: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
#5. Events by Day of Week
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 15: EDA

## Objective

Aggregate data for analysis.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
order = [
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday",
    "Sunday"
]

weekday = (
    df.groupby("day_name")
      .size()
      .reindex(order)
)

plt.figure(figsize=(10,5))

sns.barplot(
    x=weekday.index,
    y=weekday.values,
    palette="mako"
)

plt.title("Events by Day of Week")

plt.xlabel("Day")
plt.ylabel("Events")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 16: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
## Heatmap (Day × Hour)
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 17: Visualization

## Objective

Visualize relationships using a heatmap.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
heat = pd.crosstab(
    df["day_name"],
    df["hour"]
)

heat = heat.reindex(order)

plt.figure(figsize=(14,5))

sns.heatmap(
    heat,
    cmap="YlGnBu",
    annot=True,
    fmt="d"
)

plt.title("Events Heatmap (Day vs Hour)")
plt.xlabel("Hour")
plt.ylabel("Day")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 18: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
# status distribution
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 19: EDA

## Objective

Summarize categorical distributions.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
status = df["status"].value_counts()

plt.figure(figsize=(8,5))

sns.barplot(
    x=status.index,
    y=status.values,
    palette="Set2"
)

plt.title("Status Distribution")

plt.xlabel("Status")
plt.ylabel("Count")

plt.xticks(rotation=30)

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 20: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
##status overtime
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 21: EDA

## Objective

Aggregate data for analysis.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
status_time = (
    df.groupby(
        [
            "date",
            "status"
        ]
    )
    .size()
    .unstack(fill_value=0)
)

status_time.plot(
    figsize=(15,6),
    linewidth=2
)

plt.title("Status Trend Over Time")

plt.xlabel("Date")
plt.ylabel("Events")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 22: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
# cumulative
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 23: EDA

## Objective

Aggregate data for analysis.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
daily = df.groupby("date").size()

cumulative = daily.cumsum()

plt.figure(figsize=(15,6))

plt.plot(
    cumulative.index,
    cumulative.values,
    linewidth=3
)

plt.title("Cumulative Events")

plt.xlabel("Date")
plt.ylabel("Total Events")

plt.xticks(rotation=45)

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 24: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
#Boxplot (Daily event by month)
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 25: Data Preprocessing

## Objective

Convert timestamps into datetime objects.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
daily_df = (
    df.groupby("date")
      .size()
      .reset_index(name="events")
)

daily_df["date"] = pd.to_datetime(daily_df["date"])
daily_df["month"] = daily_df["date"].dt.strftime("%b")

month_order = ["Dec", "Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul"]

plt.figure(figsize=(10,6))

sns.boxplot(
    x="month",
    y="events",
    data=daily_df,
    order=month_order,
    palette="Pastel1"
)

plt.title("Distribution of Daily Events by Month")
plt.xlabel("Month")
plt.ylabel("Daily Events")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 26: EDA

## Objective

Aggregate data for analysis.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
monthly = df.groupby("month").size().reset_index(name="events")

plt.figure(figsize=(10,5))

sns.lineplot(
    data=monthly,
    x="month",
    y="events",
    marker="o",
    linewidth=2.5
)

plt.title("Monthly Events Trend")
plt.xlabel("Month")
plt.ylabel("Events")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 27: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
##################################################
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 28: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
##################################################
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 29: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 30: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
for col in df.columns:
    print(col)
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 31: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
# Status distribution
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 32: EDA

## Objective

Summarize categorical distributions.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
status = df["status"].value_counts()

plt.figure(figsize=(8,5))

sns.barplot(
    x=status.index,
    y=status.values,
    hue=status.index,
    legend=False,
    palette="viridis"
)

plt.title("Status Distribution")
plt.xlabel("Status")
plt.ylabel("Count")

for i, v in enumerate(status.values):
    plt.text(i, v+5, str(v), ha='center')

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 33: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
#qualified distribution
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 34: EDA

## Objective

Summarize categorical distributions.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
qualified = df["qualified_by"].fillna("Unknown").value_counts()

plt.figure(figsize=(10,5))

sns.barplot(
    x=qualified.index,
    y=qualified.values,
    hue=qualified.index,
    legend=False,
    palette="Set2"
)

plt.xticks(rotation=45)

plt.title("Qualified By Distribution")
plt.ylabel("Count")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 35: EDA

## Objective

Summarize categorical distributions.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
events = df["name"].value_counts().head(15)

plt.figure(figsize=(10,6))

sns.barplot(
    y=events.index,
    x=events.values,
    hue=events.index,
    legend=False,
    palette="crest"
)

plt.title("Top Event Types")
plt.xlabel("Count")
plt.ylabel("Event")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 36: EDA

## Objective

Aggregate data for analysis.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
visitor_events = df.groupby("cid").size()

plt.figure(figsize=(10,4))

sns.histplot(
    visitor_events,
    bins=1000,
    kde=True
)

plt.title("Events Per Visitor")
plt.xlabel("Number of Events")
plt.ylabel("Visitors")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 37: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
#Active visitors
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 38: EDA

## Objective

Summarize categorical distributions.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
top = df["cid"].value_counts().head(20)

plt.figure(figsize=(12,7))

sns.barplot(
    x=top.values,
    y=top.index,
    hue=top.index,
    legend=False,
    palette="rocket"
)

plt.title("Top 20 Active Visitors")
plt.xlabel("Events")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 39: EDA

## Objective

Summarize categorical distributions.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
pro = df["is_professional"].value_counts()
print(pro)
plt.figure(figsize=(6,5))

plt.pie(
    pro.values,
    labels=pro.index,
    autopct="%1.1f%%",
    startangle=80
)

plt.title("Professional Users")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 40: EDA

## Objective

Summarize categorical distributions.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
roles = df["role"].fillna("Unknown").value_counts().head(15)
print(roles)

plt.figure(figsize=(12,6))

sns.barplot(
    x=roles.values,
    y=roles.index,
    hue=roles.index,
    legend=False,
    palette="flare"
)

plt.title("Top Roles")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 41: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 42: EDA

## Objective

Summarize categorical distributions.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
exp = df["experience"].fillna("Unknown").value_counts()
print(exp)

plt.figure(figsize=(10,5))

sns.barplot(
    x=exp.index,
    y=exp.values,
    hue=exp.index,
    legend=False,
    palette="magma"
)

plt.xticks(rotation=45)

plt.title("Experience Distribution")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 43: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
plt.figure(figsize=(10,5))

sns.histplot(
    df["qualification_score"].dropna(),
    bins=30,
    kde=True
)

plt.title("Qualification Score Distribution")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 44: EDA

## Objective

Identify spread and outliers.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
plt.figure(figsize=(8,5))

sns.boxplot(
    x=df["qualification_score"]
)

plt.title("Qualification Score Boxplot")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 45: EDA

## Objective

Summarize categorical distributions.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
wa = df["wa_verified"].fillna(False).value_counts()
print(wa)

plt.figure(figsize=(6,5))

plt.pie(
    wa.values,
    labels=wa.index,
    autopct="%1.1f%%"
)

plt.title("WhatsApp Verification")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 46: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
ads = {
    "gclid": df["gclid"].notna().sum(),
    "gbraid": df["gbraid"].notna().sum(),
    "wbraid": df["wbraid"].notna().sum()
}

plt.figure(figsize=(8,5))

sns.barplot(
    x=list(ads.keys()),
    y=list(ads.values())
)

plt.title("Google Ads Identifier Presence")

plt.ylabel("Count")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 47: EDA

## Objective

Summarize categorical distributions.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
upload = df["gads_uploaded_at"].notna().value_counts()
print(upload)
plt.figure(figsize=(6,5))

plt.pie(
    upload.values,
    labels=["Not Uploaded","Uploaded"],
    autopct="%1.1f%%"
)

plt.title("Google Ads Upload Status")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 48: EDA

## Objective

Summarize categorical distributions.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
errors = df["gads_error"].fillna("No Error").value_counts().head(10)

print(errors)

plt.figure(figsize=(10,6))

sns.barplot(
    x=errors.values,
    y=errors.index,
    hue=errors.index,
    legend=False
)

plt.title("Top Google Ads Errors")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 49: Visualization

## Objective

Visualize relationships using a heatmap.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
plt.figure(figsize=(15,6))
sns.heatmap(
    df.isnull(),
    cbar=False,
    cmap="viridis"
)

plt.title("Missing Values Heatmap")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 50: Data Quality

## Objective

Analyze missing values.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
missing = df.isnull().mean()*100
missing = missing.sort_values(ascending=False)
print(missing)
plt.figure(figsize=(12,6))

sns.barplot(
    x=missing.values,
    y=missing.index
)

plt.title("Missing Values (%)")

plt.xlabel("Percentage")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 51: Visualization

## Objective

Visualize relationships using a heatmap.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
plt.figure(figsize=(10,8))

sns.heatmap(
    df.corr(numeric_only=True),
    annot=True,
    cmap="coolwarm"
)

plt.title("Correlation Heatmap")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 52: Visualization

## Objective

Visualize relationships using a heatmap.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
cross = pd.crosstab(
    df["status"],
    df["qualified_by"]
)

plt.figure(figsize=(10,6))

sns.heatmap(
    cross,
    annot=True,
    fmt="d",
    cmap="Blues"
)

plt.title("Status vs Qualified By")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 53: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
cross = pd.crosstab(
    df["status"],
    df["is_professional"]
)

cross.plot(
    kind="bar",
    figsize=(10,5)
)

plt.title("Professional Users by Status")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 54: Visualization

## Objective

Visualize relationships using a heatmap.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
role_exp = pd.crosstab(
    df["role"],
    df["experience"]
)

plt.figure(figsize=(14,8))

sns.heatmap(
    role_exp,
    cmap="YlGnBu"
)

plt.title("Role vs Experience")

plt.show()
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 55: Data Quality

## Objective

Identify duplicate records.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
duplicates = df["cid"].duplicated().sum()

print("Duplicate Visitors:", duplicates)

print("Unique Visitors:", df["cid"].nunique())

print("Total Events:", len(df))
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 56: General Analysis

## Objective

Perform a data analysis task.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
print("Unique gclid :", df["gclid"].nunique())
print("Unique gbraid:", df["gbraid"].nunique())
print("Unique wbraid:", df["wbraid"].nunique())
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

------------------------------------------------------------------------

# Program 57: Data Quality

## Objective

Generate descriptive statistics.

## Explanation

This section executes the above objective using
Pandas/Matplotlib/Seaborn. Review the code and generated output to
understand the result.

## Python Code

``` python
df.describe(include="all").T
```

## Expected Output

-   Executes successfully.
-   Produces tables or visualizations depending on the operation.

## Learning Points

-   Understand the APIs used.
-   Interpret the output before moving to the next analysis.

# Conclusion

This notebook contains **57 code programs** covering data loading,
preprocessing, time series analysis, EDA, visualization, business
analytics, and data quality assessment.
