# Introduction to MySQL

## What is MySQL?

MySQL is a popular open-source relational database management system (RDBMS).

**Simple Definition:** MySQL is free software that helps you store and manage data in tables.

---

## Key Features of MySQL

| Feature | Description |
|---------|-------------|
| **Open-Source** | Free to use and modify |
| **Fast** | Quick data retrieval and processing |
| **Reliable** | Stable and trustworthy |
| **Scalable** | Works for small and large applications |
| **Cross-Platform** | Runs on Windows, Linux, Mac |
| **ANSI SQL Compliant** | Follows standard SQL rules |
| **Easy to Use** | Simple syntax and commands |

---

## MySQL History

| Year | Event |
|------|-------|
| **1995** | MySQL first released |
| **Present** | Developed and supported by Oracle Corporation |
| **Name Origin** | Named after co-founder Monty Widenius's daughter "My" |

---

## Who Uses MySQL?

### Big Websites
- Facebook
- Twitter
- YouTube
- Airbnb
- Booking.com
- Uber
- GitHub

### Content Management Systems
- WordPress
- Drupal
- Joomla!
- Contao

### Others
- Millions of web developers worldwide
- Small to large businesses
- E-commerce platforms

---

## Building Web Applications with MySQL

To show database data on a website, you need:

| Component | Purpose |
|-----------|---------|
| **RDBMS (MySQL)** | Store and manage data |
| **Server-Side Language (PHP, Python, Node.js)** | Connect to database and process data |
| **SQL** | Query and retrieve data |
| **HTML/CSS** | Display and style the data |

**Example Flow:**
```
User Request → PHP/Python → SQL Query → MySQL Database → Results → HTML Page
```

---

## What is RDBMS?

RDBMS stands for Relational Database Management System.

**Simple Definition:** Software that manages databases where data is stored in related tables.

**Examples of RDBMS:**
- MySQL
- PostgreSQL
- Oracle
- Microsoft SQL Server
- Microsoft Access

**Key Point:** RDBMS uses SQL to access and manage data.

---

## What is a Database Table?

A table is a collection of related data organized in rows and columns.

**Structure:**
- **Column:** Holds specific type of information
- **Row (Record):** Individual entry in the table

**Example: Customers Table**

| CustomerID | CustomerName | ContactName | Address | City | PostalCode | Country |
|------------|--------------|-------------|---------|------|------------|---------|
| 1 | Alfreds Futterkiste | Maria Anders | Obere Str. 57 | Berlin | 12209 | Germany |
| 2 | Ana Trujillo Emparedados | Ana Trujillo | Avda. de la Constitución 2222 | México D.F. | 05021 | Mexico |
| 3 | Antonio Moreno Taquería | Antonio Moreno | Mataderos 2312 | México D.F. | 05023 | Mexico |
| 4 | Around the Horn | Thomas Hardy | 120 Hanover Sq. | London | WA1 1DP | UK |
| 5 | Berglunds snabbköp | Christina Berglund | Berguvsvägen 8 | Luleå | S-958 22 | Sweden |

**In this table:**
- **Columns:** CustomerID, CustomerName, ContactName, Address, City, PostalCode, Country
- **Rows:** 5 records (5 customers)

---

## What is a Relational Database?

A relational database stores data in multiple tables that are connected (related) to each other.

**Simple Definition:** Tables are linked using common data (keys).

---

## Example: Related Tables

### Table 1: Customers

| CustomerID | CustomerName | ContactName | City | Country |
|------------|--------------|-------------|------|---------|
| 1 | Alfreds Futterkiste | Maria Anders | Berlin | Germany |
| 2 | Ana Trujillo Emparedados | Ana Trujillo | México D.F. | Mexico |
| 3 | Antonio Moreno Taquería | Antonio Moreno | México D.F. | Mexico |
| 4 | Around the Horn | Thomas Hardy | London | UK |
| 5 | Berglunds snabbköp | Christina Berglund | Luleå | Sweden |

---

### Table 2: Orders

| OrderID | CustomerID | EmployeeID | OrderDate | ShipperID |
|---------|------------|------------|-----------|-----------|
| 10278 | 5 | 8 | 1996-08-12 | 2 |
| 10280 | 5 | 2 | 1996-08-14 | 1 |
| 10308 | 2 | 7 | 1996-09-18 | 3 |
| 10355 | 4 | 6 | 1996-11-15 | 1 |
| 10365 | 3 | 3 | 1996-11-27 | 2 |
| 10383 | 4 | 8 | 1996-12-16 | 3 |
| 10384 | 5 | 3 | 1996-12-16 | 3 |

**Relationship:** Orders table connects to Customers table using CustomerID

---

### Table 3: Shippers

| ShipperID | ShipperName | Phone |
|-----------|-------------|-------|
| 1 | Speedy Express | (503) 555-9831 |
| 2 | United Package | (503) 555-3199 |
| 3 | Federal Shipping | (503) 555-9931 |

**Relationship:** Orders table connects to Shippers table using ShipperID

---

## How Tables Are Related

```
Customers Table
    ↓ (CustomerID)
Orders Table
    ↓ (ShipperID)
Shippers Table
```

**Example Query:**
"Find all orders placed by customer 'Berglunds snabbköp' and which shipper delivered them"

**Steps:**
1. Find CustomerID for 'Berglunds snabbköp' in Customers table (CustomerID = 5)
2. Find all orders with CustomerID = 5 in Orders table
3. Find shipper names using ShipperID from Orders table

**Result:**
- Order 10278 shipped by United Package
- Order 10280 shipped by Speedy Express
- Order 10384 shipped by Federal Shipping

---

## Benefits of Relational Databases

| Benefit | Description |
|---------|-------------|
| **No Data Duplication** | Store data once, reference it many times |
| **Easy to Update** | Change data in one place, reflects everywhere |
| **Data Integrity** | Relationships ensure data stays consistent |
| **Flexible Queries** | Combine data from multiple tables easily |
| **Organized** | Data is structured and easy to understand |

---

## MySQL vs Other Databases

| Feature | MySQL | PostgreSQL | Oracle | SQL Server |
|---------|-------|------------|--------|------------|
| **Cost** | Free | Free | Paid | Paid |
| **Platform** | Cross-platform | Cross-platform | Cross-platform | Windows mainly |
| **Speed** | Very fast | Fast | Very fast | Fast |
| **Ease of Use** | Easy | Moderate | Complex | Moderate |
| **Best For** | Web apps | Complex queries | Enterprise | Microsoft stack |

---

## When to Use MySQL

**Good For:**
- Web applications
- E-commerce sites
- Content management systems
- Small to medium businesses
- Read-heavy applications
- Projects needing fast performance

**Not Ideal For:**
- Very complex transactions
- Applications needing advanced features
- Heavy write operations at massive scale

---

## Key Takeaways

- MySQL is a free, open-source database system
- Used by major websites and applications worldwide
- Stores data in tables (rows and columns)
- Tables are related using common columns (keys)
- RDBMS manages relational databases
- MySQL is fast, reliable, and easy to use
- Works on all major platforms
- Perfect for web development
- Follows standard SQL syntax

