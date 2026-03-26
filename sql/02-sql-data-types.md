# SQL Data Types

## What are Data Types?

In SQL, each column in a table must have a data type. A data type defines what kind of data the column can store.

**Why Data Types Matter:**
- Ensures data integrity (correct data in correct format)
- Improves query performance
- Enables efficient indexing
- Saves storage space

---

## 1. Numeric Data Types

Numeric data types store numbers. They allow mathematical operations like addition, subtraction, multiplication, and division.

### Exact Numeric Data Types

Used when you need precise values (like money, quantities, counts).

| Data Type | Description | Range |
|-----------|-------------|-------|
| **BIGINT** | Large integer numbers | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |
| **INT** | Standard integer values | -2,147,483,648 to 2,147,483,647 |
| **SMALLINT** | Small integers | -32,768 to 32,767 |
| **TINYINT** | Very small integers | 0 to 255 |
| **DECIMAL** | Exact fixed-point numbers | -10^38 + 1 to 10^38 - 1 |
| **NUMERIC** | Similar to DECIMAL | -10^38 + 1 to 10^38 - 1 |

**Example:**

```sql
CREATE TABLE Product_Sales (
    ProductID INT PRIMARY KEY,
    Quantity SMALLINT,
    UnitPrice DECIMAL(10,2),
    TotalAmount DECIMAL(10,2)
);
```

**Explanation:**
- `ProductID` stores standard integers
- `Quantity` stores small numbers (like 1, 2, 100)
- `UnitPrice` stores prices with 2 decimal places (like 99.99)
- `TotalAmount` stores total with 2 decimal places

### Approximate Numeric Data Types

Used for scientific measurements or when exact precision is not needed.

| Data Type | Description | Range |
|-----------|-------------|-------|
| **FLOAT** | Approximate numeric values | -1.79E+308 to 1.79E+308 |
| **REAL** | Similar to FLOAT, less precision | -3.40E+38 to 3.40E+38 |

**Example:**

```sql
CREATE TABLE Measurements (
    SensorID INT,
    Temperature FLOAT,
    Humidity REAL
);
```

**When to Use:**
- Temperature readings
- Scientific calculations
- Large range values where small errors are acceptable

---

## 2. Character and String Data Types

Character data types store text or character-based data.

### Non-Unicode Character Types

| Data Type | Description | Max Length |
|-----------|-------------|------------|
| **CHAR** | Fixed-length characters | 8,000 characters |
| **VARCHAR** | Variable-length characters | 8,000 characters |
| **VARCHAR(MAX)** | Very large variable-length text | 2^31 - 1 characters |
| **TEXT** | Large variable-length text | 2,147,483,647 characters |

**Difference between CHAR and VARCHAR:**

```
CHAR(10):
- Always uses 10 characters of space
- If you store "Hi", it adds 8 spaces: "Hi        "

VARCHAR(10):
- Uses only the space needed
- If you store "Hi", it uses only 2 characters: "Hi"
```

**Example:**

```sql
CREATE TABLE Employee_Info (
    EmpID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName CHAR(30),
    Bio TEXT
);
```

### Unicode Character Types

Unicode supports characters from any language (English, Chinese, Arabic, etc.).

| Data Type | Description | Max Length |
|-----------|-------------|------------|
| **NCHAR** | Fixed-length Unicode | 4,000 characters |
| **NVARCHAR** | Variable-length Unicode | 4,000 characters |
| **NVARCHAR(MAX)** | Very large Unicode text | 2^31 - 1 characters |

**Example:**

```sql
CREATE TABLE International_Users (
    UserID INT PRIMARY KEY,
    FullName NVARCHAR(100),
    Country NCHAR(50)
);
```

**When to Use Unicode:**
- Storing names in different languages
- International applications
- Multi-language content

---

## 3. Date and Time Data Types

Used to store date and time information.

| Data Type | Description | Storage Size | Format |
|-----------|-------------|--------------|--------|
| **DATE** | Stores date only (year, month, day) | 3 Bytes | YYYY-MM-DD |
| **TIME** | Stores time only (hour, minute, second) | 3 Bytes | HH:MM:SS |
| **DATETIME** | Stores both date and time | 8 Bytes | YYYY-MM-DD HH:MM:SS |

