# Data Visualization with Matplotlib & Seaborn

## Why Data Visualization?

> "A picture is worth a thousand rows."

Data visualization helps you:
- **Understand distributions** — is data skewed? Are there outliers?
- **Spot correlations** — which features are related?
- **Communicate insights** — tell the story behind the numbers
- **Identify data quality issues** — missing patterns, anomalies

The two most popular Python visualization libraries:

| Library        | Style               | Best For                              |
| -------------- | ------------------- | ------------------------------------- |
| **Matplotlib** | Low-level, flexible | Custom plots, full control            |
| **Seaborn**    | High-level, statistical | Statistical plots, beautiful defaults |
| **Plotly**     | Interactive         | Dashboards, web apps                  |

---

## 1. Matplotlib Basics

```python
import matplotlib.pyplot as plt
import numpy as np

# ── Figure and Axes ───────────────────────────────────────────────────────────
fig, ax = plt.subplots(figsize=(10, 6))

x = np.linspace(0, 2*np.pi, 100)

ax.plot(x, np.sin(x), label='sin(x)', color='blue', linewidth=2)
ax.plot(x, np.cos(x), label='cos(x)', color='red', linestyle='--', linewidth=2)

ax.set_title('Sine and Cosine Waves', fontsize=16, fontweight='bold')
ax.set_xlabel('x (radians)', fontsize=13)
ax.set_ylabel('y', fontsize=13)
ax.legend(fontsize=12)
ax.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

### Subplots Grid

```python
import matplotlib.pyplot as plt
import numpy as np

fig, axes = plt.subplots(2, 2, figsize=(14, 10))
fig.suptitle('Matplotlib Plot Gallery', fontsize=16, fontweight='bold')

x = np.linspace(0, 10, 100)
y = np.random.randn(200)

# Line Plot
axes[0,0].plot(x, np.sin(x), color='steelblue', linewidth=2)
axes[0,0].set_title('Line Plot — Sine Wave')
axes[0,0].set_xlabel('x')
axes[0,0].set_ylabel('sin(x)')
axes[0,0].grid(alpha=0.3)

# Histogram
axes[0,1].hist(y, bins=30, color='salmon', edgecolor='white', alpha=0.8)
axes[0,1].set_title('Histogram — Normal Distribution')
axes[0,1].set_xlabel('Value')
axes[0,1].set_ylabel('Frequency')

# Scatter Plot
np.random.seed(42)
x_scatter = np.random.randn(100)
y_scatter = 2 * x_scatter + np.random.randn(100)
axes[1,0].scatter(x_scatter, y_scatter, alpha=0.6, color='mediumseagreen',
                   edgecolors='darkgreen', s=60)
axes[1,0].set_title('Scatter Plot — Linear Correlation')
axes[1,0].set_xlabel('X')
axes[1,0].set_ylabel('Y')

# Bar Chart
categories = ['Python', 'SQL', 'ML', 'DL', 'NLP']
scores = [85, 72, 90, 78, 65]
colors = ['steelblue', 'coral', 'mediumseagreen', 'mediumpurple', 'goldenrod']
bars = axes[1,1].bar(categories, scores, color=colors, edgecolor='white', width=0.6)
axes[1,1].set_title('Bar Chart — Skill Scores')
axes[1,1].set_ylabel('Score')
axes[1,1].set_ylim(0, 100)
# Add value labels on bars
for bar in bars:
    height = bar.get_height()
    axes[1,1].text(bar.get_x() + bar.get_width()/2., height + 1,
                   f'{height}', ha='center', va='bottom', fontweight='bold')

