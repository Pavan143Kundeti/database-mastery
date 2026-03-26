# SQL Commands

## What are SQL Commands?

SQL commands are instructions used to perform operations on databases. These operations include:
- Querying data
- Creating tables
- Adding data to tables
- Modifying tables
- Deleting tables
- Setting user permissions

---

## Categories of SQL Commands

SQL commands are divided into 5 main categories:

```
┌─────────────────────────────────────────────────────────┐
│                    SQL Commands                          │
└─────────────────────────────────────────────────────────┘
                            |
        ┌───────────────────┼───────────────────┐
        |                   |                   |
    ┌───▼───┐          ┌────▼────┐        ┌────▼────┐
    │  DDL  │          │   DQL   │        │   DML   │
    │Define │          │  Query  │        │Manipulate│
    └───────┘          └─────────┘        └─────────┘
        |                                       |
    ┌───▼───┐                              ┌───▼───┐
    │  DCL  │                              │  TCL  │
    │Control│                              │Transaction│
    └───────┘                              └───────┘
```

| Category | Full Form | Purpose |
|----------|-----------|---------|
| **DDL** | Data Definition Language | Define and modify database structure |
| **DQL** | Data Query Language | Retrieve data from database |
| **DML** | Data Manipulation Language | Manipulate data in tables |
| **DCL** | Data Control Language | Control access and permissions |
| **TCL** | Transaction Control Language | Manage transactions |

---

## 1. DDL (Data Definition Language)

DDL commands define, alter, and delete database structures like tables, indexes, and schemas.

### DDL Commands:

| Command | Description | Syntax |
|---------|-------------|--------|
| **CREATE** | Create database or objects | `CREATE TABLE table_name (...)` |
| **DROP** | Delete objects from database | `DROP TABLE table_name;` |
| **ALTER** | Modify structure of database | `ALTER TABLE table_name ADD COLUMN ...` |
| **TRUNCATE** | Remove all records from table | `TRUNCATE TABLE table_name;` |
| **COMMENT** | Add comments to data dictionary | `COMMENT ON TABLE ... IS 'text'` |
| **RENAME** | Rename an existing object | `RENAME TABLE old_name TO new_name;` |

### Example:

```sql
-- Create a new table
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    hire_date DATE
);

-- Add a new column
ALTER TABLE employees ADD COLUMN salary DECIMAL(10,2);

-- Rename the table
RENAME TABLE employees TO staff;

-- Remove all records (but keep table structure)
TRUNCATE TABLE staff;

-- Delete the table completely
DROP TABLE staff;
```

### Key Points:

- **CREATE** makes new database objects
- **DROP** permanently deletes objects
- **ALTER** changes existing structure
- **TRUNCATE** removes all data but keeps structure
- **RENAME** changes object names

---

## 2. DQL (Data Query Language)

DQL is used to fetch data from the database. The main command is SELECT.

### DQL Command:

| Command | Description | Syntax |
|---------|-------------|--------|
| **SELECT** | Retrieve data from database | `SELECT column1, column2 FROM table_name` |

### SELECT Clauses:

| Clause | Description | Example |
|--------|-------------|---------|
| **FROM** | Specifies the table | `FROM employees` |
| **WHERE** | Filters rows | `WHERE department = 'Sales'` |
| **GROUP BY** | Groups rows with same values | `GROUP BY department` |
| **HAVING** | Filters grouped results | `HAVING COUNT(*) > 5` |
| **DISTINCT** | Removes duplicate rows | `SELECT DISTINCT department` |
| **ORDER BY** | Sorts results | `ORDER BY hire_date DESC` |
| **LIMIT** | Limits number of rows returned | `LIMIT 10` |

**Note:** FROM, WHERE, GROUP BY, HAVING, ORDER BY, DISTINCT, and LIMIT are clauses of SELECT, not separate commands.

### Example:

