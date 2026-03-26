# SQL Queries & Operations

This guide covers essential SQL queries and operations for retrieving, inserting, updating, and deleting data.

---

## Table of Contents
1. [SELECT Query](#select-query)
2. [INSERT INTO Statement](#insert-into-statement)
3. [UPDATE Statement](#update-statement)
4. [DELETE Statement](#delete-statement)
5. [WHERE Clause](#where-clause)
6. [Aliases](#aliases)

---

## SELECT Query

The SQL SELECT statement retrieves data from one or more tables and displays it in rows and columns.

**Key Features:**
- Fetches all columns using `*` or specific columns by name
- Filters and sorts data using WHERE and ORDER BY
- Supports grouping, aggregation, and relationships

### Syntax:

```sql
SELECT column1, column2, ...
FROM table_name;
```

**Parameters:**
- `column1, column2`: Columns you want to retrieve
- `table_name`: Name of the table you're querying

---

### Example Setup: Customer Table

First, let's create a Customer table for our examples:

```sql
CREATE TABLE Customer (
    CustomerID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Country VARCHAR(50),
    Age INT
);

INSERT INTO Customer VALUES
(1, 'Luca', 'Bianchi', 'Italy', 23),
(2, 'Aiko', 'Tanaka', 'Japan', 21),
(3, 'Carlos', 'Gomez', 'Spain', 24),
(4, 'Sofia', 'Müller', 'Germany', 22),
(5, 'Ethan', 'Johnson', 'USA', 25),
(6, 'Isabella', 'Rossi', 'Italy', 21);
```

**Customer Table:**

| CustomerID | FirstName | LastName | Country | Age |
|------------|-----------|----------|---------|-----|
| 1 | Luca | Bianchi | Italy | 23 |
| 2 | Aiko | Tanaka | Japan | 21 |
| 3 | Carlos | Gomez | Spain | 24 |
| 4 | Sofia | Müller | Germany | 22 |
| 5 | Ethan | Johnson | USA | 25 |
| 6 | Isabella | Rossi | Italy | 21 |

---

### Example 1: Select Specific Columns

Retrieve only CustomerID and FirstName:

```sql
SELECT CustomerID, FirstName FROM Customer;
```

**Output:**

| CustomerID | FirstName |
|------------|-----------|
| 1 | Luca |
| 2 | Aiko |
| 3 | Carlos |
| 4 | Sofia |
| 5 | Ethan |
| 6 | Isabella |

---

### Example 2: Select All Columns

Fetch all fields from the Customer table:

```sql
SELECT * FROM Customer;
```

**Output:** (Shows all columns from the Customer table)

---

### Example 3: SELECT with WHERE Clause

Filter customers who are 21 years old:

```sql
SELECT FirstName FROM Customer WHERE Age = 21;
```

**Output:**

| FirstName |
|-----------|
| Aiko |
| Isabella |

---

### Example 4: SELECT with GROUP BY Clause

Count the number of customers from each country:

```sql
SELECT Country, COUNT(*) AS customer_count
FROM Customer
GROUP BY Country;
```

**Output:**

| Country | customer_count |
|---------|----------------|
| Italy | 2 |
| Japan | 1 |
| Spain | 1 |
| Germany | 1 |
| USA | 1 |

---

### Example 5: SELECT with DISTINCT Clause

Fetch unique countries from the Customer table:

```sql
SELECT DISTINCT Country FROM Customer;
```

**Output:**

| Country |
|---------|
| Italy |
| Japan |
| Spain |
| Germany |
| USA |

---

### Example 6: SELECT with HAVING Clause

Find countries that have 2 or more customers:

```sql
SELECT Country, COUNT(*) AS customer_count
FROM Customer
GROUP BY Country
HAVING COUNT(*) >= 2;
```

**Output:**

| Country | customer_count |
|---------|----------------|
| Italy | 2 |

---

### Example 7: SELECT with ORDER BY Clause

Sort results by Age in descending order:

```sql
SELECT * FROM Customer ORDER BY Age DESC;
```

**Output:**

| CustomerID | FirstName | LastName | Country | Age |
|------------|-----------|----------|---------|-----|
| 5 | Ethan | Johnson | USA | 25 |
| 3 | Carlos | Gomez | Spain | 24 |
| 1 | Luca | Bianchi | Italy | 23 |
| 4 | Sofia | Müller | Germany | 22 |
| 2 | Aiko | Tanaka | Japan | 21 |
| 6 | Isabella | Rossi | Italy | 21 |

---

## INSERT INTO Statement

The SQL INSERT INTO statement adds new records into a table.

**Key Features:**
- Can insert a single row or multiple rows at once
- Supports inserting data directly or from another table
- Can insert into all columns or specific ones

---

### 1. Inserting Data into All Columns

Insert data into all columns without specifying column names:

**Syntax:**

```sql
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

**Example Setup: Student Table**

```sql
CREATE TABLE Student (
    ROLL_NO INT PRIMARY KEY,
    NAME VARCHAR(50),
    ADDRESS VARCHAR(100),
    PHONE VARCHAR(15),
    AGE INT
);

INSERT INTO Student (ROLL_NO, NAME, ADDRESS, PHONE, AGE)
VALUES
(1, 'Liam', 'New York', 'xxxxxxxxxx', 18),
(2, 'Sophia', 'Berlin', 'xxxxxxxxxx', 18),
(3, 'Akira', 'Tokyo', 'xxxxxxxxxx', 20),
(4, 'Carlos', 'Tokyo', 'xxxxxxxxxx', 18);
```

**Student Table:**

| ROLL_NO | NAME | ADDRESS | PHONE | AGE |
|---------|------|---------|-------|-----|
| 1 | Liam | New York | xxxxxxxxxx | 18 |
| 2 | Sophia | Berlin | xxxxxxxxxx | 18 |
| 3 | Akira | Tokyo | xxxxxxxxxx | 20 |
| 4 | Carlos | Tokyo | xxxxxxxxxx | 18 |

**Insert without column names:**

```sql
INSERT INTO Student
VALUES (5, 'Isabella', 'Rome', 'xxxxxxxxxx', 19);
```

**Result:**

| ROLL_NO | NAME | ADDRESS | PHONE | AGE |
|---------|------|---------|-------|-----|
| 5 | Isabella | Rome | xxxxxxxxxx | 19 |

---

### 2. Inserting Data into Specific Columns

Insert data into only certain columns:

**Syntax:**

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

**Example:**

```sql
INSERT INTO Student (ROLL_NO, NAME, AGE)
VALUES (6, 'Hiroshi', 19);
```

**Result:**

| ROLL_NO | NAME | ADDRESS | PHONE | AGE |
|---------|------|---------|-------|-----|
| 6 | Hiroshi | NULL | NULL | 19 |

**Note:** Columns not included are filled with NULL or default values.

---

### 3. Inserting Multiple Rows at Once

Insert multiple rows in a single query:

**Syntax:**

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES
(value1, value2, ...),
(value1, value2, ...),
(value1, value2, ...);
```

**Example:**

```sql
INSERT INTO Student (ROLL_NO, NAME, ADDRESS, PHONE, AGE)
VALUES
(7, 'Mateo Garcia', 'Madrid', 'xxxxxxxxxx', 15),
(8, 'Hana Suzuki', 'Osaka', 'xxxxxxxxxx', 18),
(9, 'Oliver Jensen', 'Copenhagen', 'xxxxxxxxxx', 17),
(10, 'Amelia Brown', 'London', 'xxxxxxxxxx', 17);
```

**Benefits:**
- Faster than multiple single inserts
- Reduces database operations
- For very large data (>1000 rows), use bulk insert

---

### 4. Inserting Data from One Table into Another

Copy data from one table to another using INSERT INTO SELECT:

**Example Setup: OldStudent Table**

```sql
CREATE TABLE OldStudent (
    ROLL_NO INT,
    NAME VARCHAR(50),
    AGE INT
);

INSERT INTO OldStudent VALUES
(11, 'Emma', 22),
(12, 'Noah', 21),
(13, 'Olivia', 23);
```

**OldStudent Table:**

| ROLL_NO | NAME | AGE |
|---------|------|-----|
| 11 | Emma | 22 |
| 12 | Noah | 21 |
| 13 | Olivia | 23 |

#### Method 1: Insert All Columns

```sql
INSERT INTO Student
SELECT * FROM OldStudent;
```

#### Method 2: Insert Specific Columns

```sql
INSERT INTO Student (NAME, AGE)
SELECT NAME, AGE FROM OldStudent;
```

**Note:** Columns not included (ROLL_NO, ADDRESS, PHONE) are filled with NULL.

#### Method 3: Insert Specific Rows Based on Condition

```sql
INSERT INTO Student
SELECT * FROM OldStudent
WHERE Age > 20;
```

**Result:** Only students older than 20 are copied.

---

## UPDATE Statement

The SQL UPDATE statement modifies existing data in a table.

**Key Features:**
- WHERE clause specifies which rows to update
- Without WHERE, all rows are modified
- Can update single or multiple columns

### Syntax:

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

**Parameters:**
- `table_name`: Table you want to update
- `SET`: Column(s) and their new values
- `WHERE`: Filters specific rows to update

**Note:** Without WHERE, all rows will be updated!

---

### Example 1: Update Single Column

Update Age to 25 for customer named 'Isabella':

```sql
UPDATE Customer
SET Age = 25
WHERE FirstName = 'Isabella';

SELECT * FROM Customer;
```

**Before:**

| CustomerID | FirstName | LastName | Country | Age |
|------------|-----------|----------|---------|-----|
| 6 | Isabella | Rossi | Italy | 21 |

**After:**

| CustomerID | FirstName | LastName | Country | Age |
|------------|-----------|----------|---------|-----|
| 6 | Isabella | Rossi | Italy | 25 |

---

### Example 2: Update Multiple Columns

Update both FirstName and Country for CustomerID 1:

```sql
UPDATE Customer
SET FirstName = 'John', Country = 'Spain'
WHERE CustomerID = 1;
```

**Before:**

| CustomerID | FirstName | LastName | Country | Age |
|------------|-----------|----------|---------|-----|
| 1 | Luca | Bianchi | Italy | 23 |

**After:**

| CustomerID | FirstName | LastName | Country | Age |
|------------|-----------|----------|---------|-----|
| 1 | John | Bianchi | Spain | 23 |

**Note:** Use comma (,) to separate multiple column updates.

---

### Example 3: UPDATE Without WHERE Clause

**Warning:** This updates ALL rows!

```sql
UPDATE Customer
SET FirstName = 'Mike';

SELECT * FROM Customer;
```

**Result:** All customers' FirstName becomes 'Mike'.

| CustomerID | FirstName | LastName | Country | Age |
|------------|-----------|----------|---------|-----|
| 1 | Mike | Bianchi | Spain | 23 |
| 2 | Mike | Tanaka | Japan | 21 |
| 3 | Mike | Gomez | Spain | 24 |
| 4 | Mike | Müller | Germany | 22 |
| 5 | Mike | Johnson | USA | 25 |
| 6 | Mike | Rossi | Italy | 25 |

**Always use WHERE clause to avoid updating all rows!**

---

## DELETE Statement

The SQL DELETE statement removes specific rows from a table while keeping the table structure intact.

**Key Features:**
- Removes rows based on conditions
- Retains table schema, constraints, and indexes
- Can delete single row, multiple rows, or all rows
- Different from DROP (which deletes entire table)

### Syntax:

```sql
DELETE FROM table_name
WHERE condition;
```

**Parameters:**
- `table_name`: Table from which to delete rows
- `WHERE`: Condition to filter rows to delete

**Note:** Without WHERE, all records are deleted!

---

### Example Setup: Employees Table

```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    NAME VARCHAR(50),
    Email VARCHAR(100),
    Department VARCHAR(50)
);

INSERT INTO Employees VALUES
(1, 'Alice', 'alice@example.com', 'Development'),
(2, 'Bob', 'bob@example.com', 'HR'),
(3, 'Charlie', 'charlie@example.com', 'Sales'),
(4, 'Diana', 'diana@example.com', 'Marketing'),
(5, 'Ethan', 'ethan@example.com', 'Sales'),
(6, 'Fiona', 'fiona@example.com', 'HR'),
(7, 'George', 'george@example.com', 'Development');
```

**Employees Table:**

| EmployeeID | NAME | Email | Department |
|------------|------|-------|------------|
| 1 | Alice | alice@example.com | Development |
| 2 | Bob | bob@example.com | HR |
| 3 | Charlie | charlie@example.com | Sales |
| 4 | Diana | diana@example.com | Marketing |
| 5 | Ethan | ethan@example.com | Sales |
| 6 | Fiona | fiona@example.com | HR |
| 7 | George | george@example.com | Development |

---

### Example 1: Delete Single Record

Delete the employee with EmployeeID 5:

```sql
DELETE FROM Employees
WHERE EmployeeID = 5;

SELECT * FROM Employees;
```

**Result:** Row with EmployeeID 5 (Ethan) is removed.

---

### Example 2: Delete Multiple Records

Delete all employees from the Development department:

```sql
DELETE FROM Employees
WHERE Department = 'Development';

SELECT * FROM Employees;
```

**Result:** Rows with EmployeeID 1 (Alice) and 7 (George) are removed.

---

### Example 3: Delete All Records

Delete all records from the table:

```sql
DELETE FROM Employees;
```

**Result:** All records are deleted, but table structure remains.

---

### Rolling Back DELETE Operations

Since DELETE is a DML operation, it can be rolled back in a transaction:

```sql
BEGIN TRANSACTION;

DELETE FROM Employees
WHERE Department = 'Development';

-- If needed, rollback the deletion
ROLLBACK;
```

**Explanation:** ROLLBACK undoes the DELETE, restoring deleted records.

---

## WHERE Clause

The SQL WHERE clause filters rows based on conditions. It's used with SELECT, UPDATE, and DELETE statements.

### Syntax:

```sql
SELECT column1, column2
FROM table_name
WHERE column_name operator value;
```

**Parameters:**
- `column_name`: Column to filter on
- `operator`: Comparison logic (=, <, >, LIKE, etc.)
- `value`: Value or pattern to filter against

---

### Example Setup: Emp1 Table

```sql
CREATE TABLE Emp1 (
    EmpID INT PRIMARY KEY,
    Name VARCHAR(50),
    Country VARCHAR(50),
    Age INT
);

INSERT INTO Emp1 VALUES
(1, 'Liam', 'USA', 24),
(2, 'Sophia', 'Germany', 22),
(3, 'Lucas', 'France', 23),
(4, 'Emma', 'Canada', 24),
(5, 'Oliver', 'UK', 21);
```

**Emp1 Table:**

| EmpID | Name | Country | Age |
|-------|------|---------|-----|
| 1 | Liam | USA | 24 |
| 2 | Sophia | Germany | 22 |
| 3 | Lucas | France | 23 |
| 4 | Emma | Canada | 24 |
| 5 | Oliver | UK | 21 |

---

### Example 1: WHERE with Logical Operators

Fetch employees with Age equal to 24:

```sql
SELECT * FROM Emp1 WHERE Age = 24;
```

**Output:**

| EmpID | Name | Country | Age |
|-------|------|---------|-----|
| 1 | Liam | USA | 24 |
| 4 | Emma | Canada | 24 |

---

### Example 2: WHERE with Comparison Operators

Fetch EmpID, Name, and Country of employees with Age greater than 21:

```sql
SELECT EmpID, Name, Country
FROM Emp1
WHERE Age > 21;
```

**Output:**

| EmpID | Name | Country |
|-------|------|---------|
| 1 | Liam | USA |
| 2 | Sophia | Germany |
| 3 | Lucas | France |
| 4 | Emma | Canada |

---

### Example 3: WHERE with BETWEEN Operator

Find employees whose age is between 22 and 24 (inclusive):

```sql
SELECT * FROM Emp1
WHERE Age BETWEEN 22 AND 24;
```

**Output:**

| EmpID | Name | Country | Age |
|-------|------|---------|-----|
| 1 | Liam | USA | 24 |
| 2 | Sophia | Germany | 22 |
| 3 | Lucas | France | 23 |
| 4 | Emma | Canada | 24 |

---

### Example 4: WHERE with LIKE Operator

Find employees whose name starts with 'L':

```sql
SELECT * FROM Emp1
WHERE Name LIKE 'L%';
```

**Output:**

| EmpID | Name | Country | Age |
|-------|------|---------|-----|
| 1 | Liam | USA | 24 |
| 3 | Lucas | France | 23 |

**Wildcards:**
- `%` - Zero or more characters
- `_` - Exactly one character

---

### Example 5: WHERE with IN Operator

Find employees where Age is 21 or 23:

```sql
SELECT Name FROM Emp1
WHERE Age IN (21, 23);
```

**Output:**

| Name |
|------|
| Lucas |
| Oliver |

---

### Operators Used in WHERE Clause

| Operator | Description |
|----------|-------------|
| `>` | Greater than |
| `>=` | Greater than or equal to |
| `<` | Less than |
| `<=` | Less than or equal to |
| `=` | Equal to |
| `<>` or `!=` | Not equal to |
| `BETWEEN` | In an inclusive range |
| `LIKE` | Search for a pattern |
| `IN` | Specify multiple possible values |

---

## Aliases

Aliases provide temporary names for columns or tables to make queries cleaner and easier to understand.

**Benefits:**
- Make queries more readable
- Simplify complex queries
- Useful for aggregate data and calculations

---

### 1. Column Aliases

A column alias renames a column temporarily in the query output.

**Syntax:**

```sql
SELECT column_name AS alias_name
FROM table_name;
```

**Note:** The `AS` keyword is optional.

**Example:**

```sql
SELECT CustomerID AS id FROM Customer;
```

**Output:**

| id |
|----|
| 1 |
| 2 |
| 3 |
| 4 |
| 5 |
| 6 |

---

### 2. Table Aliases

A table alias gives a table a temporary name for the query duration.

**Syntax:**

```sql
SELECT alias.column_name
FROM table_name AS alias;
```

**Example:**

```sql
SELECT c1.FirstName, c1.Country
FROM Customer AS c1, Customer AS c2
WHERE c1.Age = c2.Age AND c1.Country = c2.Country;
```

**Explanation:**
- Uses table aliases (c1 and c2) for the same Customer table
- Finds customers who share the same age and country
- Displays matching customer's name and country

---

### 3. Combining Column and Table Aliases

Fetch customers aged 21 or older with renamed columns:

```sql
SELECT c.FirstName AS Name, c.Country AS Location
FROM Customer AS c
WHERE c.Age >= 21;
```

**Output:**

| Name | Location |
|------|----------|
| Luca | Italy |
| Aiko | Japan |
| Carlos | Spain |
| Sofia | Germany |
| Ethan | USA |
| Isabella | Italy |

---

## Query Execution Order

Understanding the order in which SQL processes clauses:

```
1. FROM       - Get data from table(s)
2. WHERE      - Filter rows
3. GROUP BY   - Group rows
4. HAVING     - Filter groups
5. SELECT     - Choose columns
6. DISTINCT   - Remove duplicates
7. ORDER BY   - Sort results
8. LIMIT      - Limit rows returned
```

---

## Best Practices

### SELECT Queries:
1. Specify column names instead of `*` for better performance
2. Use WHERE to filter data at the database level
3. Use indexes on columns frequently used in WHERE clauses
4. Use LIMIT to restrict large result sets

### INSERT Operations:
1. Always specify column names for clarity
2. Use multiple-row inserts for better performance
3. Validate data before inserting
4. Use transactions for critical inserts

### UPDATE Operations:
1. Always use WHERE clause (unless updating all rows intentionally)
2. Test with SELECT first to verify which rows will be affected
3. Backup data before large updates
4. Use transactions for critical updates

### DELETE Operations:
1. Always use WHERE clause (unless deleting all rows intentionally)
2. Test with SELECT first to verify which rows will be deleted
3. Backup data before deleting
4. Consider using soft deletes (marking as deleted) instead

### WHERE Clause:
1. Use appropriate operators for data types
2. Use indexes on filtered columns
3. Combine conditions with AND/OR logically
4. Use parentheses for complex conditions

### Aliases:
1. Use meaningful alias names
2. Keep aliases short but descriptive
3. Use table aliases in JOIN operations
4. Use column aliases for calculated fields

---

## Key Takeaways

- **SELECT** retrieves data from tables with filtering and sorting
- **INSERT INTO** adds new records to tables
- **UPDATE** modifies existing records (always use WHERE!)
- **DELETE** removes records (always use WHERE!)
- **WHERE** filters rows based on conditions
- **Aliases** provide temporary names for better readability
- Always test queries with SELECT before UPDATE or DELETE
- Use transactions for critical operations
- Backup data before major changes
