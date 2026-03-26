# Data Constraints & Aggregate Functions

This comprehensive guide covers SQL constraints for maintaining data integrity and aggregate functions for data analysis and reporting.

---

## Table of Contents
1. [Data Constraints](#data-constraints)
   - [NOT NULL Constraint](#not-null-constraint)
   - [PRIMARY KEY Constraint](#primary-key-constraint)
2. [Aggregate Functions](#aggregate-functions)
   - [COUNT() Function](#count-function)
   - [SUM() Function](#sum-function)
   - [MAX() Function](#max-function)
   - [MIN() Function](#min-function)
   - [AVG() Function](#avg-function)

---

## Data Constraints

Data constraints are rules applied to columns to ensure data integrity and accuracy.

**Why Use Constraints?**
- Enforce data validity and consistency
- Prevent invalid data entry
- Maintain relationships between tables
- Ensure data quality

---

## NOT NULL Constraint

The NOT NULL constraint ensures a column must always contain a value and cannot be left empty.

**Key Features:**
- Enforces mandatory fields
- Prevents NULL values from being inserted or updated
- Applicable at the column level
- Unlike PRIMARY KEY, it doesn't require uniqueness

### Syntax:

```sql
CREATE TABLE table_name (
    column1 data_type(size) NOT NULL,
    column2 data_type(size) NOT NULL,
    ...
);
```

---

### Example Setup: Student Table

```sql
CREATE TABLE Student (
    StuID INT NOT NULL,
    FullName VARCHAR(50),
    City VARCHAR(50)
);

-- Insert valid data
INSERT INTO Student (StuID, FullName, City) VALUES
(101, 'Bob', 'London'),
(102, 'Lucas', 'London');
```

**Student Table:**

| StuID | FullName | City |
|-------|----------|------|
| 101 | Bob | London |
| 102 | Lucas | London |

**Explanation:**
- StuID is defined as NOT NULL, so it must always have a value
- The database will reject any NULL value inserted into StuID
- FullName and City can be NULL (no constraint applied)

---

### Attempting to Insert NULL

```sql
INSERT INTO Student (StuID, FullName, City)
VALUES (NULL, 'John', 'London');
```

**Error:**
```
Column 'StuID' cannot be NULL
```

**Why It Fails:**
- The insert fails because StuID has a NULL value
- A NOT NULL column requires a valid value
- The database rejects NULL to maintain data integrity

---

### NOT NULL Constraint Syntax Options

#### 1. During Table Creation:

```sql
CREATE TABLE table_name (
    column1 data_type(size) NOT NULL,
    column2 data_type(size) NOT NULL,
    ...
);
```

#### 2. Modifying an Existing Table:

```sql
ALTER TABLE table_name
MODIFY column_name data_type(size) NOT NULL;
```

---

### Example 1: Applying NOT NULL in Table Creation

Create an Emp table with NOT NULL constraint on EmpID:

```sql
CREATE TABLE Emp (
    EmpID INT NOT NULL PRIMARY KEY,
    Name VARCHAR(50),
    Country VARCHAR(50),
    Age INT(2),
    Salary INT(10)
);

-- Insert valid rows (EmpID has values, so inserts succeed)
INSERT INTO Emp (EmpID, Name, Country, Age, Salary) VALUES
(1, 'John', 'USA', 28, 35000),
(2, 'Emma', 'Canada', 32, 55000),
(3, 'Lucas', 'Germany', 26, 42000);
```

**Emp Table:**

| EmpID | Name | Country | Age | Salary |
|-------|------|---------|-----|--------|
| 1 | John | USA | 28 | 35000 |
| 2 | Emma | Canada | 32 | 55000 |
| 3 | Lucas | Germany | 26 | 42000 |

**Success:** All rows insert successfully because EmpID has valid (non-NULL) values.

---

### Attempting Invalid Insert:

```sql
INSERT INTO Emp (EmpID, Name, Country, Age, Salary)
VALUES (NULL, 'Oliver', 'France', 30, 50000);
```

**Error:**
```
Column 'EmpID' cannot be NULL
```

**Explanation:** Fails because EmpID cannot be NULL due to the NOT NULL constraint.

---

### Example 2: Adding NOT NULL to an Existing Table

Add NOT NULL constraint to an existing column:

**Original Student Table (allows NULL):**

| StuID | FullName | City |
|-------|----------|------|
| 101 | Bob | London |
| 102 | Lucas | London |

```sql
ALTER TABLE Student
MODIFY COLUMN StuID INT NOT NULL;
```

**Result:** StuID cannot store NULL values anymore.

**Testing the Constraint:**

```sql
INSERT INTO Student (StuID, FullName, City)
VALUES (NULL, 'Mark', 'London');
```

**Error:**
```
Column 'StuID' cannot be NULL
```

---

## PRIMARY KEY Constraint

The PRIMARY KEY constraint uniquely identifies each record in a table and ensures strong data integrity.

**Key Features:**
- Ensures all values are unique
- Does not allow NULL values
- Only one primary key per table (can be composite)
- Automatically creates a unique index for faster searches

### Syntax:

#### With CREATE TABLE:

```sql
CREATE TABLE table_name (
    column1 data_type,
    column2 data_type,
    ...
    PRIMARY KEY (column1, column2)
);
```

#### With ALTER TABLE:

```sql
ALTER TABLE table_name
ADD CONSTRAINT constraint_name
PRIMARY KEY (column1, column2, ...);
```

---

### Types of PRIMARY KEYS

| Type | Description | Example |
|------|-------------|---------|
| **Simple Primary Key** | Single column | `PRIMARY KEY (EmpID)` |
| **Composite Primary Key** | Multiple columns | `PRIMARY KEY (OrderID, ProductID)` |

---

### Example Setup: Employees Table

```sql
CREATE TABLE Employees (
    EmpID INT PRIMARY KEY,
    EmpName VARCHAR(50),
    Department VARCHAR(50)
);

-- Insert valid data
INSERT INTO Employees (EmpID, EmpName, Department) VALUES
(101, 'Bob', 'Sales'),
(102, 'Lucas', 'HR');
```

**Employees Table:**

| EmpID | EmpName | Department |
|-------|---------|------------|
| 101 | Bob | Sales |
| 102 | Lucas | HR |

**Explanation:**
- EmpID is the primary key, so it must be unique and cannot be NULL
- If you try inserting duplicate or NULL EmpID values, the database will throw an error

---

### Attempting Invalid Inserts:

#### Duplicate Primary Key:

```sql
INSERT INTO Employees VALUES (101, 'Alice', 'IT');
```

**Error:**
```
Duplicate entry '101' for key 'PRIMARY'
```

#### NULL Primary Key:

```sql
INSERT INTO Employees VALUES (NULL, 'Emma', 'Finance');
```

**Error:**
```
Column 'EmpID' cannot be NULL
```

---

### Example 1: Create PRIMARY KEY in SQL

Create a new table with primary key:

```sql
CREATE TABLE Persons (
    PersonID INT PRIMARY KEY,
    LastName VARCHAR(255),
    FirstName VARCHAR(255),
    Age INT
);

INSERT INTO Persons VALUES
(1, 'Johnson', 'Emily', 25),
(2, 'Walker', 'James', 30);
```

**Persons Table:**

| PersonID | LastName | FirstName | Age |
|----------|----------|-----------|-----|
| 1 | Johnson | Emily | 25 |
| 2 | Walker | James | 30 |

**Explanation:** PersonID is the primary key, so it must be unique.

---

### Example 2: Verify SQL Primary Key Creation

Test the primary key constraint:

#### Duplicate Value Test:

```sql
INSERT INTO Persons VALUES (1, 'Brown', 'Olivia', 27);
```

**Error:**
```
Duplicate entry '1' for key 'PRIMARY'
```

**Explanation:** First query fails because PersonID = 1 is already used.

#### NULL Value Test:

```sql
INSERT INTO Persons VALUES (NULL, 'Miller', 'Ethan', 32);
```

**Error:**
```
Column 'PersonID' cannot be NULL
```

**Explanation:** Second query fails because primary keys cannot be NULL.

---

### Example 3: Add PRIMARY KEY to an Existing Table

**Original Person Table (no primary key):**

| PersonID | LastName | FirstName | Age |
|----------|----------|-----------|-----|
| 1 | Johnson | Emily | 25 |
| 2 | Walker | James | 30 |

```sql
ALTER TABLE Person
ADD CONSTRAINT PK_Person PRIMARY KEY (PersonID);
```

**Result:**
- Adds a primary key to the table
- Ensures PersonID is unique and cannot be NULL
- Assigns the name PK_Person to the constraint

**Testing:**

```sql
INSERT INTO Person VALUES (1, 'Williams', 'Sophia', 24);
INSERT INTO Person VALUES (NULL, 'Davis', 'Liam', 29);
```

**Errors:**
- First insert fails: PersonID = 1 already exists
- Second insert fails: Primary keys cannot be NULL

---

### Benefits of Using Primary Keys

| Benefit | Description |
|---------|-------------|
| **Unique Identification** | Each record has a unique identifier |
| **Faster Searches** | Automatically creates an index for quick lookups |
| **Data Integrity** | Prevents duplicate or NULL values |
| **Relationships** | Supports reliable table relationships through foreign keys |
| **Data Consistency** | Keeps data clean and organized |

---

## Aggregate Functions

Aggregate functions perform calculations on a set of values and return a single result.

**Common Uses:**
- Summarize data for reports
- Calculate totals, averages, and counts
- Find minimum and maximum values
- Analyze trends and patterns

---

## COUNT() Function

The COUNT() function returns the number of rows or non-null values in a column.

**Key Features:**
- Counts all rows or rows with a condition
- Counts unique values using COUNT(DISTINCT column)
- Commonly used with GROUP BY and HAVING

### Syntax:

```sql
-- Count all rows
SELECT COUNT(*) FROM table_name;

-- Count distinct values
SELECT COUNT(DISTINCT column_name) FROM table_name;
```

---

### Example Setup: Employee Table

```sql
CREATE TABLE Employee (
    EmpID INT PRIMARY KEY,
    Name VARCHAR(50),
    Department VARCHAR(50),
    Salary INT
);

INSERT INTO Employee VALUES
(1, 'Alice', 'IT', 60000),
(2, 'Bob', 'HR', 50000),
(3, 'Charlie', 'IT', 65000),
(4, 'Diana', 'Sales', 55000),
(5, 'Eve', 'HR', 52000);
```

**Employee Table:**

| EmpID | Name | Department | Salary |
|-------|------|------------|--------|
| 1 | Alice | IT | 60000 |
| 2 | Bob | HR | 50000 |
| 3 | Charlie | IT | 65000 |
| 4 | Diana | Sales | 55000 |
| 5 | Eve | HR | 52000 |

---

### Basic COUNT Examples:

```sql
-- Count total employees
SELECT COUNT(*) AS TotalEmployees FROM Employee;

-- Count unique departments
SELECT COUNT(DISTINCT Department) AS UniqueDepartments FROM Employee;
```

**Output:**

| TotalEmployees |
|----------------|
| 5 |

| UniqueDepartments |
|-------------------|
| 3 |

---

### Example Setup: Customers Table

```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    Name VARCHAR(50),
    Country VARCHAR(50),
    Age INT
);

INSERT INTO Customers VALUES
(1, 'Carlos', 'Spain', 28),
(2, 'Maria', 'Spain', 35),
(3, 'Juan', 'Mexico', 42),
(4, 'Sofia', 'Spain', 29),
(5, 'Raj', 'India', 31),
(6, 'Priya', 'India', 27),
(7, 'Luis', 'Mexico', 38),
(8, 'Anna', 'Spain', 33),
(9, 'Hans', 'Germany', 45);
```

**Customers Table:**

| CustomerID | Name | Country | Age |
|------------|------|---------|-----|
| 1 | Carlos | Spain | 28 |
| 2 | Maria | Spain | 35 |
| 3 | Juan | Mexico | 42 |
| 4 | Sofia | Spain | 29 |
| 5 | Raj | India | 31 |
| 6 | Priya | India | 27 |
| 7 | Luis | Mexico | 38 |
| 8 | Anna | Spain | 33 |
| 9 | Hans | Germany | 45 |

---

### Example 1: Counting All Rows with COUNT(*)

```sql
SELECT COUNT(*) AS TotalCustomers FROM Customers;
```

**Output:**

| TotalCustomers |
|----------------|
| 9 |

**Explanation:** Counts all rows in the table, including those with NULL values.

---

### Example 2: Counting Unique Values with COUNT(DISTINCT)

```sql
SELECT COUNT(DISTINCT Country) FROM Customers;
```

**Output:**

| COUNT(DISTINCT Country) |
|-------------------------|
| 4 |

**Explanation:** Counts unique countries (Spain, Mexico, India, Germany).

---

### Example 3: Count Rows with Condition Using CASE WHEN

```sql
SELECT COUNT(CASE WHEN Age > 30 THEN 1 ELSE NULL END) AS Adults
FROM Customers;
```

**Output:**

| Adults |
|--------|
| 5 |

**Explanation:**
- CASE WHEN checks if Age > 30
- Returns 1 if true, NULL if false
- COUNT() counts non-NULL values

---

### Example 4: Count Rows in Groups Using GROUP BY

```sql
SELECT Country, COUNT(*) AS CustomerCount
FROM Customers
GROUP BY Country;
```

**Output:**

| Country | CustomerCount |
|---------|---------------|
| Spain | 4 |
| Mexico | 2 |
| India | 2 |
| Germany | 1 |

**Explanation:** Groups by Country and counts customers in each group.

---

### Example 5: Filter Groups Using HAVING

```sql
SELECT Country, COUNT(*) AS CustomerCount
FROM Customers
GROUP BY Country
HAVING COUNT(*) > 2;
```

**Output:**

| Country | CustomerCount |
|---------|---------------|
| Spain | 4 |

**Explanation:** Shows only countries with more than 2 customers.

---

### Best Practices for COUNT()

#### 1. Optimize for Large Datasets:

```sql
-- Create index for faster counting
CREATE INDEX idx_country ON Customers(Country);

-- Faster COUNT query
SELECT COUNT(*) FROM Customers WHERE Country = 'Spain';
```

#### 2. Avoid Complex Queries:

```sql
-- Complex query (may be slow)
SELECT COUNT(*) FROM Customers
WHERE (Country = 'Spain' OR Country = 'France')
AND Age > 30
AND City = 'Barcelona';
```

**Tip:** Break complex queries into smaller parts for better performance.

---

## SUM() Function

The SUM() function calculates the total value of a numeric column.

**Key Features:**
- Returns a single total of selected numeric values
- Works with WHERE, GROUP BY, and HAVING
- Supports expressions and DISTINCT

### Syntax:

```sql
SELECT SUM(column_name) FROM table_name;
```

---

### Example Setup: Orders Table

```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    Product VARCHAR(50),
    Amount DECIMAL(10,2)
);

INSERT INTO Orders VALUES
(1, 'Laptop', 1200.00),
(2, 'Mouse', 25.00),
(3, 'Keyboard', 75.00),
(4, 'Monitor', 300.00);
```

**Orders Table:**

| OrderID | Product | Amount |
|---------|---------|--------|
| 1 | Laptop | 1200.00 |
| 2 | Mouse | 25.00 |
| 3 | Keyboard | 75.00 |
| 4 | Monitor | 300.00 |

```sql
SELECT SUM(Amount) AS TotalAmount FROM Orders;
```

**Output:**

| TotalAmount |
|-------------|
| 1600.00 |

---

### Example Setup: Sales Table

```sql
CREATE TABLE Sales (
    SaleID INT PRIMARY KEY,
    Product VARCHAR(50),
    Quantity INT,
    Price DECIMAL(10,2)
);

INSERT INTO Sales VALUES
(1, 'Laptop', 5, 1000.00),
(2, 'Mouse', 20, 25.00),
(3, 'Keyboard', 15, 50.00),
(4, 'Laptop', 3, 1000.00);
```

**Sales Table:**

| SaleID | Product | Quantity | Price |
|--------|---------|----------|-------|
| 1 | Laptop | 5 | 1000.00 |
| 2 | Mouse | 20 | 25.00 |
| 3 | Keyboard | 15 | 50.00 |
| 4 | Laptop | 3 | 1000.00 |

---

### Example 1: SUM() with One Column

```sql
SELECT SUM(Price) AS TotalPrice FROM Sales;
```

**Output:**

| TotalPrice |
|------------|
| 2075.00 |

**Explanation:** Adds all values in the Price column.

---

### Example 2: SUM() with an Expression

```sql
SELECT SUM(Quantity * Price) AS TotalRevenue FROM Sales;
```

**Output:**

| TotalRevenue |
|--------------|
| 9250.00 |

**Explanation:** Multiplies Quantity × Price for each row, then sums the results.

---

### Example 3: SUM() with GROUP BY

```sql
SELECT Product, SUM(Quantity * Price) AS TotalRevenue
FROM Sales
GROUP BY Product;
```

**Output:**

| Product | TotalRevenue |
|---------|--------------|
| Laptop | 8000.00 |
| Mouse | 500.00 |
| Keyboard | 750.00 |

**Explanation:** Calculates revenue separately for each product.

---

### Example 4: SUM() with DISTINCT

```sql
SELECT SUM(DISTINCT Price) AS SumDistinctPrice FROM Sales;
```

**Output:**

| SumDistinctPrice |
|------------------|
| 1075.00 |

**Explanation:** Adds only unique Price values (1000 + 25 + 50).

---

### Example 5: SUM() with HAVING

```sql
SELECT Product, SUM(Quantity * Price) AS TotalRevenue
FROM Sales
GROUP BY Product
HAVING SUM(Quantity * Price) > 2000;
```

**Output:**

| Product | TotalRevenue |
|---------|--------------|
| Laptop | 8000.00 |

**Explanation:** Shows only products with revenue greater than 2000.

---

## MAX() Function

The MAX() function returns the maximum value from a specified column.

**Key Features:**
- Returns the highest value from the column
- Supports numeric, date, and text data types
- Skips NULL values during calculation

### Syntax:

```sql
SELECT MAX(column_name) FROM table_name;
```

---

### Example Setup: SalesData Table

```sql
CREATE TABLE SalesData (
    SaleID INT PRIMARY KEY,
    Product VARCHAR(50),
    Price DECIMAL(10,2)
);

INSERT INTO SalesData VALUES
(1, 'Laptop', 1200.00),
(2, 'Mouse', 25.00),
(3, 'Keyboard', 75.00),
(4, 'Monitor', 300.00);
```

**SalesData Table:**

| SaleID | Product | Price |
|--------|---------|-------|
| 1 | Laptop | 1200.00 |
| 2 | Mouse | 25.00 |
| 3 | Keyboard | 75.00 |
| 4 | Monitor | 300.00 |

```sql
SELECT MAX(Price) AS HighestPrice FROM SalesData;
```

**Output:**

| HighestPrice |
|--------------|
| 1200.00 |

---

### Example Setup: Products Table

```sql
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    product_name VARCHAR(50),
    category VARCHAR(50),
    price DECIMAL(10,2),
    total_sales DECIMAL(10,2),
    sale_date DATE
);

INSERT INTO Products VALUES
(1, 'Laptop', 'Electronics', 1200.00, 60000.00, '2024-01-15'),
(2, 'Mouse', 'Electronics', 25.00, 5000.00, '2024-02-10'),
(3, 'Desk', 'Furniture', 300.00, 45000.00, '2024-01-20'),
(4, 'Chair', 'Furniture', 150.00, 30000.00, '2024-03-05');
```

**Products Table:**

| ProductID | product_name | category | price | total_sales | sale_date |
|-----------|--------------|----------|-------|-------------|-----------|
| 1 | Laptop | Electronics | 1200.00 | 60000.00 | 2024-01-15 |
| 2 | Mouse | Electronics | 25.00 | 5000.00 | 2024-02-10 |
| 3 | Desk | Furniture | 300.00 | 45000.00 | 2024-01-20 |
| 4 | Chair | Furniture | 150.00 | 30000.00 | 2024-03-05 |

---

### Example 1: Find Maximum Price

```sql
SELECT MAX(total_sales) AS [Highest Total Sales] FROM Products;
```

**Output:**

| Highest Total Sales |
|---------------------|
| 60000.00 |

---

### Example 2: MAX() with Condition

```sql
SELECT MAX(price) AS [Highest Price in Electronics]
FROM Products
WHERE category = 'Electronics';
```

**Output:**

| Highest Price in Electronics |
|------------------------------|
| 1200.00 |

---

### Example 3: Find Latest Sale Date

```sql
SELECT MAX(sale_date) AS [Latest Sale Date] FROM Products;
```

**Output:**

| Latest Sale Date |
|------------------|
| 2024-03-05 |

---

### Example 4: MAX() with GROUP BY

```sql
SELECT product_name, MAX(total_sales) AS [Top Sales Amount]
FROM Products
GROUP BY product_name;
```

**Output:**

| product_name | Top Sales Amount |
|--------------|------------------|
| Laptop | 60000.00 |
| Mouse | 5000.00 |
| Desk | 45000.00 |
| Chair | 30000.00 |

---

### Example 5: MAX() in Subqueries

```sql
SELECT * FROM Products
WHERE total_sales = (SELECT MAX(total_sales) FROM Products);
```

**Output:**

| ProductID | product_name | category | price | total_sales | sale_date |
|-----------|--------------|----------|-------|-------------|-----------|
| 1 | Laptop | Electronics | 1200.00 | 60000.00 | 2024-01-15 |

---

### Example 6: MAX() with HAVING

```sql
SELECT product_name, MAX(total_sales) AS HighestSale
FROM Products
GROUP BY product_name
HAVING MAX(total_sales) > 50000;
```

**Output:**

| product_name | HighestSale |
|--------------|-------------|
| Laptop | 60000.00 |

---

## MIN() Function

The MIN() function returns the minimum value from a specified column.

**Key Features:**
- Returns the lowest value from the column
- Supports numeric, date, and text data types
- Skips NULL values during calculation

### Syntax:

```sql
SELECT MIN(column_name) FROM table_name;
```

### Example:

```sql
SELECT MIN(price) AS LowestPrice FROM Products;
```

**Output:**

| LowestPrice |
|-------------|
| 25.00 |

---

## AVG() Function

The AVG() function calculates the average of a numeric column.

**Key Features:**
- Finds the average or mean using only non-null values
- Useful for quick comparisons and basic data analysis
- Ignores NULL entries

### Syntax:

```sql
SELECT AVG(column_name) FROM table_name;
```

---

### Example Setup: Student Table

```sql
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50),
    Marks INT
);

INSERT INTO Student VALUES
(1, 'Alice', 85),
(2, 'Bob', 90),
(3, 'Charlie', 78),
(4, 'Diana', 92);
```

**Student Table:**

| StudentID | Name | Marks |
|-----------|------|-------|
| 1 | Alice | 85 |
| 2 | Bob | 90 |
| 3 | Charlie | 78 |
| 4 | Diana | 92 |

```sql
SELECT AVG(Marks) AS AverageMarks FROM Student;
```

**Output:**

| AverageMarks |
|--------------|
| 86.25 |

---

### Example Setup: student_scores Table

```sql
CREATE TABLE student_scores (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(50),
    subject VARCHAR(50),
    score INT
);

INSERT INTO student_scores VALUES
(1, 'Alice', 'Math', 85),
(2, 'Bob', 'Math', 90),
(3, 'Charlie', 'Science', 88),
(4, 'Diana', 'Science', 92),
(5, 'Eve', 'Math', 78);
```

**student_scores Table:**

| student_id | student_name | subject | score |
|------------|--------------|---------|-------|
| 1 | Alice | Math | 85 |
| 2 | Bob | Math | 90 |
| 3 | Charlie | Science | 88 |
| 4 | Diana | Science | 92 |
| 5 | Eve | Math | 78 |

---

### Example 1: Overall Average Score

```sql
SELECT AVG(score) AS overall_average_score FROM student_scores;
```

**Output:**

| overall_average_score |
|-----------------------|
| 86.60 |

---

### Example 2: Average Score per Subject

```sql
SELECT subject, AVG(score) AS average_score
FROM student_scores
GROUP BY subject;
```

**Output:**

| subject | average_score |
|---------|---------------|
| Math | 84.33 |
| Science | 90.00 |

---

### Example 3: Average for Specific Subject

```sql
SELECT AVG(score) AS average_science_score
FROM student_scores
WHERE subject = 'Science';
```

**Output:**

| average_science_score |
|-----------------------|
| 90.00 |

---

### Example 4: AVG() with HAVING

```sql
SELECT subject, AVG(score) AS average_score
FROM student_scores
GROUP BY subject
HAVING AVG(score) > 85;
```

**Output:**

| subject | average_score |
|---------|---------------|
| Science | 90.00 |

---

### Considerations for AVG()

| Consideration | Description |
|---------------|-------------|
| **NULL Handling** | AVG() ignores NULL values, which may impact results |
| **Numeric Columns Only** | Works only on numeric data—causes errors on non-numeric columns |
| **Precision** | Can return long decimals; round the result when needed |

### Rounding AVG() Results:

```sql
SELECT ROUND(AVG(score), 2) AS average_score FROM student_scores;
```

---

## Aggregate Functions Comparison

| Function | Purpose | Example | Output |
|----------|---------|---------|--------|
| **COUNT()** | Count rows or values | `COUNT(*)` | 5 |
| **SUM()** | Calculate total | `SUM(Amount)` | 1600.00 |
| **MAX()** | Find highest value | `MAX(Price)` | 1200.00 |
| **MIN()** | Find lowest value | `MIN(Price)` | 25.00 |
| **AVG()** | Calculate average | `AVG(Marks)` | 86.25 |

---

## Using Aggregate Functions Together

```sql
SELECT
    COUNT(*) AS TotalProducts,
    SUM(total_sales) AS TotalSales,
    AVG(price) AS AveragePrice,
    MAX(price) AS HighestPrice,
    MIN(price) AS LowestPrice
FROM Products;
```

**Output:**

| TotalProducts | TotalSales | AveragePrice | HighestPrice | LowestPrice |
|---------------|------------|--------------|--------------|-------------|
| 4 | 140000.00 | 418.75 | 1200.00 | 25.00 |

---

## Key Takeaways

### Constraints:
- NOT NULL ensures columns always have values
- PRIMARY KEY uniquely identifies records and prevents NULL
- Constraints maintain data integrity and quality
- Use ALTER TABLE to add constraints to existing tables

### Aggregate Functions:
- COUNT() counts rows or non-null values
- SUM() calculates totals of numeric columns
- MAX() finds the highest value
- MIN() finds the lowest value
- AVG() calculates the average
- All aggregate functions ignore NULL values
- Use with GROUP BY for grouped calculations
- Use with HAVING to filter grouped results
- Combine multiple aggregate functions for comprehensive analysis
