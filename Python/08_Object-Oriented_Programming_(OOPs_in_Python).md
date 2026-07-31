# OOPs Concept in Python

OOPS (Object-Oriented Programming System) is a programming paradigm that organizes software design around **objects** — entities that contain both **data (attributes)** and **behaviors (methods)**.

Its goal is to make code **modular, secure, reusable, and easier to maintain**.

## Python's Four Core OOP Pillars

| Pillar            | Core Idea                                    |
| ----------------- | -------------------------------------------- |
| **Encapsulation** | Bundle data and methods, hide internal details |
| **Inheritance**   | A class can inherit from another class        |
| **Polymorphism**  | Same method name, different behavior          |
| **Abstraction**   | Show only what's necessary, hide complexity   |

---

## 1. Classes and Objects

A **class** is a blueprint/template.  
An **object** is an instance created from that blueprint.

```python
# Define a class
class Dog:
    # Class attribute (shared by all instances)
    species = "Canis lupus familiaris"

    # Instance method
    def bark(self):
        print("Woof!")

# Create objects (instances)
dog1 = Dog()
dog2 = Dog()

dog1.bark()             # Woof!
print(Dog.species)      # Canis lupus familiaris
print(type(dog1))       # <class '__main__.Dog'>
```

---

## 2. The `__init__` Constructor

The `__init__` method is called automatically when an object is created.
It initializes the object's attributes.

```python
class Student:
    def __init__(self, name, age, marks):
        self.name = name      # instance attribute
        self.age = age
        self.marks = marks

    def display(self):
        print(f"Name: {self.name}, Age: {self.age}, Marks: {self.marks}")

# Creating objects
s1 = Student("Arun", 20, 88)
s2 = Student("Priya", 21, 95)

s1.display()   # Name: Arun, Age: 20, Marks: 88
s2.display()   # Name: Priya, Age: 21, Marks: 95
```

### `self` Explained

`self` refers to the current instance of the class. It must be the first parameter of every instance method.

```python
class Circle:
    def __init__(self, radius):
        self.radius = radius       # 'self.radius' is the instance attribute

    def area(self):
        import math
        return math.pi * self.radius ** 2

c = Circle(5)
print(c.area())    # 78.53981633974483
```

---

## 3. Methods and Attributes

### Instance Attributes vs Class Attributes

```python
class Employee:
    company = "JeeviAcademy"   # Class attribute — shared by all

    def __init__(self, name, salary):
        self.name = name         # Instance attribute — unique per object
        self.salary = salary

    def display(self):
        print(f"{self.name} works at {Employee.company}, Salary: {self.salary}")

e1 = Employee("Ravi", 50000)
e2 = Employee("Meena", 65000)

e1.display()   # Ravi works at JeeviAcademy, Salary: 50000
e2.display()   # Meena works at JeeviAcademy, Salary: 65000
```

### Instance Methods, Class Methods, Static Methods

```python
class BankAccount:
    bank_name = "Python Bank"  # class attribute

    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance

    # Instance method — operates on the instance
    def deposit(self, amount):
        self.balance += amount
        print(f"Deposited ₹{amount}. New balance: ₹{self.balance}")

    def withdraw(self, amount):
        if amount > self.balance:
            print("Insufficient balance!")
        else:
            self.balance -= amount
            print(f"Withdrawn ₹{amount}. New balance: ₹{self.balance}")

    # Class method — operates on the class itself
    @classmethod
    def get_bank_name(cls):
        return cls.bank_name

    # Static method — no access to class or instance
    @staticmethod
    def is_valid_amount(amount):
        return amount > 0

acc = BankAccount("Arun", 1000)
acc.deposit(500)       # Deposited ₹500. New balance: ₹1500
acc.withdraw(200)      # Withdrawn ₹200. New balance: ₹1300

print(BankAccount.get_bank_name())      # Python Bank
print(BankAccount.is_valid_amount(-5))  # False
```

---

## 4. Inheritance

**Inheritance** allows a child class to inherit attributes and methods from a parent class, promoting code reuse.

