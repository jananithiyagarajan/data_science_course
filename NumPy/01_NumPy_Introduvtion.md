# NumPy Introduction

## What is NumPy?

**NumPy** (Numerical Python) is the fundamental library for scientific computing in Python.
It provides support for large, multi-dimensional arrays and matrices, along with a large collection of mathematical functions.

### Why NumPy?

| Feature              | Python List         | NumPy Array              |
| -------------------- | ------------------- | ------------------------ |
| Speed                | Slow (Python loops) | Fast (C-optimized)       |
| Memory               | More (object refs)  | Less (fixed type)        |
| Math Operations      | Manual loops needed | Vectorized (no loops)    |
| Multi-dimensional    | Nested lists only   | Native N-D arrays        |
| Broadcasting         | Not supported       | Built-in                 |

---

## 1. Installing NumPy

```bash
pip install numpy
```

Check installation:
```python
import numpy as np
print(np.__version__)  # e.g., 1.26.4
```

---

## 2. Importing NumPy

```python
import numpy as np    # 'np' is the universal alias
```

---

## 3. NumPy Arrays — Basics

The core data structure in NumPy is the **ndarray** (n-dimensional array).

### Creating Arrays from Lists

```python
import numpy as np

# 1D Array
arr1 = np.array([1, 2, 3, 4, 5])
print(arr1)         # [1 2 3 4 5]
print(type(arr1))   # <class 'numpy.ndarray'>

# 2D Array (Matrix)
arr2 = np.array([[1, 2, 3],
                 [4, 5, 6],
                 [7, 8, 9]])
print(arr2)

# 3D Array
arr3 = np.array([[[1, 2], [3, 4]],
                 [[5, 6], [7, 8]]])
print(arr3)
```

### Array with Specific Data Types

```python
arr_int   = np.array([1, 2, 3], dtype=np.int32)
arr_float = np.array([1, 2, 3], dtype=np.float64)
arr_bool  = np.array([True, False, True], dtype=bool)
arr_str   = np.array(["a", "b", "c"], dtype=str)
```

### Built-in Array Creation Functions

```python
# Zeros
zeros = np.zeros((3, 4))           # 3x4 matrix of 0.0
zeros_int = np.zeros((2, 3), dtype=int)  # Integer zeros

# Ones
ones = np.ones((2, 3))             # 2x3 matrix of 1.0

# Full (all same value)
fives = np.full((3, 3), 5)         # 3x3 matrix of 5

# Empty (uninitialized — values are garbage)
empty = np.empty((2, 2))

# Identity Matrix
eye = np.eye(4)                    # 4x4 identity matrix

# arange (like Python range)
arr = np.arange(0, 10, 2)          # [0 2 4 6 8]
arr = np.arange(1, 11)             # [1 2 3 4 5 6 7 8 9 10]

# linspace (evenly spaced over interval)
arr = np.linspace(0, 1, 5)         # [0.   0.25 0.5  0.75 1.  ]
arr = np.linspace(0, 100, 11)      # 11 evenly spaced points from 0 to 100

# logspace
arr = np.logspace(0, 3, 4)         # [1. 10. 100. 1000.]
```

---

## 4. Array Attributes

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

print(arr.shape)    # (2, 3) — 2 rows, 3 columns
print(arr.ndim)     # 2 — number of dimensions
print(arr.size)     # 6 — total elements
print(arr.dtype)    # int64
print(arr.itemsize) # 8 bytes per element
print(arr.nbytes)   # total bytes in memory
```

---

## 5. Indexing and Slicing

### 1D Array Indexing

```python
arr = np.array([10, 20, 30, 40, 50])

print(arr[0])     # 10 — first element
print(arr[-1])    # 50 — last element
print(arr[2])     # 30

# Modify element
arr[0] = 99
print(arr)        # [99 20 30 40 50]
```

### 1D Array Slicing

```python
arr = np.array([10, 20, 30, 40, 50, 60, 70])

print(arr[1:4])   # [20 30 40] — index 1 to 3
print(arr[:3])    # [10 20 30] — first 3
print(arr[3:])    # [40 50 60 70] — from index 3
print(arr[::2])   # [10 30 50 70] — every 2nd
print(arr[::-1])  # [70 60 50 40 30 20 10] — reversed
```

### 2D Array Indexing

```python
arr = np.array([[1, 2, 3],
                [4, 5, 6],
                [7, 8, 9]])

print(arr[0, 0])    # 1 — row 0, col 0
print(arr[1, 2])    # 6 — row 1, col 2
print(arr[-1, -1])  # 9 — last row, last col
```

### 2D Array Slicing

```python
# Rows 0-1, All columns
print(arr[0:2, :])
# [[1 2 3]
#  [4 5 6]]

