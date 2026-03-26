# Introduction to SQL

## What is SQL?

SQL stands for Structured Query Language. It is a standard language used to interact with relational databases.

- SQL lets you access and manipulate databases
- SQL became a standard of ANSI (American National Standards Institute) in 1986
- SQL became a standard of ISO (International Organization for Standardization) in 1987

---

## What Can SQL Do?

SQL allows you to perform many operations on databases:

| Operation | Description |
|-----------|-------------|
| Execute queries | Run commands to get information from database |
| Retrieve data | Get data from database tables |
| Insert records | Add new data into tables |
| Update records | Modify existing data |
| Delete records | Remove data from tables |
| Create databases | Make new databases |
| Create tables | Make new tables in a database |
| Create stored procedures | Save SQL code for reuse |
| Create views | Make virtual tables |
| Set permissions | Control who can access data |

---

## SQL is a Standard - BUT...

Although SQL is an ANSI/ISO standard, different database systems have their own versions.

However, all compliant systems support at least the major commands in a similar way:
- SELECT
- UPDATE
- DELETE
- INSERT
- WHERE

**Note:** Most SQL database programs have their own extra features beyond the SQL standard.

---

## Using SQL in Web Development

To build a website that shows data from a database, you need:

1. **RDBMS database program** (MS Access, SQL Server, MySQL, etc.)
2. **Server-side scripting language** (PHP, ASP, Python, etc.)
3. **SQL** to get the data you want
4. **HTML/CSS** to style the page

---

## RDBMS (Relational Database Management System)

RDBMS stands for Relational Database Management System.

- RDBMS is the foundation for SQL
- Used by all modern database systems like:
  - MS SQL Server
  - IBM DB2
  - Oracle
  - MySQL
  - PostgreSQL
  - Microsoft Access

### How RDBMS Stores Data

Data in RDBMS is stored in database objects called **tables**.

**Table:** A collection of related data organized in columns and rows.

### Example: Customers Table

```
CustomerID | CustomerName | ContactName | Address | City | PostalCode | Country
-----------|--------------|-------------|---------|------|------------|--------
1          | John Store   | John Doe    | 123 St  | NYC  | 10001      | USA
2          | Jane Shop    | Jane Smith  | 456 Ave | LA   | 90001      | USA
```

**Column:** A vertical entity in a table (like CustomerID, CustomerName)

**Record (Row):** A horizontal entity in a table. Each individual entry in a table.

---

## Writing Your First SQL Query

Before running SQL queries, you need to set up a database server like MySQL, PostgreSQL, or SQLite.

### Steps to Set Up MySQL Environment:

1. Install MySQL on your system
2. Start MySQL Server
3. Access MySQL Command Line

### Hello World Example

```sql
-- 1. Create a database
CREATE DATABASE test_db;

-- 2. Use the database
USE test_db;

-- 3. Create a table
CREATE TABLE greetings (
    id INT AUTO_INCREMENT PRIMARY KEY,
    message VARCHAR(255)
);

-- 4. Insert a message
INSERT INTO greetings (message) VALUES ('Hello, World!');

-- 5. Retrieve the message
SELECT message FROM greetings;
```

**Output:** Hello, World!

**Try it:** Replace "Hello, World!" with your name and see your name displayed!

---

## Why Learn SQL?

SQL is essential for managing and querying data across various technologies:

### Key Use Cases

| Field | How SQL is Used |
|-------|-----------------|
| **Data Science & Analytics** | Query large datasets, clean data, generate reports and insights |
| **Machine Learning & AI** | Prepare and manage data for training models |
| **Web Development** | Manage user data, transactions, content in websites |
| **Cloud and Big Data** | Work with cloud databases (Amazon RDS, Azure SQL) and Big Data platforms |
| **Blockchain Systems** | Manage off-chain data alongside decentralized ledgers |

---

## Before and After SQL

### Before SQL
- Data was stored in files
- Hard to search and manage
- No standard way to access data

### After SQL
- Data organized in tables
- Easy to search and retrieve
- Standard language across systems

---

## How SQL Works