```sql
-- Basic SELECT
SELECT first_name, last_name FROM employees;

-- SELECT with WHERE
SELECT first_name, last_name, hire_date
FROM employees
WHERE department = 'Sales'
ORDER BY hire_date DESC;

-- SELECT with GROUP BY
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;

-- SELECT with HAVING
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;

-- SELECT with DISTINCT
SELECT DISTINCT department FROM employees;

-- SELECT with LIMIT
SELECT * FROM employees LIMIT 5;
```

### Query Execution Order:

```
1. FROM       - Get data from table
2. WHERE      - Filter rows
3. GROUP BY   - Group rows
4. HAVING     - Filter groups
5. SELECT     - Choose columns
6. DISTINCT   - Remove duplicates
7. ORDER BY   - Sort results
8. LIMIT      - Limit rows returned
```

---

## 3. DML (Data Manipulation Language)

DML commands manipulate data stored in database tables.

### DML Commands:

| Command | Description | Syntax |
|---------|-------------|--------|
| **INSERT** | Add new records | `INSERT INTO table_name (...) VALUES (...)` |
| **UPDATE** | Modify existing records | `UPDATE table_name SET ... WHERE ...` |
| **DELETE** | Remove records | `DELETE FROM table_name WHERE ...` |

### Example:

```sql
-- INSERT: Add new record
INSERT INTO employees (first_name, last_name, department)
VALUES ('Jane', 'Smith', 'HR');

-- INSERT: Add multiple records
INSERT INTO employees (first_name, last_name, department)
VALUES
    ('John', 'Doe', 'IT'),
    ('Alice', 'Brown', 'Sales'),
    ('Bob', 'Wilson', 'Marketing');

-- UPDATE: Modify existing record
UPDATE employees
SET department = 'Marketing'
WHERE first_name = 'Jane' AND last_name = 'Smith';

-- UPDATE: Modify multiple records
UPDATE employees
SET salary = salary * 1.10
WHERE department = 'Sales';

-- DELETE: Remove specific record
DELETE FROM employees
WHERE employee_id = 5;

-- DELETE: Remove multiple records
DELETE FROM employees
WHERE department = 'Marketing';
```

### Key Differences:

| Operation | What It Does | Example |
|-----------|--------------|---------|
| **INSERT** | Adds new rows | Add new employee |
| **UPDATE** | Changes existing rows | Change employee salary |
| **DELETE** | Removes rows | Remove employee |

**Warning:** Always use WHERE clause with UPDATE and DELETE to avoid changing/removing all rows!

---

## 4. DCL (Data Control Language)

DCL commands control access to data by granting or revoking permissions.

### DCL Commands:

| Command | Description | Syntax |
|---------|-------------|--------|
| **GRANT** | Give permissions to user | `GRANT privilege ON object TO user` |
| **REVOKE** | Remove permissions from user | `REVOKE privilege ON object FROM user` |

### Common Privileges:

| Privilege | Description |
|-----------|-------------|
| **SELECT** | Read data from table |
| **INSERT** | Add new records |
| **UPDATE** | Modify existing records |
| **DELETE** | Remove records |
| **ALL** | All privileges |

### Example:

```sql
-- GRANT: Give SELECT permission
GRANT SELECT ON employees TO user_name;

-- GRANT: Give multiple permissions
GRANT SELECT, UPDATE ON employees TO user_name;

-- GRANT: Give all permissions
GRANT ALL ON employees TO user_name;

-- GRANT: With option to grant to others
GRANT SELECT ON employees TO user_name WITH GRANT OPTION;

-- REVOKE: Remove permission
REVOKE UPDATE ON employees FROM user_name;

-- REVOKE: Remove all permissions
REVOKE ALL ON employees FROM user_name;
```

### Use Cases:

- Database administrators use DCL to manage user access
- Restrict sensitive data access
- Give read-only access to analysts
- Give full access to developers

---

## 5. TCL (Transaction Control Language)

TCL commands manage transactions. A transaction is a group of tasks that must all succeed or all fail together.

### TCL Commands:

| Command | Description | Syntax |
|---------|-------------|--------|
| **BEGIN TRANSACTION** | Start a new transaction | `BEGIN TRANSACTION;` |
| **COMMIT** | Save all changes permanently | `COMMIT;` |
| **ROLLBACK** | Undo all changes | `ROLLBACK;` |
| **SAVEPOINT** | Create a checkpoint | `SAVEPOINT savepoint_name;` |

### Transaction Flow:

```
BEGIN TRANSACTION
    |
    ├─> Execute SQL commands
    |
    ├─> SAVEPOINT (optional checkpoint)
    |
    ├─> More SQL commands
    |
    └─> Decision:
        ├─> COMMIT (save all changes)
        └─> ROLLBACK (undo all changes)
```

### Example:

```sql
-- Start transaction
BEGIN TRANSACTION;

-- Make changes
UPDATE employees SET department = 'Marketing' WHERE department = 'Sales';

-- Create a savepoint
SAVEPOINT before_update;

-- Make more changes
UPDATE employees SET department = 'IT' WHERE department = 'HR';

-- Undo to savepoint (keeps first update)
ROLLBACK TO SAVEPOINT before_update;

-- Save all changes permanently
COMMIT;
```

### Real-World Example: Bank Transfer

```sql
BEGIN TRANSACTION;

-- Deduct money from Account A
UPDATE accounts SET balance = balance - 1000 WHERE account_id = 1;

-- Add money to Account B
UPDATE accounts SET balance = balance + 1000 WHERE account_id = 2;

-- If both succeed, save changes
COMMIT;

-- If any fails, undo everything
-- ROLLBACK;
```

### Key Points:

- **Transaction:** Group of operations that succeed or fail together
- **COMMIT:** Makes changes permanent
- **ROLLBACK:** Undoes all changes since BEGIN TRANSACTION
- **SAVEPOINT:** Creates a checkpoint to rollback to

---

## Command Categories Summary

### Quick Reference Table:

| Category | Commands | Purpose | Example |
|----------|----------|---------|---------|
| **DDL** | CREATE, DROP, ALTER, TRUNCATE, RENAME | Define structure | `CREATE TABLE employees (...)` |
| **DQL** | SELECT | Query data | `SELECT * FROM employees` |
| **DML** | INSERT, UPDATE, DELETE | Manipulate data | `INSERT INTO employees VALUES (...)` |
| **DCL** | GRANT, REVOKE | Control access | `GRANT SELECT ON employees TO user` |
| **TCL** | BEGIN, COMMIT, ROLLBACK, SAVEPOINT | Manage transactions | `COMMIT;` |

---

## When to Use Each Category

| Scenario | Category | Command |
|----------|----------|---------|
| Create a new table | DDL | CREATE TABLE |
| Add a new column | DDL | ALTER TABLE |
| Get employee list | DQL | SELECT |
| Add new employee | DML | INSERT |
| Update salary | DML | UPDATE |
| Remove employee | DML | DELETE |
| Give user access | DCL | GRANT |
| Remove user access | DCL | REVOKE |
| Save changes | TCL | COMMIT |
| Undo changes | TCL | ROLLBACK |

---

## Important Notes

### DDL Commands:
- Auto-commit (changes are permanent immediately)
- Cannot be rolled back
- Affect database structure

### DML Commands:
- Can be rolled back (if in a transaction)
- Affect data in tables
- Should use WHERE clause to avoid affecting all rows

### DQL Commands:
- Read-only (don't change data)
- Can be complex with multiple clauses
- Most commonly used SQL command

### DCL Commands:
- Require admin privileges
- Control who can access what
- Important for security

### TCL Commands:
- Ensure data consistency
- Protect against partial updates
- Critical for financial and sensitive operations

---

## Key Takeaways

- SQL commands are grouped into 5 categories: DDL, DQL, DML, DCL, TCL
- DDL defines database structure (CREATE, ALTER, DROP)
- DQL retrieves data (SELECT)
- DML manipulates data (INSERT, UPDATE, DELETE)
- DCL controls access (GRANT, REVOKE)
- TCL manages transactions (COMMIT, ROLLBACK)
- Always use WHERE clause with UPDATE and DELETE
- Use transactions for operations that must succeed or fail together
