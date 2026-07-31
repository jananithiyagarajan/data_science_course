# employees2 table

| emp_id | name      | department | salary | join_date  |
| ------ | --------- | ---------- | ------ | ---------- |
| 101    | Arjun     | HR         | 35000  | 2021-03-15 |
| 102    | Meera     | IT         | 55000  | 2020-07-10 |
| 103    | Rahul     | Finance    | 45000  | 2022-01-05 |
| 104    | Sneha     | IT         | 60000  | 2019-11-20 |
| 105    | Kiran     | Marketing  | 30000  | 2021-06-01 |
| 106    | Priya     | Finance    | 48000  | 2020-02-12 |
| 107    | Varun     | IT         | 62000  | 2023-04-18 |
| 108    | Divya     | HR         | 37000  | 2021-09-22 |
| 109    | Sanjay    | Marketing  | 32000  | 2018-12-11 |
| 110    | Kavya     | IT         | 58000  | 2022-06-25 |
| 111    | Rohit     | Finance    | 52000  | 2019-08-30 |
| 112    | Aishwarya | HR         | 40000  | 2023-01-14 |
| 113    | Mohan     | Marketing  | 28000  | 2020-10-03 |
| 114    | Teja      | IT         | 65000  | 2017-05-19 |
| 115    | Vignesh   | Finance    | 47000  | 2021-12-09 |


## Create employees2 Table

```CREATE TABLE employees2 (
    emp_id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50),
    salary INT,
    join_date DATE
);
```
## Insert Data into employees2 Table

```
INSERT INTO employees2 (emp_id, name, department, salary, join_date) VALUES
(101, 'Arjun', 'HR', 35000, '2021-03-15'),
(102, 'Meera', 'IT', 55000, '2020-07-10'),
(103, 'Rahul', 'Finance', 45000, '2022-01-05'),
(104, 'Sneha', 'IT', 60000, '2019-11-20'),
(105, 'Kiran', 'Marketing', 30000, '2021-06-01'),
(106, 'Priya', 'Finance', 48000, '2020-02-12'),
(107, 'Varun', 'IT', 62000, '2023-04-18'),
(108, 'Divya', 'HR', 37000, '2021-09-22'),
(109, 'Sanjay', 'Marketing', 32000, '2018-12-11'),
(110, 'Kavya', 'IT', 58000, '2022-06-25'),
(111, 'Rohit', 'Finance', 52000, '2019-08-30'),
(112, 'Aishwarya', 'HR', 40000, '2023-01-14'),
(113, 'Mohan', 'Marketing', 28000, '2020-10-03'),
(114, 'Teja', 'IT', 65000, '2017-05-19'),
(115, 'Vignesh', 'Finance', 47000, '2021-12-09');
```

# Interview Questions

1. Write a query to display all records from the employees table.

```
SELECT * FROM employees2;
```

2. Write a query to display only emp_id and name.

4. Write a query to find all employees working in the IT department.
5. Write a query to find employees whose salary is greater than 40,000.
6. Write a query to sort employees by salary in descending order.
7. Write a query to get the highest salary from the employees table.
8. Write a query to count the number of employees in each department.
9. Write a query to find employees who joined after 2021-01-01.
10. Write a query to find the total salary paid to all employees.
11. Write a query to update the salary of employee with emp_id 103 to 50000.
12. Write a query to delete an employee who works in the Marketing department.
13. Write a query to find the minimum salary in the IT department.
14. Write a query to display employees whose name starts with 'S'.
15. Write a query to find employees where salary is between 30,000 and 50,000.
16. Write a query to group employees by department and show average salary.
17. Write a query to display employees ordered by join_date (oldest first).
18. Write a query to rename the column "name" to "employee_name" in output (using alias).

---

## Intermediate Interview Questions

19. What is the difference between `WHERE` and `HAVING`?
20. Write a query to find the second-highest salary from `employees2`.
21. What is a JOIN? Write an INNER JOIN between `employees2` and a `departments` table.
22. What is the difference between `INNER JOIN`, `LEFT JOIN`, and `RIGHT JOIN`?
23. Write a query using a subquery to find employees earning above the average salary.
24. What is a `VIEW`? Create a view showing only IT department employees.
25. What is the difference between `DELETE`, `TRUNCATE`, and `DROP`?
26. Write a query to find duplicate names in the `employees2` table.
27. What is `GROUP BY`? How is it different from `ORDER BY`?
28. Write a query to count employees per department, showing only departments with more than 2 employees.

---

## Advanced Interview Questions

