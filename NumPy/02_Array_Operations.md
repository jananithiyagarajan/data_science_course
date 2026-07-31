# NumPy Array Operations

## Overview

Once you can create and index arrays, the next step is performing **mathematical operations** on them.
NumPy's power comes from **vectorized operations** — no Python loops needed!

---

## 1. Arithmetic Operations

NumPy supports element-wise arithmetic operations between arrays.

### Element-wise Operations

```python
import numpy as np

a = np.array([10, 20, 30, 40])
b = np.array([2, 4, 5, 8])

print(a + b)     # [12 24 35 48]
print(a - b)     # [ 8 16 25 32]
print(a * b)     # [ 20  80 150 320]
print(a / b)     # [ 5.  5.  6.  5.]
print(a // b)    # [ 5  5  6  5]  — floor division
print(a % b)     # [0 0 0 0]  — modulus
print(a ** 2)    # [ 100  400  900 1600] — power
```

### Scalar Operations

```python
arr = np.array([1, 2, 3, 4, 5])

print(arr + 10)   # [11 12 13 14 15]
print(arr * 3)    # [ 3  6  9 12 15]
print(arr / 2)    # [0.5 1.  1.5 2.  2.5]
print(arr ** 2)   # [ 1  4  9 16 25]
```

### 2D Array Operations

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

print(A + B)    # [[ 6  8] [10 12]]
print(A * B)    # [[ 5 12] [21 32]]  — element-wise!

# Matrix multiplication (dot product)
print(np.dot(A, B))   # [[19 22] [43 50]]
print(A @ B)          # Same result — modern Python syntax
```

---

## 2. Broadcasting Rules in NumPy

**Broadcasting** allows NumPy to perform operations on arrays of different shapes, by automatically expanding the smaller array to match.

### Rule Summary

1. If arrays have different numbers of dimensions, the smaller one is padded with 1s on the **left**.
2. Arrays with size 1 along a dimension are stretched to match the other array.
3. If shapes don't match and neither dimension is 1 → **Error**.

### Example 1: Scalar + Array (simplest case)

```python
arr = np.array([1, 2, 3, 4])
print(arr + 10)   # [11 12 13 14] — scalar is broadcast to [10,10,10,10]
```

### Example 2: 1D + 2D

```python
A = np.array([[1, 2, 3],
              [4, 5, 6]])    # shape (2, 3)

b = np.array([10, 20, 30])  # shape (3,) → broadcast to (2,3)

print(A + b)
# [[11 22 33]
#  [14 25 36]]
```

### Example 3: Column + Row Broadcast

```python
col = np.array([[1],    # shape (3, 1)
                [2],
                [3]])

row = np.array([10, 20, 30, 40])  # shape (4,) → (1, 4)

result = col + row     # shape becomes (3, 4)
print(result)
# [[11 21 31 41]
#  [12 22 32 42]
#  [13 23 33 43]]
```

### Broadcasting Visualization

```
Shape:    (3, 1)  +  (4,)
Step 1:   (3, 1)  +  (1, 4)   [pad to same ndim]
Step 2:   (3, 4)  +  (3, 4)   [stretch size-1 dims]
Result:   (3, 4)
```

### Invalid Broadcasting

```python
a = np.array([[1, 2, 3]])   # (1, 3)
b = np.array([[1, 2]])      # (1, 2)

# a + b  → ❌ ValueError — shapes (1,3) and (1,2) incompatible
```

---

## 3. Universal Functions (ufuncs)

NumPy's **ufuncs** (universal functions) are vectorized mathematical operations that apply element-wise to arrays — much faster than Python loops.

### Mathematical ufuncs

```python
arr = np.array([1, 4, 9, 16, 25])

print(np.sqrt(arr))    # [1. 2. 3. 4. 5.]
print(np.abs(arr))     # [1 4 9 16 25]

arr2 = np.array([-3, -1, 0, 2, 5])
print(np.abs(arr2))    # [3 1 0 2 5]
print(np.sign(arr2))   # [-1 -1  0  1  1]
print(np.square(arr))  # [ 1 16 81 256 625]
```

### Exponential and Logarithm

```python
arr = np.array([1, 2, 3, 4, 5])

print(np.exp(arr))      # e^x: [2.718 7.389 20.09 54.6 148.4]
print(np.exp2(arr))     # 2^x: [2.  4.  8. 16. 32.]
print(np.log(arr))      # natural log: [0.    0.693 1.099 1.386 1.609]
print(np.log2(arr))     # log base 2:  [0.    1.    1.585 2.    2.322]
print(np.log10(arr))    # log base 10: [0.    0.301 0.477 0.602 0.699]
```

### Trigonometric Functions

```python
angles = np.array([0, np.pi/6, np.pi/4, np.pi/3, np.pi/2])

print(np.sin(angles))   # [0.   0.5  0.707 0.866 1.   ]
print(np.cos(angles))   # [1.   0.866 0.707 0.5  0.   ]
print(np.tan(angles))   # [0.   0.577 1.   1.732  inf ]

# Inverse trig
print(np.arcsin([0, 0.5, 1]))  # [0.    0.524 1.571]
print(np.degrees(np.pi))       # 180.0
print(np.radians(180))         # 3.14159...
```

### Comparison ufuncs

```python
a = np.array([1, 2, 3, 4, 5])
b = np.array([3, 2, 1, 4, 6])

