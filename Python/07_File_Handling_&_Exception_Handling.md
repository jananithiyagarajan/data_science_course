# File Handling and Exception Handling in Python

## Part 1: File Handling

File handling allows you to create, read, write, and delete files from your Python program.

---

## 1. Opening a File

Use the built-in `open()` function to work with files.

### Syntax
```python
file = open("filename.txt", mode)
```

### File Modes

| Mode  | Description                                        |
| ----- | -------------------------------------------------- |
| `r`   | Read (default) — file must exist                   |
| `w`   | Write — creates file if not exists, overwrites     |
| `a`   | Append — adds to end of file, creates if not exists |
| `x`   | Create — creates new file, error if exists         |
| `r+`  | Read and Write                                     |
| `b`   | Binary mode (add to any mode: `rb`, `wb`)          |

---

## 2. Writing to a File

```python
# Write mode — creates or overwrites
file = open("students.txt", "w")
file.write("Arun\n")
file.write("Priya\n")
file.write("Kumar\n")
file.close()
```

### Append to a File

```python
file = open("students.txt", "a")
file.write("Kavya\n")
file.close()
```

---

## 3. Reading a File

### Read Entire File
```python
file = open("students.txt", "r")
content = file.read()
print(content)
file.close()
```

### Read Line by Line
```python
file = open("students.txt", "r")
for line in file:
    print(line.strip())
file.close()
```

### `readline()` vs `readlines()`
```python
file = open("students.txt", "r")
first_line = file.readline()    # Read one line
print(first_line)

all_lines = file.readlines()    # List of all remaining lines
print(all_lines)
file.close()
```

---

## 4. The `with` Statement (Best Practice)

The `with` statement automatically closes the file, even if an error occurs.

```python
# Write using 'with'
with open("notes.txt", "w") as f:
    f.write("Data Science is awesome!\n")
    f.write("Python is powerful.\n")

# Read using 'with'
with open("notes.txt", "r") as f:
    for line in f:
        print(line.strip())
```

> Always prefer `with open(...)` over manually calling `file.close()`.

---

## 5. File Operations with `os` Module

```python
import os

# Check if file exists
print(os.path.exists("notes.txt"))    # True

# Get file size
print(os.path.getsize("notes.txt"))   # size in bytes

# Rename a file
os.rename("notes.txt", "renamed.txt")

# Delete a file
os.remove("renamed.txt")

# List files in directory
print(os.listdir("."))
```

---

## 6. Working with CSV Files

```python
import csv

# Write CSV
with open("students.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerow(["Name", "Age", "Marks"])  # Header
    writer.writerow(["Arun", 20, 88])
    writer.writerow(["Priya", 21, 95])
    writer.writerow(["Kumar", 19, 72])

# Read CSV
with open("students.csv", "r") as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)
```

---

## Part 2: Exception Handling

An **exception** is an error that occurs during program execution.
Without handling exceptions, the program crashes.

```python
print(10 / 0)       # ZeroDivisionError
int("hello")        # ValueError
open("ghost.txt")   # FileNotFoundError
```

---

## 1. The `try-except` Block

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")
```

---

## 2. Catching Multiple Exceptions

```python
try:
    num = int(input("Enter a number: "))
    result = 100 / num
    print(f"Result: {result}")
except ValueError:
    print("Invalid input! Please enter a number.")
except ZeroDivisionError:
    print("Cannot divide by zero!")
```

---

## 3. The `else` and `finally` Clauses

```python
try:
    num = int(input("Enter a number: "))
    result = 100 / num
except (ValueError, ZeroDivisionError) as e:
    print(f"Error: {e}")
else:
    # Runs only if NO exception occurred
    print(f"Success! Result = {result}")
finally:
    # Always runs — whether exception occurred or not
    print("Execution complete.")
```

### Typical Use Case: File with `finally`

```python
file = None
try:
    file = open("data.txt", "r")
    content = file.read()
    print(content)
except FileNotFoundError:
    print("File not found!")
finally:
    if file:
        file.close()   # Always close the file
```

---

## 4. Raising Exceptions with `raise`

You can intentionally raise exceptions.

```python
def set_age(age):
    if age < 0 or age > 150:
        raise ValueError(f"Invalid age: {age}. Must be between 0 and 150.")
    print(f"Age set to {age}")

