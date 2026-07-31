---
## 1.Looping Statements
A looping statement in Python is a programming structure that allows you to repeat a block of code multiple times —
either a fixed number of times or until a specific condition becomes False.

### Types
**1. `for` loop**

**2. `while` loop**

**3. Nested loop**

#### 1.`for` loop (the most Pythonic loop)
for iterates over any iterable (list, tuple, string, dict, set, file, generator, etc.).

#### Syntax
<p style="center">
<img width="351" height="98" alt="image" src="https://github.com/user-attachments/assets/b5eadfaf-e7e8-4eba-aa50-b8b20e8579da" />
</p>

#### Examples
```
# Example-1
# Write a program to print numbers from 1 to 20 using a for loop.
for i in range(1,20+1):
    print(i)
```

```
# Example-2
# Print all even numbers between 1 and 50.
for i in range(1,50+1):
    if i%2==0:
        print(i)
```

```
# Example-3
# Print all odd numbers between 1 and 30.
for i in range(1,30+1):
    if i%2==1:
        print(i)
```
```
# Example-4
# Find the sum of numbers from 1 to 10.
total=0
for i in range(10+1):
    total=total+i
    
print(total)
```

```
# Example-5
# Print the multiplication table of 5 using a for loop.
# Method-1
table=5
for i in range(10+1):
    product=i*table
    print(f"{i}*{table}={product}")

# Method-2

# Method-3
table=int(input("Enter the table"))
for i in range(10+1):
    print(f"{i}*{table}={i*table}")

```

```
# Example-6
# Print the square of each number from 1 to 10.
for i in range(1,10+1):
    square=i ** 2
    print(f"the square of {i} is {square}")
```

```
# Example-7
# Take a string (e.g., "Python") and print each character on a new line.
text="python"
for i in text:
    print(i)
```

```
# Example-8
# Given a list fruits = ["apple", "banana", "cherry"], print each fruit.
fruits = ["apple", "banana", "cherry"]
for i in fruits:
    print(i)
```

```
# Example-9
# Print numbers from 10 down to 1.
num=10
for i in range(num,0,-1):
    print(i)
```

```
# Example-10
# Count how many vowels are in the string "hello world" using a for loop.
count=0
word="hello world"
vowels="aeiou"
#vowels=['a','e','i','o','u']
for i in word:
    if i in vowels:
        count=count+1

print(count)

```
## while loop
### 2.What is a while loop?
A while loop is a **control flow statement** in Python that repeatedly executes a block of code as long as a **given condition is true**.

“Keep doing this until the condition becomes false.”

### Basic Syntax
<p align="center">
    <img width="177" height="58" alt="image" src="https://github.com/user-attachments/assets/a1e43591-858b-4221-a3e0-6b6a2e94e318" />
</p>

**Example-1**
Counting Numbers
```
count = 1
while count <= 5:
    print(count)
    count = count + 1
```
### **Important: Avoid Infinite Loops!**
If you **forget to update** the condition, the loop may **never stop**.
<p align="center">
    <img width="314" height="114" alt="image" src="https://github.com/user-attachments/assets/8bbd2235-2fbb-4b9e-9213-6122daa7e0c6" />
</p>


**example-2**
While Loop with User Input
```
password = ""
while password != "bhuvi123":
    password = input("Enter password: ")

print("Access granted!")
```
## Flow Control Statements
continue

break

pass

### continue

Skips the remaining code for the current iteration and goes to the next one.

**Example**

```
i = 0
while i < 5:
    i += 1
    if i == 3:
        continue
    print(i)
```

### Break

Ends (breaks) the loop immediately, even if the loop condition is still true.

**Example**

```
i = 1
while i <= 10:
    if i == 5:
        break
    print(i)
    i += 1
```

### Pass
Does nothing — it’s used when a statement is syntactically required, but you don’t want to run any code there.

**Example**
```
for i in range(5):
    if i == 3:
        pass  # do nothing
    else:
        print(i)
```

### Summary Table

| Keyword    | Meaning                  | Effect on Loop          |
| ---------- | ------------------------ | ----------------------- |
| `break`    | Exit the loop completely | Stops the loop          |
| `continue` | Skip current iteration   | Goes to next iteration  |
| `pass`     | Do nothing (placeholder) | Loop continues normally |

## `while` loop Interview Questions

#### Q1-Write a program using a while loop to print numbers from 1 to 10.
```
num=1
while num<=10:
    print(num)
    num+=1
```

#### Q2-Use a while loop to find the sum of the first N natural numbers (for example, N = 5 → 1+2+3+4+5).

```
n=1
result=0
while n<=5:
    result=result+n
    n=n+1

print(result)
```

#### Q3-Write a program to count how many digits are in a number (e.g. 9876 → 4 digits).

```python
num = int(input("Enter a number: "))
count = 0
temp = abs(num)   # handle negative numbers

if temp == 0:
    count = 1
else:
    while temp > 0:
        temp = temp // 10
        count += 1

print(f"Number of digits: {count}")
```

---

## 🎯 Student Tasks – Module 02: Loops & Loop Control

### Task 1: Multiplication Table Generator (Easy)
**Objective**: Practice for loops with range.

**Instructions**:
1. Ask the user to enter a number.
2. Print its multiplication table from 1 to 12.
3. Format: `5 x 3 = 15`

**Expected Output**:
```
Enter a number: 7
7 x 1 = 7
7 x 2 = 14
...
7 x 12 = 84
```

---

### Task 2: Number Pattern & Sum (Medium)
**Objective**: Practice nested loops and while loops.

**Instructions**:
1. Print the following star pattern (5 rows):
   ```
   *
   **
   ***
   ****
   *****
   ```
2. Then use a while loop to find the sum of all even numbers from 1 to 50.
3. Use `continue` to skip odd numbers.

**Expected Output**:
```
*
**
***
****
*****

Sum of even numbers (1-50): 650
```

---

### Task 3: Guess the Number Game (Challenge)
**Objective**: Combine while loop, break, and continue.

**Instructions**:
1. Store a secret number (e.g., 42).
2. Use a while loop to keep asking the user to guess.
3. Use `break` when the user guesses correctly.
4. Use `continue` to skip and print hints if the guess is out of range (< 1 or > 100).
5. Count and display the number of attempts.
6. Give hints: `"Too high"` or `"Too low"`.

**Expected Output**:
```
Guess the number (1-100): 50
Too high! Try again.
Guess the number (1-100): 30
Too low! Try again.
Guess the number (1-100): 42
🎉 Correct! You guessed it in 3 attempts.
```

---