29. What are Window Functions? Write a query using `ROW_NUMBER()` to rank employees by salary within each department.
30. What is a CTE (Common Table Expression)? Write a query using WITH clause.
31. What is the difference between `RANK()`, `DENSE_RANK()`, and `ROW_NUMBER()`?
32. Write a query using `LAG()` to find the salary difference between consecutive employees.
33. What are indexes? How do they improve query performance?
34. What is normalization? Explain 1NF, 2NF, 3NF with examples.
35. What is the difference between a Clustered and Non-clustered index?
36. Write a stored procedure that takes a department name and returns the average salary.
37. What is a trigger? Write an AFTER UPDATE trigger to log salary changes.
38. What is ACID? Explain with an example transaction.

---

## 🎯 Student Tasks – SQL Interview Prep

### Task 1: Solve Basic Interview Questions (Easy)
**Objective**: Write and test SQL solutions for beginner interview questions.

**Instructions**:
Use the `employees2` table provided above.
Solve questions 1–18 by writing actual SQL queries and running them.
For each query, write:
- The SQL statement
- Expected output (sample 2–3 rows)

**Focus Areas**:
- SELECT with WHERE, LIKE, BETWEEN
- ORDER BY, GROUP BY
- Aggregate functions (COUNT, SUM, AVG, MAX, MIN)
- UPDATE, DELETE

**Sample Solutions**:
```sql
-- Q2: Display only emp_id and name
SELECT emp_id, name FROM employees2;

-- Q4: IT department employees
SELECT * FROM employees2 WHERE department = 'IT';

-- Q7: Highest salary
SELECT MAX(salary) AS highest_salary FROM employees2;

-- Q8: Employee count per department
SELECT department, COUNT(*) AS emp_count
FROM employees2
GROUP BY department;

-- Q15: Salary between 30,000 and 50,000
SELECT * FROM employees2
WHERE salary BETWEEN 30000 AND 50000;

-- Q16: Average salary per department
SELECT department, ROUND(AVG(salary), 2) AS avg_salary
FROM employees2
GROUP BY department
ORDER BY avg_salary DESC;
```

---

### Task 2: Intermediate Interview Challenge (Medium)
**Objective**: Solve the intermediate questions (19–28) and explain your reasoning.

**Instructions**:
For each question, write:
1. The SQL query
2. A one-line explanation of WHY this approach works

**Focus Areas**:
- Subqueries
- JOINs (INNER, LEFT, RIGHT)
- Views
- Duplicate detection
- HAVING vs WHERE

**Sample Solutions**:
```sql
-- Q20: Second highest salary
SELECT MAX(salary) AS second_highest
FROM employees2
WHERE salary < (SELECT MAX(salary) FROM employees2);

-- Q23: Above-average salary (subquery)
SELECT name, salary
FROM employees2
WHERE salary > (SELECT AVG(salary) FROM employees2)
ORDER BY salary DESC;

-- Q26: Find duplicate names
SELECT name, COUNT(*) AS count
FROM employees2
GROUP BY name
HAVING COUNT(*) > 1;

-- Q28: Departments with more than 2 employees
SELECT department, COUNT(*) AS emp_count
FROM employees2
GROUP BY department
HAVING COUNT(*) > 2;
```

---

### Task 3: Mock SQL Interview (Challenge)
**Objective**: Simulate a complete SQL interview session.

**Instructions**:
Using a more complex schema (create all three tables):
```sql
CREATE TABLE departments (dept_id INT PRIMARY KEY, dept_name VARCHAR(50), budget INT);
CREATE TABLE employees3 (emp_id INT PRIMARY KEY, name VARCHAR(50), dept_id INT, salary INT, join_date DATE, FOREIGN KEY (dept_id) REFERENCES departments(dept_id));
CREATE TABLE projects (proj_id INT PRIMARY KEY, proj_name VARCHAR(50), dept_id INT, status VARCHAR(20));
```

Solve these advanced problems (time yourself — 5 min per question):

1. Find the top 3 highest-paid employees in each department (Window Function).
2. List departments with total salary budget > 50% of average across all departments.
3. Find employees who have been with the company the longest in each department.
4. Write a CTE that recursively finds all employees under a specific manager.
5. Create a stored procedure that promotes an employee (salary × 1.2) and logs to an audit table.

**Time target**: 25 minutes for all 5 problems.

**Scoring**:
```
Problem 1: 20 pts (Window Functions)
Problem 2: 20 pts (Subquery + HAVING)
Problem 3: 20 pts (Aggregate + JOIN)
Problem 4: 20 pts (Recursive CTE)
Problem 5: 20 pts (Stored Procedure + Triggers)

Total: 100 pts
Pass mark: 70 pts (Data Analyst level)
Full marks: 95+ pts (Senior Data Engineer level)
```

---

