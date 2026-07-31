# Python Data Structures
## Intoduction
Python provides a rich set of built-in and specialized data structures that allow efficient storage, retrieval, transformation, and organization of data.

## Built-in Data Structures
List

Tuples

sets

Dictionaries

## **List []**:

A **list** in Python is a dynamic, mutable, ordered data structure used to store multiple items in a single variable.
It is one of the most commonly used data structures in Python because it is flexible, easy to use, and supports a wide variety of operations.

Python lists:
- Can store **mixed data types**
- Can grow or shrink dynamically
- Are implemented internally as **dynamic arrays** (resizable arrays)

Lists are ideal when you need an ordered, modifiable sequence of elements.

---

**Key Characteristics of Lists**

#### ✔ Ordered
Elements in a list maintain the order in which they are inserted.

```
x = [10, 20, 30]
# index: 0   1   2

#### ✔ Mutable (can be changed)

You can modify elements, append items, delete items, etc.

```
x[1] = 99  # allowed

#### ✔ Allows Duplicate Values

You can store different data types together.

```
x = [10, "hello", 3.14, True]
```

#### ✔ Dynamic Size

Lists grow and shrink automatically as elements are added or removed.

#### ✔ Indexing & Slicing Supported

Allows accessing individual elements or sublists.

```
x = [10, 20, 30, 40, 50]
print(x[2])    # 30
print(x[1:4])  # [20, 30, 40]
```

### Examples of List Usage
#### Creating List

```
numbers = [1, 2, 3, 4]
mixed = [10, "hello", 3.14]
empty = []
```

#### Adding Elements

```
numbers.append(5)        # Add at end
numbers.insert(1, 10)    # Insert 10 at index 1
numbers.extend([6, 7])   # Add multiple items
```

#### Removing Elements

```
numbers.pop()        # Removes last element
numbers.pop(1)       # Removes element at index 1
numbers.remove(3)    # Removes first occurrence of 3
del numbers[0]       # Delete index 0
```
#### Searching

```
if 10 in numbers:
    print("Found")

print(numbers.index(10))   # returns index of 10
```

#### Iteration

```
for n in numbers:
    print(n)
```

#### List Comprehension

```
squares = [x*x for x in range(1, 6)]
```

### Advantages of Lists

✅ 1. Highly Flexible

Can store mixed data types and support multiple operations.

✅ 2. Fast Random Access (O(1))

Indexing is very fast because lists are implemented as dynamic arrays.

✅ 3. Easy to Modify

You can:

add

remove

update

elements easily.

✅ 4. Supports Powerful Built-in Methods

append(), extend(), insert(), remove(), sort(), reverse(), etc.

✅ 5. Dynamic Resizing

The list automatically expands or shrinks as needed.

✅ 6. Great for Iteration

Works smoothly with loops, list comprehensions, and functions like map() or filter().

### Disadvantages of Lists

❌ 1. Slow for Insert/Delete in Middle (O(n))

Because elements must shift to maintain order.

❌ 2. Higher Memory Usage

Since Python lists store references, they consume more memory than arrays in other languages.

❌ 3. Not Type-Safe

Mixed data types can lead to logic errors if not handled carefully.

❌ 4. Searching is O(n)

Linear search is required unless the list is sorted.

❌ 5. Not Efficient for Queue Operations

pop(0) or insert(0, value) is slow because entire list shifts.

### Summary Table

| Feature           | Details                                   |
| ----------------- | ----------------------------------------- |
| Ordered           | Yes                                       |
| Mutable           | Yes                                       |
| Duplicate Allowed | Yes                                       |
| Heterogeneous     | Yes                                       |
| Implementation    | Dynamic Array                             |
| Best Use Cases    | Iteration, random access, general storage |

----

## **Tuple ()**

A tuple in Python is an immutable, ordered data structure used to store multiple items in a single variable.
Unlike lists, tuples cannot be modified after creation, making them ideal for fixed, secure, and read-only data.

Python tuples:

Cannot be changed after creation (immutable)

Can store mixed data types

Maintain order

Are stored more efficiently than lists

Are commonly used for:

Fixed collections of values

Returning multiple values from functions

Using as dictionary keys (because they are hashable)

Tuples are ideal when you want to protect data from modification and ensure stable, predictable structure.

### Key Characteristics of Tuples

### ✔ Ordered

Tuples preserve the order of elements.

```
t = (10, 20, 30)
# index: 0   1   2
```

