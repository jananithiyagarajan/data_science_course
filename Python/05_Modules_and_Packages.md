# Python Modules and Packages

## What is a Module?

A **module** is a Python file (`.py`) that contains functions, classes, and variables that you can reuse in other programs.

Instead of rewriting code, you can import and use existing modules.

```python
# math_utils.py (a module)
def add(a, b):
    return a + b

def multiply(a, b):
    return a * b
```

---

## 1. Importing Modules

### 1.1 `import` Statement

Imports the entire module. Access items using `module.item`.

```python
import math

print(math.pi)        # 3.141592653589793
print(math.sqrt(16))  # 4.0
print(math.ceil(4.3)) # 5
```

### 1.2 `from … import`

Import specific items directly — no need for the module prefix.

```python
from math import sqrt, pi

print(sqrt(25))  # 5.0
print(pi)        # 3.141592653589793
```

### 1.3 `import … as` (Alias)

Give a module or function a shorter name.

```python
import numpy as np
import pandas as pd
from datetime import datetime as dt

print(dt.now())
```

### 1.4 Import Everything (`*`)

Import all items from a module (generally not recommended for large modules).

```python
from math import *
print(sin(0))    # 0.0
print(cos(0))    # 1.0
```

### Summary Table

| Syntax                    | Usage                                | Example                        |
| ------------------------- | ------------------------------------ | ------------------------------ |
| `import module`           | Import whole module                  | `import math`                  |
| `from module import name` | Import specific item                 | `from math import sqrt`        |
| `import module as alias`  | Import with short name               | `import numpy as np`           |
| `from module import *`    | Import all (avoid in large projects) | `from math import *`           |

---

## 2. Standard Libraries

Python comes with a rich set of built-in modules — no installation needed.

### 2.1 `math` Module

```python
import math

print(math.sqrt(144))     # 12.0
print(math.pow(2, 10))    # 1024.0
print(math.floor(3.9))    # 3
print(math.ceil(3.1))     # 4
print(math.log(100, 10))  # 2.0
print(math.pi)            # 3.14159...
print(math.e)             # 2.71828...
```

### 2.2 `random` Module

```python
import random

print(random.random())          # Random float between 0 and 1
print(random.randint(1, 100))   # Random int between 1 and 100
print(random.choice(['a', 'b', 'c']))  # Random item from list

numbers = [1, 2, 3, 4, 5]
random.shuffle(numbers)
print(numbers)

sample = random.sample(range(100), 5)  # 5 unique random numbers
print(sample)
```

### 2.3 `os` Module

Interact with the operating system.

```python
import os

print(os.getcwd())         # Current working directory
print(os.listdir('.'))     # List files in current directory
os.mkdir('new_folder')     # Create a new folder
os.rename('old.txt', 'new.txt')  # Rename file
os.remove('file.txt')      # Delete a file
print(os.path.exists('data.csv'))  # Check if file exists
print(os.path.join('folder', 'file.txt'))  # Safe path joining
```

### 2.4 `sys` Module

```python
import sys

print(sys.version)         # Python version
print(sys.platform)        # OS platform
print(sys.argv)            # Command-line arguments
sys.exit(0)                # Exit the program (use with caution)
```

### 2.5 `datetime` Module

```python
from datetime import datetime, date, timedelta

now = datetime.now()
print(now)                            # 2025-07-31 10:30:00.123456
print(now.strftime("%d/%m/%Y %H:%M")) # Formatted date

today = date.today()
print(today)                          # 2025-07-31

# Date arithmetic
tomorrow = today + timedelta(days=1)
print(tomorrow)
```

### 2.6 `collections` Module

```python
from collections import Counter, defaultdict, OrderedDict

# Counter: count occurrences
words = ["apple", "banana", "apple", "cherry", "banana", "apple"]
count = Counter(words)
print(count)              # Counter({'apple': 3, 'banana': 2, ...})
print(count.most_common(2))  # [('apple', 3), ('banana', 2)]

# defaultdict: dict with default value
dd = defaultdict(int)
dd["score"] += 10
print(dd["score"])        # 10
print(dd["new_key"])      # 0 (no KeyError!)
```

---

## 3. Creating Custom Modules

### Step 1: Create a Python file (`greet.py`)

```python
# greet.py
def say_hello(name):
    return f"Hello, {name}!"

def say_goodbye(name):
    return f"Goodbye, {name}!"

PI = 3.14159
```

### Step 2: Import and use it

```python
# main.py (in the same folder)
import greet

print(greet.say_hello("Arun"))     # Hello, Arun!
print(greet.say_goodbye("Priya"))  # Goodbye, Priya!
print(greet.PI)                    # 3.14159
```