print(np.greater(a, b))        # [False False  True False False]
print(np.equal(a, b))          # [False  True False  True False]
print(np.maximum(a, b))        # [3 2 3 4 6]
print(np.minimum(a, b))        # [1 2 1 4 5]
```

### Rounding

```python
arr = np.array([1.23, 4.567, -2.89, 0.5])

print(np.round(arr, 1))   # [ 1.2  4.6 -2.9  0.5]
print(np.floor(arr))      # [ 1.  4. -3.  0.]
print(np.ceil(arr))       # [ 2.  5. -2.  1.]
print(np.trunc(arr))      # [ 1.  4. -2.  0.]
```

---

## 4. Aggregate Functions

Aggregate functions reduce an array to a single value (or a reduced dimension).

### Basic Aggregations

```python
arr = np.array([[1, 2, 3],
                [4, 5, 6],
                [7, 8, 9]])

# Across entire array
print(np.sum(arr))      # 45
print(np.min(arr))      # 1
print(np.max(arr))      # 9
print(np.mean(arr))     # 5.0
print(np.median(arr))   # 5.0
print(np.std(arr))      # Standard deviation: 2.581...
print(np.var(arr))      # Variance: 6.666...
print(np.prod(arr))     # Product of all: 362880
```

### Aggregation Along an Axis

```python
# axis=0 → operate along rows (column-wise result)
print(np.sum(arr, axis=0))   # [12 15 18]
print(np.max(arr, axis=0))   # [7 8 9]

# axis=1 → operate along columns (row-wise result)
print(np.sum(arr, axis=1))   # [ 6 15 24]
print(np.mean(arr, axis=1))  # [2. 5. 8.]
```

### Other Useful Aggregations

```python
arr = np.array([3, 1, 4, 1, 5, 9, 2, 6])

print(np.argmax(arr))   # 5 — index of max value
print(np.argmin(arr))   # 1 — index of min value
print(np.sort(arr))     # [1 1 2 3 4 5 6 9]
print(np.argsort(arr))  # indices that would sort the array
print(np.unique(arr))   # [1 2 3 4 5 6 9]
print(np.cumsum(arr))   # cumulative sum: [3 4 8 9 14 23 25 31]
print(np.cumprod(np.array([1,2,3,4])))  # [1 2 6 24]
```

### Statistical Functions

```python
data = np.array([12, 45, 23, 67, 34, 89, 56, 12, 78, 34])

print(np.percentile(data, 25))    # Q1: 27.25
print(np.percentile(data, 50))    # Median/Q2: 39.5
print(np.percentile(data, 75))    # Q3: 64.25
print(np.percentile(data, [25, 50, 75]))  # All at once

# Correlation
x = np.array([1, 2, 3, 4, 5])
y = np.array([2, 4, 5, 4, 5])
print(np.corrcoef(x, y))   # Correlation matrix
print(np.cov(x, y))        # Covariance matrix
```

---

## 🎯 Student Tasks – Module 11: Array Operations

### Task 1: Arithmetic & Broadcasting (Easy)
**Objective**: Practice element-wise operations and broadcasting.

**Instructions**:
1. Create two 3×3 arrays `A` and `B` with values 1–9 and 9–1.
2. Perform: `A + B`, `A - B`, `A * B`, `A / B`.
3. Add a row vector `[10, 20, 30]` to each row of `A` using broadcasting.
4. Create a column vector `[[1], [2], [3]]` and add it to `A`.
5. Print shapes and results.

**Expected Output**:
```
A + B:
[[10 10 10]
 [10 10 10]
 [10 10 10]]

A + row_vec (broadcast):
[[11 22 33]
 [14 25 36]
 [17 28 39]]
```

---

### Task 2: Universal Functions on Real Data (Medium)
**Objective**: Apply ufuncs to simulate real computations.

**Instructions**:
1. Simulate 20 temperature readings (Celsius) using `np.random.uniform(20, 45, 20)`.
2. Convert all to Fahrenheit using `F = C * 9/5 + 32`.
3. Find: mean, min, max, std deviation.
4. Classify each temperature:
   - < 30°C → "Cool"
   - 30–40°C → "Warm"
   - > 40°C → "Hot"
   (Hint: use `np.where()`)
5. Count how many are in each category.

**Expected Output**:
```
Temperatures (C): [24.3 38.5 41.2 ...]
Temperatures (F): [75.7 101.3 106.2 ...]

Stats: Mean=34.2°C | Min=21.1°C | Max=43.5°C | Std=6.2°C

Categories: ['Cool' 'Warm' 'Hot' ...]
Cool: 4  |  Warm: 12  |  Hot: 4
```

---

### Task 3: Statistical Analysis with NumPy (Challenge)
**Objective**: Use aggregate functions for comprehensive data analysis.

**Instructions**:
Simulate student exam scores for 50 students across 5 subjects:
```python
scores = np.random.randint(40, 100, size=(50, 5))
subjects = ["Maths", "Science", "English", "History", "CS"]
```
1. Calculate per-student: total, average, min, max.
2. Calculate per-subject: mean, std, pass rate (score ≥ 50).
3. Find the top 5 students by total score.
4. Find which subject has the highest failure rate.
5. Find the percentile rank of each student.
6. Print a comprehensive report.

**Expected Output**:
```
=== Student Exam Analysis ===
Number of Students: 50
Number of Subjects: 5

Per Subject Stats:
Subject    Mean   Std    Pass Rate
Maths      72.4   12.3   94.0%
Science    68.1   15.2   88.0%
...

Top 5 Students by Total Score:
Student 23: 463/500
Student 7:  458/500
...

Highest Failure Rate: History (16%)
```

---
