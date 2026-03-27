# MySQL Database Operations

## Database Operations Overview

| Operation | Command | Purpose |
|-----------|---------|---------|
| **Create Database** | CREATE DATABASE | Make new database |
| **Drop Database** | DROP DATABASE | Delete database |
| **Show Databases** | SHOW DATABASES | List all databases |
| **Use Database** | USE | Select database to work with |

---

## CREATE DATABASE

Creates a new database.

### Definition
The CREATE DATABASE statement creates a new empty database where you can store tables and data.

### Syntax
```sql
CREATE DATABASE database_name;
```

### Example
```sql
CREATE DATABASE testDB;
```

### Output
```
Query OK, 1 row affected (0.02 sec)
```

### Check Created Database
```sql
SHOW DATABASES;
```

### Output
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| testDB             |
+--------------------+
4 rows in set (0.00 sec)
```

### Important Notes
- Requires admin privileges
- Database name must be unique
- Cannot contain spaces (use underscore instead)

---

## DROP DATABASE

Deletes an existing database permanently.

### Definition
The DROP DATABASE statement removes a database and all its tables and data. This action cannot be undone.

### Syntax
```sql
DROP DATABASE database_name;
```

### Example
```sql
DROP DATABASE testDB;
```

### Output
```
Query OK, 0 rows affected (0.05 sec)
```

### Verification
```sql
SHOW DATABASES;
```

### Output
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.00 sec)
```

### Warning
⚠️ Dropping a database deletes ALL data permanently. Always backup before dropping!

---

## Database Operations Comparison

| Operation | CREATE DATABASE | DROP DATABASE |
|-----------|-----------------|---------------|
| **Purpose** | Create new database | Delete database |
| **Reversible** | Yes (can drop) | No (permanent) |
| **Requires Admin** | Yes | Yes |
| **Affects Data** | No (empty database) | Yes (all data lost) |
| **Syntax** | CREATE DATABASE name; | DROP DATABASE name; |

---

## Table Operations Overview

| Operation | Command | Purpose |
|-----------|---------|---------|
| **Create Table** | CREATE TABLE | Make new table |
| **Drop Table** | DROP TABLE | Delete table |
| **Truncate Table** | TRUNCATE TABLE | Delete data, keep structure |
| **Alter Table** | ALTER TABLE | Modify table structure |
| **Show Tables** | SHOW TABLES | List all tables |
| **Describe Table** | DESC or DESCRIBE | Show table structure |

---

## CREATE TABLE

Creates a new table in database.

### Definition
The CREATE TABLE statement creates a new table with specified columns and data types.

### Syntax
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
    ...
);
```

### Example
```sql
CREATE TABLE Persons (
    PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
);
```

### Output
```
Query OK, 0 rows affected (0.03 sec)
```

### View Table Structure
```sql
DESC Persons;
```

### Output
```
+-----------+--------------+------+-----+---------+-------+
| Field     | Type         | Null | Key | Default | Extra |
+-----------+--------------+------+-----+---------+-------+
| PersonID  | int          | YES  |     | NULL    |       |
| LastName  | varchar(255) | YES  |     | NULL    |       |
| FirstName | varchar(255) | YES  |     | NULL    |       |
| Address   | varchar(255) | YES  |     | NULL    |       |
| City      | varchar(255) | YES  |     | NULL    |       |
+-----------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)
```

### Empty Table View
```sql
SELECT * FROM Persons;
```

### Output
```
Empty set (0.00 sec)
```

---

## CREATE TABLE from Existing Table

Copies structure and data from another table.

### Syntax
```sql
CREATE TABLE new_table AS
SELECT column1, column2, ...
FROM existing_table
WHERE condition;
```

### Example 1: Copy Specific Columns
```sql
CREATE TABLE TestTable AS
SELECT CustomerName, ContactName
FROM Customers;
```

### Example 2: Copy All Columns
```sql
CREATE TABLE CustomersBackup AS
SELECT * FROM Customers;
```

### Example 3: Copy with Condition
```sql
CREATE TABLE GermanCustomers AS
SELECT * FROM Customers
WHERE Country = 'Germany';
```

### Comparison Table

| Method | Copies Structure | Copies Data | Copies Constraints |
|--------|------------------|-------------|-------------------|
| **CREATE TABLE** | Yes | No | No |
| **CREATE TABLE AS SELECT** | Yes | Yes | No |

---

## DROP TABLE

Deletes table and all its data.

### Definition
The DROP TABLE statement permanently removes a table from the database.

### Syntax
```sql
DROP TABLE table_name;
```

### Example
```sql
DROP TABLE Shippers;
```

### Output
```
Query OK, 0 rows affected (0.02 sec)
```

### Warning
⚠️ All data in the table will be lost permanently!

---

## TRUNCATE TABLE

Deletes all data but keeps table structure.

### Definition
TRUNCATE TABLE removes all rows from a table but keeps the table structure intact.

### Syntax
```sql
TRUNCATE TABLE table_name;
```

### Example
```sql
TRUNCATE TABLE Customers;
```

### Output
```
Query OK, 0 rows affected (0.04 sec)
```

---

## DROP vs TRUNCATE vs DELETE

| Feature | DROP TABLE | TRUNCATE TABLE | DELETE FROM |
|---------|------------|----------------|-------------|
| **Removes** | Table + Data | Data only | Data (with WHERE) |
| **Structure** | Deleted | Kept | Kept |
| **Speed** | Fast | Very fast | Slow |
| **Rollback** | No | No | Yes (with transaction) |
| **WHERE Clause** | No | No | Yes |
| **Auto-increment** | Reset | Reset | Not reset |

### Examples

```sql
-- DROP: Removes everything
DROP TABLE Customers;