# All rows, columns 1-2
print(arr[:, 1:3])
# [[2 3]
#  [5 6]
#  [8 9]]

# Submatrix
print(arr[0:2, 1:3])
# [[2 3]
#  [5 6]]

# Entire column
print(arr[:, 1])    # [2 5 8]

# Entire row
print(arr[0, :])    # [1 2 3]
```

### Boolean Indexing (Masking)

```python
arr = np.array([5, 12, 3, 18, 7, 25, 9])

# Get all elements > 10
mask = arr > 10
print(mask)         # [False  True False  True False  True False]
print(arr[mask])    # [12 18 25]

# Shorthand
print(arr[arr > 10])  # [12 18 25]

# Multiple conditions
print(arr[(arr > 5) & (arr < 20)])  # [12 18  7  9]
```

### Fancy Indexing (Index Array)

```python
arr = np.array([10, 20, 30, 40, 50])
indices = [0, 2, 4]
print(arr[indices])   # [10 30 50]

# 2D Fancy Indexing
arr2d = np.array([[1, 2], [3, 4], [5, 6]])
print(arr2d[[0, 2]])  # Rows 0 and 2
```

---

## 6. NumPy Data Types

| dtype      | Description             |
| ---------- | ----------------------- |
| `int8/16/32/64` | Integer (8/16/32/64 bits) |
| `float16/32/64` | Float (16/32/64 bits) |
| `complex64/128` | Complex numbers      |
| `bool`     | True/False              |
| `str_`     | String                  |
| `object`   | Python objects          |

```python
arr = np.array([1, 2, 3])
print(arr.dtype)              # int64

# Convert dtype
arr_float = arr.astype(float)
print(arr_float.dtype)        # float64
```

---

## 🎯 Student Tasks – Module 10: NumPy Introduction

### Task 1: Array Creation & Attributes (Easy)
**Objective**: Practice creating arrays and accessing their properties.

**Instructions**:
1. Create the following arrays and print shape, dtype, ndim, size for each:
   - 1D: `[5, 10, 15, 20, 25, 30]`
   - 2D: 4×3 matrix of zeros
   - 2D: 3×3 identity matrix
   - 1D: 10 evenly spaced values from 0 to 1
2. Create a 3D array of shape (2, 3, 4) filled with ones.
3. Create an array from 1 to 20 (step 3) using `arange()`.

**Expected Output**:
```
1D Array: [5 10 15 20 25 30]
Shape: (6,), dtype: int64, ndim: 1, size: 6

Zeros (4x3):
[[0. 0. 0.]
 [0. 0. 0.]
 [0. 0. 0.]
 [0. 0. 0.]]
Shape: (4, 3)

linspace(0, 1, 10): [0.    0.111 ... 1.   ]

arange(1, 20, 3): [ 1  4  7 10 13 16 19]
```

---

### Task 2: Indexing & Slicing Practice (Medium)
**Objective**: Practice various indexing and slicing techniques.

**Instructions**:
1. Create a 5×5 matrix with values 1–25.
2. Extract:
   - First row
   - Last column
   - 3×3 submatrix from center
   - Every other row
3. Use boolean indexing to:
   - Find all elements > 15
   - Replace all elements < 10 with 0
4. Use fancy indexing to select rows 0, 2, and 4.

**Expected Output**:
```
5x5 Matrix:
[[ 1  2  3  4  5]
 [ 6  7  8  9 10]
 [11 12 13 14 15]
 [16 17 18 19 20]
 [21 22 23 24 25]]

First row: [1 2 3 4 5]
Last column: [ 5 10 15 20 25]
Center 3x3:
[[ 7  8  9]
 [12 13 14]
 [17 18 19]]

Elements > 15: [16 17 18 19 20 21 22 23 24 25]
```

---

### Task 3: NumPy Image Simulation (Challenge)
**Objective**: Apply NumPy array operations to a real-world-like scenario.

**Instructions**:
Simulate a grayscale image as a NumPy 2D array (values 0–255).
1. Create a random 10×10 "image" array with values 0–255.
2. Perform:
   - Normalize it to 0–1 range: `(pixel - min) / (max - min)`
   - Invert it: `255 - pixel_value`
   - Extract a 5×5 crop from the top-left corner
   - Find the row and column of the brightest pixel
   - Calculate mean brightness, min, max
3. Create a simple gradient image: row values increase left to right.

**Expected Output**:
```
Original Image (10x10):
[[183  42 ...]]

Normalized (0-1):
[[0.717 0.165 ...]]

Inverted:
[[ 72 213 ...]]

Crop (5x5): [first 5 rows, cols]
Brightest Pixel: row=3, col=7 (value=254)
Mean brightness: 127.3
```

---
