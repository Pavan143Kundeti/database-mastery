# MySQL SQL Commands

## What is SQL?

SQL (Structured Query Language) is the standard language for working with relational databases.

**What SQL Does:**
- Insert data into database
- Search and retrieve data
- Update existing data
- Delete data from database

---

## Important SQL Rules

| Rule | Description |
|------|-------------|
| **Case Insensitive** | SELECT = select = SeLeCt (all same) |
| **Convention** | Write SQL keywords in UPPERCASE |
| **Semicolon** | End each statement with ; |
| **Quotes** | Use single quotes for text values |
| **No Quotes** | Don't use quotes for numbers |

---

## Most Important SQL Commands

| Command | What it does |
|---------|--------------|
| **SELECT** | Get data from database |
| **INSERT INTO** | Add new data |
| **UPDATE** | Change existing data |
| **DELETE** | Remove data |
| **CREATE DATABASE** | Make new database |
| **CREATE TABLE** | Make new table |
| **ALTER TABLE** | Modify table structure |
| **DROP TABLE** | Delete table |
| **CREATE INDEX** | Create search key |
| **DROP INDEX** | Delete search key |

---

## Demo Database: Customers Table

| CustomerID | CustomerName | ContactName | Address | City | PostalCode | Country |
|------------|--------------|-------------|---------|------|------------|---------|
| 1 | Alfreds Futterkiste | Maria Anders | Obere Str. 57 | Berlin | 12209 | Germany |
| 2 | Ana Trujillo Emparedados | Ana Trujillo | Avda. de la Constitución 2222 | México D.F. | 05021 | Mexico |
| 3 | Antonio Moreno Taquería | Antonio Moreno | Mataderos 2312 | México D.F. | 05023 | Mexico |
| 4 | Around the Horn | Thomas Hardy | 120 Hanover Sq. | London | WA1 1DP | UK |
| 5 | Berglunds snabbköp | Christina Berglund | Berguvsvägen 8 | Luleå | S-958 22 | Sweden |

---

## SELECT Statement

Gets data from database.

### Syntax

```sql
-- Select specific columns
SELECT column1, column2 FROM table_name;

-- Select all columns
SELECT * FROM table_name;
```

### Examples

```sql
-- Get specific columns
SELECT CustomerName, City, Country FROM Customers;

-- Get all columns
SELECT * FROM Customers;
```

---

## SELECT DISTINCT

Returns only unique (different) values.

### Syntax

```sql
SELECT DISTINCT column1, column2 FROM table_name;
```

### Examples

```sql
-- Get unique countries
SELECT DISTINCT Country FROM Customers;

-- Count unique countries
SELECT COUNT(DISTINCT Country) FROM Customers;
```

**Without DISTINCT:** Germany, Germany, Mexico, Mexico, UK (duplicates)

**With DISTINCT:** Germany, Mexico, UK (unique only)

---

## WHERE Clause

Filters records based on conditions.

### Syntax

```sql
SELECT column1, column2 FROM table_name WHERE condition;
```

### Examples

```sql
-- Text value (use quotes)
SELECT * FROM Customers WHERE Country = 'Mexico';

-- Numeric value (no quotes)
SELECT * FROM Customers WHERE CustomerID = 1;
```

---

## WHERE Operators

| Operator | Description | Example |
|----------|-------------|---------|
| **=** | Equal | WHERE Country = 'Mexico' |
| **>** | Greater than | WHERE CustomerID > 5 |
| **<** | Less than | WHERE CustomerID < 5 |
| **>=** | Greater than or equal | WHERE CustomerID >= 5 |
| **<=** | Less than or equal | WHERE CustomerID <= 5 |
| **<>** or **!=** | Not equal | WHERE Country <> 'Mexico' |
| **BETWEEN** | Between range | WHERE CustomerID BETWEEN 1 AND 5 |
| **LIKE** | Search pattern | WHERE City LIKE 'L%' |
| **IN** | Multiple values | WHERE Country IN ('Mexico', 'UK') |

