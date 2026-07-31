# Advanced NumPy Operations

## Overview

This module covers the powerful advanced features of NumPy: reshaping arrays, linear algebra, random number generation, and advanced operations that power data science and machine learning workflows.

---

## 1. Reshaping and Manipulating Arrays

### `reshape()` — Change Shape Without Changing Data

```python
import numpy as np

arr = np.arange(1, 13)      # [1 2 3 4 5 6 7 8 9 10 11 12]
print(arr.shape)            # (12,)

# Reshape to 2D
matrix = arr.reshape(3, 4)
print(matrix)
# [[ 1  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]

# Reshape to 3D
tensor = arr.reshape(2, 3, 2)
print(tensor.shape)         # (2, 3, 2)

# Use -1 to let NumPy calculate one dimension
arr.reshape(-1, 4)          # Auto-calculate rows
arr.reshape(3, -1)          # Auto-calculate columns
arr.reshape(-1)             # Flatten to 1D
```

### `flatten()` vs `ravel()`

```python
matrix = np.array([[1, 2, 3], [4, 5, 6]])

# flatten() — returns a COPY
flat_copy = matrix.flatten()
flat_copy[0] = 999
print(matrix[0, 0])   # 1 — original unchanged

# ravel() — returns a VIEW (memory efficient)
flat_view = matrix.ravel()
flat_view[0] = 999
print(matrix[0, 0])   # 999 — original changed!
```

### `transpose()` — Swap Axes

```python
arr = np.array([[1, 2, 3],
                [4, 5, 6]])
print(arr.shape)            # (2, 3)

transposed = arr.T          # or arr.transpose()
print(transposed)
# [[1 4]
#  [2 5]
#  [3 6]]
print(transposed.shape)     # (3, 2)
```

### `squeeze()` and `expand_dims()`

```python
arr = np.array([[[1, 2, 3]]])   # shape (1, 1, 3)

squeezed = np.squeeze(arr)
print(squeezed.shape)            # (3,)

arr2 = np.array([1, 2, 3])      # shape (3,)
expanded = np.expand_dims(arr2, axis=0)
print(expanded.shape)            # (1, 3)
expanded2 = np.expand_dims(arr2, axis=1)
print(expanded2.shape)           # (3, 1)
```

### Concatenation and Stacking

```python
a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6], [7, 8]])

# Concatenate along rows (axis=0)
print(np.concatenate([a, b], axis=0))
# [[1 2]
#  [3 4]
#  [5 6]
#  [7 8]]

# Concatenate along columns (axis=1)
print(np.concatenate([a, b], axis=1))
# [[1 2 5 6]
#  [3 4 7 8]]

# vstack — vertical stack
print(np.vstack([a, b]))      # Same as axis=0

# hstack — horizontal stack
print(np.hstack([a, b]))      # Same as axis=1

# Stack along new axis
c = np.stack([a, b], axis=0)
print(c.shape)   # (2, 2, 2)
```

### Splitting Arrays

```python
arr = np.arange(1, 13)

# Split into 3 equal parts
parts = np.split(arr, 3)
print(parts)   # [array([1,2,3,4]), array([5,6,7,8]), array([9,10,11,12])]

# Split at specific indices
parts = np.split(arr, [4, 8])
print(parts)   # [array([1,2,3,4]), array([5,6,7,8]), array([9,10,11,12])]

# vsplit and hsplit for 2D
matrix = arr.reshape(4, 3)
top, bottom = np.vsplit(matrix, 2)
left, right = np.hsplit(matrix, 3)
```

---

## 2. Linear Algebra with NumPy

NumPy's `linalg` module provides comprehensive linear algebra functions — fundamental for ML and data science.

### Matrix Multiplication

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

# Dot product / Matrix multiplication
print(np.dot(A, B))
print(A @ B)    # Equivalent (Python 3.5+)
# [[19 22]
#  [43 50]]
```

### Transpose

```python
A = np.array([[1, 2, 3], [4, 5, 6]])
print(A.T)
print(np.transpose(A))
```

### Determinant

```python
A = np.array([[1, 2], [3, 4]])
det = np.linalg.det(A)
print(det)   # -2.0000000000000004
```

### Inverse of a Matrix

```python
A = np.array([[2, 1], [5, 3]])
A_inv = np.linalg.inv(A)
print(A_inv)
# [[ 3. -1.]
#  [-5.  2.]]

# Verify: A @ A_inv should be identity
print(A @ A_inv)   # [[1. 0.] [0. 1.]]
```

### Solving Linear Equations (Ax = b)

```python
# Solve: 2x + y = 5, x + 3y = 10
A = np.array([[2, 1], [1, 3]])
b = np.array([5, 10])