try:
    set_age(-5)
except ValueError as e:
    print(f"Error: {e}")
```

---

## 5. Custom Exceptions

Create your own exception classes by inheriting from `Exception`.

```python
class InsufficientBalanceError(Exception):
    def __init__(self, balance, amount):
        self.balance = balance
        self.amount = amount
        super().__init__(f"Cannot withdraw ₹{amount}. Balance is only ₹{balance}.")

class BankAccount:
    def __init__(self, balance):
        self.balance = balance

    def withdraw(self, amount):
        if amount > self.balance:
            raise InsufficientBalanceError(self.balance, amount)
        self.balance -= amount
        return self.balance

account = BankAccount(1000)

try:
    account.withdraw(2000)
except InsufficientBalanceError as e:
    print(f"Transaction Failed: {e}")
```

---

## 6. Common Built-in Exceptions

| Exception             | When it occurs                         |
| --------------------- | -------------------------------------- |
| `ValueError`          | Invalid value for operation            |
| `TypeError`           | Wrong type used                        |
| `ZeroDivisionError`   | Division by zero                       |
| `FileNotFoundError`   | File does not exist                    |
| `IndexError`          | List index out of range                |
| `KeyError`            | Dictionary key not found               |
| `AttributeError`      | Attribute doesn't exist on object      |
| `NameError`           | Variable not defined                   |
| `ImportError`         | Module not found                       |
| `StopIteration`       | Iterator has no more items             |
| `OverflowError`       | Numeric result too large               |
| `MemoryError`         | Out of memory                          |

---

## 7. Exception Hierarchy

All exceptions inherit from `BaseException`. The common base for user-defined exceptions is `Exception`.

```
BaseException
 ├── SystemExit
 ├── KeyboardInterrupt
 └── Exception
      ├── ValueError
      ├── TypeError
      ├── OSError
      │    └── FileNotFoundError
      ├── ArithmeticError
      │    └── ZeroDivisionError
      ├── IndexError
      ├── KeyError
      └── (your custom exceptions)
```

---

## 🎯 Student Tasks – File Handling & Exception Handling

### Task 1: Student Score Recorder (Easy)
**Objective**: Practice basic file write and read operations.

**Instructions**:
1. Ask the user to enter 5 student names and their scores.
2. Write each entry to a file `scores.txt` in format `Name: Score`.
3. Read back the file and print each line.
4. Calculate and print the highest score from the file.

**Expected Output**:
```
--- Scores Saved ---
Arun: 88
Priya: 95
Kumar: 72
Kavya: 80
Ravi: 65

Highest Score: Priya with 95
```

---

### Task 2: Safe Calculator with Exception Handling (Medium)
**Objective**: Handle multiple exception types gracefully.

**Instructions**:
1. Build a calculator that takes two numbers and an operator (+, -, *, /).
2. Handle:
   - `ValueError` if input is not a number
   - `ZeroDivisionError` if dividing by zero
   - Invalid operator → raise a custom `InvalidOperatorError`
3. Use `finally` to print `"Calculation attempt complete."` every time.
4. Log any errors to a file `error_log.txt` with the error message and timestamp.

**Expected Output**:
```
Enter num1: 10
Enter num2: 0
Enter operator: /
Error: Cannot divide by zero!
Calculation attempt complete.
[Error logged to error_log.txt]
```

---

### Task 3: CSV Contact Book (Challenge)
**Objective**: Combine file handling, CSV, and exception handling.

**Instructions**:
1. Build a contact book that stores: `name`, `phone`, `email` in a CSV file `contacts.csv`.
2. Implement:
   - `add_contact(name, phone, email)` — append to CSV
   - `search_contact(name)` — read CSV and find by name
   - `list_all_contacts()` — display all
   - `delete_contact(name)` — remove by name (rewrite file)
3. Handle `FileNotFoundError` if the CSV doesn't exist yet.
4. Validate phone (must be 10 digits) and email (must contain `@`).

**Expected Output**:
```
Contact 'Arun' added successfully.

Searching for 'Priya'...
Name: Priya | Phone: 9876543210 | Email: priya@mail.com

All Contacts (3 found):
1. Arun | 9876543211 | arun@mail.com
2. Priya | 9876543210 | priya@mail.com
3. Kumar | 9876543212 | kumar@mail.com
```

---