---

## AND, OR, NOT Operators

Combine multiple conditions.

### Syntax

```sql
-- AND: All conditions must be true
SELECT * FROM table_name WHERE condition1 AND condition2;

-- OR: At least one condition must be true
SELECT * FROM table_name WHERE condition1 OR condition2;

-- NOT: Condition must be false
SELECT * FROM table_name WHERE NOT condition;
```

### Examples

```sql
-- AND Example
SELECT * FROM Customers WHERE Country = 'Germany' AND City = 'Berlin';

-- OR Example
SELECT * FROM Customers WHERE City = 'Berlin' OR City = 'London';

-- NOT Example
SELECT * FROM Customers WHERE NOT Country = 'Germany';

-- Combining (use parentheses)
SELECT * FROM Customers 
WHERE Country = 'Germany' AND (City = 'Berlin' OR City = 'München');

-- Multiple NOT
SELECT * FROM Customers 
WHERE NOT Country = 'Germany' AND NOT Country = 'USA';
```

---

## ORDER BY Keyword

Sorts results in ascending or descending order.

### Syntax

```sql
SELECT column1, column2 FROM table_name 
ORDER BY column1 ASC|DESC;
```

**Default:** ASC (ascending)

### Examples

```sql
-- Sort by country (ascending)
SELECT * FROM Customers ORDER BY Country;

-- Sort by country (descending)
SELECT * FROM Customers ORDER BY Country DESC;

-- Sort by multiple columns
SELECT * FROM Customers ORDER BY Country, CustomerName;

-- Mixed sorting
SELECT * FROM Customers ORDER BY Country ASC, CustomerName DESC;
```

---

## INSERT INTO Statement

Adds new records to table.

### Syntax

```sql
-- Specify columns
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);

-- All columns (no need to specify column names)
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

### Examples

```sql
-- Insert with column names
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway');

-- Insert only specific columns
INSERT INTO Customers (CustomerName, City, Country)
VALUES ('Cardinal', 'Stavanger', 'Norway');
```

**Note:** Auto-increment columns (like CustomerID) are generated automatically.

---

## NULL Values

NULL means no value (empty field).

**Important:** NULL is NOT the same as 0 or empty string.

### Syntax

```sql
-- Find NULL values
SELECT column_names FROM table_name WHERE column_name IS NULL;

-- Find NOT NULL values
SELECT column_names FROM table_name WHERE column_name IS NOT NULL;
```

### Examples

```sql
-- Find customers with no address
SELECT CustomerName, Address FROM Customers WHERE Address IS NULL;

-- Find customers with address
SELECT CustomerName, Address FROM Customers WHERE Address IS NOT NULL;
```

**Wrong:** `WHERE Address = NULL` ❌

**Correct:** `WHERE Address IS NULL` ✓

---

## UPDATE Statement

Modifies existing records.

### Syntax

```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

**Warning:** Without WHERE clause, ALL records will be updated!

### Examples

```sql
-- Update single record
UPDATE Customers
SET ContactName = 'Alfred Schmidt', City = 'Frankfurt'
WHERE CustomerID = 1;

-- Update multiple records
UPDATE Customers
SET PostalCode = '00000'
WHERE Country = 'Mexico';

-- Update all records (dangerous!)
UPDATE Customers
SET PostalCode = '00000';
```

---

## DELETE Statement

Removes records from table.

### Syntax

```sql
DELETE FROM table_name WHERE condition;
```

**Warning:** Without WHERE clause, ALL records will be deleted!

### Examples

```sql
-- Delete specific record
DELETE FROM Customers WHERE CustomerName = 'Alfreds Futterkiste';

-- Delete all records (table structure remains)
DELETE FROM Customers;
```

---

## LIMIT Clause

Limits number of records returned.

### Syntax

