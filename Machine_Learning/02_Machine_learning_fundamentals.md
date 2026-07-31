# Module 8: Machine Learning Fundamentals

> *"Data is the fuel. Machine Learning is the engine that turns data
> into intelligent decisions."*

------------------------------------------------------------------------

# Table of Contents

1.  Introduction
2.  What is Machine Learning?
3.  Traditional Programming vs Machine Learning
4.  Machine Learning Workflow
5.  Types of Machine Learning
6.  Supervised Learning
7.  Unsupervised Learning
8.  Feature Engineering
9.  Model Evaluation
10. Real-world Applications
11. Summary

------------------------------------------------------------------------

# 1. Introduction

Every day, you interact with Machine Learning.

-   Netflix recommends movies.
-   YouTube suggests videos.
-   Google predicts your search.
-   Banks detect fraud.
-   Email services filter spam.
-   ChatGPT understands your questions.

Instead of following thousands of manually written rules, these systems
**learn patterns from data**.

That is Machine Learning.

------------------------------------------------------------------------

# 2. What is Machine Learning?

Machine Learning (ML) is a branch of Artificial Intelligence that
enables computers to learn from data and make predictions or decisions
without being explicitly programmed for every situation.

Instead of writing rules manually, we provide examples.

The algorithm studies those examples and builds a mathematical model
capable of making predictions on unseen data.

------------------------------------------------------------------------

# 3. Traditional Programming vs Machine Learning

Traditional Programming

    Rules + Data
          ↓
     Program
          ↓
     Answer

Machine Learning

    Data + Correct Answers
              ↓
     Machine Learning Algorithm
              ↓
     Learns Rules (Model)

Once trained, the model predicts answers for new data.

------------------------------------------------------------------------

# 4. Machine Learning Workflow

    Business Problem
          ↓
    Collect Data
          ↓
    Clean Data
          ↓
    Feature Engineering
          ↓
    Choose Algorithm
          ↓
    Train Model
          ↓
    Evaluate Model
          ↓
    Deploy Model
          ↓
    Predict New Data

Every ML project follows this lifecycle.

------------------------------------------------------------------------

# 5. Types of Machine Learning

For beginners, the two most important categories are:

    Machine Learning
    │
    ├── Supervised Learning
    └── Unsupervised Learning

------------------------------------------------------------------------

# 6. Supervised Learning

## Learning with a Teacher

Imagine a teacher showing students questions together with the correct
answers.

The students learn from those examples.

Similarly, supervised learning receives:

-   Input data (Features)
-   Correct answers (Labels)

```{=html}
<!-- -->
```
    Labeled Data
          ↓
    ML Model
          ↓
    Learns Relationship
          ↓
    Predicts New Data

## Problem Types

    Supervised Learning
    │
    ├── Regression
    └── Classification

### Regression

Predicts a continuous numerical value.

Examples:

-   House price
-   Salary
-   Temperature
-   Sales forecast

Possible algorithms:

-   Linear Regression
-   Decision Tree Regressor
-   Random Forest Regressor
-   Support Vector Regressor
-   Neural Network Regressor

### Classification

Predicts categories.

Examples:

-   Spam / Not Spam
-   Fraud / Genuine
-   Disease / Healthy
-   Cat / Dog

Possible algorithms:

-   Logistic Regression
-   Decision Tree
-   Random Forest
-   SVM
-   KNN
-   Naive Bayes
-   Neural Network Classifier

> **Important:** Regression and Classification are **problem types**,
> not algorithms. Neural Networks are a family of algorithms that can
> solve both.

Advantages

-   High accuracy with quality labeled data.
-   Easy to evaluate.
-   Widely used in industry.

Limitations

-   Requires labeled datasets.
-   Labeling can be expensive.
-   Sensitive to data quality.

------------------------------------------------------------------------

# 7. Unsupervised Learning

## Learning without a Teacher

Imagine a basket of mixed fruits with no labels.

A child naturally groups similar fruits together.

That is how unsupervised learning works.

The algorithm receives only input data and discovers hidden patterns.

    Unlabeled Data
           ↓
    ML Algorithm
           ↓
    Patterns / Groups

## Problem Types

### Clustering

Find similar groups.

Examples:

-   Customer segmentation
-   Student grouping
-   Product grouping

Algorithms:

-   K-Means
-   DBSCAN
-   Hierarchical Clustering

### Association Rule Learning

Find relationships.

Example:

People who buy **Bread** also buy **Butter**.

Algorithms:

-   Apriori
-   FP-Growth

### Dimensionality Reduction

Reduce unnecessary features while preserving information.

Algorithms:

-   PCA
-   t-SNE
-   UMAP

### Anomaly Detection

Find unusual observations.

Examples:

-   Fraud detection
-   Network intrusion detection

Algorithms:

-   Isolation Forest
-   One-Class SVM

> Again, Clustering and Association are **problem types**. K-Means and
> Apriori are algorithms.

Advantages

-   No labels required.
-   Finds hidden insights.
-   Useful for exploratory analysis.

Limitations

-   Harder to evaluate.
-   Results may require human interpretation.

------------------------------------------------------------------------

# Supervised vs Unsupervised

  Supervised                    Unsupervised
  ----------------------------- -------------------------------
  Labeled data                  Unlabeled data
  Predict answers               Discover patterns
  Regression & Classification   Clustering, Association, etc.
  Teacher available             No teacher