x = np.linalg.solve(A, b)
print(x)   # [1. 3.]  → x=1, y=3
```

### Eigenvalues and Eigenvectors

```python
A = np.array([[4, 2], [1, 3]])

eigenvalues, eigenvectors = np.linalg.eig(A)
print("Eigenvalues:", eigenvalues)
print("Eigenvectors:\n", eigenvectors)
```

### Norms

```python
v = np.array([3, 4])
print(np.linalg.norm(v))          # L2 norm: 5.0
print(np.linalg.norm(v, ord=1))   # L1 norm: 7.0
print(np.linalg.norm(v, ord=np.inf))  # L∞ norm: 4.0
```

### Rank, Trace, SVD

```python
A = np.array([[1, 2, 3], [4, 5, 6]])

print(np.linalg.matrix_rank(A))   # 2
print(np.trace(np.eye(4)))        # 4.0 — sum of diagonal

# Singular Value Decomposition
U, S, Vt = np.linalg.svd(A)
print("U shape:", U.shape)
print("S:", S)
print("Vt shape:", Vt.shape)
```

---

## 3. Random Module

NumPy's random module is essential for simulations, machine learning, and statistical sampling.

```python
rng = np.random.default_rng(seed=42)   # Modern API (NumPy >= 1.17)
```

### Setting Seed (Reproducibility)

```python
np.random.seed(42)   # Old API
# or
rng = np.random.default_rng(42)
```

### Generating Random Numbers

```python
# Random float in [0, 1)
print(np.random.rand(3))           # [0.374 0.951 0.732]
print(np.random.rand(2, 3))        # 2x3 matrix

# Random float from standard normal (mean=0, std=1)
print(np.random.randn(4))          # [-0.23  1.72  0.51 -0.88]
print(np.random.randn(3, 3))       # 3x3 matrix

# Random integers
print(np.random.randint(1, 100))   # Single integer [1, 100)
print(np.random.randint(0, 10, size=(3, 4)))  # 3x4 matrix of ints

# Random from specific distribution
print(np.random.uniform(5, 15, size=5))       # Uniform [5, 15)
print(np.random.normal(mean=70, scale=10, size=100))  # Normal distribution
print(np.random.binomial(n=10, p=0.5, size=20))       # Binomial
print(np.random.poisson(lam=5, size=20))              # Poisson
```

### Random Sampling and Shuffling

```python
arr = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

# Shuffle (in place)
np.random.shuffle(arr)
print(arr)

# Random choice (with replacement)
choice = np.random.choice(arr, size=5)
print(choice)

# Random choice (without replacement)
sample = np.random.choice(arr, size=5, replace=False)
print(sample)
```

---

## 4. Integration with Pandas

NumPy and Pandas work seamlessly together.

```python
import numpy as np
import pandas as pd

# NumPy array → Pandas DataFrame
data = np.random.randn(100, 4)
df = pd.DataFrame(data, columns=["A", "B", "C", "D"])

# Pandas DataFrame → NumPy array
arr = df.values                # or df.to_numpy()
print(arr.shape)               # (100, 4)
print(arr.dtype)               # float64

# Apply NumPy functions on Pandas columns
df["Mean_AB"] = np.mean(df[["A", "B"]], axis=1)
df["Normalized"] = (df["A"] - np.mean(df["A"])) / np.std(df["A"])
```

---

## 5. Advanced Operations

### `np.where()` — Conditional Element Selection

```python
arr = np.array([5, -3, 8, -1, 12, -7, 0])

# Replace negatives with 0
result = np.where(arr > 0, arr, 0)
print(result)   # [ 5  0  8  0 12  0  0]

# Classify: > 5 → "High", else → "Low"
labels = np.where(arr > 5, "High", "Low")
print(labels)   # ['Low' 'Low' 'High' 'Low' 'High' 'Low' 'Low']

# 2D where
matrix = np.random.randint(0, 10, (3, 3))
result = np.where(matrix > 5, matrix, -1)
```

### `np.unique()` — Find Unique Values

```python
arr = np.array([3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5])

print(np.unique(arr))                       # [1 2 3 4 5 6 9]
unique, counts = np.unique(arr, return_counts=True)
print(unique)   # [1 2 3 4 5 6 9]
print(counts)   # [2 1 2 1 3 1 1]