```sql
SELECT column_name(s) FROM table_name
WHERE condition
LIMIT number;

-- With OFFSET (skip records)
SELECT column_name(s) FROM table_name
LIMIT number OFFSET skip_number;
```

### Examples

```sql
-- Get first 3 records
SELECT * FROM Customers LIMIT 3;

-- Get records 4-6 (skip first 3, get next 3)
SELECT * FROM Customers LIMIT 3 OFFSET 3;

-- With WHERE clause
SELECT * FROM Customers WHERE Country = 'Germany' LIMIT 3;

-- With ORDER BY
SELECT * FROM Customers ORDER BY Country LIMIT 3;
```

---

## Aggregate Functions

Perform calculations on data.

| Function | What it does | Example |
|----------|--------------|---------|
| **MIN()** | Smallest value | MIN(Price) |
| **MAX()** | Largest value | MAX(Price) |
| **COUNT()** | Number of rows | COUNT(*) |
| **AVG()** | Average value | AVG(Price) |
| **SUM()** | Total sum | SUM(Quantity) |

### Examples

```sql
-- Find lowest price
SELECT MIN(Price) FROM Products;

-- Find highest price
SELECT MAX(Price) FROM Products;

-- Count total customers
SELECT COUNT(*) FROM Customers;

-- Count customers from Germany
SELECT COUNT(*) FROM Customers WHERE Country = 'Germany';

-- Average price
SELECT AVG(Price) FROM Products;

-- Total quantity
SELECT SUM(Quantity) FROM OrderDetails;
```

---

## LIKE Operator

Searches for patterns in text.

### Wildcards

| Wildcard | Description | Example |
|----------|-------------|---------|
| **%** | Zero or more characters | 'a%' finds apple, ant, a |
| **_** | Exactly one character | 'a_' finds an, at (not apple) |

### Syntax

```sql
SELECT column1, column2 FROM table_name
WHERE column LIKE pattern;
```

### Examples

```sql
-- Starts with 'a'
SELECT * FROM Customers WHERE CustomerName LIKE 'a%';

-- Ends with 'a'
SELECT * FROM Customers WHERE CustomerName LIKE '%a';

-- Contains 'or'
SELECT * FROM Customers WHERE CustomerName LIKE '%or%';

-- Second letter is 'r'
SELECT * FROM Customers WHERE CustomerName LIKE '_r%';

-- Starts with 'a' and at least 3 characters
SELECT * FROM Customers WHERE CustomerName LIKE 'a__%';

-- Starts with 'a' and ends with 'o'
SELECT * FROM Customers WHERE CustomerName LIKE 'a%o';

-- Does NOT start with 'a'
SELECT * FROM Customers WHERE CustomerName NOT LIKE 'a%';
```

---

## IN Operator

Specifies multiple values in WHERE clause.

### Syntax

```sql
SELECT column_name(s) FROM table_name
WHERE column_name IN (value1, value2, ...);
```

### Examples

```sql
-- Multiple countries
SELECT * FROM Customers WHERE Country IN ('Germany', 'France', 'UK');

-- NOT IN
SELECT * FROM Customers WHERE Country NOT IN ('Germany', 'France', 'UK');

-- IN with subquery
SELECT * FROM Customers 
WHERE Country IN (SELECT Country FROM Suppliers);
```

---

## BETWEEN Operator

Selects values within a range.

### Syntax

```sql
SELECT column_name(s) FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

### Examples

```sql
-- Numbers
SELECT * FROM Products WHERE Price BETWEEN 10 AND 20;

-- NOT BETWEEN
SELECT * FROM Products WHERE Price NOT BETWEEN 10 AND 20;

-- Dates
SELECT * FROM Orders WHERE OrderDate BETWEEN '1996-07-01' AND '1996-07-31';