### Single Inheritance

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f"{self.name} makes a sound.")

    def eat(self):
        print(f"{self.name} is eating.")

class Dog(Animal):   # Dog inherits from Animal
    def speak(self):  # Override parent method
        print(f"{self.name} says: Woof!")

class Cat(Animal):
    def speak(self):
        print(f"{self.name} says: Meow!")

d = Dog("Buddy")
c = Cat("Whiskers")

d.speak()   # Buddy says: Woof!
c.speak()   # Whiskers says: Meow!
d.eat()     # Buddy is eating. (inherited from Animal)
```

### `super()` — Calling Parent's Method

```python
class Vehicle:
    def __init__(self, brand, speed):
        self.brand = brand
        self.speed = speed

    def describe(self):
        print(f"Brand: {self.brand}, Speed: {self.speed} km/h")

class Car(Vehicle):
    def __init__(self, brand, speed, fuel_type):
        super().__init__(brand, speed)   # Call parent's __init__
        self.fuel_type = fuel_type

    def describe(self):
        super().describe()               # Call parent's method
        print(f"Fuel: {self.fuel_type}")

my_car = Car("Toyota", 180, "Petrol")
my_car.describe()
# Brand: Toyota, Speed: 180 km/h
# Fuel: Petrol
```

### Multilevel Inheritance

```python
class Grandparent:
    def greet(self):
        print("Hello from Grandparent")

class Parent(Grandparent):
    def work(self):
        print("Working...")

class Child(Parent):
    def play(self):
        print("Playing...")

c = Child()
c.greet()   # Hello from Grandparent (inherited from Grandparent)
c.work()    # Working... (inherited from Parent)
c.play()    # Playing...
```

### Multiple Inheritance

```python
class Flyable:
    def fly(self):
        print("I can fly!")

class Swimmable:
    def swim(self):
        print("I can swim!")

class Duck(Flyable, Swimmable):
    def quack(self):
        print("Quack!")

d = Duck()
d.fly()     # I can fly!
d.swim()    # I can swim!
d.quack()   # Quack!
```

---

## 5. Polymorphism

**Polymorphism** means "many forms" — the same method name works differently for different classes.

### Method Overriding

```python
class Shape:
    def area(self):
        return 0

class Rectangle(Shape):
    def __init__(self, w, h):
        self.w = w
        self.h = h

    def area(self):
        return self.w * self.h

class Circle(Shape):
    def __init__(self, r):
        self.r = r

    def area(self):
        import math
        return math.pi * self.r ** 2

# Polymorphism in action
shapes = [Rectangle(4, 5), Circle(3), Rectangle(2, 7)]
for shape in shapes:
    print(f"{type(shape).__name__}: area = {shape.area():.2f}")

# Rectangle: area = 20.00
# Circle: area = 28.27
# Rectangle: area = 14.00
```

### Duck Typing

```python
class Dog:
    def sound(self):
        return "Woof"

class Cat:
    def sound(self):
        return "Meow"

class Robot:
    def sound(self):
        return "Beep Boop"

def make_sound(animal):
    print(animal.sound())   # Works for any object with a .sound() method

for obj in [Dog(), Cat(), Robot()]:
    make_sound(obj)
```

---

## 6. Encapsulation

**Encapsulation** means restricting direct access to some of an object's attributes/methods and only exposing what's necessary.

### Access Modifiers in Python

| Prefix      | Type      | Description                                |
| ----------- | --------- | ------------------------------------------ |
| `name`      | Public    | Accessible everywhere                      |
| `_name`     | Protected | Should not be accessed outside class (convention) |
| `__name`    | Private   | Name-mangled, not directly accessible      |

```python
class Person:
    def __init__(self, name, age, salary):
        self.name = name          # public
        self._age = age           # protected (convention)
        self.__salary = salary    # private (name mangled)

    def get_salary(self):         # getter
        return self.__salary

    def set_salary(self, amount): # setter
        if amount > 0:
            self.__salary = amount
        else:
            print("Invalid salary!")

p = Person("Arun", 25, 50000)
print(p.name)           # Arun (public)
print(p._age)           # 25 (accessible but should be avoided)
# print(p.__salary)     # ❌ AttributeError

