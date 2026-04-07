# MySQL to PostgreSQL

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

### Option A — Using MySQL Workbench

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

## Part 5: Create Database in PostgreSQL

Open **pgAdmin** (or psql terminal) and run:

### Step 1: Create the Database
```sql
CREATE DATABASE company_db;
```

### Step 2: Connect to the Database
```sql
-- In psql terminal:
\c company_db

-- In pgAdmin: right-click company_db → Query Tool
```

### Step 3: Create Tables (PostgreSQL Syntax)
```sql
-- Employees table
CREATE TABLE employees (
    id          SERIAL PRIMARY KEY,           -- SERIAL replaces AUTO_INCREMENT
    name        VARCHAR(100) NOT NULL,
    email       VARCHAR(150) UNIQUE NOT NULL,
    department  VARCHAR(100),
    salary      NUMERIC(10, 2),              -- NUMERIC replaces DECIMAL
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Departments table
CREATE TABLE departments (
    id          SERIAL PRIMARY KEY,
    dept_name   VARCHAR(100) NOT NULL,
    location    VARCHAR(100),
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

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