-- Text (alphabetical)
SELECT * FROM Products WHERE ProductName BETWEEN 'C' AND 'M';
```

---

## Aliases

Gives temporary name to table or column.

### Syntax

```sql
-- Column alias
SELECT column_name AS alias_name FROM table_name;

-- Table alias
SELECT column_name FROM table_name AS alias_name;
```

### Examples

```sql
-- Column alias
SELECT CustomerName AS Customer, ContactName AS Contact FROM Customers;

-- Multiple columns
SELECT CustomerName AS Customer, CONCAT(Address, ', ', City) AS Location
FROM Customers;

-- Table alias (useful in joins)
SELECT c.CustomerName, o.OrderID
FROM Customers AS c, Orders AS o
WHERE c.CustomerID = o.CustomerID;

-- Alias with spaces (use quotes)
SELECT CustomerName AS "Customer Name" FROM Customers;
```

---

## JOINS

Combines rows from two or more tables.

### Types of Joins

| Join Type | Description |
|-----------|-------------|
| **INNER JOIN** | Returns matching records from both tables |
| **LEFT JOIN** | Returns all from left table + matching from right |
| **RIGHT JOIN** | Returns all from right table + matching from left |
| **CROSS JOIN** | Returns all possible combinations |
| **SELF JOIN** | Joins table to itself |

---

### INNER JOIN

Returns only matching records.

**Syntax:**
```sql
SELECT columns FROM table1
INNER JOIN table2 ON table1.column = table2.column;
```

**Example:**
```sql
SELECT Orders.OrderID, Customers.CustomerName
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

**Visual:**
```
Table1    Table2
  A         A     → Returns A (match)
  B         C     → B and C not returned
```

---

### LEFT JOIN

Returns all from left table + matching from right.

**Syntax:**
```sql
SELECT columns FROM table1
LEFT JOIN table2 ON table1.column = table2.column;
```

**Example:**
```sql
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
```

**Visual:**
```
Table1    Table2
  A         A     → Returns A (match)
  B         C     → Returns B (from left) with NULL
```

---

### RIGHT JOIN

Returns all from right table + matching from left.

**Syntax:**
```sql
SELECT columns FROM table1
RIGHT JOIN table2 ON table1.column = table2.column;
```

**Example:**
```sql
SELECT Orders.OrderID, Employees.LastName
FROM Orders
RIGHT JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID;
```

---

### CROSS JOIN

Returns all possible combinations (Cartesian product).

**Syntax:**
```sql
SELECT columns FROM table1
CROSS JOIN table2;
```

**Example:**
```sql
SELECT Customers.CustomerName, Products.ProductName
FROM Customers
CROSS JOIN Products;
```

**Result:** If 5 customers and 10 products = 50 rows

---

### SELF JOIN

Joins table to itself.

**Syntax:**
```sql
SELECT columns FROM table1 AS a, table1 AS b
WHERE condition;
```

**Example:**
```sql
SELECT A.CustomerName AS Customer1, B.CustomerName AS Customer2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID <> B.CustomerID AND A.City = B.City;
```

---

## UNION

Combines result sets of two or more SELECT statements.

### UNION vs UNION ALL

| Feature | UNION | UNION ALL |
|---------|-------|-----------|
| **Duplicates** | Removes duplicates | Keeps duplicates |
| **Speed** | Slower | Faster |
| **Use** | Unique values only | All values |

### Syntax

```sql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;

-- With duplicates
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```

### Examples

```sql
-- UNION (no duplicates)
SELECT City FROM Customers
UNION
SELECT City FROM Suppliers
ORDER BY City;

-- UNION ALL (with duplicates)
SELECT City FROM Customers
UNION ALL
SELECT City FROM Suppliers
ORDER BY City;
```

**Rules:**
- Same number of columns
- Similar data types
- Same order of columns

---

## GROUP BY

Groups rows with same values.

### Syntax

```sql
SELECT column_name(s), aggregate_function(column_name)
FROM table_name
WHERE condition
GROUP BY column_name(s);
```