-- TRUNCATE: Removes all data, keeps structure
TRUNCATE TABLE Customers;

-- DELETE: Can remove specific rows
DELETE FROM Customers WHERE Country = 'Mexico';
```

---

## ALTER TABLE

Modifies existing table structure.

### Definition
ALTER TABLE changes the structure of an existing table (add, delete, or modify columns).

---

### ALTER TABLE - ADD Column

Adds new column to table.

**Syntax:**
```sql
ALTER TABLE table_name
ADD column_name datatype;
```

**Example:**
```sql
ALTER TABLE Customers
ADD Email varchar(255);
```

**Output:**
```
Query OK, 0 rows affected (0.05 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

**Before:**
```
+-----------+--------------+
| Field     | Type         |
+-----------+--------------+
| CustomerID| int          |
| Name      | varchar(255) |
| City      | varchar(255) |
+-----------+--------------+
```

**After:**
```
+-----------+--------------+
| Field     | Type         |
+-----------+--------------+
| CustomerID| int          |
| Name      | varchar(255) |
| City      | varchar(255) |
| Email     | varchar(255) |
+-----------+--------------+
```

---

### ALTER TABLE - DROP Column

Removes column from table.

**Syntax:**
```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```

**Example:**
```sql
ALTER TABLE Customers
DROP COLUMN Email;
```

**Output:**
```
Query OK, 0 rows affected (0.06 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

---

### ALTER TABLE - MODIFY Column

Changes data type of existing column.

**Syntax:**
```sql
ALTER TABLE table_name
MODIFY COLUMN column_name new_datatype;
```

**Example:**
```sql
ALTER TABLE Persons
MODIFY COLUMN DateOfBirth year;
```

**Before:**
```
DateOfBirth: date
```

**After:**
```
DateOfBirth: year
```

---

### ALTER TABLE Operations Summary

| Operation | Syntax | Purpose |
|-----------|--------|---------|
| **ADD** | ALTER TABLE table ADD column datatype; | Add new column |
| **DROP** | ALTER TABLE table DROP COLUMN column; | Remove column |
| **MODIFY** | ALTER TABLE table MODIFY COLUMN column datatype; | Change data type |
| **RENAME** | ALTER TABLE old_name RENAME TO new_name; | Rename table |

---

## Complete ALTER TABLE Example

### Initial Table
```sql
CREATE TABLE Persons (
    ID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
);
```

**Structure:**
```
+-----------+--------------+
| Field     | Type         |
+-----------+--------------+
| ID        | int          |
| LastName  | varchar(255) |
| FirstName | varchar(255) |
| Address   | varchar(255) |
| City      | varchar(255) |
+-----------+--------------+
```

### Step 1: Add DateOfBirth Column
```sql
ALTER TABLE Persons
ADD DateOfBirth date;
```

**Structure:**
```
+-----------+--------------+
| Field     | Type         |
+-----------+--------------+
| ID        | int          |
| LastName  | varchar(255) |
| FirstName | varchar(255) |
| Address   | varchar(255) |
| City      | varchar(255) |
| DateOfBirth| date        |
+-----------+--------------+
```

### Step 2: Modify DateOfBirth to Year
```sql
ALTER TABLE Persons
MODIFY COLUMN DateOfBirth year;
```

**Structure:**
```
+-----------+--------------+
| Field     | Type         |
+-----------+--------------+
| ID        | int          |
| LastName  | varchar(255) |
| FirstName | varchar(255) |
| Address   | varchar(255) |
| City      | varchar(255) |
| DateOfBirth| year        |
+-----------+--------------+
```

### Step 3: Drop DateOfBirth Column
```sql
ALTER TABLE Persons
DROP COLUMN DateOfBirth;
```

**Final Structure:**
```
+-----------+--------------+
| Field     | Type         |
+-----------+--------------+
| ID        | int          |
| LastName  | varchar(255) |
| FirstName | varchar(255) |
| Address   | varchar(255) |
| City      | varchar(255) |
+-----------+--------------+
```

---

## MySQL Constraints

Constraints are rules applied to columns to ensure data accuracy and integrity.

### Constraints Overview

| Constraint | Purpose | Example |
|------------|---------|---------|
| **NOT NULL** | Column cannot be empty | Name NOT NULL |
| **UNIQUE** | All values must be different | Email UNIQUE |
| **PRIMARY KEY** | Unique identifier (NOT NULL + UNIQUE) | ID PRIMARY KEY |
| **FOREIGN KEY** | Links to another table | CustomerID FOREIGN KEY |
| **CHECK** | Values must meet condition | Age >= 18 |
| **DEFAULT** | Sets default value | Status DEFAULT 'Active' |
| **AUTO_INCREMENT** | Automatically generates numbers | ID AUTO_INCREMENT |

---

## NOT NULL Constraint

Ensures column cannot have NULL values.

### Definition
NOT NULL constraint forces a column to always contain a value. You cannot insert or update a record without providing a value for this column.

### Syntax
```sql
CREATE TABLE table_name (
    column1 datatype NOT NULL,
    column2 datatype NOT NULL
);
```

### Example
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);
```

### Testing NOT NULL

**Valid Insert:**
```sql
INSERT INTO Persons (ID, LastName, FirstName, Age)
VALUES (1, 'Hansen', 'Ola', 30);
```
✓ Success

**Invalid Insert:**
```sql
INSERT INTO Persons (ID, LastName)
VALUES (2, 'Svendson');
```
❌ Error: Column 'FirstName' cannot be null

### Add NOT NULL to Existing Column
```sql
ALTER TABLE Persons
MODIFY COLUMN Age int NOT NULL;
```

---

## UNIQUE Constraint

Ensures all values in column are different.

### Definition
UNIQUE constraint prevents duplicate values in a column. Each value must be unique.

### Syntax
```sql
CREATE TABLE table_name (
    column1 datatype UNIQUE,
    column2 datatype
);
```

### Example
```sql
CREATE TABLE Persons (
    ID int NOT NULL UNIQUE,
    LastName varchar(255),
    FirstName varchar(255),
    Email varchar(255) UNIQUE
);
```

### Testing UNIQUE

**Valid Insert:**
```sql
INSERT INTO Persons (ID, LastName, FirstName, Email)
VALUES (1, 'Hansen', 'Ola', 'ola@example.com');
```
✓ Success

**Invalid Insert (Duplicate Email):**
```sql
INSERT INTO Persons (ID, LastName, FirstName, Email)
VALUES (2, 'Svendson', 'Tove', 'ola@example.com');
```
❌ Error: Duplicate entry 'ola@example.com' for key 'Email'

### Named UNIQUE Constraint
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    Email varchar(255),
    CONSTRAINT UC_Person UNIQUE (ID, Email)
);
```

### Add UNIQUE to Existing Column
```sql
ALTER TABLE Persons
ADD UNIQUE (Email);
```

### Drop UNIQUE Constraint
```sql
ALTER TABLE Persons
DROP INDEX UC_Person;
```

---

## PRIMARY KEY Constraint

Uniquely identifies each record in table.

### Definition
PRIMARY KEY = NOT NULL + UNIQUE. Each table can have only ONE primary key.

### Syntax
```sql
CREATE TABLE table_name (
    column1 datatype PRIMARY KEY,
    column2 datatype
);
```

### Example
```sql
CREATE TABLE Persons (
    ID int NOT NULL PRIMARY KEY,
    LastName varchar(255),
    FirstName varchar(255),
    Age int
);
```

### Composite Primary Key
```sql
CREATE TABLE OrderDetails (
    OrderID int NOT NULL,
    ProductID int NOT NULL,
    Quantity int,
    PRIMARY KEY (OrderID, ProductID)
);
```

### Add PRIMARY KEY to Existing Table
```sql
ALTER TABLE Persons
ADD PRIMARY KEY (ID);
```

### Drop PRIMARY KEY
```sql
ALTER TABLE Persons
DROP PRIMARY KEY;
```

---

## FOREIGN KEY Constraint

Links two tables together.

### Definition
FOREIGN KEY is a column that references the PRIMARY KEY of another table. It maintains referential integrity.

### Syntax
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    FOREIGN KEY (column1) REFERENCES other_table(column)
);
```