### ✔ Immutable (cannot be changed)
You cannot modify, add, or delete elements once created.

```
t = (1, 2, 3)
# t[1] = 99  ❌ Not allowed – TypeError
```

### ✔ Allows Duplicate Values
Tuples can contain repeated values.

```
t = (1, 2, 2, 3)
```

### ✔ Supports Heterogeneous Data
Tuples can store different data types.

```
t = (10, "hello", 3.14, True)
```

### ✔ Indexing & Slicing Supported
You can access individual items or sub-tuples.

```
t = (10, 20, 30, 40, 50)
print(t[2])    # 30
print(t[1:4])  # (20, 30, 40)
```

#### ✔ Hashable (if all elements are hashable)
Tuples can be used as keys in dictionaries.

```
coords = {(10, 20): "Location A"}
```

### Examples of Tuples Usage
#### **Creating Tuples**

```
t1 = (1, 2, 3)
t2 = ("apple", 10, 3.14)
t3 = ()            # empty tuple
t4 = (5,)          # single-element tuple (comma required)
```

#### **Accessing Elements**

```
t = (10, 20, 30)
print(t[0])   # 10
print(t[-1])  # 30
```

#### **Tuple Unpaking**
Useful for returning multiple values.

```
point = (5, 10)
x, y = point
```

#### **Iteration**

```
for item in (1, 2, 3):
    print(item)
```

#### **Membership Test**

```
t = (1, 2, 3)
print(2 in t)   # True
```

#### Concatenation & Repetition

```
a = (1, 2)
b = (3, 4)
print(a + b)     # (1,2,3,4)

print(a * 3)     # (1,2,1,2,1,2)
```

#### Using Tuples as Dictionary Keys

```
location = {}
location[(10.5, 20.7)] = "Home"
```

### **Advantages of Tuples**

✅ 1. Faster than Lists

Since they are immutable, Python can optimize memory and access speed.

✅ 2. Immutable (Data Safety)

Prevents accidental modification.

Useful in:

Multi-threaded programs

Configuration values

Constant datasets

✅ 3. Hashable

Can be used as keys in dictionaries and elements of sets.

✅ 4. Lower Memory Usage

Tuples consume less memory than lists.

✅ 5. Ideal for Fixed Data

Such as coordinates, RGB values, dates, database records, etc.

### **Disadvantages of Tuples**

❌ 1. Immutable

You cannot:

Add elements

Remove elements

Modify elements

This limits flexibility.

❌ 2. Not Suitable for Dynamic Data

If the data changes frequently, use a list instead.

❌ 3. Fewer Built-in Methods

Tuples only support:

count()

index()

Unlike lists (append, remove, insert, etc.).

❌ 4. Harder to Modify Complex Data Structures

If you need to update values often, tuple is not ideal.

### Summary Table

| Feature           | Details                                     |
| ----------------- | ------------------------------------------- |
| Ordered           | Yes                                         |
| Mutable           | ❌ No                                        |
| Duplicate Allowed | Yes                                         |
| Heterogeneous     | Yes                                         |
| Implementation    | Immutable sequence                          |
| Best Use Cases    | Fixed data, keys in dict, efficient reading |

## **set {}**

A set in Python is an unordered, mutable, and unique-element data structure used to store a collection of distinct items.

Python sets:

Store only unique values

Are unordered

Are mutable

Provide O(1) average time complexity for add, remove, check operations

Support powerful set operations: union, intersection, difference, symmetric difference

Sets are ideal when you need fast membership testing, duplicate removal, or mathematical set operations.

### Key Characteristics of Sets

✔ Unordered

Set elements do not maintain insertion order.
You cannot rely on indexing.

```
s = {10, 20, 30}
# No index positions
```

✔ Contains Only Unique Elements

Duplicates are automatically removed.

```
s = {1, 2, 2, 3}
print(s)   # {1, 2, 3}
```

✔ Mutable (can be changed)

You can add or remove elements.

```
s = {1, 2, 3}
s.add(4)
s.remove(2)
```

✔ Only Hashable Elements Allowed

You can store:

int

float

string

tuple

But not:

lists

dicts

sets

```
s = {1, "hello", (2, 3)}   # valid
# s = {[1, 2], 3}         # ❌ TypeError
```

✔ No Indexing or Slicing

This will give an error:

```
s = {1, 2, 3}
# s[0] ❌ Not allowed
```

