# Event Time Series Analysis

## Overview

This notebook performs exploratory data analysis (EDA) and time-series
analysis on event data using Pandas, Matplotlib, and Seaborn.

## Workflow

## 1. Import Required Libraries

Import pandas for data analysis, matplotlib and seaborn for
visualization. Configure plot style for consistent charts.

``` python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Plot style
sns.set_style("whitegrid")
plt.rcParams["figure.figsize"] = (14,6)
```

## 2. Load Dataset

Read the CSV file into a pandas DataFrame.

``` python
df = pd.read_csv("jeevi_events.csv", low_memory=False)
```

## 3. Convert Date & Time

Convert timestamps to datetime, change UTC to IST, and verify the date
range.

``` python
# Convert to datetime
df["timestamp"] = pd.to_datetime(df["timestamp"], utc=True)

# Convert UTC to IST
df["timestamp"] = df["timestamp"].dt.tz_convert("Asia/Kolkata")
```

## 4. Feature Engineering

Create additional columns such as date, month, hour, and day name for
time-series analysis.

``` python
print(df["timestamp"].min())
print(df["timestamp"].max())
```

``` python
df["date"] = df["timestamp"].dt.date
df["month"] = df["timestamp"].dt.to_period("M").astype(str)
df["hour"] = df["timestamp"].dt.hour
df["day_name"] = df["timestamp"].dt.day_name()
df["day_number"] = df["timestamp"].dt.dayofweek
```

## 5. Daily Event Trend

Analyze the number of events occurring each day using a line chart.

``` python
#Daily Event Trends
```

## 6. Monthly Event Trend

Summarize events by month using a bar chart.

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

## 7. Seven-Day Moving Average

Smooth daily fluctuations with a 7-day rolling average.

``` python
#Montly Event tredns
```

## 8. Hourly Analysis

Identify peak activity hours.

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

## 9. Day of Week Analysis

Compare event counts across weekdays.

``` python
# Seven day
```

## 10. Day-Hour Heatmap

Visualize activity intensity by day and hour.

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

## 11. Status Analysis

Analyze event status distribution and status over time.

``` python
#Events by hour
```

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

## 12. Cumulative Growth

Show cumulative event growth over time.

``` python
#5. Events by Day of Week
```

## 13. Monthly Distribution

Use boxplots and line charts to compare monthly event volumes.

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

## 14. Business Insights

Analyze qualification, roles, visitor behavior, professional users,
advertising IDs, and Google Ads uploads.

``` python
## Heatmap (Day × Hour)
```

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

``` python
# status distribution
```

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

## 15. Data Quality

Check missing values, duplicates, descriptive statistics, and
correlations.

``` python
##status overtime
```

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

## Conclusion

This notebook provides a complete event analytics workflow including
preprocessing, time-series analysis, visitor analysis, qualification
insights, advertising metrics, and data quality assessment.
