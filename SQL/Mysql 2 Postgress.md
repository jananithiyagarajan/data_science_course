# MySQL to PostgreSQL — Complete Setup & Migration Guide

A complete step-by-step guide from installing MySQL & PostgreSQL to migrating your data.

---

## Part 1: Install MySQL

### Windows
1. Go to https://dev.mysql.com/downloads/installer/
2. Download **MySQL Installer**
3. Run the installer → Choose **"Developer Default"**
4. Follow the setup wizard and set your **root password**
5. Complete the installation

### macOS
```bash
brew install mysql
brew services start mysql
mysql_secure_installation   # Set root password
```

### Ubuntu / Linux
```bash
sudo apt update
sudo apt install mysql-server
sudo systemctl start mysql
sudo mysql_secure_installation   # Set root password
```

---

## Part 2: Install PostgreSQL

### Windows
1. Go to https://www.postgresql.org/download/windows/
2. Download the installer
3. Run it → Choose components: **PostgreSQL Server + pgAdmin 4**
4. Set your **postgres user password**
5. Keep default port **5432**
6. Complete the installation

### macOS
```bash
brew install postgresql
brew services start postgresql
```

### Ubuntu / Linux
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql
```

---

## Part 3: Create Database & Tables in MySQL

Open **MySQL Workbench** (or MySQL terminal) and run:

### Step 1: Create the Database
```sql
CREATE DATABASE company_db;
USE company_db;
```

### Step 2: Create Tables

```sql
-- Employees table
CREATE TABLE employees (
    id          INT AUTO_INCREMENT PRIMARY KEY,
    name        VARCHAR(100) NOT NULL,
    email       VARCHAR(150) UNIQUE NOT NULL,
    department  VARCHAR(100),
    salary      DECIMAL(10, 2),
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Departments table
CREATE TABLE departments (
    id          INT AUTO_INCREMENT PRIMARY KEY,
    dept_name   VARCHAR(100) NOT NULL,
    location    VARCHAR(100),
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Step 3: Insert Your Company Data
```sql
INSERT INTO departments (dept_name, location) VALUES
('Engineering', 'Chennai'),
('Marketing',   'Mumbai'),
('HR',          'Delhi');

INSERT INTO employees (name, email, department, salary) VALUES
('Alice Johnson', 'alice@company.com', 'Engineering', 85000.00),
('Bob Smith',     'bob@company.com',   'Marketing',   60000.00),
('Carol White',   'carol@company.com', 'Engineering', 92000.00),
('David Brown',   'david@company.com', 'HR',          55000.00);
```

### Step 4: Verify Data in MySQL
```sql
SELECT * FROM employees;
SELECT * FROM departments;
```

---

## Part 4: Export Data from MySQL

### Option A — Using MySQL Workbench (No Terminal Needed)

1. Open MySQL Workbench → Connect to your database
2. Go to **Server** → **Data Export**
3. Select `company_db` and all tables
4. Choose **"Export to Self-Contained File"**
5. Set file path (e.g., `C:/exports/company_db.sql`)
6. Click **"Start Export"**

### Option B — Using Terminal
```bash
mysqldump -u root -p company_db > company_db.sql
```
> Enter your MySQL root password when prompted.

---

## Part 5: Import the SQL Backup into PostgreSQL

> ✅ Since you already exported a `.sql` backup file from MySQL, **you do NOT need to manually create the database or tables**. The backup file already contains all `CREATE DATABASE`, `CREATE TABLE`, and `INSERT` statements — it handles everything automatically.

### Step 1: Clean the MySQL dump (removes MySQL-specific syntax)

Run these in your terminal before importing:

```bash
sed -i 's/`/"/g' /path/to/company_db.sql
sed -i 's/ENGINE=InnoDB//g' /path/to/company_db.sql
sed -i 's/DEFAULT CHARSET=utf8mb4//g' /path/to/company_db.sql
sed -i 's/DEFAULT CHARSET=utf8//g' /path/to/company_db.sql
sed -i 's/AUTO_INCREMENT=[0-9]*//g' /path/to/company_db.sql
sed -i 's/AUTO_INCREMENT//' /path/to/company_db.sql
sed -i '/^SET /d' /path/to/company_db.sql
sed -i '/^\/\*/d' /path/to/company_db.sql
sed -i 's/\/\*![0-9]*[^*]*\*\///g' /path/to/company_db.sql
sed -i '/^CREATE DATABASE/d' /path/to/company_db.sql
sed -i '/^USE /d' /path/to/company_db.sql
sed -i '/^LOCK TABLES/d' /path/to/company_db.sql
sed -i '/^UNLOCK TABLES/d' /path/to/company_db.sql
sed -i 's/COLLATE=[^ ;]*//g' /path/to/company_db.sql
sed -i 's/COLLATE [^ ,)]*//g' /path/to/company_db.sql
sed -i 's/UNIQUE KEY "[^"]*" (\("[^"]*"\))/UNIQUE (\1)/g' /path/to/company_db.sql
```
#### or
```bash
sed -i '
s/`/"/g;
s/ENGINE=InnoDB//g;
s/DEFAULT CHARSET=utf8mb4//g;
s/DEFAULT CHARSET=utf8//g;
s/AUTO_INCREMENT=[0-9]*//g;
s/AUTO_INCREMENT//g;
/^SET /d;
/^\/\*/d;
s/\/\*![0-9]*[^*]*\*\///g;
/^CREATE DATABASE/d;
/^USE /d;
/^LOCK TABLES/d;
/^UNLOCK TABLES/d;
s/COLLATE=[^ ;]*//g;
s/COLLATE [^ ,)]*//g;
s/UNIQUE KEY "[^"]*" (\("[^"]*"\))/UNIQUE (\1)/g;
' demo.sql
```


# Sed Cleanup Commands — Explained

Each command below strips MySQL-specific syntax from the dump file to make it compatible with PostgreSQL.

> **Pattern:** `sed -i 's/find/replace/g'` → find and replace text
> **Pattern:** `sed -i '/pattern/d'` → find and delete the entire line

---

## 1. Replace Backticks with Double Quotes

```bash
sed -i 's/`/"/g' /path/to/company_db.sql
```

**What it does:** MySQL wraps table and column names in backticks `` ` `` — PostgreSQL doesn't understand them. This replaces every backtick with a double quote, which is the PostgreSQL standard.

| | Example |
|---|---|
| Before (MySQL) | `` `employees` `` |
| After (PostgreSQL) | `"employees"` |

---

## 2. Remove ENGINE=InnoDB

```bash
sed -i 's/ENGINE=InnoDB//g' /path/to/company_db.sql
```

**What it does:** InnoDB is MySQL's storage engine. PostgreSQL doesn't have storage engines — this keyword causes a syntax error if left in.

| | Example |
|---|---|
| Before | `CREATE TABLE employees (...) ENGINE=InnoDB;` |
| After | `CREATE TABLE employees (...);` |

---

## 3. Remove DEFAULT CHARSET

```bash
sed -i 's/DEFAULT CHARSET=utf8mb4//g' /path/to/company_db.sql
sed -i 's/DEFAULT CHARSET=utf8//g' /path/to/company_db.sql
```

**What it does:** MySQL sets character encoding using `DEFAULT CHARSET`. PostgreSQL handles encoding at the database level — this line is meaningless and causes errors in PostgreSQL.

| | Example |
|---|---|
| Before | `DEFAULT CHARSET=utf8mb4` |
| After | *(removed)* |

---

## 4. Remove AUTO_INCREMENT Value

```bash
sed -i 's/AUTO_INCREMENT=[0-9]*//g' /path/to/company_db.sql
```

**What it does:** MySQL tracks the next auto increment number like `AUTO_INCREMENT=5`. PostgreSQL uses `SERIAL` / sequences instead — this MySQL-only attribute must be removed.

| | Example |
|---|---|
| Before | `AUTO_INCREMENT=5` |
| After | *(removed)* |

---

## 5. Remove AUTO_INCREMENT Keyword

```bash
sed -i 's/AUTO_INCREMENT//' /path/to/company_db.sql
```

**What it does:** The previous command removed `AUTO_INCREMENT=5` (with a number). This one removes any remaining `AUTO_INCREMENT` keyword that appears without a number — typically inside column definitions.

| | Example |
|---|---|
| Before | `"id" int NOT NULL AUTO_INCREMENT` |
| After | `"id" int NOT NULL` |

---

## 6. Delete SET Lines

```bash
sed -i '/^SET /d' /path/to/company_db.sql
```

**What it does:** MySQL dumps include `SET` statements to configure session variables like timezone, charset, and foreign key checks. These are MySQL-only and not valid in PostgreSQL — the entire line is deleted.

| | Example |
|---|---|
| Before | `SET FOREIGN_KEY_CHECKS=0;` |
| After | *(entire line deleted)* |

---

## 7. Delete Comment Lines Starting with `/*`

```bash
sed -i '/^\/\*/d' /path/to/company_db.sql
```

**What it does:** MySQL adds comment lines starting with `/*` at the top of dump files (version info, dump metadata). This deletes those full comment lines.

| | Example |
|---|---|
| Before | `/* MySQL dump 10.13 Distrib 8.0.45 */` |
| After | *(entire line deleted)* |

---

## 8. Remove MySQL Inline Version Comments

```bash
sed -i 's/\/\*![0-9]*[^*]*\*\///g' /path/to/company_db.sql
```

**What it does:** MySQL uses special inline version comments like `/*!40100 DEFAULT CHARACTER SET utf8mb4 */` — these run only on MySQL versions above a certain number. PostgreSQL doesn't understand this syntax at all, so they are removed inline (not the whole line, just this part).

| | Example |
|---|---|
| Before | `CREATE DATABASE "company_db" /*!40100 DEFAULT CHARACTER SET utf8mb4 */;` |
| After | `CREATE DATABASE "company_db";` |

---

## 9. Delete CREATE DATABASE Line

```bash
sed -i '/^CREATE DATABASE/d' /path/to/company_db.sql
```

**What it does:** MySQL's `CREATE DATABASE IF NOT EXISTS` syntax with inline charset options is not valid in PostgreSQL. The entire line is deleted — you manually create the database in PostgreSQL before importing.

| | Example |
|---|---|
| Before | `CREATE DATABASE IF NOT EXISTS "company_db" /*!40100 DEFAULT CHARACTER SET utf8mb4 */;` |
| After | *(entire line deleted)* |

---

## 10. Delete USE Statement

```bash
sed -i '/^USE /d' /path/to/company_db.sql
```

**What it does:** MySQL uses `USE db_name;` to switch databases. In PostgreSQL you connect to a database directly using `\c` or the `-d` flag — the `USE` command doesn't exist.

| | Example |
|---|---|
| Before | `USE "company_db";` |
| After | *(entire line deleted)* |

---

## 11. Delete LOCK and UNLOCK TABLES

```bash
sed -i '/^LOCK TABLES/d' /path/to/company_db.sql
sed -i '/^UNLOCK TABLES/d' /path/to/company_db.sql
```

**What it does:** MySQL uses `LOCK TABLES` and `UNLOCK TABLES` to prevent other users from editing data during the dump. PostgreSQL doesn't support these commands — both lines are deleted.

| | Example |
|---|---|
| Before | `LOCK TABLES "employees" WRITE;` |
| After | *(entire line deleted)* |

---

## 12. Remove COLLATE

```bash
sed -i 's/COLLATE=[^ ;]*//g' /path/to/company_db.sql
sed -i 's/COLLATE [^ ,)]*//g' /path/to/company_db.sql
```

**What it does:** `COLLATE` sets how MySQL sorts and compares text (e.g. case sensitivity). PostgreSQL handles collation differently and doesn't accept MySQL's collation names — so they must be stripped out.

| | Example |
|---|---|
| Before | `COLLATE=utf8mb4_0900_ai_ci` |
| After | *(removed)* |

---

## 13. Fix UNIQUE KEY Syntax

```bash
sed -i 's/UNIQUE KEY "[^"]*" (\("[^"]*"\))/UNIQUE (\1)/g' /path/to/company_db.sql
```

**What it does:** MySQL defines unique constraints as `UNIQUE KEY "name" ("column")`. PostgreSQL only understands `UNIQUE ("column")` — the `KEY` keyword and constraint name in this position are not valid.

| | Example |
|---|---|
| Before | `UNIQUE KEY "email" ("email")` |
| After | `UNIQUE ("email")` |

---


---

*Replace `/path/to/company_db.sql` with the actual path to your exported SQL file.*
> **Windows users:** Run these in Git Bash or WSL — they won't work in Command Prompt.

### Step 2: Import directly into PostgreSQL

```bash
psql -U postgres -f /path/to/company_db.sql
```

That's it — the `.sql` file automatically creates the database, tables, and inserts all data.

> **If you still get syntax errors** after cleaning, use pgLoader instead (see Part 6 Method 1) — it handles all MySQL-to-PostgreSQL conversion automatically with zero manual editing.

---

## Part 6: Migrate Data — MySQL → PostgreSQL

### Method 1: pgLoader (Best for Full Migration ✅)

#### Install pgLoader

**Ubuntu / Linux**
```bash
sudo apt install pgloader
```

**macOS**
```bash
brew install pgloader
```

**Windows** → Use WSL:
```
1. Open PowerShell as Administrator
2. Run: wsl --install
3. Restart PC → Open Ubuntu
4. Run: sudo apt install pgloader
```

#### Run Migration (One Command)
```bash
pgloader mysql://root:your_mysql_password@localhost/company_db \
         postgresql://postgres:your_postgres_password@localhost/company_db
```

> Replace `your_mysql_password` and `your_postgres_password` with your actual passwords.

pgLoader will automatically migrate all tables, data, indexes, and convert data types.

---

## Part 7: Verify Migration in PostgreSQL

```sql
-- Check all tables exist
\dt

-- Verify row counts match MySQL
SELECT COUNT(*) FROM employees;
SELECT COUNT(*) FROM departments;

-- Preview data
SELECT * FROM employees LIMIT 10;
SELECT * FROM departments LIMIT 10;
```

---



# DBeaver Installation and Connection Guide

## 1. Introduction

DBeaver is a universal database tool that supports multiple databases such as MySQL, PostgreSQL, Oracle, SQL Server, and more. It is widely used for database management, querying, and migration.

---

## 2. Installation

### 2.1 Windows Installation

1. Go to the official website:
   [https://dbeaver.io/download/](https://dbeaver.io/download/)

2. Download the **DBeaver Community Edition (Windows Installer)**.

3. Run the downloaded `.exe` file.

4. Follow the installation steps:

   * Click Next
   * Accept license agreement
   * Choose installation location
   * Click Install

5. Click Finish and launch DBeaver.

---

### 2.2 Ubuntu Installation

```bash
sudo apt update
sudo apt install dbeaver -y
```

---

## 3. First Launch

When you open DBeaver for the first time:

* You may be asked to create a workspace
* Click **OK** to proceed

---

## 4. Create Database Connection

### 4.1 Open Connection Wizard

1. Click **Database → New Database Connection**
2. Or click the **plug icon** (top left)

---

### 4.2 Select Database Type

Choose your database:

* MySQL
* PostgreSQL

Click **Next**

---

### 4.3 Enter Connection Details

#### For MySQL

| Field    | Value               |
| -------- | ------------------- |
| Host     | localhost           |
| Port     | 3306                |
| Database | your_database_name  |
| Username | root (or your user) |
| Password | your password       |

---

#### For PostgreSQL

| Field    | Value                 |
| -------- | --------------------- |
| Host     | localhost             |
| Port     | 5432                  |
| Database | postgres (or your DB) |
| Username | postgres              |
| Password | your password         |

---

### 4.4 Download Driver (First Time Only)

* Click **Download** if prompted
* Wait for driver installation

---

### 4.5 Test Connection

* Click **Test Connection**
* If successful → Click **Finish**

---

## 5. Basic Navigation

After connection:

```
Connection
 └── Databases
      └── Tables
           └── Columns
```

---

## 6. Running Queries

1. Right-click database
2. Click **SQL Editor → Open SQL Script**
3. Write query
4. Click **Execute (▶)**

---

## 7. Data Transfer (Migration)

1. Expand source database
2. Select tables
3. Right-click → **Export Data / Data Transfer**
4. Choose target database
5. Click Next → Finish

---



## MySQL vs PostgreSQL — Key Syntax Differences

| Feature          | MySQL                  | PostgreSQL                          |
|------------------|------------------------|-------------------------------------|
| Auto Increment   | `AUTO_INCREMENT`       | `SERIAL` or `GENERATED ALWAYS AS IDENTITY` |
| Decimal type     | `DECIMAL(10,2)`        | `NUMERIC(10,2)`                     |
| Boolean          | `TINYINT(1)`           | `BOOLEAN`                           |
| String quotes    | `'text'` or `"text"`   | `'text'` only                       |
| Identifiers      | `` `table_name` ``     | `"table_name"` or nothing           |
| Current time     | `NOW()`                | `NOW()` or `CURRENT_TIMESTAMP`      |
| Show tables      | `SHOW TABLES;`         | `\dt`                               |
| Use database     | `USE db_name;`         | `\c db_name`                        |

---

## 🎯 Student Tasks – MySQL to PostgreSQL Migration

### Task 1: Syntax Translation Practice (Easy)
**Objective**: Translate basic MySQL SQL to PostgreSQL-compatible syntax.

Rewrite the following MySQL statements for PostgreSQL:

```sql
-- 1. Create table with auto-increment
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    score DECIMAL(5,2)
);

-- 2. Insert and check last inserted ID
INSERT INTO students (name, score) VALUES ('Ravi', 88.5);
SELECT LAST_INSERT_ID();

-- 3. Format date
SELECT DATE_FORMAT(NOW(), '%d/%m/%Y %H:%i');

-- 4. Check if value is NULL
SELECT IFNULL(score, 0) FROM students;

-- 5. String contains
SELECT * FROM students WHERE name LIKE '%avi%';
```

**Expected PostgreSQL equivalents**:
```sql
-- 1. SERIAL for auto-increment
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    score NUMERIC(5,2)
);

-- 2. Use RETURNING to get inserted ID
INSERT INTO students (name, score) VALUES ('Ravi', 88.5) RETURNING id;

-- 3. TO_CHAR for date formatting
SELECT TO_CHAR(NOW(), 'DD/MM/YYYY HH24:MI');

-- 4. COALESCE replaces IFNULL
SELECT COALESCE(score, 0) FROM students;

-- 5. LIKE works the same
SELECT * FROM students WHERE name LIKE '%avi%';
```

---

### Task 2: Migrate a Complete Schema (Medium)
**Objective**: Migrate a full MySQL e-commerce schema to PostgreSQL.

Given MySQL schema:
```sql
CREATE TABLE categories (
    cat_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    parent_id INT DEFAULT NULL
);

CREATE TABLE products (
    prod_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL CHECK (price > 0),
    category_id INT,
    is_active TINYINT(1) DEFAULT 1,
    tags VARCHAR(255),
    created_at DATETIME DEFAULT NOW(),
    FOREIGN KEY (category_id) REFERENCES categories(cat_id)
);

CREATE TABLE orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT,
    qty INT CHECK (qty > 0),
    status ENUM('pending', 'shipped', 'delivered', 'cancelled'),
    FOREIGN KEY (product_id) REFERENCES products(prod_id)
);
```

Tasks:
1. Rewrite all 3 tables in PostgreSQL syntax.
2. Replace ENUM with a PostgreSQL `CREATE TYPE`.
3. Replace TINYINT(1) with BOOLEAN.
4. Replace VARCHAR tags with TEXT[].
5. Insert 5 rows per table.
6. Write 3 queries that use PostgreSQL-specific syntax (JSONB, ARRAY, RETURNING).

---

### Task 3: Live Migration with pgLoader (Challenge)
**Objective**: Perform a real database migration from MySQL to PostgreSQL.

**Instructions**:
1. Install MySQL and PostgreSQL (if not already done).
2. Create the `ecommerce` database in MySQL with 3 tables and 50+ rows.
3. Install pgLoader: `sudo apt install pgloader`
4. Write a pgLoader config file:
```
LOAD DATABASE
    FROM mysql://root@localhost/ecommerce
    INTO postgresql://postgres@localhost/ecommerce_pg
WITH include no drop, create tables, create indexes,
     reset sequences, foreign keys
SET work_mem to '128MB', maintenance_work_mem to '512MB'
EXCLUDING TABLE NAMES MATCHING 'temp_.*'
;
```
5. Run the migration: `pgloader config.load`
6. Verify in PostgreSQL:
   - All tables exist
   - Row counts match
   - Data types were converted correctly
7. Fix any migration errors.
8. Write a migration report documenting:
   - Tables migrated
   - Any type conversion issues
   - Performance observations

**Expected Output**:
```
=== pgLoader Migration Report ===

Source:  mysql://localhost/ecommerce
Target:  postgresql://localhost/ecommerce_pg

Tables migrated: 3
Total rows: 150

Table           MySQL Rows  PG Rows  Status
categories      10          10       ✓
products        50          50       ✓
orders          90          90       ✓

Type Conversions:
  TINYINT(1)    → BOOLEAN  ✓
  ENUM          → TEXT     (manual type cast needed)
  VARCHAR tags  → TEXT     ✓

Migration completed in 0.8 seconds.
Warnings: 2 (ENUM columns need manual type update)
```

---