✔ Fast Membership Testing

Checking membership is extremely fast (O(1)).

```
if 10 in {10, 20, 30}:
    print("Exists")
```

### Examples of Set Usage

**Creating Sets**

```
s1 = {1, 2, 3}
s2 = set([4, 5, 6])
s3 = set()        # empty set
```

**Adding Elements**

```
s = {1, 2}
s.add(3)
s.update([4, 5, 6])    # Add multiple items
```

**Removing Elements**

```
s = {1, 2, 3}
s.remove(2)   # removes 2, errors if not found
s.discard(5)  # no error if 5 not found
s.pop()       # removes a random element
s.clear()     # removes all elements
```

**Checking Membership**

```
s = {1, 2, 3}
print(2 in s)   # True
print(4 in s)   # False
```

### **Advantages of Sets**

✅ 1. Fast Membership Testing

Checking x in set is O(1) on average.

✅ 2. No Duplicates

Automatically removes duplicate values.

✅ 3. Efficient Mathematical Operations

Union, intersection, etc., are highly optimized.

✅ 4. Good for Data Cleaning

Easily remove duplicates from lists:

```
unique_list = list(set([1,2,2,3,3,4]))
```

✅ 5. Mutable but Elements Must Be Hashable

Allows adding/removing items efficiently.

### **Disadvantages of Sets**

❌ 1. Unordered

You cannot rely on element positions; indexing is not supported.

❌ 2. Cannot Store Unhashable Items

Lists, dicts, and sets cannot be stored.

❌ 3. No Duplicate Allowed

Sometimes you want duplicates (e.g., counting).

❌ 4. Random Element Removal with pop()

Not ideal for predictable removals.

❌ 5. Not Suitable for Ordered Tasks

If you need order → use list
If you need sorted order → use sorted(), list, or OrderedSet (3rd-party)

### Summary Table

| Feature            | Details                                                  |
| ------------------ | -------------------------------------------------------- |
| Ordered            | ❌ No                                                     |
| Mutable            | ✔ Yes                                                    |
| Duplicates Allowed | ❌ No                                                     |
| Heterogeneous      | ✔ Yes                                                    |
| Implementation     | Hash Table                                               |
| Best Use Cases     | Membership testing, removing duplicates, math operations |

---

## **Dictionary {}**

A dictionary in Python is a mutable, unordered (insertion-ordered since Python 3.7+) collection of **key-value pairs**.
Each key must be unique and immutable (str, int, tuple); values can be anything.

Python dictionaries:
- Store data as **key: value** pairs
- Are very fast for lookup (O(1))
- Are ideal for representing real-world objects and structured data

### Key Characteristics of Dictionaries

✔ **Key-Value Pairs**
Every entry has a key (unique) and a value.
```python
student = {"name": "Arun", "age": 22, "grade": "A"}
```

✔ **Ordered (Python 3.7+)**
Dictionaries maintain insertion order.

✔ **Mutable**
You can add, update, or delete key-value pairs.

✔ **No Duplicate Keys**
If a duplicate key is assigned, the value gets overwritten.
```python
d = {"x": 1, "x": 2}
print(d)  # {'x': 2}
```

✔ **Keys Must Be Hashable**
Keys can be: `str`, `int`, `float`, `tuple` — but not `list` or `dict`.

### Examples of Dictionary Usage

#### Creating a Dictionary
```python
# Empty dictionary
empty = {}
empty = dict()

# With data
person = {"name": "Priya", "age": 25, "city": "Chennai"}

# From keyword arguments
car = dict(brand="Toyota", model="Camry", year=2022)
```

#### Accessing Values
```python
student = {"name": "Kumar", "age": 21}

print(student["name"])          # Kumar
print(student.get("age"))       # 21
print(student.get("marks", 0))  # 0 (default if key missing)
```

#### Adding & Updating
```python
student["email"] = "kumar@example.com"   # Add new key
student["age"] = 22                      # Update existing key
student.update({"city": "Coimbatore", "age": 23})  # Multiple updates
```

#### Removing Elements
```python
d = {"a": 1, "b": 2, "c": 3}

del d["a"]           # Delete by key
d.pop("b")           # Remove and return value
d.popitem()          # Remove last inserted pair
d.clear()            # Remove all items
```