```
┌─────────────────────────────────────────────────────────┐
│                    SQL Query Execution                   │
└─────────────────────────────────────────────────────────┘

1. INPUT
   └─> User writes SQL query (SELECT, INSERT, etc.)
   
2. PARSING
   └─> System checks if query syntax is correct
   
3. OPTIMIZATION
   └─> System finds the fastest way to execute query
   
4. EXECUTION
   └─> Database runs the query
   
5. OUTPUT
   └─> Result or confirmation sent back to user
```

---

## Components of SQL System

| Component | Description |
|-----------|-------------|
| **Database** | Collection of data stored in tables |
| **Tables** | Store data with rules to keep it accurate |
| **Indexes** | Help find data faster |
| **Views** | Virtual tables created from queries |
| **Stored Procedures** | Pre-saved SQL code for tasks |
| **Transactions** | Group of actions that succeed or fail together |
| **Security & Permissions** | Control who can access data |
| **Joins** | Combine data from different tables |

---

## Rules for Writing SQL Queries

Follow these rules to write correct SQL queries:

| Rule | Description | Example |
|------|-------------|---------|
| **Semicolon** | End statements with `;` | `SELECT * FROM users;` |
| **Case-Insensitive** | Keywords can be uppercase or lowercase | `SELECT` or `select` |
| **Whitespace** | Use spaces and new lines for readability | Allowed |
| **Reserved Words** | Don't use SQL keywords as names | Avoid naming table "SELECT" |
| **String Values** | Use single quotes for text | `'John'` |
| **Comments** | Single-line: `--` Multi-line: `/* */` | `-- This is a comment` |
| **Naming Rules** | Start with letter, max 30 chars, use letters/numbers/underscore | `customer_name` |

---

## Different SQL Commands

SQL commands are grouped into 5 categories:

### 1. DDL (Data Definition Language)
Commands to create, change, and delete database objects.

| Command | Description |
|---------|-------------|
| CREATE | Creates new table, view, or object |
| ALTER | Modifies existing database object |
| DROP | Deletes entire table or object |
| TRUNCATE | Removes all records but keeps table structure |
| RENAME | Changes name of database object |

### 2. DML (Data Manipulation Language)
Commands to add, modify, and delete data.

| Command | Description |
|---------|-------------|
| INSERT | Adds new records |
| UPDATE | Modifies existing records |
| DELETE | Removes records |

### 3. DQL (Data Query Language)
Commands to retrieve data.

| Command | Description |
|---------|-------------|
| SELECT | Retrieves data from tables |

### 4. DCL (Data Control Language)
Commands to manage user access.

| Command | Description |
|---------|-------------|
| GRANT | Gives permissions to user |
| REVOKE | Removes permissions from user |

### 5. TCL (Transaction Control Language)
Commands to manage transactions.

| Command | Description |
|---------|-------------|
| COMMIT | Saves all changes permanently |
| ROLLBACK | Reverts changes made in transaction |
| SAVEPOINT | Sets a point to rollback to |

---

## Benefits of SQL

- **Efficient:** Works fast even with large data
- **Standard:** Works on most database systems
- **Scalable:** Handles small to very large databases
- **Flexible:** Supports advanced logic

## Limitations of SQL

- **Complex:** Advanced optimization can be difficult
- **Scalability Issues:** Not ideal for huge unstructured data
- **Different Versions:** Some features vary by database
- **Not Real-Time:** Traditional SQL not built for real-time analytics

---

## Applications of SQL

| Industry | Use Case |
|----------|----------|
| **E-Commerce** | Manage orders, products, inventory |
| **Healthcare** | Maintain patient records, appointments |
| **Banking** | Analyze transactions, generate reports |
| **Web Development** | Power dynamic websites with user data |
| **Machine Learning** | Prepare data for AI models |

---

## Key Takeaways

- SQL is the standard language for databases
- SQL works with relational databases (data in tables)
- SQL has 5 command types: DDL, DML, DQL, DCL, TCL
- SQL is used across many industries and technologies
- Learning SQL opens doors to data science, web development, and more