# Unique rows in 2D
matrix = np.array([[1, 2], [3, 4], [1, 2], [5, 6]])
print(np.unique(matrix, axis=0))  # [[1 2] [3 4] [5 6]]
```

### `np.clip()` — Limit Values

```python
arr = np.array([1, 5, 10, 15, 20, 25])
clipped = np.clip(arr, 5, 20)
print(clipped)   # [ 5  5 10 15 20 20]
```

### `np.select()` — Multiple Conditions

```python
arr = np.array([10, 25, 50, 75, 90])
conditions = [arr < 20, arr < 50, arr < 80, arr >= 80]
choices     = ["Low", "Medium", "High", "Very High"]
result = np.select(conditions, choices, default="Unknown")
print(result)   # ['Low' 'Medium' 'High' 'High' 'Very High']
```

### `np.searchsorted()` — Binary Search

```python
sorted_arr = np.array([10, 20, 30, 40, 50])
print(np.searchsorted(sorted_arr, 25))   # 2 — insert at index 2
print(np.searchsorted(sorted_arr, 30))   # 2 (left) or 3 (right)
```

### `np.isin()` — Membership Test

```python
arr = np.array([1, 2, 3, 4, 5])
targets = [2, 4, 6]
print(np.isin(arr, targets))   # [False  True False  True False]
print(arr[np.isin(arr, targets)])  # [2 4]
```

### Structured Arrays

```python
dt = np.dtype([("name", "U20"), ("age", "i4"), ("score", "f4")])
students = np.array([
    ("Arun", 20, 88.5),
    ("Priya", 21, 95.0),
    ("Kumar", 19, 72.3)
], dtype=dt)

print(students["name"])    # ['Arun' 'Priya' 'Kumar']
print(students["score"])   # [88.5 95.  72.3]
```

---

## 🎯 Student Tasks – Module 12: Advanced NumPy Operations

### Task 1: Reshape & Linear Algebra Basics (Easy)
**Objective**: Practice reshaping and basic linear algebra.

**Instructions**:
1. Create an array `np.arange(1, 25)` and reshape to (4, 6), (6, 4), and (2, 3, 4).
2. Transpose each 2D version.
3. Create a 3×3 matrix and calculate:
   - Its determinant
   - Its inverse
   - Verify: `A @ A_inv == Identity`
4. Solve: `3x + y = 9`, `x + 2y = 8`.

**Expected Output**:
```
Original: [ 1  2  3 ... 24]
Reshaped (4,6): shape (4, 6)
Transposed: shape (6, 4)

Determinant: -306.0
Solution: x = 2.0, y = 3.0
Verification: A @ A_inv ≈ Identity ✅
```

---

### Task 2: Random Simulation & where() (Medium)
**Objective**: Use random generation and conditional operations.

**Instructions**:
1. Simulate dice rolls: roll 2 dice 1000 times (`np.random.randint(1, 7, size=(1000, 2))`).
2. Calculate the sum of each roll.
3. Count frequency of each possible sum (2–12).
4. Find the most common sum.
5. Use `np.where()` to label sums as "Lucky" (7 or 11), "Craps" (2, 3, 12), or "Point" (rest).
6. Count how many of each label.

**Expected Output**:
```
Sum Frequencies:
2:  28   3:  56   4:  83   5: 110   6: 137
7: 169   8: 139   9: 111  10:  83  11:  57  12:  27

Most common sum: 7 (169 times)

Labels:
Lucky (7 or 11): 226
Craps (2, 3, 12):111
Point (others): 663
```

---

### Task 3: Complete Data Analysis with NumPy (Challenge)
**Objective**: Combine all advanced NumPy skills for a full analysis task.

**Instructions**:
You're given a dataset of 100 students with scores in 5 subjects.

```python
np.random.seed(0)
scores = np.random.randint(30, 100, (100, 5))
```

1. **Reshape & Stats**:
   - Calculate mean, std, min, max per student AND per subject.
2. **Normalization**: Normalize each subject's scores to [0, 100] range.
3. **Rank students**: Calculate each student's rank by total score using `np.argsort()`.
4. **Grade assignment**: Use `np.select()` to assign grades (A/B/C/D/F) per subject.
5. **Linear Algebra**: Compute the covariance matrix of the 5 subjects. Find which two subjects are most correlated.
6. **SVD**: Perform SVD on the scores matrix. Print the shape of U, S, Vt.
7. **Save**: Convert to structured array with student IDs and export info.

**Expected Output**:
```
=== Comprehensive NumPy Analysis ===
Scores Shape: (100, 5)

Per Subject Means: [68.4 72.1 65.9 70.2 67.8]
Per Student Means (first 5): [72.4 65.2 78.0 60.8 73.6]

Most correlated subjects: Subject 2 & Subject 4 (r=0.87)

SVD: U(100,5), S(5,), Vt(5,5)

Grade Distribution:
Subject 1: A:12, B:28, C:35, D:18, F:7
...

Top 5 Students by Total Score:
Rank 1: Student #47, Total: 489
```

---
