# SQL CREATE TABLE

## What is CREATE TABLE?

The CREATE TABLE statement defines a new table in a database. It specifies:
- Table name
- Column names
- Data types for each column
- Constraints (rules for data)

This command creates the structure for storing and organizing data.

---

## Syntax

```sql
CREATE TABLE table_name (
    column1 datatype(size),
    column2 datatype(size),
    column3 datatype(size),
    ...
    columnN datatype(size)
);
```

**Parameters:**
- `table_name`: Name you assign to the new table
- `column1, column2, ...`: Names of the columns
- `datatype(size)`: Data type and size of each column

---

## Example: Create a Customer Table

Let's create a Customer table to store customer information.

```sql
CREATE TABLE Customer (
    CustomerID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Country VARCHAR(50),
    Age INT CHECK (Age >= 0 AND Age <= 99),
    Phone INT(10)
);
```

### Explanation:

| Column | Data Type | Description |
|--------|-----------|-------------|
| **CustomerID** | INT PRIMARY KEY | Unique identifier for each customer |
| **FirstName** | VARCHAR(50) | Customer's first name (up to 50 characters) |
| **LastName** | VARCHAR(50) | Customer's last name (up to 50 characters) |
| **Country** | VARCHAR(50) | Customer's country |
| **Age** | INT CHECK | Age with constraint (0-99) |
| **Phone** | INT(10) | Phone number (10 digits) |

### Constraints Used:

- **PRIMARY KEY**: Ensures CustomerID is unique and not null
- **CHECK**: Ensures Age is between 0 and 99

**Note:** In real scenarios, use VARCHAR for phone numbers to allow leading zeros and formatting.

---

## Inserting Data into the Table

After creating the table, use INSERT INTO to add data.

```sql
INSERT INTO Customer (CustomerID, FirstName, LastName, Country, Age, Phone)
VALUES
    (1, 'Luca', 'Bianchi', 'Italy', 23, 1234567890),
    (2, 'Aiko', 'Tanaka', 'Japan', 21, 2345678901),
    (3, 'Carlos', 'Gomez', 'Spain', 24, 3456789012),
    (4, 'Sofia', 'Müller', 'Germany', 22, 4567890123),
    (5, 'Ethan', 'Johnson', 'USA', 25, 5678901234);
```

### View the Data:

```sql
SELECT * FROM Customer;
```

### Output:

| CustomerID | FirstName | LastName | Country | Age | Phone |
|------------|-----------|----------|---------|-----|-------|
| 1 | Luca | Bianchi | Italy | 23 | 1234567890 |
| 2 | Aiko | Tanaka | Japan | 21 | 2345678901 |
| 3 | Carlos | Gomez | Spain | 24 | 3456789012 |
| 4 | Sofia | Müller | Germany | 22 | 4567890123 |
| 5 | Ethan | Johnson | USA | 25 | 5678901234 |

**Tip:** For large amounts of data, use bulk inserts or import from external files for better performance.

---

## Create Table from Existing Table

You can create a new table based on an existing table's structure and data.

### Syntax:

```sql
CREATE TABLE new_table_name AS
SELECT column1, column2, ...
FROM existing_table_name
WHERE condition;
```

### Example:

Create a new table `SubTable` with only CustomerID and FirstName from Customer table.

```sql
CREATE TABLE SubTable AS
SELECT CustomerID, FirstName
FROM Customer;
```

### View the New Table:

```sql
SELECT * FROM SubTable;
```

### Output:

| CustomerID | FirstName |
|------------|-----------|
| 1 | Luca |
| 2 | Aiko |
| 3 | Carlos |
| 4 | Sofia |
| 5 | Ethan |

### Copy Entire Table:

Use `*` to copy all columns:

```sql
CREATE TABLE CustomerBackup AS
SELECT * FROM Customer;
```

**Use Cases:**
- Creating backups
- Quick data migrations
- Creating test tables
- Extracting specific data

---

## Common Constraints

Constraints are rules applied to columns to ensure data integrity.

| Constraint | Description | Example |
|------------|-------------|---------|
| **PRIMARY KEY** | Unique identifier, cannot be NULL | `CustomerID INT PRIMARY KEY` |
| **NOT NULL** | Column cannot have NULL values | `FirstName VARCHAR(50) NOT NULL` |
| **UNIQUE** | All values must be unique | `Email VARCHAR(100) UNIQUE` |
| **CHECK** | Values must meet a condition | `Age INT CHECK (Age >= 18)` |
| **DEFAULT** | Sets default value if none provided | `Country VARCHAR(50) DEFAULT 'USA'` |
| **FOREIGN KEY** | Links to another table's primary key | `FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)` |

### Example with Multiple Constraints:

```sql
CREATE TABLE Employee (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    Age INT CHECK (Age >= 18 AND Age <= 65),
    Department VARCHAR(50) DEFAULT 'General',
    Salary DECIMAL(10,2) CHECK (Salary > 0)
);
```

---

## CREATE TABLE IF NOT EXISTS

Avoid errors if table already exists:

```sql
CREATE TABLE IF NOT EXISTS Customer (
    CustomerID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50)
);
```

**What It Does:**
- Checks if table exists
- Creates table only if it doesn't exist
- Prevents error messages

---

## View Table Structure

After creating a table, view its structure:

```sql
-- Method 1: DESCRIBE
DESC Customer;

-- Method 2: SHOW COLUMNS
SHOW COLUMNS FROM Customer;

-- Method 3: SHOW CREATE TABLE
SHOW CREATE TABLE Customer;
```

### Output Example (DESC):

| Field | Type | Null | Key | Default | Extra |
|-------|------|------|-----|---------|-------|
| CustomerID | int | NO | PRI | NULL | |
| FirstName | varchar(50) | YES | | NULL | |
| LastName | varchar(50) | YES | | NULL | |
| Country | varchar(50) | YES | | NULL | |
| Age | int | YES | | NULL | |
| Phone | int(10) | YES | | NULL | |

---

## Practical Examples

### Example 1: Products Table

```sql
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(100) NOT NULL,
    Category VARCHAR(50),
    Price DECIMAL(10,2) CHECK (Price > 0),
    Stock INT DEFAULT 0,
    CreatedDate DATE DEFAULT CURRENT_DATE
);
```

### Example 2: Orders Table with Foreign Key

```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    OrderDate DATE NOT NULL,
    TotalAmount DECIMAL(10,2),
    FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
);
```

### Example 3: Users Table with Multiple Constraints

```sql
CREATE TABLE Users (
    UserID INT PRIMARY KEY AUTO_INCREMENT,
    Username VARCHAR(50) NOT NULL UNIQUE,
    Email VARCHAR(100) NOT NULL UNIQUE,
    Password VARCHAR(255) NOT NULL,
    Age INT CHECK (Age >= 13),
    IsActive BOOLEAN DEFAULT TRUE,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## Tips for Using CREATE TABLE

### 1. Use Appropriate Data Types
Choose data types that match your data:
- `INT` for whole numbers
- `VARCHAR` for variable-length text
- `DECIMAL` for precise numbers (money)
- `DATE` for dates
- `BOOLEAN` for true/false values

### 2. Define Constraints
Always define constraints to ensure data integrity:
- Use PRIMARY KEY for unique identifiers
- Use NOT NULL for required fields
- Use CHECK for value validation
- Use FOREIGN KEY for relationships

### 3. Use IF NOT EXISTS
Prevent errors when table might already exist:
```sql
CREATE TABLE IF NOT EXISTS Customer (...);
```

### 4. Choose Good Names
- Use descriptive names (e.g., `Customer`, `Orders`)
- Use singular or plural consistently
- Avoid SQL keywords as names
- Use underscores for multi-word names (e.g., `order_items`)

### 5. Plan Before Creating
- Design table structure on paper first
- Identify relationships between tables
- Determine which columns need constraints
- Consider future data growth

---

## Modifying Tables After Creation

If you need to change the table structure after creation, use ALTER TABLE:

```sql
-- Add a new column
ALTER TABLE Customer ADD Email VARCHAR(100);

-- Modify a column
ALTER TABLE Customer MODIFY Phone VARCHAR(15);

-- Drop a column
ALTER TABLE Customer DROP COLUMN Phone;

-- Rename a column
ALTER TABLE Customer RENAME COLUMN FirstName TO First_Name;
```

---

## Common Errors and Solutions

### Error 1: Table Already Exists
```sql
-- Problem: CREATE TABLE Customer (...);
-- Error: Table 'Customer' already exists

-- Solution: Use IF NOT EXISTS
CREATE TABLE IF NOT EXISTS Customer (...);
```

### Error 2: Invalid Data Type
```sql
-- Problem: Age VARCHAR(50)
-- Issue: Age should be INT, not VARCHAR

-- Solution: Use correct data type
Age INT
```

### Error 3: Missing Primary Key
```sql
-- Problem: No PRIMARY KEY defined
-- Issue: Can't uniquely identify rows

-- Solution: Add PRIMARY KEY
CustomerID INT PRIMARY KEY
```

---

## Key Takeaways

- CREATE TABLE defines a new table structure
- Specify table name, column names, and data types
- Use constraints to ensure data integrity
- PRIMARY KEY ensures unique identification
- Use IF NOT EXISTS to avoid errors
- View table structure with DESC or SHOW COLUMNS
- Can create tables from existing tables using CREATE TABLE AS SELECT
- Always plan table structure before creating
- Use appropriate data types for each column
- Define constraints to maintain data quality