### Example
```sql
-- Parent table
CREATE TABLE Persons (
    PersonID int NOT NULL PRIMARY KEY,
    LastName varchar(255),
    FirstName varchar(255)
);

-- Child table
CREATE TABLE Orders (
    OrderID int NOT NULL PRIMARY KEY,
    OrderNumber int,
    PersonID int,
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);
```

### Visual Representation

```
Persons Table (Parent)          Orders Table (Child)
+----------+---------+          +--------+--------+----------+
| PersonID | Name    |          | OrderID| Number | PersonID |
+----------+---------+          +--------+--------+----------+
| 1        | John    |    ←───  | 101    | 5001   | 1        |
| 2        | Mary    |    ←───  | 102    | 5002   | 2        |
+----------+---------+          | 103    | 5003   | 1        |
                                +--------+--------+----------+
```

### Named FOREIGN KEY
```sql
CREATE TABLE Orders (
    OrderID int PRIMARY KEY,
    PersonID int,
    CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID)
    REFERENCES Persons(PersonID)
);
```

### Add FOREIGN KEY to Existing Table
```sql
ALTER TABLE Orders
ADD FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);
```

### Drop FOREIGN KEY
```sql
ALTER TABLE Orders
DROP FOREIGN KEY FK_PersonOrder;
```

