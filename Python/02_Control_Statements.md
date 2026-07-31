## 1. Control Statements
A control statement in Python is used to control the flow of execution of a program —
that means deciding which part of the code should run and when.

### Types of Control Statements
<p align="center">
if

else

elif
</p>

### 1.1 if Statement:

The if statement is a decision-making statement.

It allows your program to execute a block of code only if a certain condition is true.

#### Syntax
<p align="center">
<img width="369" height="103" alt="image" src="https://github.com/user-attachments/assets/8519d604-3e40-4861-ae78-0cff6c9ebf9f" />
</p>


#### Example
```
age = 20

if age >= 18:
    print("You are eligible to vote.")
```
### 1.2 if...else Statement
Used when you want to do one thing if the condition is true, and something else if it’s false.

#### Syntax

<p align="center">
<img width="433" height="154" alt="image" src="https://github.com/user-attachments/assets/b67754c8-2306-4e06-bcd4-04521ef510c5" />
</p>

#### Example
```
age = 16

if age >= 18:
    print("Eligible to vote")
else:
    print("Not eligible to vote")
```
### 1.3 elif Statement
Used when you have multiple conditions to check.

#### Syntax
<p align="center">
<img width="623" height="254" alt="image" src="https://github.com/user-attachments/assets/5f613ec7-debe-4512-abaf-6af45aeee885" />
</p>

#### Example

```
marks = 85

if marks >= 90:
    print("Grade A")
elif marks >= 75:
    print("Grade B")
elif marks >= 60:
    print("Grade C")
else:
    print("Fail")
```
### 1.4 Nested `if`

A `nested if` statement means an if statement **inside another** if statement.

It allows you to check multiple conditions one inside another — useful for complex decision-making.

#### Syntax
<p align="center">
<img width="654" height="251" alt="image" src="https://github.com/user-attachments/assets/2d2cc0a9-b243-41d0-b01a-ef26bfc6c508" />
</p>

#### Example
```
age = 20
citizen = True

if citizen:
    if age >= 18:
        print("You are eligible to vote.")
    else:
        print("You must be 18 or older to vote.")
else:
    print("You must be a citizen to vote.")
```
### 1.5 Indentation is Important
```
if True:
print("Hello")   # ❌ Error — no indentation

if True:
    print("Hello")  # ✅ Correct
```
### 1.6 Summary

| **Type**           | **Description**                    | **Example**              |
| ------------------ | ---------------------------------- | ------------------------ |
| `if`               | Executes code if condition is true | `if x > 0:`              |
| `if...else`        | Either one block executes          | `if x>0: else:`          |
| `if...elif...else` | Multiple conditions                | `if x>0 elif x==0 else:` |
| Nested `if`        | One `if` inside another            | `if x>0: if x%2==0:`     |

---

## 🎯 Student Tasks – Module 02: Control Flow (Conditional Statements)

### Task 1: Age Category Checker (Easy)
**Objective**: Practice if/elif/else with user input.

**Instructions**:
1. Ask the user to enter their age.
2. Print the appropriate category:
   - 0–12: `"Child"`
   - 13–17: `"Teenager"`
   - 18–59: `"Adult"`
   - 60+: `"Senior"`

**Expected Output**:
```
Enter your age: 15
You are a Teenager.
```

---

### Task 2: Grade & Result System (Medium)
**Objective**: Practice nested if and elif chains.

**Instructions**:
1. Ask the user to enter marks (0–100).
2. Print the grade and result:
   - 90–100 → Grade A+, Pass
   - 75–89  → Grade A, Pass
   - 60–74  → Grade B, Pass
   - 45–59  → Grade C, Pass
   - Below 45 → Grade F, Fail
3. If marks are below 0 or above 100, print `"Invalid marks entered."`.

**Expected Output**:
```
Enter marks: 82
Grade: A  |  Result: Pass
```

---

### Task 3: Login System Simulator (Challenge)
**Objective**: Combine comparison and logical operators.

**Instructions**:
1. Store a correct username and password in variables.
2. Ask the user to input username and password.
3. Use `and` to check both together.
4. If username is correct but password is wrong → `"Wrong password"`.
5. If username is wrong → `"User not found"`.
6. If both correct → `"Welcome, [username]! Login successful."`.
7. Allow 3 attempts (hint: use a counter variable with if conditions).

**Expected Output**:
```
Enter username: admin
Enter password: wrong123
Wrong password. Attempts left: 2
```

---