### Examples

```sql
-- Count customers per country
SELECT Country, COUNT(*) AS CustomerCount
FROM Customers
GROUP BY Country;

-- Total orders per customer
SELECT CustomerID, COUNT(OrderID) AS TotalOrders
FROM Orders
GROUP BY CustomerID;

-- With ORDER BY
SELECT Country, COUNT(*) AS CustomerCount
FROM Customers
GROUP BY Country
ORDER BY CustomerCount DESC;
```

---

## HAVING Clause

Filters groups (used with GROUP BY).

**Difference:** WHERE filters rows, HAVING filters groups.

### Syntax

```sql
SELECT column_name(s), aggregate_function(column_name)
FROM table_name
GROUP BY column_name(s)
HAVING condition;
```

### Examples

```sql
-- Countries with more than 5 customers
SELECT Country, COUNT(*) AS CustomerCount
FROM Customers
GROUP BY Country
HAVING COUNT(*) > 5;

-- Customers with more than 10 orders
SELECT CustomerID, COUNT(OrderID) AS TotalOrders
FROM Orders
GROUP BY CustomerID
HAVING COUNT(OrderID) > 10;

-- Combined WHERE and HAVING
SELECT Country, COUNT(*) AS CustomerCount
FROM Customers
WHERE Country NOT IN ('USA', 'UK')
GROUP BY Country
HAVING COUNT(*) > 3;
```

---

## EXISTS Operator

Tests if subquery returns any records.

### Syntax

```sql
SELECT column_name(s) FROM table_name
WHERE EXISTS (SELECT column_name FROM table_name WHERE condition);
```

### Examples

```sql
-- Suppliers with products under 20
SELECT SupplierName
FROM Suppliers
WHERE EXISTS (SELECT ProductName FROM Products 
              WHERE Products.SupplierID = Suppliers.SupplierID 
              AND Price < 20);

-- NOT EXISTS
SELECT SupplierName
FROM Suppliers
WHERE NOT EXISTS (SELECT ProductName FROM Products 
                  WHERE Products.SupplierID = Suppliers.SupplierID 
                  AND Price < 20);
```

---

## ANY and ALL Operators

Compare value to set of values.

### Syntax

```sql
-- ANY: True if any subquery value meets condition
SELECT column_name(s) FROM table_name
WHERE column_name operator ANY (SELECT column_name FROM table_name WHERE condition);

-- ALL: True if all subquery values meet condition
SELECT column_name(s) FROM table_name
WHERE column_name operator ALL (SELECT column_name FROM table_name WHERE condition);
```

### Examples

```sql
-- ANY: Products with price equal to any order detail price
SELECT ProductName FROM Products
WHERE ProductID = ANY (SELECT ProductID FROM OrderDetails WHERE Quantity = 10);

-- ALL: Products with price greater than all products in category 1
SELECT ProductName FROM Products
WHERE Price > ALL (SELECT Price FROM Products WHERE CategoryID = 1);
```

---

## INSERT SELECT

Copies data from one table to another.

### Syntax

```sql
INSERT INTO table2 (column1, column2)
SELECT column1, column2 FROM table1
WHERE condition;
```

### Examples

```sql
-- Copy all suppliers to customers
INSERT INTO Customers (CustomerName, City, Country)
SELECT SupplierName, City, Country FROM Suppliers;

-- Copy specific suppliers
INSERT INTO Customers (CustomerName, City, Country)
SELECT SupplierName, City, Country FROM Suppliers
WHERE Country = 'Germany';
```

---

## CASE Statement

Creates conditional logic (if-then-else).

### Syntax

```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE result
END
```

### Examples