---

## CHECK Constraint

Ensures values meet specific condition.

### Definition
CHECK constraint limits the values that can be placed in a column.

### Syntax
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype CHECK (condition)
);
```

### Example
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255),
    Age int CHECK (Age >= 18)
);
```

### Multiple Conditions
```sql
CREATE TABLE Products (
    ProductID int PRIMARY KEY,
    ProductName varchar(255),
    Price decimal(10,2) CHECK (Price > 0),
    Stock int CHECK (Stock >= 0)
);
```

### Named CHECK Constraint
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    Age int,
    City varchar(255),
    CONSTRAINT CHK_Person CHECK (Age >= 18 AND City = 'Sandnes')
);
```

### Add CHECK to Existing Table
```sql
ALTER TABLE Persons
ADD CHECK (Age >= 18);
```

### Drop CHECK Constraint
```sql
ALTER TABLE Persons
DROP CHECK CHK_Person;
```

---

## DEFAULT Constraint

Sets default value for column.

### Definition
DEFAULT constraint provides a default value when no value is specified.

### Syntax
```sql
CREATE TABLE table_name (
    column1 datatype DEFAULT default_value
);
```

### Example
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255),
    FirstName varchar(255),
    City varchar(255) DEFAULT 'Sandnes',
    OrderDate date DEFAULT CURRENT_DATE()
);
```

### Testing DEFAULT

**Insert without City:**
```sql
INSERT INTO Persons (ID, LastName, FirstName)
VALUES (1, 'Hansen', 'Ola');
```

**Result:**
```
+----+----------+-----------+---------+
| ID | LastName | FirstName | City    |
+----+----------+-----------+---------+
| 1  | Hansen   | Ola       | Sandnes |
+----+----------+-----------+---------+
```

### Add DEFAULT to Existing Column
```sql
ALTER TABLE Persons
ALTER City SET DEFAULT 'Sandnes';
```

### Drop DEFAULT
```sql
ALTER TABLE Persons
ALTER City DROP DEFAULT;
```

---

## CREATE INDEX

Creates index for faster searches.

### Definition
INDEX improves query performance by creating a quick lookup structure. Like an index in a book.

### Syntax
```sql
-- Regular index (allows duplicates)
CREATE INDEX index_name
ON table_name (column1, column2, ...);

-- Unique index (no duplicates)
CREATE UNIQUE INDEX index_name
ON table_name (column1, column2, ...);
```

### Example
```sql
-- Single column index
CREATE INDEX idx_lastname
ON Persons (LastName);

-- Multiple column index
CREATE INDEX idx_name
ON Persons (LastName, FirstName);

-- Unique index
CREATE UNIQUE INDEX idx_email
ON Persons (Email);
```

### Drop INDEX
```sql
ALTER TABLE Persons
DROP INDEX idx_lastname;
```

### Index Comparison

| Feature | Without Index | With Index |
|---------|---------------|------------|
| **Search Speed** | Slow | Fast |
| **Insert Speed** | Fast | Slower |
| **Storage** | Less | More |
| **Best For** | Small tables | Large tables |

