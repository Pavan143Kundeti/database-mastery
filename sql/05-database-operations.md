# SQL Database Operations

## What are SQL Databases?

SQL databases (also called relational databases) store and manage structured data in a tabular format.

**Key Features:**
- Data stored in tables with rows and columns
- Uses SQL language to interact with data
- Enables creating, updating, retrieving, and managing data

---

## Why Do SQL Databases Exist?

| Reason | Description |
|--------|-------------|
| **Efficient Data Handling** | Store, retrieve, and manipulate structured data effectively |
| **Reliability** | Ensure data consistency and integrity |
| **Complex Relationships** | Model relationships between different data entities |
| **Transaction Support** | Reliable transaction processing with consistency |
| **Scalability** | Handle growing data volumes while maintaining performance |
| **ACID Compliance** | Guarantee data accuracy and reliability |
| **Universal Adoption** | Widely used across industries |

### ACID Properties:

```
A - Atomicity     : All operations succeed or all fail
C - Consistency   : Data remains valid after transactions
I - Isolation     : Transactions don't interfere with each other
D - Durability    : Committed data is permanently saved
```

---

## How SQL Databases Work

SQL databases use a client-server system:

```
┌─────────────────────────────────────────────────────────┐
│              How SQL Databases Work                      │
└─────────────────────────────────────────────────────────┘

CLIENT                    SERVER                    DATABASE
  |                         |                          |
  |--1. Send SQL Query----->|                          |
  |                         |--2. Parse Query--------->|
  |                         |--3. Optimize Query------>|
  |                         |--4. Execute Query------->|
  |                         |<-5. Retrieve Data--------|
  |<-6. Return Results------|                          |
```

### Components:

1. **Data Storage**
   - Data stored in tables
   - Each table has rows (records) and columns (attributes)
   - Each column defines a specific attribute
   - Each row represents a unique entity

2. **Query Processing**
   - User submits SQL query
   - Server parses the command
   - Optimizes query for performance
   - Executes the query

3. **Data Retrieval**
   - Use SELECT to retrieve data
   - Use WHERE to filter results
   - Server returns data in structured format

---

## SQL Database Management

### 1. CREATE Database

Creates a new empty database.

**Syntax:**
```sql
CREATE DATABASE database_name;
```

**Example:**
```sql
CREATE DATABASE test_db;
```

**What It Does:**
- Creates an empty database
- Acts as a container for tables and objects
- Allows you to organize and manage data

---

### 2. SELECT Database

Selects a database to work with in the current session.

**Syntax:**
```sql
USE database_name;
```

**Example:**
```sql
USE test_db;
```

**What It Does:**
- Makes the database active
- All subsequent queries run on this database
- Essential when working with multiple databases

---

### 3. RENAME Database

Renames an existing database.

**Syntax:**
```sql
RENAME DATABASE old_name TO new_name;
```

**Example:**
```sql
RENAME DATABASE test_db TO new_test_db;
```

**Note:** Not all database systems support this command. Check your database documentation.

---

### 4. DROP Database

Permanently deletes a database and all its contents.

**Syntax:**
```sql
DROP DATABASE database_name;
```

**Example:**
```sql
DROP DATABASE test_db;
```

**Warning:**
- This action is irreversible
- Deletes all tables, data, and objects
- Use with extreme caution
- Frees up system resources

---

## Database Operations Summary

| Operation | Command | Purpose | Reversible? |
|-----------|---------|---------|-------------|
| **Create** | `CREATE DATABASE` | Make new database | Yes (use DROP) |
| **Select** | `USE` | Choose active database | Yes (use another USE) |
| **Rename** | `RENAME DATABASE` | Change database name | Yes (rename again) |
| **Drop** | `DROP DATABASE` | Delete database | No (permanent) |

---

## SQL vs NoSQL Databases

### Comparison Table:

| Feature | SQL Databases | NoSQL Databases |
|---------|---------------|-----------------|
| **Data Structure** | Tables with rows and columns | Key-value, documents, columns, graphs |
| **Schema** | Fixed schema (predefined) | Flexible schema (dynamic) |
| **Data Integrity** | ACID compliance | Eventual consistency |
| **Scalability** | Vertical (add more power) | Horizontal (add more servers) |
| **Query Language** | SQL | Various (MongoDB, Cassandra, etc.) |
| **Transactions** | Full ACID support | Limited transaction support |
| **Use Cases** | Structured data, complex queries | Unstructured data, scalability |
| **Examples** | MySQL, PostgreSQL, SQL Server | MongoDB, Cassandra, Redis |
| **Relationships** | Complex joins supported | Limited relationship support |
| **Flexibility** | Less flexible | Highly flexible |

### When to Use SQL:
- Structured data with clear relationships
- Need for complex queries and joins
- Financial transactions requiring ACID compliance
- Data integrity is critical

### When to Use NoSQL:
- Unstructured or semi-structured data
- Need for horizontal scalability
- Rapid development with changing requirements
- High-volume, high-velocity data

---

## Practical Example: Complete Database Workflow

```sql
-- Step 1: Create a new database
CREATE DATABASE company_db;

-- Step 2: Select the database
USE company_db;

-- Step 3: Create tables (covered in next section)
-- (We'll learn this next)

-- Step 4: Work with data
-- (Insert, update, delete operations)

-- Step 5: If needed, rename database
RENAME DATABASE company_db TO enterprise_db;

-- Step 6: If no longer needed, drop database
DROP DATABASE enterprise_db;
```

---

## Best Practices

### Creating Databases:
1. Use descriptive names (e.g., `sales_db`, `inventory_db`)
2. Follow naming conventions (lowercase, underscores)
3. Plan database structure before creating

### Selecting Databases:
1. Always verify you're in the correct database
2. Use `SELECT DATABASE();` to check current database

### Renaming Databases:
1. Backup data before renaming
2. Update application connection strings
3. Test thoroughly after renaming

### Dropping Databases:
1. Always backup before dropping
2. Verify database name before executing
3. Consider archiving instead of dropping
4. Use `IF EXISTS` to avoid errors:
   ```sql
   DROP DATABASE IF EXISTS test_db;
   ```

---

## Common Database Commands

```sql
-- Show all databases
SHOW DATABASES;

-- Check current database
SELECT DATABASE();

-- Create database with character set
CREATE DATABASE my_db CHARACTER SET utf8mb4;

-- Create database if it doesn't exist
CREATE DATABASE IF NOT EXISTS my_db;

-- Drop database if it exists
DROP DATABASE IF EXISTS my_db;
```

---

## Key Takeaways

- SQL databases store structured data in tables
- ACID properties ensure data reliability
- Four main database operations: CREATE, SELECT (USE), RENAME, DROP
- CREATE DATABASE makes a new database
- USE selects the active database
- DROP DATABASE permanently deletes a database
- SQL databases are ideal for structured data with complex relationships
- NoSQL databases are better for unstructured data and scalability
- Always backup before dropping or renaming databases
