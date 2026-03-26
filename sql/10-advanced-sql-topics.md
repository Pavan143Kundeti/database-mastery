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
