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
