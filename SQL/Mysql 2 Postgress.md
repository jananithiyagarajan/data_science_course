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

### Method 2: CSV Export/Import (No Extra Tools Needed)

#### Step 1: Export Each Table as CSV from MySQL Workbench
1. Run: `SELECT * FROM employees;`
2. Click the **Export** icon in the result grid
3. Save as `employees.csv`
4. Repeat for each table

#### Step 2: Import CSV into PostgreSQL
```sql
-- Import employees
COPY employees (id, name, email, department, salary, created_at)
FROM '/path/to/employees.csv'
DELIMITER ',' CSV HEADER;

-- Import departments
COPY departments (id, dept_name, location, created_at)
FROM '/path/to/departments.csv'
DELIMITER ',' CSV HEADER;
```

> **Windows path example:** `C:/exports/employees.csv`  
> **Mac/Linux path example:** `/home/yourname/exports/employees.csv`

> If you get a permission error, use `\copy` instead of `COPY`.

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

## Migration Method Comparison

| | pgLoader | CSV Method |
|---|---|---|
| **Effort** | ⭐ Very Easy | Moderate |
| **Auto type conversion** | ✅ Yes | ❌ Manual |
| **Indexes migrated** | ✅ Yes | ❌ Must recreate |
| **Large data** | ✅ Handles well | Can be slow |
| **Requires terminal** | Yes | No |
| **Best for** | Full DB migration | Small/medium tables |

---

## Troubleshooting

**Cannot connect to MySQL**
```
Access denied for user 'root'@'localhost'
```
→ Check your password. Reset with: `ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';`

---

**COPY permission denied in PostgreSQL**
```
ERROR: could not open file: Permission denied
```
→ Use `\copy` instead of `COPY`, or move the CSV to `/tmp/` folder.

---

**pgLoader data type error**
```
ERROR: invalid input syntax for type
```
→ Check for null values or special characters in your MySQL data before migrating.

---

**Port conflict**
```
ERROR: could not connect to server: Connection refused (port 5432)
```
→ Make sure PostgreSQL service is running:
```bash
# Linux/Mac
sudo systemctl start postgresql

# Windows
# Open Services → Find PostgreSQL → Start
```

---

*Complete guide: MySQL installation → database creation → data export → PostgreSQL setup → import & verification.*