print(p.get_salary())   # 50000 (via getter)
p.set_salary(60000)
print(p.get_salary())   # 60000
```

### Using `@property` (Pythonic Getters/Setters)

```python
class Temperature:
    def __init__(self, celsius):
        self.__celsius = celsius

    @property
    def celsius(self):
        return self.__celsius

    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Temperature below absolute zero!")
        self.__celsius = value

    @property
    def fahrenheit(self):
        return (self.__celsius * 9/5) + 32

t = Temperature(25)
print(t.celsius)     # 25
print(t.fahrenheit)  # 77.0
t.celsius = 100
print(t.fahrenheit)  # 212.0
```

---

## OOP Summary Table

| Concept          | Keyword/Feature            | Purpose                                    |
| ---------------- | -------------------------- | ------------------------------------------ |
| Class            | `class`                    | Blueprint for objects                      |
| Object           | `ClassName()`              | Instance of a class                        |
| Constructor      | `__init__`                 | Initialize object attributes               |
| Instance method  | `def method(self):`        | Method that works on the instance          |
| Class method     | `@classmethod`             | Method that works on the class             |
| Static method    | `@staticmethod`            | Utility method; no access to class/instance |
| Inheritance      | `class Child(Parent):`     | Reuse and extend parent class              |
| `super()`        | `super().__init__()`       | Call parent class method                   |
| Polymorphism     | Method overriding          | Same method, different behavior            |
| Encapsulation    | `__private`, `@property`   | Protect internal data                      |

---

## 🎯 Student Tasks – Module 05: OOPs in Python

### Task 1: Library Book System (Easy)
**Objective**: Practice classes, `__init__`, and instance methods.

**Instructions**:
1. Create a class `Book` with attributes: `title`, `author`, `pages`, `is_available` (default `True`).
2. Add methods:
   - `checkout()` — sets `is_available = False` and prints a message.
   - `return_book()` — sets `is_available = True` and prints a message.
   - `display()` — prints all book details.
3. Create 2 book objects, check one out, and display both.

**Expected Output**:
```
Title: Python Basics | Author: John | Pages: 350 | Available: True
Title: Data Science | Author: Jane | Pages: 480 | Available: True

'Python Basics' has been checked out.

Title: Python Basics | Author: John | Pages: 350 | Available: False
```

---

### Task 2: Animal Kingdom with Inheritance (Medium)
**Objective**: Practice inheritance and method overriding.

**Instructions**:
1. Create a base class `Animal` with `name`, `age`, and methods `speak()` and `describe()`.
2. Create subclasses: `Dog`, `Cat`, `Bird` — each overrides `speak()`.
3. Create a class `Pet(Animal)` that adds an `owner` attribute and overrides `describe()` to include owner info.
4. Create a list of 4 mixed animals and iterate — calling `speak()` and `describe()` on each.

**Expected Output**:
```
Buddy (Dog, 3 yrs): Woof!
Whiskers (Cat, 5 yrs): Meow!
Tweety (Bird, 2 yrs): Tweet!
Max (Pet Dog, 4 yrs) owned by Arun: Woof!
```

---

### Task 3: Bank Account OOP System (Challenge)
**Objective**: Combine encapsulation, inheritance, and polymorphism.

**Instructions**:
1. Create class `BankAccount` with private `__balance`, methods: `deposit()`, `withdraw()`, `get_balance()`.
2. Create `SavingsAccount(BankAccount)` that adds `interest_rate` and method `add_interest()`.
3. Create `CurrentAccount(BankAccount)` that adds `overdraft_limit` — allow withdrawal up to limit.
4. Use `@property` for the balance attribute.
5. Create one object of each and demonstrate all operations.

**Expected Output**:
```
=== Savings Account ===
Initial Balance: ₹10000
After Deposit ₹5000: ₹15000
After Interest (5%): ₹15750

=== Current Account ===
Initial Balance: ₹5000
After Withdrawal ₹7000 (with overdraft): ₹-2000
Overdraft used: ₹2000
```

---
