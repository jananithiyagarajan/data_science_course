# Function

A function in Python is a block of reusable code that performs a specific task.

Instead of writing the same code again and again, you can put it inside a function and call it whenever needed.

# Use of Functions

Code reusability (write once, use many times).

Organized & clean code.

Easy debugging.

Avoid repetition.

## Syntax

<p align="center">
<img width="1009" height="164" alt="Screenshot 2025-09-06 123321 1" src="https://github.com/user-attachments/assets/7e08980c-93ff-4986-90b6-bf32f14de3fb" />
</p>

where

def → keyword to define a function.

function_name → name of the function (should be meaningful).

parameters → (optional) values passed into the function.

return → (optional) sends a value back to the caller.

## Example

<p align="center">
<img width="496" height="154" alt="Screenshot 2025-09-06 122849" src="https://github.com/user-attachments/assets/d3d691c4-43b5-49c2-9b38-08e294de5c20" />
</p>

<p align="center">
<img width="864" height="297" alt="image 3" src="https://github.com/user-attachments/assets/74ab6396-5cf7-41a0-845e-fdb3a136abc6" />
</p>

## Arguments

Information can be passed into functions as arguments.

Arguments are specified after the function name, inside the parentheses. You can add as many arguments as you want, just separate them with a comma.

### Example

```
def my_function(fname):
  print(fname + " Refsnes")

my_function("Emil")
my_function("Tobias")
my_function("Linus")
```

## Function without parameters
<p align="center">
<img width="606" height="188" alt="Screenshot 2025-09-06 122849 1" src="https://github.com/user-attachments/assets/3633d303-f4fe-4d26-9505-6eb24429a5ce" />
</p>

 ## Function with parameters
<p align="center">
<img width="660" height="260" alt="Screenshot 2025-09-06 125039 1" src="https://github.com/user-attachments/assets/5950775d-17f8-42bc-bda1-03d500459840" />
</p>

## Function with return value
<p align="center">
<img width="834" height="383" alt="Screenshot 2025-09-06 130927 1" src="https://github.com/user-attachments/assets/f2513385-781b-4b53-97d8-4879524374f0" />
</p>

## Default parameter value
<p align="center">
<img width="678" height="326" alt="Screenshot 2025-09-06 131143 1" src="https://github.com/user-attachments/assets/df9c8618-93e4-4fd0-98ab-862503075652" />
</p>

## Decorators

Decorators let you add extra behavior to a function, without changing the function's code.

A decorator is a function that takes another function as input and returns a new function.

Define the decorator first, then apply it with @decorator_name above the function.

### Example

```
def changecase(func):
  def myinner():
    return func().upper()
  return myinner

@changecase
def myfunction():
  return "Hello Sally"

print(myfunction())
```




# Lambda Function (Anonymous Function)

A lambda function in Python is a small, anonymous (nameless) function defined using the lambda keyword.
It is typically used for short, simple operations when defining a full function using def is unnecessary.

## Syntax

<p align="center">
  <img width="323" height="42" alt="image" src="https://github.com/user-attachments/assets/46294fc8-42d8-4d49-b998-b039811307c2" />

</p>

It can have any number of arguments.

It can have only one expression (no multiple statements).

The expression is evaluated and returned automatically.


### Normal Function
```
def add(a, b):
    return a + b
```

### Lambda Version

```
add = lambda a, b: a + b
```

### Usage

```
print(add(5, 3))  # Output: 8
```

## When to Use Lambda Functions

**1. map()**

**2. filter()**

**3. sorted()**

## Example

### Square of a number

```
square = lambda x: x * x
print(square(5))
```
### Add two numbers

```
add = lambda a, b: a + b
print(add(10, 20)) 
```
### Using lambda with `map()`

```
nums = [1, 2, 3, 4]
squares = list(map(lambda x: x*x, nums))
print(squares)  # Output: [1, 4, 9, 16]
```

