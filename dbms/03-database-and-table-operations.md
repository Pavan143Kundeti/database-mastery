# Database & Table Operations

## Why SQL Databases Exist

SQL databases store data in tables (rows and columns). They are used everywhere because:

| Reason | What it means |
|--------|---------------|
| **Efficient** | Find and manage data quickly |
| **Reliable** | Data stays correct and safe |
| **Relationships** | Connect different data together |
| **Transactions** | Changes happen completely or not at all |
| **Scalable** | Works with small or huge amounts of data |
| **ACID** | Guarantees data accuracy |

**Simple Example:** A bank needs to track accounts, transactions, customers - SQL databases keep all this organized and safe.

---

## How SQL Databases Work

SQL uses a client-server system:

```
┌─────────┐         ┌──────────────┐
│  Client │────────▶│    Server    │
│  (You)  │◀────────│  (Database)  │
└─────────┘         └──────────────┘
```

**Three Steps:**

1. **Data Storage:** Data saved in tables (rows and columns)
2. **Query Processing:** Server reads your SQL command and executes it
3. **Data Retrieval:** Server sends back the results

---

## Database Operations

### 1. CREATE Database

Creates a new empty database.

**Syntax:**
```sql
CREATE DATABASE database_name;
```

**Example:**
```sql
CREATE DATABASE school_db;
```

**What happens:** A new database called "school_db" is created. It's empty - no tables yet.

---

### 2. USE Database

Selects which database to work with.

**Syntax:**
```sql
USE database_name;
```

**Example:**
```sql
USE school_db;
```

**What happens:** All your next commands will work on "school_db".

---

### 3. RENAME Database

Changes the name of a database.

**Syntax:**
```sql
RENAME DATABASE old_name TO new_name;
```

**Example:**
```sql
RENAME DATABASE school_db TO college_db;
```

**Note:** Not all databases support this command. Some require other methods.

---

### 4. DROP Database

Deletes a database permanently.

**Syntax:**
```sql
DROP DATABASE database_name;
```

**Example:**
```sql
DROP DATABASE school_db;
```

**Warning:** This deletes everything - all tables and data. Cannot be undone!

---

## Table Operations

### CREATE TABLE

Creates a new table inside a database.

**Syntax:**
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype
);
```

**Example:**
```sql
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50),
    Age INT,
    Course VARCHAR(50)
);
```

**What happens:** A table called "Student" is created with 4 columns.

---

## Data Types (Quick Reference)

| Data Type | What it stores | Example |
|-----------|----------------|---------|
| **INT** | Whole numbers | 25, 100, -5 |
| **VARCHAR(size)** | Text (variable length) | 'John', 'Hello' |
| **DATE** | Date | '2024-01-15' |
| **DECIMAL(p,s)** | Decimal numbers | 99.99, 1234.56 |

---

## Constraints

Rules to keep data correct.

| Constraint | What it does | Example |
|------------|--------------|---------|
| **PRIMARY KEY** | Unique ID for each row | StudentID |
| **NOT NULL** | Cannot be empty | Name NOT NULL |
| **UNIQUE** | All values must be different | Email UNIQUE |
| **CHECK** | Must meet condition | Age >= 18 |
| **DEFAULT** | Sets default value | Status DEFAULT 'Active' |

---

## Example: Creating a Customer Table

```sql
CREATE TABLE Customer (
    CustomerID INT PRIMARY KEY,
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    Country VARCHAR(50),
    Age INT CHECK (Age >= 0 AND Age <= 99),
    Phone VARCHAR(15)
);
```

**Explanation:**
- CustomerID: Unique number for each customer
- FirstName, LastName: Cannot be empty (NOT NULL)
- Age: Must be between 0 and 99 (CHECK)
- Phone: Stores phone number as text

---

## Inserting Data

After creating a table, add data using INSERT.

**Syntax:**
```sql
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

**Example:**
```sql
INSERT INTO Customer (CustomerID, FirstName, LastName, Country, Age, Phone)
VALUES (1, 'John', 'Smith', 'USA', 25, '1234567890');
```

**Multiple Rows:**
```sql
INSERT INTO Customer VALUES
(1, 'John', 'Smith', 'USA', 25, '1234567890'),
(2, 'Maria', 'Garcia', 'Spain', 30, '9876543210'),
(3, 'Li', 'Wang', 'China', 28, '5555555555');
```

---

## Create Table from Existing Table

Copy structure and data from another table.

**Syntax:**
```sql
CREATE TABLE new_table AS
SELECT column1, column2
FROM existing_table;
```

**Example:**
```sql
CREATE TABLE CustomerBackup AS
SELECT CustomerID, FirstName, LastName
FROM Customer;
```

**What happens:** New table "CustomerBackup" is created with selected columns and all data copied.

**Copy All Columns:**
```sql
CREATE TABLE CustomerBackup AS
SELECT * FROM Customer;
```

---

## SQL vs NoSQL

| Feature | SQL | NoSQL |
|---------|-----|-------|
| **Structure** | Tables (rows & columns) | Flexible (documents, key-value) |
| **Schema** | Fixed | Flexible |
| **Best For** | Structured data | Unstructured data |
| **Examples** | MySQL, PostgreSQL | MongoDB, Redis |
| **Relationships** | Strong (joins) | Weak |
| **Scaling** | Vertical (bigger server) | Horizontal (more servers) |

**When to use SQL:**
- Banking, e-commerce, school systems
- Need complex queries and relationships
- Data structure is clear and fixed

**When to use NoSQL:**
- Social media, big data, real-time apps
- Data structure changes often
- Need very fast reads/writes

---

## Important Tips

### 1. Check if Table Exists
```sql
CREATE TABLE IF NOT EXISTS Student (...);
```
Prevents error if table already exists.

### 2. View Table Structure
```sql
DESC table_name;
```
Shows all columns and their data types.

### 3. Modify Table After Creation
```sql
ALTER TABLE Student ADD Email VARCHAR(50);
```
Adds new column to existing table.

### 4. Delete Table
```sql
DROP TABLE table_name;
```
Deletes table permanently.

---

## Key Takeaways

- CREATE DATABASE makes new database
- USE selects which database to work with
- DROP DATABASE deletes database (careful!)
- CREATE TABLE makes new table with columns
- Use constraints to keep data correct
- INSERT adds data to tables
- Can copy tables using CREATE TABLE AS SELECT
- SQL is best for structured data with relationships
- Always use IF NOT EXISTS to avoid errors