plt.tight_layout()
plt.savefig('matplotlib_gallery.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

## 2. Seaborn for Statistical Visualization

```python
import seaborn as sns
import pandas as pd
import matplotlib.pyplot as plt

# Load the Tips dataset (classic dataset)
tips = sns.load_dataset('tips')
print(tips.head())
print(tips.describe())

# Set theme
sns.set_theme(style='whitegrid', palette='muted')

fig, axes = plt.subplots(2, 3, figsize=(18, 11))
fig.suptitle('Seaborn Statistical Plots', fontsize=17, fontweight='bold')

# 1. Distribution Plot
sns.histplot(tips['total_bill'], kde=True, ax=axes[0,0], color='steelblue')
axes[0,0].set_title('Distribution — Total Bill')

# 2. Box Plot
sns.boxplot(x='day', y='total_bill', data=tips, ax=axes[0,1], palette='Set2')
axes[0,1].set_title('Box Plot — Bill by Day')

# 3. Scatter + Regression
sns.regplot(x='total_bill', y='tip', data=tips, ax=axes[0,2],
            scatter_kws={'alpha': 0.5}, line_kws={'color': 'red'})
axes[0,2].set_title('Scatter + Regression Line')

# 4. Violin Plot
sns.violinplot(x='sex', y='tip', hue='smoker', data=tips,
               ax=axes[1,0], split=True, palette='pastel')
axes[1,0].set_title('Violin Plot — Tips by Gender & Smoker')

# 5. Count Plot
sns.countplot(x='day', hue='sex', data=tips, ax=axes[1,1], palette='Set1')
axes[1,1].set_title('Count Plot — Visits by Day & Gender')

# 6. Heatmap (Correlation Matrix)
numeric_cols = tips.select_dtypes(include='number')
corr = numeric_cols.corr()
sns.heatmap(corr, annot=True, fmt='.2f', cmap='coolwarm',
            ax=axes[1,2], square=True, linewidths=0.5)
axes[1,2].set_title('Correlation Heatmap')

plt.tight_layout()
plt.show()
```

---

## 3. EDA Visualization Workflow

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Load Titanic dataset
url = "https://raw.githubusercontent.com/datasciencedojo/datasets/master/titanic.csv"
df = pd.read_csv(url)

print(f"Shape: {df.shape}")
print(df.head())
print(df.info())
print(df.describe())

# ── 1. Missing Data Heatmap ────────────────────────────────────────────────────
plt.figure(figsize=(12, 5))
sns.heatmap(df.isnull(), yticklabels=False, cbar=True, cmap='viridis', cbar_kws={'label': 'Missing'})
plt.title('Missing Data Pattern', fontsize=14)
plt.show()

# ── 2. Target Distribution ─────────────────────────────────────────────────────
fig, axes = plt.subplots(1, 2, figsize=(12, 5))
survival_counts = df['Survived'].value_counts()
axes[0].pie(survival_counts, labels=['Died (0)', 'Survived (1)'],
            colors=['#e74c3c', '#2ecc71'], autopct='%1.1f%%', startangle=90,
            wedgeprops={'edgecolor': 'white', 'linewidth': 2})
axes[0].set_title('Survival Distribution', fontsize=14)

sns.countplot(x='Survived', hue='Sex', data=df, ax=axes[1], palette='Set1')
axes[1].set_xticklabels(['Died', 'Survived'])
axes[1].set_title('Survival by Gender', fontsize=14)
plt.tight_layout()
plt.show()

# ── 3. Age Distribution by Survival ──────────────────────────────────────────
fig, axes = plt.subplots(1, 2, figsize=(14, 5))
for survived, color, label in [(0, '#e74c3c', 'Died'), (1, '#2ecc71', 'Survived')]:
    axes[0].hist(df[df['Survived'] == survived]['Age'].dropna(),
                 bins=30, alpha=0.6, color=color, label=label, edgecolor='white')
axes[0].set_title('Age Distribution by Survival')
axes[0].set_xlabel('Age')
axes[0].legend()

sns.boxplot(x='Pclass', y='Age', hue='Survived', data=df, ax=axes[1], palette='Set1')
axes[1].set_title('Age by Passenger Class and Survival')
plt.tight_layout()
plt.show()

# ── 4. Fare vs Age Scatter ─────────────────────────────────────────────────────
plt.figure(figsize=(10, 6))
scatter = plt.scatter(df['Age'], df['Fare'],
                      c=df['Survived'], cmap='RdYlGn',
                      alpha=0.6, s=50, edgecolors='grey', linewidths=0.3)
plt.colorbar(scatter, label='Survived (0=No, 1=Yes)')
plt.xlabel('Age')
plt.ylabel('Fare')
plt.title('Age vs Fare — Coloured by Survival')
plt.tight_layout()
plt.show()

# ── 5. Correlation Heatmap ────────────────────────────────────────────────────
plt.figure(figsize=(10, 7))
corr = df[['Survived','Pclass','Age','SibSp','Parch','Fare']].corr()
mask = np.triu(np.ones_like(corr, dtype=bool))  # upper triangle mask
sns.heatmap(corr, mask=mask, annot=True, fmt='.2f',
            cmap='coolwarm', center=0,
            square=True, linewidths=0.5,
            cbar_kws={'shrink': 0.8})
plt.title('Feature Correlation Matrix', fontsize=14)
plt.tight_layout()
plt.show()
```

---

## 4. Advanced Seaborn Plots

```python
import seaborn as sns
import pandas as pd
import matplotlib.pyplot as plt

penguins = sns.load_dataset('penguins').dropna()

# ── Pair Plot ─────────────────────────────────────────────────────────────────
g = sns.pairplot(penguins, hue='species', palette='Set2',
                 plot_kws={'alpha': 0.6}, diag_kind='kde')
g.fig.suptitle('Penguin Feature Pairplot', y=1.02, fontsize=14)
plt.show()

# ── FacetGrid ────────────────────────────────────────────────────────────────
g = sns.FacetGrid(penguins, col='species', hue='island',
                  col_wrap=3, height=4, palette='Set1')
g.map(plt.scatter, 'bill_length_mm', 'flipper_length_mm', alpha=0.7)
g.add_legend()
g.fig.suptitle('Bill Length vs Flipper Length by Species', y=1.02)
plt.show()

# ── Cluster Map (Hierarchical Heatmap) ───────────────────────────────────────
numeric_df = penguins.select_dtypes(include='number').dropna()
from sklearn.preprocessing import StandardScaler
scaled = pd.DataFrame(
    StandardScaler().fit_transform(numeric_df),
    columns=numeric_df.columns
)
sns.clustermap(scaled.corr(), annot=True, fmt='.2f',
               cmap='vlag', center=0, figsize=(8, 8))
plt.title('Clustered Correlation Heatmap')
plt.show()
```

---

## 5. Matplotlib Styling and Export

```python
import matplotlib.pyplot as plt
import numpy as np

# Available styles
print(plt.style.available)
# ['seaborn-v0_8', 'ggplot', 'fivethirtyeight', 'dark_background', ...]

# Using a style
with plt.style.context('seaborn-v0_8-whitegrid'):
    fig, ax = plt.subplots(figsize=(10, 5))
    x = np.linspace(0, 4*np.pi, 200)
    ax.plot(x, np.sin(x), lw=2.5, label='sin(x)')
    ax.plot(x, np.cos(x), lw=2.5, label='cos(x)', linestyle='--')
    ax.legend(fontsize=12)
    ax.set_title('Styled Plot', fontsize=15)
    plt.tight_layout()

    # Save in multiple formats
    plt.savefig('plot.png', dpi=300, bbox_inches='tight')   # PNG for presentations
    plt.savefig('plot.svg', bbox_inches='tight')             # SVG for web
    plt.savefig('plot.pdf', bbox_inches='tight')             # PDF for reports
    plt.show()
```

---

## 🎯 Student Tasks – Data Visualization

### Task 1: Matplotlib Practice (Easy)
**Objective**: Build 4 fundamental chart types from scratch.

**Instructions**:
Create a figure with 2×2 subplots using this dataset:
```python
import numpy as np, pandas as pd
np.random.seed(42)
months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec']
sales  = [45, 52, 61, 58, 70, 85, 92, 88, 75, 63, 55, 95]
costs  = [30, 35, 40, 38, 45, 55, 58, 52, 48, 42, 38, 60]
scores = np.random.normal(75, 12, 200)   # student scores
```

Build:
1. **Line chart** (top-left): Sales vs Costs over 12 months with legend.
2. **Bar chart** (top-right): Monthly profit (sales - costs) with bars coloured green/red for positive/negative.
3. **Histogram** (bottom-left): Student scores distribution with mean line marked.
4. **Pie chart** (bottom-right): Total revenue by quarter (Q1–Q4).

Add proper titles, labels, and save as `matplotlib_task1.png`.

**Expected Output**:
```
Figure saved: matplotlib_task1.png (1200×900 px)
4 subplots displayed:
  - Line chart with dual series
  - Profit bars (11 green, 1 red example)
  - Score histogram with mean=74.8 line
  - Quarterly pie with percentages
```

---

### Task 2: EDA Visualization Dashboard (Medium)
**Objective**: Create a complete visual EDA for a real dataset.

**Instructions**:
Use the Titanic dataset (`pd.read_csv('titanic.csv')` or the URL above):

Create a 3×3 visualization dashboard:
1. **Missing data heatmap** — show which columns have missing values.
2. **Survival rate pie chart** — with percentages.
3. **Age distribution (KDE)** — survived vs not survived overlaid.
4. **Survival by class bar chart** — stacked/grouped.
5. **Fare distribution boxplot** — by passenger class.
6. **Correlation heatmap** — numeric features only.
7. **Countplot** — survival by embarkation port.
8. **Scatter** — age vs fare, coloured by survival.
9. **Violin plot** — fare by sex and survival.

Style requirements:
- Use `seaborn.set_theme(style='whitegrid')`
- Consistent color palette throughout
- All axes labelled, all plots titled
- Save as `titanic_eda.png`

**Expected Output**:
```
Titanic EDA Dashboard (3×3 grid):
  Row 1: Missing data | Survival pie | Age KDE
  Row 2: Class survival | Fare boxplot | Correlation
  Row 3: Embarkation count | Age/Fare scatter | Violin
  
Key Insights noted:
  - 38% survived
  - Women survived 3× more than men
  - 1st class survival rate 63% vs 3rd class 24%
  - Fare correlated with Pclass (-0.55)
```

---

### Task 3: Complete Data Story with Visualizations (Challenge)
**Objective**: Tell a complete data story with professional-quality charts.

**Instructions**:
Choose one dataset (at least 1000+ rows with categorical + numerical + date columns):
- Suggestions: supermarket sales, COVID-19 data, IPL cricket stats, Zomato restaurants

Create a "data story" with 8+ visualizations answering these questions:
1. What is the overall distribution of the key metric?
2. How does it vary over time? (time series with trend line)
3. Which categories are top performers? (top 10 bar chart)
4. Are there outliers? (boxplot with outlier annotation)
5. What features correlate with the outcome? (heatmap)
6. Are there seasonal patterns? (monthly/weekly aggregation plot)
7. How do two key variables relate? (scatter with regression + color dimension)
8. Comparison across 3+ groups. (faceted/grouped plot)

Requirements:
- Use a consistent visual theme throughout
- Add text annotations to highlight key insights
- Export as a multi-page PDF report
- Write 1-sentence insights below each chart

**Expected Output**:
```
Data Story: Supermarket Sales Analysis (2024)

Plot 1: Total Sales Distribution
  → "Sales are right-skewed — most transactions under ₹500"

Plot 2: Monthly Sales Trend
  → "Clear peak in December (+42% vs average)"

Plot 3: Top 10 Product Categories
  → "Electronics dominates (34% of revenue)"

...all 8 plots with insights...

Report saved: sales_story.pdf (8 pages)
```

---
