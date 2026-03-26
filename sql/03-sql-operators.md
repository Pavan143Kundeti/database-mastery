# SQL Operators

## What are SQL Operators?

SQL operators are symbols or keywords used to perform operations on data in SQL queries.

**What They Do:**
- Perform calculations, comparisons, and logical checks
- Enable filtering, calculating, and updating data
- Essential for query optimization and accurate data management

---

## 1. Arithmetic Operators

Arithmetic operators perform mathematical operations on numeric data.

| Operator | Description | Example |
|----------|-------------|---------|
| `+` | Addition | `Salary + Bonus` |
| `-` | Subtraction | `Salary - Deduction` |
| `*` | Multiplication | `Price * Quantity` |
| `/` | Division | `Total / Count` |
| `%` | Modulus (remainder) | `Number % 10` |

### Example:

```sql
-- Create table
CREATE TABLE Employees (
    EmpID INT,
    EmpName VARCHAR(50),
    Salary INT,
    Bonus INT
);

-- Insert data
INSERT INTO Employees (EmpID, EmpName, Salary, Bonus) VALUES
(1, 'Amit', 40000, 5000),
(2, 'Neha', 50000, 7000),
(3, 'Ravi', 30000, 3000);

-- Use arithmetic operators
SELECT
    EmpName,
    Salary,
    Bonus,
    Salary + Bonus AS Total_Income,
    Salary - Bonus AS After_Bonus_Deduction,
    Salary * 0.10 AS Ten_Percent_Salary,
    Salary / 12 AS Monthly_Salary,
    Salary % 10000 AS Salary_Remainder
FROM Employees;
```

### Output:

| EmpName | Salary | Bonus | Total_Income | After_Bonus_Deduction | Ten_Percent_Salary | Monthly_Salary | Salary_Remainder |
|---------|--------|-------|--------------|----------------------|-------------------|----------------|------------------|
| Amit | 40000 | 5000 | 45000 | 35000 | 4000 | 3333.33 | 0 |
| Neha | 50000 | 7000 | 57000 | 43000 | 5000 | 4166.67 | 0 |
| Ravi | 30000 | 3000 | 33000 | 27000 | 3000 | 2500 | 0 |

---

## 2. Comparison Operators

Comparison operators compare values and return TRUE or FALSE.

| Operator | Description | Example |
|----------|-------------|---------|
| `=` | Equal to | `Marks = 85` |
| `!=` or `<>` | Not equal to | `Age != 18` |
| `>` | Greater than | `Salary > 50000` |
| `<` | Less than | `Marks < 60` |
| `>=` | Greater than or equal to | `Age >= 18` |
| `<=` | Less than or equal to | `Price <= 100` |

### Example:

```sql
-- Create table
CREATE TABLE Students (
    ID INT,
    Name VARCHAR(50),
    Marks INT
);

-- Insert data
INSERT INTO Students VALUES
(1, 'Amit', 85),
(2, 'Neha', 70),
(3, 'Ravi', 55);

-- Use comparison operator
SELECT * FROM Students WHERE Marks >= 70;
```

### Output:

| ID | Name | Marks |
|----|------|-------|
| 1 | Amit | 85 |
| 2 | Neha | 70 |

**Explanation:** Only students with marks 70 or more are selected.

---

## 3. Logical Operators

Logical operators combine or manipulate conditions.

| Operator | Description | Example |
|----------|-------------|---------|
| `AND` | Both conditions must be TRUE | `Marks >= 70 AND Age >= 18` |
| `OR` | At least one condition must be TRUE | `Marks < 60 OR Age < 18` |
| `NOT` | Reverses the condition | `NOT Marks >= 70` |

### Example:

```sql
-- Create table
CREATE TABLE Students (
    ID INT,
    Name VARCHAR(50),
    Marks INT,
    Age INT
);

-- Insert data
INSERT INTO Students VALUES
(1, 'Amit', 85, 18),
(2, 'Neha', 70, 19),
(3, 'Ravi', 55, 17);

-- AND: Both conditions must be true
SELECT * FROM Students WHERE Marks >= 70 AND Age >= 18;

-- OR: Either condition can be true
SELECT * FROM Students WHERE Marks < 60 OR Age < 18;

-- NOT: Reverses the condition
SELECT * FROM Students WHERE NOT Marks >= 70;
```

### Output Examples:

**AND Query:**
| ID | Name | Marks | Age |
|----|------|-------|-----|
| 1 | Amit | 85 | 18 |
| 2 | Neha | 70 | 19 |

**OR Query:**
| ID | Name | Marks | Age |
|----|------|-------|-----|
| 3 | Ravi | 55 | 17 |

**NOT Query:**
| ID | Name | Marks | Age |
|----|------|-------|-----|
| 3 | Ravi | 55 | 17 |

---

## 4. Bitwise Operators

Bitwise operators perform operations on individual bits of binary values.

| Operator | Description | Example |
|----------|-------------|---------|
| `&` | Bitwise AND | `Permissions & 2` |
| `|` | Bitwise OR | `Permissions | 4` |
| `^` | Bitwise XOR (exclusive OR) | `Permissions ^ 2` |
| `~` | Bitwise NOT | `~Permissions` |

### Example: Permission System

```sql
-- Create table
CREATE TABLE Users (
    UserID INT,
    UserName VARCHAR(50),
    Permissions INT  -- Stores permission flags
);

-- Insert data
-- Permission flags: Read=1, Write=2, Execute=4
INSERT INTO Users (UserID, UserName, Permissions) VALUES
(1, 'Amit', 1),   -- Read only
(2, 'Neha', 3),   -- Read + Write (1+2=3)
(3, 'Ravi', 7);   -- Read + Write + Execute (1+2+4=7)

-- Check if user has Write permission (Bitwise AND)
SELECT * FROM Users WHERE Permissions & 2 = 2;

-- Add Execute permission (Bitwise OR)
UPDATE Users SET Permissions = Permissions | 4 WHERE UserName = 'Neha';

-- Remove Read permission (Bitwise AND + NOT)
UPDATE Users SET Permissions = Permissions & ~1 WHERE UserName = 'Ravi';

-- Toggle Write permission (Bitwise XOR)
UPDATE Users SET Permissions = Permissions ^ 2 WHERE UserName = 'Amit';
```

### Understanding Binary Permissions:

```
Read    = 1 = 001 (binary)
Write   = 2 = 010 (binary)
Execute = 4 = 100 (binary)

Read + Write = 3 = 011 (binary)
All three    = 7 = 111 (binary)
```

---

## 5. Compound Operators

Compound operators combine an operation with assignment in one step.

| Operator | Description | Example |
|----------|-------------|---------|
| `+=` | Add and assign | `Salary = Salary + 5000` |
| `-=` | Subtract and assign | `Salary = Salary - 2000` |
| `*=` | Multiply and assign | `Salary = Salary * 2` |
| `/=` | Divide and assign | `Salary = Salary / 2` |
| `%=` | Modulus and assign | `Salary = Salary % 10000` |

### Example:

```sql
-- Create table
CREATE TABLE Employees (
    EmpID INT,
    EmpName VARCHAR(50),
    Salary INT
);

-- Insert data
INSERT INTO Employees VALUES
(1, 'Amit', 40000),
(2, 'Neha', 50000),
(3, 'Ravi', 30000);

-- Increase salary by 5000
UPDATE Employees SET Salary = Salary + 5000;

-- Reduce salary by 2000
UPDATE Employees SET Salary = Salary - 2000 WHERE EmpName = 'Ravi';

-- Double salary
UPDATE Employees SET Salary = Salary * 2 WHERE EmpName = 'Neha';

-- Divide salary
UPDATE Employees SET Salary = Salary / 2 WHERE EmpName = 'Amit';
```

---

## 6. Special Operators

Special operators serve specific functions for filtering and checking data.

| Operator | Description | Example |
|----------|-------------|---------|
| `BETWEEN` | Checks if value is within a range | `Marks BETWEEN 60 AND 90` |
| `IN` | Checks if value matches any in a list | `Name IN ('Amit', 'Ravi')` |
| `LIKE` | Pattern matching on strings | `Name LIKE 'N%'` |
| `IS NULL` | Checks if value is NULL | `Marks IS NULL` |
| `IS NOT NULL` | Checks if value is not NULL | `Marks IS NOT NULL` |
| `EXISTS` | Checks if subquery returns any rows | `EXISTS (SELECT ...)` |

### Example:

```sql
-- Create table
CREATE TABLE Students (
    ID INT,
    Name VARCHAR(50),
    Marks INT
);

-- Insert data
INSERT INTO Students VALUES
(1, 'Amit', 85),
(2, 'Neha', 70),
(3, 'Ravi', 55),
(4, 'Kiran', NULL);

-- BETWEEN: Range check
SELECT * FROM Students WHERE Marks BETWEEN 60 AND 90;

-- IN: Match any value in list
SELECT * FROM Students WHERE Name IN ('Amit', 'Ravi');

-- LIKE: Pattern matching
SELECT * FROM Students WHERE Name LIKE 'N%';  -- Names starting with N

-- IS NULL: Check for NULL values
SELECT * FROM Students WHERE Marks IS NULL;

-- EXISTS: Check if subquery returns rows
SELECT * FROM Students s
WHERE EXISTS (
    SELECT * FROM Students WHERE Marks > 80
);
```

### Output Examples:

**BETWEEN Query:**
| ID | Name | Marks |
|----|------|-------|
| 1 | Amit | 85 |
| 2 | Neha | 70 |

**IN Query:**
| ID | Name | Marks |
|----|------|-------|
| 1 | Amit | 85 |
| 3 | Ravi | 55 |

**LIKE Query:**
| ID | Name | Marks |
|----|------|-------|
| 2 | Neha | 70 |

**IS NULL Query:**
| ID | Name | Marks |
|----|------|-------|
| 4 | Kiran | NULL |

---

## LIKE Pattern Matching

The LIKE operator uses wildcards for pattern matching:

| Wildcard | Description | Example | Matches |
|----------|-------------|---------|---------|
| `%` | Zero or more characters | `'A%'` | Amit, Anil, Ajay |
| `_` | Exactly one character | `'_mit'` | Amit, Smit |
| `%text%` | Contains text | `'%kumar%'` | Rajkumar, Kumar, Kumari |
| `text%` | Starts with text | `'Raj%'` | Raj, Rajesh, Rajkumar |
| `%text` | Ends with text | `'%kumar'` | Rajkumar, Ashok kumar |

### Examples:

```sql
-- Names starting with 'A'
SELECT * FROM Students WHERE Name LIKE 'A%';

-- Names ending with 'i'
SELECT * FROM Students WHERE Name LIKE '%i';

-- Names containing 'eh'
SELECT * FROM Students WHERE Name LIKE '%eh%';

-- Names with exactly 4 characters
SELECT * FROM Students WHERE Name LIKE '____';
```

---

## Operator Precedence

When multiple operators are used, SQL follows this order:

1. `()` - Parentheses (highest priority)
2. `*`, `/`, `%` - Multiplication, Division, Modulus
3. `+`, `-` - Addition, Subtraction
4. `=`, `!=`, `<`, `>`, `<=`, `>=` - Comparison
5. `NOT` - Logical NOT
6. `AND` - Logical AND
7. `OR` - Logical OR (lowest priority)

### Example:

```sql
-- Without parentheses
SELECT * FROM Students WHERE Marks > 60 OR Age < 18 AND Name = 'Amit';
-- Evaluated as: Marks > 60 OR (Age < 18 AND Name = 'Amit')

-- With parentheses (clearer)
SELECT * FROM Students WHERE (Marks > 60 OR Age < 18) AND Name = 'Amit';
```

---

## Quick Reference

| Category | Operators | Use For |
|----------|-----------|---------|
| **Arithmetic** | `+`, `-`, `*`, `/`, `%` | Math calculations |
| **Comparison** | `=`, `!=`, `>`, `<`, `>=`, `<=` | Comparing values |
| **Logical** | `AND`, `OR`, `NOT` | Combining conditions |
| **Bitwise** | `&`, `|`, `^`, `~` | Binary operations |
| **Compound** | `+=`, `-=`, `*=`, `/=`, `%=` | Update with operation |
| **Special** | `BETWEEN`, `IN`, `LIKE`, `IS NULL`, `EXISTS` | Advanced filtering |

---

## Key Takeaways

- Operators perform operations on data in SQL queries
- Arithmetic operators: `+`, `-`, `*`, `/`, `%` for calculations
- Comparison operators: `=`, `!=`, `>`, `<` for comparing values
- Logical operators: `AND`, `OR`, `NOT` for combining conditions
- Special operators: `BETWEEN`, `IN`, `LIKE`, `IS NULL` for advanced filtering
- Use parentheses to control operator precedence
- LIKE operator uses `%` and `_` wildcards for pattern matching
