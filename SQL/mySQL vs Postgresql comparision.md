# MySQL vs PostgreSQL — Complete Comparison Guide

## 1. Overview

| Feature  | MySQL              | PostgreSQL             |
| -------- | ------------------ | ---------------------- |
| Type     | Relational DB      | Object-relational DB   |
| Focus    | Speed & simplicity | Features & correctness |
| Use Case | Web apps           | Enterprise, analytics  |

---

## 2. Data Types Comparison

### Numeric Types

| MySQL         | PostgreSQL             | Notes                |
| ------------- | ---------------------- | -------------------- |
| INT           | INTEGER                | Same                 |
| BIGINT        | BIGINT                 | Same                 |
| DECIMAL(p,s)  | NUMERIC(p,s)           | Same                 |
| FLOAT, DOUBLE | REAL, DOUBLE PRECISION | Naming differs       |
| UNSIGNED      | ❌ Not supported        | Important difference |

### String Types

| MySQL      | PostgreSQL | Notes                 |
| ---------- | ---------- | --------------------- |
| VARCHAR(n) | VARCHAR(n) | Same                  |
| TEXT       | TEXT       | Same                  |
| CHAR(n)    | CHAR(n)    | Same                  |
| ❌          | CITEXT     | Case-insensitive text |

### Date & Time

| MySQL     | PostgreSQL |
| --------- | ---------- |
| DATE      | DATE       |
| TIME      | TIME       |
| DATETIME  | ❌          |
| TIMESTAMP | TIMESTAMP  |

### Boolean

| MySQL      | PostgreSQL |
| ---------- | ---------- |
| TINYINT(1) | BOOLEAN    |

### JSON

| MySQL | PostgreSQL  |
| ----- | ----------- |
| JSON  | JSON, JSONB |

### ENUM

MySQL:

```sql
status ENUM('active','inactive')
```

PostgreSQL:

```sql
CREATE TYPE status_type AS ENUM ('active','inactive');
```

### Arrays

| MySQL | PostgreSQL |
| ----- | ---------- |
| ❌     | TEXT[]     |

---

## 3. Query Comparison

### Create Table

MySQL:

```sql
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100)
);
```

PostgreSQL:

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100)
);
```

---

### Insert

```sql
INSERT INTO users (name) VALUES ('Siva');
```

(Same in both)

---

### Pagination

MySQL:

```sql
SELECT * FROM users LIMIT 5,10;
```

PostgreSQL:

```sql
SELECT * FROM users LIMIT 10 OFFSET 5;
```

### Boolean

MySQL:

```sql
WHERE is_active = 1;
```

PostgreSQL:

```sql
WHERE is_active = TRUE;
```

---

## 🎯 Student Tasks – MySQL vs PostgreSQL

### Task 1: Identify the Differences (Easy)
**Objective**: Spot and fix MySQL queries that would break in PostgreSQL and vice versa.

**Instructions**:
Convert the following MySQL queries to work in PostgreSQL:

```sql
-- MySQL Queries (convert these to PostgreSQL):
-- 1. AUTO_INCREMENT usage
CREATE TABLE users (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(50));

-- 2. LIMIT/OFFSET
SELECT * FROM products LIMIT 10 OFFSET 20;

-- 3. Date formatting
SELECT DATE_FORMAT(NOW(), '%d-%m-%Y') AS today;

-- 4. String concatenation
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM employees;

-- 5. IFNULL
SELECT IFNULL(salary, 0) AS salary FROM employees;
```

**Expected PostgreSQL equivalents**:
```sql
-- 1. Use SERIAL or GENERATED
CREATE TABLE users (id SERIAL PRIMARY KEY, name VARCHAR(50));

-- 2. Same LIMIT/OFFSET syntax (compatible!)
SELECT * FROM products LIMIT 10 OFFSET 20;

-- 3. PostgreSQL date formatting
SELECT TO_CHAR(NOW(), 'DD-MM-YYYY') AS today;

-- 4. Both CONCAT and || work in PostgreSQL
SELECT first_name || ' ' || last_name AS full_name FROM employees;

-- 5. PostgreSQL uses COALESCE
SELECT COALESCE(salary, 0) AS salary FROM employees;
```

---

### Task 2: Feature Exploration (Medium)
**Objective**: Explore features unique to each database.

**Instructions**:

**MySQL exclusive features to test**:
1. Use `ENUM` data type: Create a table with gender ENUM('Male','Female','Other').
2. Use `REPLACE INTO`: Insert a row; run REPLACE INTO with same PK to auto-update.
3. Use `GROUP_CONCAT()`: Concatenate employee names per department.

**PostgreSQL exclusive features to test**:
1. Use `ARRAY` type: Store phone numbers as an array.
2. Use JSON/JSONB: Store product attributes as JSONB, query by JSON key.
3. Use Window Functions with `FILTER`: Calculate conditional aggregates.
4. Use `RETURNING`: INSERT and immediately get the inserted row back.

```sql
-- PostgreSQL JSONB example
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    attributes JSONB
);

INSERT INTO products (name, attributes)
VALUES ('Laptop', '{"brand": "Dell", "ram": 16, "storage": 512}')
RETURNING id, name;

-- Query by JSONB key
SELECT name, attributes->>'brand' AS brand
FROM products
WHERE (attributes->>'ram')::int > 8;
```

---

### Task 3: Database Migration Challenge (Challenge)
**Objective**: Migrate a real MySQL schema to PostgreSQL.

**Instructions**:
Given this MySQL schema:
```sql
-- MySQL
CREATE TABLE orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    order_date DATETIME DEFAULT NOW(),
    total_amount DECIMAL(10,2),
    status ENUM('pending','processing','shipped','delivered'),
    is_active TINYINT(1) DEFAULT 1,
    notes TEXT,
    tags VARCHAR(255)  -- comma-separated: "urgent,fragile"
);

DELIMITER //
CREATE PROCEDURE GetOrdersByStatus(IN status_filter VARCHAR(20))
BEGIN
    SELECT * FROM orders WHERE status = status_filter
    ORDER BY order_date DESC;
END //
DELIMITER ;
```

Tasks:
1. Convert the CREATE TABLE to PostgreSQL syntax.
2. Replace ENUM with a CHECK constraint or a type.
3. Replace TINYINT(1) with BOOLEAN.
4. Replace comma-separated tags with an ARRAY column.
5. Rewrite the stored procedure as a PostgreSQL FUNCTION.
6. Migrate 10 sample rows from MySQL format to PostgreSQL.

**Expected Output**:
```sql
-- PostgreSQL version
CREATE TYPE order_status AS ENUM('pending','processing','shipped','delivered');

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    order_date TIMESTAMP DEFAULT NOW(),
    total_amount DECIMAL(10,2),
    status order_status,
    is_active BOOLEAN DEFAULT TRUE,
    notes TEXT,
    tags TEXT[]   -- PostgreSQL array instead of CSV string
);

CREATE OR REPLACE FUNCTION get_orders_by_status(status_filter order_status)
RETURNS TABLE(LIKE orders) AS $$
BEGIN
    RETURN QUERY
    SELECT * FROM orders
    WHERE status = status_filter
    ORDER BY order_date DESC;
END;
$$ LANGUAGE plpgsql;
```

---