### Using lambda with `filter()`

```python
nums = [1, 2, 3, 4, 5, 6]
even = list(filter(lambda x: x % 2 == 0, nums))
print(even)  # Output: [2, 4, 6]
```

### Using lambda with `sorted()`

```python
students = [("Arun", 85), ("Priya", 92), ("Kumar", 78)]
sorted_students = sorted(students, key=lambda s: s[1], reverse=True)
print(sorted_students)
# [('Priya', 92), ('Arun', 85), ('Kumar', 78)]
```

---

## Return Values

A function can send back data to the caller using `return`.

### Single Return Value
```python
def square(n):
    return n * n

result = square(5)
print(result)   # 25
```

### Multiple Return Values
Python functions can return multiple values as a tuple.
```python
def min_max(numbers):
    return min(numbers), max(numbers)

low, high = min_max([3, 1, 7, 2, 9])
print(low, high)   # 1 9
```

### Early Return
```python
def divide(a, b):
    if b == 0:
        return None
    return a / b

result = divide(10, 0)
if result is None:
    print("Cannot divide by zero")
```

---

## Scope (Variable Scope)

Scope defines where a variable is accessible. Python follows the **LEGB rule**:

| Scope           | Description                                      |
| --------------- | ------------------------------------------------ |
| **L**ocal       | Variables defined inside a function              |
| **E**nclosing   | Variables in enclosing (outer) functions         |
| **G**lobal      | Variables defined at the top level of the script |
| **B**uilt-in    | Python built-ins like `print`, `len`, `type`     |

### Local vs Global
```python
city = "Chennai"   # global

def show():
    name = "Arun"  # local
    print(name, city)  # can access both

show()
# print(name)  # ❌ NameError - local variable not accessible outside
```

### The `global` Keyword
Use `global` to modify a global variable from inside a function.
```python
count = 0

def increment():
    global count
    count += 1

increment()
increment()
print(count)   # 2
```

### The `nonlocal` Keyword
Use `nonlocal` to modify a variable from an enclosing (not global) function.
```python
def outer():
    x = 10
    def inner():
        nonlocal x
        x += 5
    inner()
    print(x)   # 15

outer()
```

---

## 🎯 Student Tasks – Module 03 & 04: Functions & Modules

### Task 1: Temperature Converter (Easy)
**Objective**: Practice defining and calling functions with return values.

**Instructions**:
1. Write `celsius_to_fahrenheit(c)` — Formula: `F = (C × 9/5) + 32`
2. Write `fahrenheit_to_celsius(f)` for the reverse.
3. Ask the user for a temperature and unit (`C` or `F`).
4. Call the right function and print the converted result.

**Expected Output**:
```
Enter temperature: 100
Enter unit (C/F): C
100°C = 212.0°F
```

---

### Task 2: Lambda & Higher-Order Functions (Medium)
**Objective**: Practice lambda with `map`, `filter`, `sorted`.

**Instructions**:
1. Use `map()` with lambda to get the square of each number in a list.
2. Use `filter()` with lambda to keep only numbers divisible by 3.
3. Use `sorted()` with lambda to sort `(name, score)` tuples by score descending.

**Expected Output**:
```
Squares: [16, 81, 9, 144, 49, 36, 225]
Divisible by 3: [9, 3, 12, 6, 15]
Sorted by score: [('Priya', 95), ('Arun', 88), ('Kumar', 72)]
```

---

### Task 3: Scope Explorer (Challenge)
**Objective**: Demonstrate understanding of local, global, and enclosing scope.

**Instructions**:
1. Create a global variable `total = 0`.
2. Write `add_to_total(amount)` that uses `global` to update it.
3. Write a closure `make_multiplier(factor)` that returns an inner function.
4. Demonstrate `nonlocal` with a counter in nested functions.

**Expected Output**:
```
Total after 50: 50
Total after 30: 80
double(7) = 14
counter = 3
```

---

