#### Iterating Over a Dictionary
```python
info = {"name": "Bhuvi", "age": 20, "city": "Madurai"}

# Keys
for key in info:
    print(key)

# Values
for value in info.values():
    print(value)

# Key-Value pairs
for key, value in info.items():
    print(f"{key}: {value}")
```

#### Dictionary Comprehension
```python
squares = {x: x**2 for x in range(1, 6)}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

### Common Dictionary Methods

| Method             | Description                        | Example                         |
| ------------------ | ---------------------------------- | ------------------------------- |
| `d.keys()`         | Returns all keys                   | `dict_keys(['name', 'age'])`    |
| `d.values()`       | Returns all values                 | `dict_values(['Arun', 22])`     |
| `d.items()`        | Returns key-value pairs            | `dict_items([('name','Arun')])` |
| `d.get(k, def)`    | Get value or default               | `d.get('x', 0)`                 |
| `d.update(dict2)`  | Merge another dict in              | `d.update({'city': 'Chennai'})` |
| `d.pop(key)`       | Remove and return value            | `d.pop('age')`                  |
| `d.popitem()`      | Remove last item                   | `d.popitem()`                   |
| `d.clear()`        | Remove all items                   | `d.clear()`                     |
| `key in d`         | Check if key exists                | `'name' in d → True`            |
| `d.copy()`         | Shallow copy                       | `d2 = d.copy()`                 |
| `d.setdefault(k)`  | Set default if key missing         | `d.setdefault('score', 0)`      |

### Advantages of Dictionaries

✅ 1. **Fast Lookup (O(1))**
Finding a value by key is extremely fast.

✅ 2. **Intuitive Data Representation**
Model real-world objects naturally (e.g., a student record).

✅ 3. **Flexible Values**
Values can be any type: lists, tuples, other dicts, functions.

✅ 4. **Dynamic**
Easily add, update, and remove entries.

### Disadvantages of Dictionaries

❌ 1. **No Duplicate Keys**
Only one value per key — duplicate keys overwrite.

❌ 2. **More Memory than Lists**
Due to the hash table implementation.

❌ 3. **Keys Must Be Immutable**
Cannot use lists or other dicts as keys.

### Summary Table

| Feature              | Details                              |
| -------------------- | ------------------------------------ |
| Ordered (3.7+)       | ✔ Yes                               |
| Mutable              | ✔ Yes                               |
| Duplicate Keys       | ❌ No                                |
| Heterogeneous Values | ✔ Yes                               |
| Implementation       | Hash Table                          |
| Best Use Cases       | Lookup tables, configs, JSON-like data |

---

## 🎯 Student Tasks – Module 03: Data Structures

### Task 1: Student Record Manager (Easy)
**Objective**: Practice dictionary creation and access.

**Instructions**:
1. Create a dictionary to store a student's: name, age, marks (as a list of 5 subjects), and city.
2. Print each field with a label.
3. Calculate and print the average marks.
4. Add a new field `"passed"` — set to `True` if average ≥ 50, else `False`.

**Expected Output**:
```
Name: Kavya
Age: 20
Marks: [85, 90, 78, 92, 88]
Average: 86.6
City: Coimbatore
Passed: True
```

---

### Task 2: Shopping Cart Simulator (Medium)
**Objective**: Combine lists, dictionaries, and loops.

**Instructions**:
1. Create a list of dictionaries where each dict has: `name`, `price`, `quantity`.
2. Calculate the total cost for each item (`price * quantity`).
3. Calculate the grand total.
4. Print a formatted bill.
5. Find the most expensive item.

**Expected Output**:
```
--- Shopping Bill ---
Rice      : ₹50 x 2 = ₹100
Milk      : ₹30 x 3 = ₹90
Bread     : ₹25 x 1 = ₹25

Grand Total: ₹215
Most Expensive: Rice (₹50)
```

---

### Task 3: Word Frequency Counter (Challenge)
**Objective**: Use dictionaries for data analysis.

**Instructions**:
1. Take a sentence as input from the user.
2. Split it into words and convert all to lowercase.
3. Count the frequency of each word using a dictionary.
4. Print the words sorted by frequency (highest first).
5. Print the most repeated word.
6. Remove common stop words: `["the", "a", "an", "is", "in", "of", "and"]`.

**Expected Output**:
```
Enter sentence: The cat sat on the mat and the cat
Word Frequencies (stop words removed):
  cat   : 2
  sat   : 1
  mat   : 1
  on    : 1

Most repeated word: 'cat' (2 times)
```

---