**Example:**

```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    OrderDate DATE,
    OrderTime TIME,
    ShippedAt DATETIME
);
```

**Sample Data:**

```
OrderID | OrderDate  | OrderTime | ShippedAt
--------|------------|-----------|-------------------
1       | 2026-03-27 | 14:30:00  | 2026-03-28 09:15:00
2       | 2026-03-26 | 10:00:00  | 2026-03-27 16:45:00
```

---

## 4. Binary Data Types

Binary data types store binary data like images, videos, or files.

| Data Type | Description | Max Length |
|-----------|-------------|------------|
| **BINARY** | Fixed-length binary data | 8,000 bytes |
| **VARBINARY** | Variable-length binary data | 8,000 bytes |
| **IMAGE** | Stores binary data as images | 2,147,483,647 bytes |

**Example:**

```sql
CREATE TABLE Product_Images (
    ImageID INT PRIMARY KEY,
    ImageName VARCHAR(100),
    ImageData VARBINARY(MAX)
);
```

**Use Cases:**
- Storing product images
- Storing PDF files
- Storing audio/video files

---

## 5. Boolean Data Type

Boolean stores logical values: TRUE or FALSE.

**Note:** In some databases like SQLite, there is no separate BOOLEAN type. Instead:
- `1` represents TRUE
- `0` represents FALSE

**Example:**

```sql
CREATE TABLE User_Status (
    UserID INT PRIMARY KEY,
    IsActive INTEGER,
    IsVerified INTEGER
);
```

**Sample Data:**

```
UserID | IsActive | IsVerified
-------|----------|------------
1      | 1        | 1          (Active and Verified)
2      | 0        | 1          (Inactive but Verified)
3      | 1        | 0          (Active but Not Verified)
```

---

## 6. Special Data Types

### XML Data Type

Stores XML data and allows XML manipulation.

**Example:**

```sql
CREATE TABLE XML_Records (
    RecordID INT PRIMARY KEY,
    ConfigData XML
);
```

**Use Case:** Storing configuration files, structured documents

### Spatial Data Type (GEOMETRY)

Stores planar spatial data like points, lines, and polygons.

**Example:**

```sql
CREATE TABLE Locations (
    LocationID INT PRIMARY KEY,
    Area GEOMETRY
);
```

**Use Case:** Mapping applications, GPS coordinates, geographic data

---

## Quick Reference Table

| Category | Data Types | Common Use |
|----------|------------|------------|
| **Exact Numbers** | INT, BIGINT, DECIMAL | IDs, quantities, prices |
| **Approximate Numbers** | FLOAT, REAL | Scientific data, measurements |
| **Text** | VARCHAR, CHAR, TEXT | Names, descriptions, content |
| **Unicode Text** | NVARCHAR, NCHAR | Multi-language text |
| **Date/Time** | DATE, TIME, DATETIME | Timestamps, schedules |
| **Binary** | VARBINARY, IMAGE | Files, images, videos |
| **Boolean** | INTEGER (0/1) | Flags, status indicators |
| **Special** | XML, GEOMETRY | Structured data, maps |

---

## Choosing the Right Data Type

### Tips:

1. **Use the smallest data type** that fits your data
   - Don't use BIGINT if INT is enough
   - Saves storage space and improves performance

2. **Use VARCHAR instead of CHAR** for variable-length text
   - Saves space when text length varies

3. **Use DECIMAL for money** values
   - Exact precision needed for financial data

4. **Use DATE/TIME types** for dates
   - Don't store dates as strings
   - Enables date calculations and sorting

5. **Use appropriate length**
   - VARCHAR(50) for names
   - VARCHAR(255) for emails
   - TEXT for long descriptions

---

## Key Takeaways

- Data types define what kind of data a column can store
- Choosing correct data types ensures data integrity and performance
- Numeric types: INT for whole numbers, DECIMAL for exact decimals, FLOAT for approximate
- Text types: VARCHAR for variable text, CHAR for fixed text
- Date types: DATE for dates, TIME for time, DATETIME for both
- Binary types: VARBINARY for files and images
- Boolean: Use INTEGER with 0/1 in databases without BOOLEAN type