```sql
-- Categorize products by price
SELECT ProductName, Price,
CASE
    WHEN Price < 10 THEN 'Cheap'
    WHEN Price BETWEEN 10 AND 50 THEN 'Medium'
    ELSE 'Expensive'
END AS PriceCategory
FROM Products;

-- Order priority
SELECT OrderID, Quantity,
CASE
    WHEN Quantity > 30 THEN 'High'
    WHEN Quantity > 10 THEN 'Medium'
    ELSE 'Low'
END AS Priority
FROM OrderDetails;
```

---

## NULL Functions

Handle NULL values.

| Function | Description |
|----------|-------------|
| **IFNULL()** | Returns alternative if NULL |
| **COALESCE()** | Returns first non-NULL value |
| **NULLIF()** | Returns NULL if two values are equal |

### Examples

```sql
-- IFNULL: Replace NULL with 0
SELECT ProductName, IFNULL(Price, 0) AS Price
FROM Products;

-- COALESCE: First non-NULL value
SELECT ProductName, COALESCE(Price, AlternatePrice, 0) AS Price
FROM Products;

-- NULLIF: Return NULL if equal
SELECT NULLIF(10, 10);  -- Returns NULL
SELECT NULLIF(10, 5);   -- Returns 10
```

---

## Stored Procedures

Saved SQL code that can be reused.

### Syntax

```sql
-- Create procedure
DELIMITER //
CREATE PROCEDURE procedure_name()
BEGIN
    SQL statements;
END //
DELIMITER ;

-- Call procedure
CALL procedure_name();
```

### Examples

```sql
-- Simple procedure
DELIMITER //
CREATE PROCEDURE SelectAllCustomers()
BEGIN
    SELECT * FROM Customers;
END //
DELIMITER ;

-- Call it
CALL SelectAllCustomers();

-- With parameters
DELIMITER //
CREATE PROCEDURE SelectCustomersByCity(IN city_name VARCHAR(50))
BEGIN
    SELECT * FROM Customers WHERE City = city_name;
END //
DELIMITER ;

-- Call with parameter
CALL SelectCustomersByCity('London');
```

---

## Comments

Add notes to SQL code.

### Syntax

```sql
-- Single line comment
SELECT * FROM Customers; -- This is a comment

/* Multi-line
   comment */
SELECT * FROM Customers;

# MySQL specific single line comment
SELECT * FROM Customers; # This is also a comment
```

---

## MySQL Operators

### Arithmetic Operators

| Operator | Description | Example |
|----------|-------------|---------|
| **+** | Addition | SELECT 10 + 5; |
| **-** | Subtraction | SELECT 10 - 5; |
| **\*** | Multiplication | SELECT 10 * 5; |
| **/** | Division | SELECT 10 / 5; |
| **%** | Modulo (remainder) | SELECT 10 % 3; |

### Comparison Operators

| Operator | Description |
|----------|-------------|
| **=** | Equal |
| **!=** or **<>** | Not equal |
| **>** | Greater than |
| **<** | Less than |
| **>=** | Greater than or equal |
| **<=** | Less than or equal |

### Logical Operators

| Operator | Description |
|----------|-------------|
| **AND** | All conditions must be true |
| **OR** | At least one condition must be true |
| **NOT** | Reverses condition |
| **BETWEEN** | Within range |
| **IN** | Matches any value in list |
| **LIKE** | Pattern matching |
| **IS NULL** | Tests for NULL |
| **EXISTS** | Tests if subquery returns rows |

---

## Key Takeaways

- SQL is case-insensitive but use UPPERCASE for keywords
- Always use WHERE with UPDATE and DELETE to avoid changing all records
- Use quotes for text, no quotes for numbers
- LIMIT controls how many records to return
- Aggregate functions: MIN, MAX, COUNT, AVG, SUM
- LIKE uses % (any characters) and _ (one character)
- Joins combine tables: INNER, LEFT, RIGHT, CROSS, SELF
- GROUP BY groups rows, HAVING filters groups
- UNION combines results, UNION ALL keeps duplicates
- CASE creates conditional logic
- Stored procedures save reusable SQL code