------------------------------------------------------------------------

# 8. Feature Engineering

Feature Engineering is the process of selecting, cleaning, transforming,
or creating useful information (features) that helps a model learn
better.

Example dataset:

  Feature      Useful?
  ------------ ---------
  Area         ✅
  Bedrooms     ✅
  Age          ✅
  Wall Color   ❌

Common tasks:

-   Remove irrelevant features
-   Handle missing values
-   Encode categorical data
-   Scale numerical data
-   Create new features

Better features often improve model performance more than changing the
algorithm.

------------------------------------------------------------------------

# 9. Model Evaluation

A model should perform well not only on training data but also on unseen
data.

Typical process:

    Training Data
          ↓
    Train Model
          ↓
    Test Data
          ↓
    Performance Metrics

## Classification Metrics

-   Accuracy
-   Precision
-   Recall
-   F1-Score
-   Confusion Matrix

## Regression Metrics

-   MAE
-   MSE
-   RMSE
-   R² Score

Evaluation helps answer:

-   Is the model accurate?
-   Is it overfitting?
-   Can it generalize to new data?

------------------------------------------------------------------------

# 10. Real-world Applications

Healthcare

-   Disease prediction
-   Medical image analysis

Finance

-   Fraud detection
-   Credit scoring

E-commerce

-   Product recommendations
-   Customer segmentation

Education

-   Student performance prediction

Transportation

-   Traffic prediction
-   Route optimization

------------------------------------------------------------------------

# 11. Summary

Key ideas to remember:

-   Machine Learning learns patterns from data.
-   Supervised Learning uses labeled data.
-   Unsupervised Learning uses unlabeled data.
-   Regression, Classification, Clustering, and Association are
    **problem types**.
-   Algorithms are the methods used to solve those problems.
-   Neural Networks are algorithms that can solve both regression and
    classification tasks.
-   Feature Engineering improves the quality of input data.
-   Model Evaluation measures how well a model performs on unseen data.

## What's Next?

After understanding these fundamentals, you are ready to study
individual Machine Learning algorithms such as:

-   Linear Regression
-   Logistic Regression
-   Decision Trees
-   Random Forest
-   K-Means
-   PCA
-   Neural Networks

Each algorithm is designed to solve specific problem types and has its
own strengths and limitations.

---

## 🎯 Student Tasks – Module 19: ML Fundamentals

### Task 1: Model Evaluation Metrics (Easy)
**Objective**: Calculate and interpret model evaluation metrics.

**Instructions**:
Given the following predictions for a binary classification model:
```
Actual:    [1, 0, 1, 1, 0, 1, 0, 0, 1, 1]
Predicted: [1, 0, 0, 1, 0, 1, 1, 0, 1, 0]
```
1. Calculate manually: TP, TN, FP, FN.
2. Calculate: Accuracy, Precision, Recall, F1-Score.
3. Verify using `sklearn.metrics`.
4. Plot the Confusion Matrix using seaborn.
5. Explain when Precision matters more than Recall (and vice versa).

**Expected Output**:
```
TP=5, TN=3, FP=1, FN=1

Accuracy:  0.80
Precision: 0.83
Recall:    0.71
F1-Score:  0.77

Confusion Matrix:
[[3 1]
 [1 5]]
```

---

### Task 2: Train-Test Split & Cross-Validation (Medium)
**Objective**: Practice proper model evaluation techniques.

**Instructions**:
Using `sklearn.datasets.load_diabetes()` (regression task):
1. Split into 80/20 train-test (random_state=42).
2. Train a `LinearRegression` model.
3. Evaluate: MAE, MSE, RMSE, R² on test set.
4. Perform 5-fold cross-validation and report mean ± std of R².
5. Plot predicted vs actual values.
6. Discuss: How does CV score compare to single train-test split?

**Expected Output**:
```
Train-Test Split:
  MAE:  44.3
  MSE:  2900.1
  RMSE: 53.9
  R²:   0.48

5-Fold Cross-Validation:
  R² per fold: [0.45, 0.52, 0.41, 0.50, 0.48]
  Mean R²: 0.47 ± 0.04
```

---

### Task 3: Overfitting & Regularization Explorer (Challenge)
**Objective**: Understand overfitting and how regularization controls it.

**Instructions**:
Using the California Housing dataset:
1. Train a `PolynomialFeatures(degree=10) + LinearRegression` pipeline.
2. Print train and test R² scores — observe overfitting.
3. Apply Ridge Regression (L2) and Lasso Regression (L1) with various alpha values.
4. Plot: train R² and test R² for different alpha values.
5. Find the optimal alpha for Ridge and Lasso.
6. Compare all 4 models in a final report table.

**Expected Output**:
```
=== Model Comparison ===
Model                    Train R²   Test R²
------------------------------------------
Polynomial (degree=10)   0.99       0.23    ← Overfitting!
Ridge (alpha=1.0)        0.78       0.71
Ridge (alpha=0.01)       0.91       0.68
Lasso (alpha=0.01)       0.79       0.72
Lasso (alpha=10.0)       0.55       0.53    ← Underfitting

Best Model: Ridge (alpha=1.0) with Test R²=0.71
```

---