---

## AUTO_INCREMENT

Automatically generates unique numbers.

### Definition
AUTO_INCREMENT generates a unique number automatically when a new record is inserted.

### Syntax
```sql
CREATE TABLE table_name (
    column1 datatype AUTO_INCREMENT PRIMARY KEY
);
```

### Example
```sql
CREATE TABLE Persons (
    ID int NOT NULL AUTO_INCREMENT,
    LastName varchar(255),
    FirstName varchar(255),
    PRIMARY KEY (ID)
);
```

### Testing AUTO_INCREMENT

**Insert without ID:**
```sql
INSERT INTO Persons (LastName, FirstName)
VALUES ('Hansen', 'Ola');

INSERT INTO Persons (LastName, FirstName)
VALUES ('Svendson', 'Tove');
```

**Result:**
```
+----+----------+-----------+
| ID | LastName | FirstName |
+----+----------+-----------+
| 1  | Hansen   | Ola       |
| 2  | Svendson | Tove      |
+----+----------+-----------+
```

### Start AUTO_INCREMENT at Different Value
```sql
ALTER TABLE Persons AUTO_INCREMENT = 100;
```

---

## MySQL Dates

Working with date and time data.

### Date Data Types

| Data Type | Format | Range | Example |
|-----------|--------|-------|---------|
| **DATE** | YYYY-MM-DD | 1000-01-01 to 9999-12-31 | 2024-03-27 |
| **DATETIME** | YYYY-MM-DD HH:MI:SS | 1000-01-01 to 9999-12-31 | 2024-03-27 14:30:00 |
| **TIMESTAMP** | YYYY-MM-DD HH:MI:SS | 1970-01-01 to 2038-01-19 | 2024-03-27 14:30:00 |
| **TIME** | HH:MI:SS | -838:59:59 to 838:59:59 | 14:30:00 |
| **YEAR** | YYYY | 1901 to 2155 | 2024 |

### Example
```sql
CREATE TABLE Orders (
    OrderID int PRIMARY KEY,
    ProductName varchar(255),
    OrderDate DATE,
    OrderTime TIME,
    OrderDateTime DATETIME
);
```

### Insert Date Values
```sql
INSERT INTO Orders (OrderID, ProductName, OrderDate, OrderTime, OrderDateTime)
VALUES (1, 'Laptop', '2024-03-27', '14:30:00', '2024-03-27 14:30:00');
```

### Query with Dates
```sql
-- Exact date
SELECT * FROM Orders WHERE OrderDate = '2024-03-27';

-- Date range
SELECT * FROM Orders WHERE OrderDate BETWEEN '2024-01-01' AND '2024-12-31';

-- Current date
SELECT * FROM Orders WHERE OrderDate = CURDATE();
```

---

## MySQL Views

Virtual table based on SQL query.

### Definition
A VIEW is a virtual table that shows results of a stored query. It doesn't store data itself.

### Syntax
```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

### Example
```sql
-- Create view
CREATE VIEW CustomerView AS
SELECT CustomerName, City, Country
FROM Customers
WHERE Country = 'Germany';

-- Use view like a table
SELECT * FROM CustomerView;
```

### View with JOIN
```sql
CREATE VIEW OrderSummary AS
SELECT 
    Customers.CustomerName,
    Orders.OrderID,
    Orders.OrderDate
FROM Customers
INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
```

### Update View
```sql
CREATE OR REPLACE VIEW CustomerView AS
SELECT CustomerName, City, Country, ContactName
FROM Customers
WHERE Country = 'Germany';
```

### Drop View
```sql
DROP VIEW CustomerView;
```

### Benefits of Views

| Benefit | Description |
|---------|-------------|
| **Simplicity** | Hide complex queries |
| **Security** | Restrict access to specific columns |
| **Reusability** | Use same query multiple times |
| **Consistency** | Same logic applied everywhere |

---

## Key Takeaways

- CREATE DATABASE makes new database, DROP DATABASE deletes it
- CREATE TABLE makes new table, DROP TABLE deletes it
- TRUNCATE removes data but keeps structure
- ALTER TABLE modifies table structure (ADD, DROP, MODIFY columns)
- Constraints ensure data integrity: NOT NULL, UNIQUE, PRIMARY KEY, FOREIGN KEY, CHECK, DEFAULT
- PRIMARY KEY = NOT NULL + UNIQUE (one per table)
- FOREIGN KEY links tables together
- INDEX speeds up searches
- AUTO_INCREMENT generates unique numbers automatically
- Views are virtual tables based on queries
- Always backup before dropping databases or tables

