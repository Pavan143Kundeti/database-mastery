# Advanced SQL Topics

This comprehensive guide covers advanced SQL concepts including subqueries, window functions, stored procedures, triggers, performance tuning, and transactions.

---

## Table of Contents
1. [SQL Subqueries](#sql-subqueries)
2. [Window Functions](#window-functions)
3. [Stored Procedures](#stored-procedures)
4. [SQL Triggers](#sql-triggers)
5. [Performance Tuning](#performance-tuning)
6. [SQL Transactions](#sql-transactions)

---

## SQL Subqueries

A subquery is a query nested inside another SQL query. It allows complex filtering, aggregation, and data manipulation.

**Why Use Subqueries?**
- Filter rows based on results from another query
- Apply aggregate functions dynamically
- Update data using values from other tables
- Delete rows based on conditions from another query

### Syntax:

```sql
SELECT column_name
FROM table_name
WHERE column_name expression operator
    (SELECT column_name FROM table_name WHERE ...);
```

**Note:** While there is no universal syntax, subqueries are commonly used in SELECT statements.

---

### Example Setup: Students Table

```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50),
    Score INT
);

INSERT INTO Students VALUES
(1, 'Alice', 85),
(2, 'Bob', 92),
(3, 'Charlie', 78),
(4, 'Diana', 95),
(5, 'Eve', 88);
```

**Students Table:**

| StudentID | Name | Score |
|-----------|------|-------|
| 1 | Alice | 85 |
| 2 | Bob | 92 |
| 3 | Charlie | 78 |
| 4 | Diana | 95 |
| 5 | Eve | 88 |

### Basic Subquery Example:

```sql
SELECT * FROM Students
WHERE Score > (SELECT AVG(Score) FROM Students);
```

**Output:**

| StudentID | Name | Score |
|-----------|------|-------|
| 2 | Bob | 92 |
| 4 | Diana | 95 |
| 5 | Eve | 88 |

**Explanation:**
- The subquery calculates the average score (87.6)
- The main query returns students whose score is higher than the average

---

### SQL Clauses for Subqueries

| Clause | Description |
|--------|-------------|
| **WHERE** | Filters rows based on subquery results |
| **FROM** | Treats subquery as a temporary (derived) table |
| **HAVING** | Filters grouped results using subquery values |

---

## Types of Subqueries

### Example Setup: Employees and Departments Tables

```sql
CREATE TABLE Employees (
    EmpID INT PRIMARY KEY,
    Name VARCHAR(50),
    DepartmentID INT,
    Salary DECIMAL(10,2)
);

INSERT INTO Employees VALUES
(1, 'Alice', 101, 60000),
(2, 'Bob', 102, 55000),
(3, 'Charlie', 101, 65000),
(4, 'Diana', 103, 50000),
(5, 'Eve', 102, 58000);

CREATE TABLE Departments (
    DepartmentID INT PRIMARY KEY,
    DeptName VARCHAR(50),
    Location VARCHAR(50)
);

INSERT INTO Departments VALUES
(101, 'IT', 'New York'),
(102, 'HR', 'New York'),
(103, 'Sales', 'Boston');
```

**Employees Table:**

| EmpID | Name | DepartmentID | Salary |
|-------|------|--------------|--------|
| 1 | Alice | 101 | 60000 |
| 2 | Bob | 102 | 55000 |
| 3 | Charlie | 101 | 65000 |
| 4 | Diana | 103 | 50000 |
| 5 | Eve | 102 | 58000 |

**Departments Table:**

| DepartmentID | DeptName | Location |
|--------------|----------|----------|
| 101 | IT | New York |
| 102 | HR | New York |
| 103 | Sales | Boston |

---

### 1. Single-Row Subquery

Returns only one value.

**Characteristics:**
- Returns exactly one row
- Used with comparison operators (=, >, <)

**Example:**

```sql
SELECT * FROM Employees
WHERE Salary = (SELECT MAX(Salary) FROM Employees);
```

**Output:**

| EmpID | Name | DepartmentID | Salary |
|-------|------|--------------|--------|
| 3 | Charlie | 101 | 65000 |

**Explanation:**
- Finds the highest salary (65000)
- Returns the employee with that salary

---

### 2. Multi-Row Subquery

Returns more than one value.

**Characteristics:**
- Returns multiple rows
- Requires operators: IN, ANY, or ALL

**Example:**

```sql
SELECT * FROM Employees
WHERE DepartmentID IN (
    SELECT DepartmentID FROM Departments WHERE Location = 'New York'
);
```

**Output:**

| EmpID | Name | DepartmentID | Salary |
|-------|------|--------------|--------|
| 1 | Alice | 101 | 60000 |
| 2 | Bob | 102 | 55000 |
| 3 | Charlie | 101 | 65000 |
| 5 | Eve | 102 | 58000 |

**Explanation:**
- Identifies departments in New York (101, 102)
- Returns employees in those departments

---

### 3. Correlated Subquery

Depends on the outer query for its values.

**Characteristics:**
- References columns from the outer query
- Executed once for each row of the outer query
- Slower for large datasets

**Example:**

```sql
SELECT e.Name, e.Salary
FROM Employees e
WHERE e.Salary > (
    SELECT AVG(Salary)
    FROM Employees
    WHERE DepartmentID = e.DepartmentID
);
```

**Output:**

| Name | Salary |
|------|--------|
| Alice | 60000 |
| Charlie | 65000 |
| Eve | 58000 |

**Explanation:**
- Calculates average salary within each employee's department
- Returns employees whose salary is higher than their department's average

---

## Examples of Using SQL Subqueries

### Example Setup: Student_Info and Student_Section Tables

```sql
CREATE TABLE Student_Info (
    ROLL_NO INT PRIMARY KEY,
    NAME VARCHAR(50),
    LOCATION VARCHAR(50),
    PHONE_NUMBER VARCHAR(15)
);

INSERT INTO Student_Info VALUES
(101, 'Sophia', 'London', '1234567890'),
(102, 'Emma', 'Toronto', '2345678901'),
(103, 'Olivia', 'Berlin', '3456789012'),
(201, 'Liam', 'London', '4567890123');

CREATE TABLE Student_Section (
    ROLL_NO INT,
    SECTION CHAR(1)
);

INSERT INTO Student_Section VALUES
(101, 'A'),
(102, 'B'),
(103, 'A'),
(201, 'A');
```

**Student_Info Table:**

| ROLL_NO | NAME | LOCATION | PHONE_NUMBER |
|---------|------|----------|--------------|
| 101 | Sophia | London | 1234567890 |
| 102 | Emma | Toronto | 2345678901 |
| 103 | Olivia | Berlin | 3456789012 |
| 201 | Liam | London | 4567890123 |

**Student_Section Table:**

| ROLL_NO | SECTION |
|---------|---------|
| 101 | A |
| 102 | B |
| 103 | A |
| 201 | A |

---

### Example 1: Subquery in WHERE Clause

```sql
SELECT NAME, LOCATION, PHONE_NUMBER
FROM Student_Info
WHERE ROLL_NO IN (
    SELECT ROLL_NO FROM Student_Section WHERE SECTION = 'A'
);
```

**Output:**

| NAME | LOCATION | PHONE_NUMBER |
|------|----------|--------------|
| Sophia | London | 1234567890 |
| Olivia | Berlin | 3456789012 |
| Liam | London | 4567890123 |

**Explanation:**
- Subquery finds roll numbers of students in section A
- Outer query fetches details for those students

---

### Example 2: Subquery with DELETE

```sql
DELETE FROM Student_Info
WHERE ROLL_NO IN (
    SELECT ROLL_NO FROM Student_Info
    WHERE ROLL_NO <
### Example 2: Subquery with DELETE

```sql
DELETE FROM Student_Info
WHERE ROLL_NO IN (
    SELECT ROLL_NO FROM Student_Info
    WHERE ROLL_NO <= 101 OR ROLL_NO = 201
);
```

**Explanation:**
- Subquery selects roll numbers 101 and 201
- Outer query deletes students with those roll numbers

---

### Example 3: Subquery with UPDATE

```sql
UPDATE Student_Info
SET NAME = 'Geeks'
WHERE LOCATION IN (
    SELECT LOCATION FROM Student_Info
    WHERE LOCATION IN ('London', 'Berlin')
);
```

**Explanation:**
- Subquery selects locations 'London' and 'Berlin'
- Outer query updates NAME for students in those locations

---

### Example 4: Subquery in FROM Clause

```sql
SELECT NAME, PHONE_NUMBER
FROM (
    SELECT NAME, PHONE_NUMBER, LOCATION
    FROM Student_Info
    WHERE LOCATION LIKE 'T%'
) AS subquery_table;
```

**Output:**

| NAME | PHONE_NUMBER |
|------|--------------|
| Emma | 2345678901 |

**Explanation:**
- Subquery fetches students whose location starts with "T" (Toronto)
- Outer query selects only NAME and PHONE_NUMBER from the result

---

### Example 5: Subquery with JOIN

```sql
SELECT s.NAME, s.LOCATION, ns.SECTION
FROM Student_Info s
INNER JOIN (
    SELECT ROLL_NO, SECTION
    FROM Student_Section
    WHERE SECTION = 'A'
) ns ON s.ROLL_NO = ns.ROLL_NO;
```

**Output:**

| NAME | LOCATION | SECTION |
|------|----------|---------|
| Sophia | London | A |
| Olivia | Berlin | A |
| Liam | London | A |

**Explanation:**
- Subquery extracts roll numbers of students in section A
- JOIN combines with Student_Info to get complete details

---

## Window Functions

Window functions perform calculations across a set of rows related to the current row, without collapsing the result into a single value.

**Key Features:**
- Use OVER clause to define the "window" of rows
- PARTITION BY divides data into groups
- ORDER BY specifies order within each group
- Functions: SUM(), AVG(), ROW_NUMBER(), RANK(), DENSE_RANK()

### Syntax:

```sql
SELECT column_name1,
       window_function(column_name2)
       OVER ([PARTITION BY column_name3] [ORDER BY column_name4]) AS new_column
FROM table_name;
```

**Parameters:**
- `window_function`: Aggregate or ranking function
- `column_name1`: Column(s) to display
- `column_name2`: Column used by the window function
- `column_name3`: Column for grouping (PARTITION BY)
- `column_name4`: Column for ordering (ORDER BY)
- `new_column`: Alias for the result
- `table_name`: Table to select data from

---

### Types of Window Functions

```
┌─────────────────────────────────────────────────────────┐
│              Window Functions in SQL                     │
└─────────────────────────────────────────────────────────┘
                            |
        ┌───────────────────┴───────────────────┐
        |                                       |
    ┌───▼───────────┐                  ┌───────▼────────┐
    │   Aggregate   │                  │    Ranking     │
    │   Functions   │                  │   Functions    │
    └───────────────┘                  └────────────────┘
    - SUM()                            - RANK()
    - AVG()                            - DENSE_RANK()
    - COUNT()                          - ROW_NUMBER()
    - MAX()                            - PERCENT_RANK()
    - MIN()
```

---

### Example Setup: Employee Table

```sql
CREATE TABLE employee (
    Name VARCHAR(50),
    Age INT,
    Department VARCHAR(50),
    Salary DECIMAL(10,2)
);

INSERT INTO employee VALUES
('Andrew', 30, 'Finance', 50000),
('Brian', 28, 'Finance', 50000),
('Charles', 35, 'Finance', 20000),
('Daniel', 32, 'Sales', 30000),
('Ethan', 29, 'Sales', 20000);
```

**Employee Table:**

| Name | Age | Department | Salary |
|------|-----|------------|--------|
| Andrew | 30 | Finance | 50000 |
| Brian | 28 | Finance | 50000 |
| Charles | 35 | Finance | 20000 |
| Daniel | 32 | Sales | 30000 |
| Ethan | 29 | Sales | 20000 |

---

## 1. Aggregate Window Functions

Calculate aggregates over a window while retaining individual rows.

### AVG() Window Function

Calculate average salary within each department:

```sql
SELECT Name, Age, Department, Salary,
       AVG(Salary) OVER(PARTITION BY Department) AS Avg_Salary
FROM employee;
```

**Output:**

| Name | Age | Department | Salary | Avg_Salary |
|------|-----|------------|--------|------------|
| Andrew | 30 | Finance | 50000 | 40000 |
| Brian | 28 | Finance | 50000 | 40000 |
| Charles | 35 | Finance | 20000 | 40000 |
| Daniel | 32 | Sales | 30000 | 25000 |
| Ethan | 29 | Sales | 20000 | 25000 |

**Explanation:**
- Finance: (50,000 + 50,000 + 20,000) ÷ 3 = 40,000
- Sales: (30,000 + 20,000) ÷ 2 = 25,000
- Average displayed for each employee in the same department

---

## 2. Ranking Window Functions

Provide rankings of rows within a partition.

### RANK() Function

Assigns ranks with gaps for duplicates:

```sql
SELECT Name, Department, Salary,
       RANK() OVER(PARTITION BY Department ORDER BY Salary DESC) AS emp_rank
FROM employee;
```

**Output:**

| Name | Department | Salary | emp_rank |
|------|------------|--------|----------|
| Andrew | Finance | 50000 | 1 |
| Brian | Finance | 50000 | 1 |
| Charles | Finance | 20000 | 3 |
| Daniel | Sales | 30000 | 1 |
| Ethan | Sales | 20000 | 2 |

**Explanation:**
- Finance: Andrew and Brian both earn 50,000, so both get rank 1. Next salary gets rank 3 (rank 2 is skipped)
- Sales: Daniel gets rank 1, Ethan gets rank 2

---

### DENSE_RANK() Function

Assigns ranks without skipping numbers:

```sql
SELECT Name, Department, Salary,
       DENSE_RANK() OVER(PARTITION BY Department ORDER BY Salary DESC) AS emp_dense_rank
FROM employee;
```

**Output:**

| Name | Department | Salary | emp_dense_rank |
|------|------------|--------|----------------|
| Andrew | Finance | 50000 | 1 |
| Brian | Finance | 50000 | 1 |
| Charles | Finance | 20000 | 2 |
| Daniel | Sales | 30000 | 1 |
| Ethan | Sales | 20000 | 2 |

**Explanation:**
- Finance: Andrew and Brian get rank 1. Charles gets rank 2 (no gap)
- Sales: Daniel gets rank 1, Ethan gets rank 2

---

### ROW_NUMBER() Function

Assigns unique numbers to each row:

```sql
SELECT Name, Department, Salary,
       ROW_NUMBER() OVER(PARTITION BY Department ORDER BY Salary DESC) AS emp_row_no
FROM employee;
```

**Output:**

| Name | Department | Salary | emp_row_no |
|------|------------|--------|------------|
| Andrew | Finance | 50000 | 1 |
| Brian | Finance | 50000 | 2 |
| Charles | Finance | 20000 | 3 |
| Daniel | Sales | 30000 | 1 |
| Ethan | Sales | 20000 | 2 |

**Explanation:**
- Each row gets a unique number, even if salaries are the same
- Finance: Andrew is 1, Brian is 2, Charles is 3
- Sales: Daniel is 1, Ethan is 2

---

### PERCENT_RANK() Function

Shows relative position as a percentage:

**Formula:** `PERCENT_RANK() = (RANK - 1) / (Total Rows in Partition - 1)`

```sql
SELECT Name, Department, Salary,
       PERCENT_RANK() OVER(PARTITION BY Department ORDER BY Salary DESC) AS emp_percent_rank
FROM employee;
```

**Output:**

| Name | Department | Salary | emp_percent_rank |
|------|------------|--------|------------------|
| Andrew | Finance | 50000 | 0.00 |
| Brian | Finance | 50000 | 0.00 |
| Charles | Finance | 20000 | 1.00 |
| Daniel | Sales | 30000 | 0.00 |
| Ethan | Sales | 20000 | 1.00 |

**Explanation:**
- Finance: Andrew & Brian (highest) = 0.00, Charles (lowest) = 1.00
- Sales: Daniel (highest) = 0.00, Ethan (lowest) = 1.00
- Shows relative position within department

---

### Fixing Window Function Issues

| Issue | Solution |
|-------|----------|
| **Incorrect Partitioning** | Without PARTITION BY, whole table is one group |
| **Wrong Order** | ORDER BY controls calculation order in the window |
| **Slow Performance** | Use indexes on partitioned/ordered columns |

---

## Stored Procedures

Stored procedures group SQL statements and business logic into a single reusable unit that runs inside the database.

**Why Use Stored Procedures?**
- Execute multiple SQL statements as one operation
- Accept input and return output values
- Reduce repeated code in applications
- Enforce consistent business rules across systems

### Syntax:

```sql
CREATE PROCEDURE procedure_name
    (parameter1 data_type, parameter2 data_type, ...)
AS
BEGIN
    -- SQL statements to be executed
END
```

**Components:**
- `CREATE PROCEDURE`: Creates a new stored procedure
- `@parameters`: Used to pass values into the procedure
- `BEGIN...END`: Contains the SQL code that runs

---

### Types of Stored Procedures

| Type | Description | Example |
|------|-------------|---------|
| **System Stored Procedures** | Predefined by SQL Server for admin tasks | sp_help, sp_rename |
| **User-Defined Procedures** | Created by users for business operations | Calculate sales, process orders |
| **Extended Stored Procedures** | Execute external programs (C, C++) | Connect to external tools |
| **CLR Stored Procedures** | Written in .NET languages (C#) | Complex string processing, web services |

---

### Benefits of Stored Procedures

| Benefit | Description |
|---------|-------------|
| **Performance Optimization** | Precompiled, runs faster |
| **Security** | Limits direct table access, protects sensitive data |
| **Code Reusability** | Same procedure used in many applications |
| **Reduced Network Traffic** | Multiple operations in one call |
| **Easy Maintenance** | Update once, affects all uses |

---

### Example: Creating a Stored Procedure

```sql
CREATE PROCEDURE GetCustomersByCountry
    @Country VARCHAR(50)
AS
BEGIN
    SELECT CustomerName, ContactName
    FROM Customers
    WHERE Country = @Country;
END;
```

**Executing the Procedure:**

```sql
EXEC GetCustomersByCountry @Country = 'Sri lanka';
```

**Output:**

| CustomerName | ContactName |
|--------------|-------------|
| Customer1 | Contact1 |
| Customer2 | Contact2 |

**Explanation:**
- Procedure takes Country as parameter
- Returns CustomerName and ContactName for that country
- Can be reused for any country

---

### Real-World Use Cases

| Use Case | Description |
|----------|-------------|
| **Order Processing** | Insert orders, update stock, generate invoices |
| **Employee Management** | Calculate salaries, deduct taxes, create payslips |
| **Data Validation** | Check existing data before inserting new records |
| **Audit Logs** | Track changes to sensitive data for security |

---

### Best Practices for Stored Procedures

1. **Keep it Simple**: Write small, clear procedures for better readability
2. **Use Error Handling**: Use TRY...CATCH to manage errors safely
3. **Avoid Cursors**: Use set-based queries instead when possible
4. **Use Parameters**: Don't hardcode values; use parameters for flexibility
5. **Document Code**: Add comments explaining complex logic

---

## SQL Triggers

A trigger is a special stored procedure that runs automatically when an INSERT, UPDATE, or DELETE operation occurs on a table.

**Why Use Triggers?**
- Automatically update related tables
- Enforce business rules and data integrity
- Keep audit logs of data changes
- Perform actions when data is modified

### Syntax:

```sql
CREATE TRIGGER [trigger_name]
[BEFORE | AFTER]
{INSERT | UPDATE | DELETE}
ON [table_name]
FOR EACH ROW
BEGIN
    -- SQL statements
END;
```

**Components:**
- Trigger has a name and runs on a specific table
- Activates before or after an action
- Runs SQL code automatically for each affected row

---

### Example: Automatically Track When a User Record Is Updated

**Step 1: Create Main Table**

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(100),
    updated_at TIMESTAMP
);
```

**Step 2: Create Trigger**

```sql
CREATE TRIGGER update_timestamp
BEFORE UPDATE ON users
FOR EACH ROW
BEGIN
    SET NEW.updated_at = CURRENT_TIMESTAMP;
END;
```

**Step 3: Insert Sample Data**

```sql
INSERT INTO users (id, name, email)
VALUES (1, 'Amit', 'amit@example.com');
```

**Output:**

| id | name | email | updated_at |
|----|------|-------|------------|
| 1 | Amit | amit@example.com | NULL |

**Step 4: Update a Record**

```sql
UPDATE users
SET email = 'amit_new@example.com'
WHERE id = 1;
```

**Output:**

| id | name | email | updated_at |
|----|------|-------|------------|
| 1 | Amit | amit_new@example.com | 2026-03-27 14:30:45 |

**Explanation:** Trigger automatically updates updated_at field when record is modified.

---

## Types of SQL Triggers

### 1. DDL Triggers

Run when DDL commands (CREATE, ALTER, DROP) are used.

**Use Case:** Track or prevent changes to database structure.

**Example: Prevent Table Deletions**

```sql
CREATE TRIGGER prevent_table_creation
ON DATABASE
FOR CREATE_TABLE, ALTER_TABLE, DROP_TABLE
AS
BEGIN
    PRINT 'You cannot create, drop, or alter tables in this database';
    ROLLBACK;
END;
```

---

### 2. DML Triggers

Fire when data is manipulated (INSERT, UPDATE, DELETE).

**Use Case:** Validate data, log changes, cascade updates.

**Example: Prevent Unauthorized Updates**

```sql
CREATE TRIGGER prevent_update
ON students
FOR UPDATE, INSERT, DELETE
AS
BEGIN
    RAISERROR ('You cannot insert, update, or delete rows in this table.', 16, 1);
END;
```

**Note:** RAISERROR prevents the DML operation from completing.

---

### 3. Logon Triggers

Run when a user logs in.

**Use Case:** Track logins, control access, limit sessions.

**Example: Track User Logins**

```sql
CREATE TRIGGER track_logon
ON LOGON
AS
BEGIN
    PRINT 'A new user has logged in.';
END;
```

---

### Real-World Use Cases

#### 1. Automatically Updating Related Tables

```sql
CREATE TRIGGER update_student_score
AFTER UPDATE ON student_grades
FOR EACH ROW
BEGIN
    UPDATE total_scores
    SET score = score + NEW.grade
    WHERE student_id = NEW.student_id;
END;
```

**Use Case:** When grades are updated, total score updates automatically.

---

#### 2. Data Validation (Before Insert)

```sql
CREATE TRIGGER validate_grade
BEFORE INSERT ON student_grades
FOR EACH ROW
BEGIN
    IF NEW.grade < 0 OR NEW.grade > 100 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Invalid grade value.';
    END IF;
END;
```

**Use Case:** Ensures grades are between 0 and 100 before insertion.

---

### Viewing and Managing Triggers

List all triggers in SQL Server:

```sql
SELECT name, is_instead_of_trigger
FROM sys.triggers
WHERE type = 'TR';
```

**Output:**
- `name`: The name of the trigger
- `is_instead_of_trigger`: Whether it's an INSTEAD OF trigger
- `type = 'TR'`: Filters to show only triggers

---

### BEFORE and AFTER Triggers

| Type | When It Runs | Use Case |
|------|--------------|----------|
| **BEFORE** | Before the action | Validation, changing values |
| **AFTER** | After the action | Logging, updating other tables |

**Example: BEFORE Trigger for Calculations**

```sql
CREATE TRIGGER stud_marks
AFTER INSERT ON Student
FOR EACH ROW
BEGIN
    UPDATE Student
    SET
        total = NEW.subj1 + NEW.subj2 + NEW.subj3,
        per = (NEW.subj1 + NEW.subj2 + NEW.subj3) * 60.0 / 100
    WHERE tid = NEW.tid;
END;
```

**Use Case:** Automatically calculates total and percentage when marks are inserted.

---

## Performance Tuning

SQL performance tuning optimizes queries to make database operations faster and more efficient.

**Why Performance Tuning?**
- Reduces response times
- Minimizes server load
- Improves resource utilization
- Prevents downtime

**Factors Affecting Performance:**
- Table size (millions of rows)
- Complex joins
- Aggregations on large datasets
- Concurrency (multiple simultaneous queries)
- Missing or improper indexes

---

### Ways to Find Slow SQL Queries

#### 1. Creating an Execution Plan

SQL Server Management Studio shows how SQL Server processes a query.

**Steps:**
1. Select "Database Engine Query" from toolbar
2. Enter the query
3. Select "Include Actual Execution Plan" from Query option
4. Press F5 or click "Execute"
5. View execution plan in "Execution Pane" tab

**Use Case:** Identify missing indexes or unnecessary table scans.

---

#### 2. Monitor Resource Usage

Track CPU, memory, and disk usage using Windows Performance Monitor.

**Use Case:** Highlight performance bottlenecks.

---

#### 3. Use SQL DMVs to Find Slow Queries

Dynamic Management Views (DMVs) track query performance.

**Example:**

```sql
SELECT * FROM sys.dm_exec_query_stats;
```

**Use Case:** Track query performance, execution plans, resource consumption.

---

## SQL Query Optimization Techniques

### 1. SELECT Fields Instead of SELECT *

Using SELECT * retrieves all columns, increasing processing time.

**Inefficient:**
```sql
SELECT * FROM GeeksTable;
```

**Efficient:**
```sql
SELECT FirstName, LastName, Address, City, State, Zip
FROM GeeksTable;
```

**Why:** Only query the data you actually need.

---

### 2. Avoid SELECT DISTINCT

SELECT DISTINCT groups every field, requiring lots of computing power.

**Inefficient:**
```sql
SELECT DISTINCT FirstName, LastName, State
FROM GeeksTable;
```

**Efficient:**
```sql
SELECT FirstName, LastName, State
FROM GeeksTable
WHERE State IS NOT NULL;
```

**Why:** Refine query to return unique results naturally.

---

### 3. Use INNER JOIN Instead of WHERE for Joins

Joining with WHERE can be inefficient.

**Inefficient:**
```sql
SELECT GFG1.CustomerID, GFG1.Name, GFG1.LastSaleDate
FROM GFG1, GFG2
WHERE GFG1.CustomerID = GFG2.CustomerID;
```

**Efficient:**
```sql
SELECT GFG1.CustomerID, GFG1.Name, GFG1.LastSaleDate
FROM GFG1
INNER JOIN GFG2 ON GFG1.CustomerID = GFG2.CustomerID;
```

**Why:** INNER JOIN is more efficient for combining tables.

---

### 4. Use WHERE Instead of HAVING

HAVING is used after aggregation and can be less efficient.

**Inefficient:**
```sql
SELECT GFG1.CustomerID, GFG1.Name, GFG1.LastSaleDate
FROM GFG1
INNER JOIN GFG2 ON GFG1.CustomerID = GFG2.CustomerID
GROUP BY GFG1.CustomerID, GFG1.Name
HAVING GFG2.LastSaleDate BETWEEN '1/1/2019' AND '12/31/2019';
```

**Efficient:**
```sql
SELECT GFG1.CustomerID, GFG1.Name, GFG1.LastSaleDate
FROM GFG1
INNER JOIN GFG2 ON GFG1.CustomerID = GFG2.CustomerID
WHERE GFG2.LastSaleDate BETWEEN '1/1/2019' AND '12/31/2019'
GROUP BY GFG1.CustomerID, GFG1.Name;
```

**Why:** WHERE filters before aggregation, speeding up the query.

---

### 5. Limit Wildcards to the End

Wildcards at the beginning make it hard to use indexes.

**Inefficient:**
```sql
SELECT City FROM GeekTable WHERE City LIKE '%No%';
```

**Efficient:**
```sql
SELECT City FROM GeekTable WHERE City LIKE 'No%';
```

**Why:** Placing wildcards at the end allows efficient index usage.

---

### 6. Use LIMIT for Sampling Query Results

Limit results to avoid querying the entire table during testing.

```sql
SELECT GFG1.CustomerID, GFG1.Name, GFG1.LastSaleDate
FROM GFG1
INNER JOIN GFG2 ON GFG1.CustomerID = GFG2.CustomerID
WHERE GFG2.LastSaleDate BETWEEN '1/1/2019' AND '12/31/2019'
GROUP BY GFG1.CustomerID, GFG1.Name
LIMIT 10;
```

**Why:** Avoid stressing the database with large queries during testing.

---

### 7. Run Queries During Off-Peak Hours

Run heavy queries when the database is less busy.

**Why:** Reduces load on the database and minimizes impact on other users.

---

## Index Tuning

Index tuning involves selecting and building indexes to speed up query processing.

**Key Points:**
- Short indexes for reduced disk space
- Distinct indexes with minimal duplicates
- Clustered indexes covering all row data
- Static data columns for clustered indexes

**Best Practices:**
- Create indexes on frequently queried columns
- Avoid over-indexing (slows down INSERT/UPDATE)
- Use composite indexes for multi-column queries
- Regularly rebuild fragmented indexes

---

## SQL Performance Tuning Tools

| Tool | Description |
|------|-------------|
| **SQL Sentry (SolarWinds)** | Monitor and optimize performance |
| **SQL Profiler (Microsoft)** | Track query execution and performance |
| **SQL Index Manager (Red Gate)** | Manage and optimize indexes |
| **SQL Diagnostic Manager (IDERA)** | Identify slow queries and bottlenecks |

---

## SQL Transactions

A transaction groups one or more SQL operations into a single unit of work to ensure reliable data processing.

**Why Use Transactions?**
- Ensures consistency by committing all changes together
- Rolls back changes if any operation fails
- Maintains data integrity

---

### ACID Properties

The reliability of transactions is ensured by ACID properties:

| Property | Description |
|----------|-------------|
| **Atomicity** | All operations succeed or all fail |
| **Consistency** | Database moves from one valid state to another |
| **Isolation** | Transactions run independently |
| **Durability** | Committed changes remain permanent |

---

## Transaction Control Commands

### 1. BEGIN TRANSACTION

Starts a new transaction.

**Syntax:**
```sql
BEGIN TRANSACTION transaction_name;
```

---

### Example: Bank Transfer Transaction

```sql
BEGIN TRANSACTION;

-- Deduct $150 from Account A
UPDATE Accounts SET Balance = Balance - 150 WHERE AccountID = 'A';

-- Add $150 to Account B
UPDATE Accounts SET Balance = Balance + 150 WHERE AccountID = 'B';

-- Commit if both operations succeed
COMMIT;
```

**Explanation:**
- Both UPDATE queries are part of a single transaction
- If all updates succeed, COMMIT saves the changes
- If any update fails, ROLLBACK cancels all changes

---

### 2. COMMIT Command

Saves all changes made during the transaction permanently.

**Syntax:**
```sql
COMMIT;
```

**Example:**

```sql
DELETE FROM Student WHERE AGE = 20;
COMMIT;
```

**Result:**
- Rows with AGE = 20 are deleted
- Deletion is now permanent and cannot be undone

---

### 3. ROLLBACK Command

Undoes all changes in the current transaction.

**Syntax:**
```sql
ROLLBACK;
```

**Example:**

```sql
DELETE FROM Student WHERE AGE = 20;
ROLLBACK;
```

**Result:**
- Rows with AGE = 20 are deleted temporarily
- ROLLBACK restores the table to its original state

---

### 4. SAVEPOINT Command

Creates a marker inside a transaction to rollback to a specific point.

**Syntax:**
```sql
SAVEPOINT SAVEPOINT_NAME;
```

**Example:**

```sql
SAVEPOINT SP1;
DELETE FROM Student WHERE AGE = 20;
SAVEPOINT SP2;
```

**Result:**
- Create SP1 before deleting
- Delete students with age 20
- Create SP2 after deleting

---

### 5. ROLLBACK TO SAVEPOINT

Rolls back to a specific savepoint.

**Syntax:**
```sql
ROLLBACK TO SAVEPOINT SAVEPOINT_NAME;
```

**Example:**

```sql
ROLLBACK TO SP1;
```

**Result:**
- Undo the delete
- Students with age 20 come back
- Table goes back to how it was at SP1

---

### 6. RELEASE SAVEPOINT Command

Removes a savepoint so you can't rollback to it.

**Syntax:**
```sql
RELEASE SAVEPOINT SAVEPOINT_NAME;
```

**Example:**

```sql
RELEASE SAVEPOINT SP2;
```

**Result:**
- SP2 is removed
- Cannot do ROLLBACK TO SP2 anymore
- Table does not change

---

## Complete Transaction Example

### Example Setup: Student Table

```sql
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50),
    Age INT
);

INSERT INTO Student VALUES
(1, 'Alice', 20),
(2, 'Bob', 22),
(3, 'Charlie', 20),
(4, 'Diana', 23);
```

**Student Table:**

| StudentID | Name | Age |
|-----------|------|-----|
| 1 | Alice | 20 |
| 2 | Bob | 22 |
| 3 | Charlie | 20 |
| 4 | Diana | 23 |

### Transaction with Savepoints:

```sql
BEGIN TRANSACTION;

-- Create first savepoint
SAVEPOINT SP1;

-- Delete students with age 20
DELETE FROM Student WHERE Age = 20;

-- Create second savepoint
SAVEPOINT SP2;

-- Update Bob's age
UPDATE Student SET Age = 25 WHERE Name = 'Bob';

-- Rollback to SP1 (undo both delete and update)
ROLLBACK TO SP1;

-- Commit the transaction
COMMIT;
```

**Result:** All changes after SP1 are undone. Table remains unchanged.

---

## Types of SQL Transactions

| Type | Description |
|------|-------------|
| **Read Transactions** | Only read data (SELECT queries) |
| **Write Transactions** | Modify data (INSERT, UPDATE, DELETE) |
| **Distributed Transactions** | Span multiple databases |
| **Implicit Transactions** | Automatically started by SQL Server |
| **Explicit Transactions** | Manually controlled with BEGIN, COMMIT, ROLLBACK |

---

## Transaction Best Practices

### 1. Keep Transactions Small
- Improves performance
- Reduces lock contention
- Minimizes blocking

### 2. Monitor Locks
- Avoid conflicts and deadlocks
- Use appropriate isolation levels

### 3. Use Batching
- Handle large data operations efficiently
- Break into smaller chunks

### 4. Error Handling
- Always use TRY...CATCH
- Rollback on errors

**Example:**

```sql
BEGIN TRANSACTION;

BEGIN TRY
    UPDATE Accounts SET Balance = Balance - 150 WHERE AccountID = 'A';
    UPDATE Accounts SET Balance = Balance + 150 WHERE AccountID = 'B';
    COMMIT;
END TRY
BEGIN CATCH
    ROLLBACK;
    PRINT 'Transaction failed and rolled back';
END CATCH;
```

---

## Real-World Transaction Example: Banking System

```sql
BEGIN TRANSACTION;

DECLARE @AccountA_Balance DECIMAL(10,2);
DECLARE @AccountB_Balance DECIMAL(10,2);
DECLARE @TransferAmount DECIMAL(10,2) = 500.00;

-- Check Account A balance
SELECT @AccountA_Balance = Balance FROM Accounts WHERE AccountID = 'A';

IF @AccountA_Balance >= @TransferAmount
BEGIN
    -- Deduct from Account A
    UPDATE Accounts SET Balance = Balance - @TransferAmount WHERE AccountID = 'A';
    
    -- Add to Account B
    UPDATE Accounts SET Balance = Balance + @TransferAmount WHERE AccountID = 'B';
    
    -- Log the transaction
    INSERT INTO TransactionLog (FromAccount, ToAccount, Amount, TransactionDate)
    VALUES ('A', 'B', @TransferAmount, GETDATE());
    
    COMMIT;
    PRINT 'Transfer successful';
END
ELSE
BEGIN
    ROLLBACK;
    PRINT 'Insufficient funds';
END
```

**Explanation:**
- Checks if Account A has sufficient balance
- Deducts from Account A and adds to Account B
- Logs the transaction
- Commits if successful, rolls back if insufficient funds

---

## Quick Reference Tables

### Subquery Types

| Type | Returns | Operator | Example |
|------|---------|----------|---------|
| **Single-Row** | One value | =, >, < | `WHERE Salary = (SELECT MAX(Salary)...)` |
| **Multi-Row** | Multiple values | IN, ANY, ALL | `WHERE ID IN (SELECT ID...)` |
| **Correlated** | Depends on outer query | Any | `WHERE Salary > (SELECT AVG(Salary)... WHERE Dept = e.Dept)` |

---

### Window Functions

| Function | Type | Purpose |
|----------|------|---------|
| **SUM()** | Aggregate | Calculate running total |
| **AVG()** | Aggregate | Calculate running average |
| **COUNT()** | Aggregate | Count rows in window |
| **RANK()** | Ranking | Rank with gaps |
| **DENSE_RANK()** | Ranking | Rank without gaps |
| **ROW_NUMBER()** | Ranking | Unique row numbers |
| **PERCENT_RANK()** | Ranking | Relative position (0-1) |

---

### Transaction Commands

| Command | Purpose | Example |
|---------|---------|---------|
| **BEGIN TRANSACTION** | Start transaction | `BEGIN TRANSACTION;` |
| **COMMIT** | Save changes | `COMMIT;` |
| **ROLLBACK** | Undo changes | `ROLLBACK;` |
| **SAVEPOINT** | Create marker | `SAVEPOINT SP1;` |
| **ROLLBACK TO** | Rollback to marker | `ROLLBACK TO SP1;` |
| **RELEASE SAVEPOINT** | Remove marker | `RELEASE SAVEPOINT SP1;` |

---

### Performance Optimization Tips

| Technique | Benefit |
|-----------|---------|
| **SELECT specific columns** | Reduces data transfer |
| **Use INNER JOIN** | More efficient than WHERE joins |
| **Use WHERE instead of HAVING** | Filters before aggregation |
| **Limit wildcards** | Allows index usage |
| **Use LIMIT** | Reduces result set size |
| **Create indexes** | Speeds up queries |
| **Run during off-peak** | Reduces load |

---

## Key Takeaways

### Subqueries:
- Nested queries for complex filtering
- Three types: single-row, multi-row, correlated
- Use in WHERE, FROM, and HAVING clauses
- Can be used with SELECT, INSERT, UPDATE, DELETE

### Window Functions:
- Perform calculations across related rows
- Use OVER clause with PARTITION BY and ORDER BY
- Aggregate functions: SUM(), AVG(), COUNT()
- Ranking functions: RANK(), DENSE_RANK(), ROW_NUMBER()

### Stored Procedures:
- Group SQL statements into reusable units
- Accept parameters and return values
- Improve performance and security
- Reduce code duplication

### Triggers:
- Automatically execute on INSERT, UPDATE, DELETE
- Three types: DDL, DML, Logon
- Use BEFORE or AFTER timing
- Enforce business rules and data integrity

### Performance Tuning:
- Use execution plans to identify bottlenecks
- Optimize queries with proper indexing
- Avoid SELECT *, use specific columns
- Use WHERE instead of HAVING when possible
- Monitor resource usage regularly

### Transactions:
- Group operations into atomic units
- ACID properties ensure reliability
- Use COMMIT to save, ROLLBACK to undo
- SAVEPOINT for partial rollbacks
- Essential for data consistency and integrity