### Using `from … import`

```python
from greet import say_hello

print(say_hello("Kumar"))  # Hello, Kumar!
```

### The `__name__` Variable

Use this to prevent code from running when the module is imported.

```python
# calculator.py
def add(a, b):
    return a + b

if __name__ == "__main__":
    # This only runs when calculator.py is executed directly
    print(add(5, 3))
```

---

## 4. Packages

A **package** is a folder containing multiple modules, with an `__init__.py` file.

```
my_package/
    __init__.py
    math_utils.py
    string_utils.py
```

### Creating a Package

```python
# my_package/__init__.py (can be empty or have imports)
from .math_utils import add, multiply

# my_package/math_utils.py
def add(a, b):
    return a + b

def multiply(a, b):
    return a * b
```

### Using the Package

```python
from my_package import add
print(add(3, 4))   # 7

# or
import my_package.math_utils as mu
print(mu.multiply(5, 6))  # 30
```

---

## 5. Virtual Environments

A **virtual environment** is an isolated Python workspace that has its own packages, separate from the global Python installation.

### Why Use Virtual Environments?

- Different projects may need different versions of the same library.
- Keeps your global Python installation clean.
- Makes projects reproducible and shareable.

### 5.1 Creating a Virtual Environment

```bash
# Create
python -m venv myenv

# Activate (Linux/Mac)
source myenv/bin/activate

# Activate (Windows)
myenv\Scripts\activate

# Deactivate
deactivate
```

### 5.2 Installing Packages with `pip`

```bash
# Install a package
pip install pandas

# Install a specific version
pip install numpy==1.24.0

# List all installed packages
pip list

# Save dependencies to a file
pip freeze > requirements.txt

# Install from requirements file
pip install -r requirements.txt

# Uninstall a package
pip uninstall pandas
```

### 5.3 `requirements.txt` Example

```
numpy==1.26.4
pandas==2.2.0
matplotlib==3.8.2
scikit-learn==1.4.0
```

### 5.4 Best Practices

- Always create a virtual environment per project.
- Never commit the `myenv/` folder to Git — add it to `.gitignore`.
- Always commit `requirements.txt` so others can recreate the environment.

---

## Module vs Package vs Library

| Term        | Description                                  | Example                   |
| ----------- | -------------------------------------------- | ------------------------- |
| **Module**  | A single `.py` file                          | `math`, `os`              |
| **Package** | A folder of modules with `__init__.py`       | `numpy`, `pandas`         |
| **Library** | A collection of packages/modules             | NumPy, Pandas, Matplotlib |
| **Framework**| A structured set of libraries with rules    | Django, FastAPI           |

---

## 🎯 Student Tasks – Module 04: Python Modules and Packages

### Task 1: Standard Library Explorer (Easy)
**Objective**: Practice using Python's built-in standard libraries.

**Instructions**:
1. Use the `math` module to:
   - Calculate the square root of 144
   - Find the value of π
   - Compute `2^10` using `math.pow()`
2. Use the `random` module to:
   - Generate a random number between 1 and 50
   - Pick a random name from a list of 5 names
3. Use the `datetime` module to print today's date in the format `DD-MM-YYYY`.

**Expected Output**:
```
sqrt(144) = 12.0
pi = 3.141592653589793
2^10 = 1024.0

Random number: 37
Random name: Priya

Today's date: 31-07-2025
```

---

### Task 2: Build Your Own Module (Medium)
**Objective**: Create and import a custom module.

**Instructions**:
1. Create a file called `student_utils.py` with these functions:
   - `get_grade(marks)` → returns letter grade (A+/A/B/C/F)
   - `calculate_average(marks_list)` → returns average
   - `is_passed(marks_list)` → returns True if average ≥ 50
2. In a separate file `main.py`, import and use all three functions.
3. Test with marks: `[88, 76, 92, 65, 80]`.

**Expected Output**:
```
Average: 80.2
Grade: A
Passed: True
```

---

### Task 3: Virtual Environment Setup & Package Manager (Challenge)
**Objective**: Set up a real virtual environment and manage packages.

**Instructions**:
1. Create a virtual environment called `ds_env`.
2. Activate it.
3. Install `pandas` and `numpy`.
4. Run `pip freeze > requirements.txt`.
5. Write a Python script that:
   - Imports both libraries
   - Creates a NumPy array `[1, 2, 3, 4, 5]` and prints its mean
   - Creates a Pandas DataFrame with 3 students and their scores
   - Prints the DataFrame
6. Deactivate the environment.

**Expected Output**:
```
NumPy Mean: 3.0

   Name  Score
0  Arun     85
1  Priya    92
2  Kumar    78
```

---
