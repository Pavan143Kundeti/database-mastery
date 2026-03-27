# PostgreSQL Database Operations

## Before You Start

**Make sure you're connected to the database!**

If you're not connected, go back to the previous chapter and follow the connection steps for either:
- SQL Shell (psql), OR
- pgAdmin 4

---

## CREATE TABLE

Creates a new table in the database.

### What is a Table?

A table is like an Excel spreadsheet - it has columns (headers) and rows (data).

**Example:** A cars table with columns: brand, model, year

---

### Step-by-Step: Create Your First Table

**We'll create a table called "cars" with 3 columns.**

---

### Using SQL Shell (psql)

**Step 1:** Make sure you see `postgres=#` prompt

**Step 2:** Type this command:

```sql
CREATE TABLE cars (
    brand VARCHAR(255),
    model VARCHAR(255),
    year INT
);
```

**Step 3:** Press Enter

**Step 4:** See the result:

```
CREATE TABLE
```

**What this means:** Table created successfully!

---

### Using pgAdmin 4

**Step 1:** Open Query Tool (right-click postgres database → Query Tool)

**Step 2:** Type the command in the text box:

```sql
CREATE TABLE cars (
    brand VARCHAR(255),
    model VARCHAR(255),
    year INT
);
```

**Step 3:** Click the Play button (▶) or press F5

**Step 4:** See the result in Messages tab:

```
CREATE TABLE

Query returned successfully in 52 msec.
```

---

### Understanding the Command

```sql
CREATE TABLE cars (
    brand VARCHAR(255),
    model VARCHAR(255),
    year INT
);
```

| Part | What it means |
|------|---------------|
| `CREATE TABLE` | Command to make new table |
| `cars` | Name of the table |
| `brand` | First column name |
| `VARCHAR(255)` | Text data type (up to 255 characters) |
| `model` | Second column name |
| `year` | Third column name |
| `INT` | Integer data type (whole numbers) |
| `;` | End of command (required!) |

---

### Common Data Types

| Data Type | What it stores | Example |
|-----------|----------------|---------|
| `VARCHAR(n)` | Text (up to n characters) | 'Ford', 'Toyota' |
| `INT` | Whole numbers | 1964, 2024 |
| `DECIMAL(p,s)` | Decimal numbers | 99.99, 1234.56 |
| `DATE` | Date | '2024-03-27' |
| `BOOLEAN` | True/False | TRUE, FALSE |

---

## Display Empty Table

Let's check if our table was created.

### Command

```sql
SELECT * FROM cars;
```

### Result in SQL Shell

```
 brand | model | year 
-------+-------+------
(0 rows)
```

**What this means:** Table exists but has no data yet (0 rows).

### Result in pgAdmin 4

**Data Output tab shows:**

```
Empty result set
```

**Messages tab shows:**

```
Query returned successfully in 23 msec.
```

---

## INSERT INTO - Add Data

Now let's add data to our table.

### Insert One Row

**Command:**

```sql
INSERT INTO cars (brand, model, year)
VALUES ('Ford', 'Mustang', 1964);
```

**Result in SQL Shell:**

```
INSERT 0 1
```

**What this means:**
- `INSERT` - Data was inserted
- `0` - Internal number (ignore it)
- `1` - One row was added

**Result in pgAdmin 4:**

```
INSERT 0 1

Query returned successfully in 45 msec.
```

---

### Understanding INSERT Command

```sql
INSERT INTO cars (brand, model, year)
VALUES ('Ford', 'Mustang', 1964);
```

| Part | What it means |
|------|---------------|
| `INSERT INTO` | Command to add data |
| `cars` | Table name |
| `(brand, model, year)` | Column names |
| `VALUES` | Keyword before data |
| `('Ford', 'Mustang', 1964)` | Actual data to insert |

**Important Rules:**
- Text values use single quotes: `'Ford'`
- Numbers don't use quotes: `1964`
- Must end with semicolon: `;`

---

### View the Data

**Command:**

```sql
SELECT * FROM cars;
```

**Result in SQL Shell:**

```
 brand |  model  | year 
-------+---------+------
 Ford  | Mustang | 1964
(1 row)
```

**Result in pgAdmin 4:**

**Data Output tab shows a table:**

| brand | model | year |
|-------|-------|------|
| Ford | Mustang | 1964 |

---

### Insert Multiple Rows

You can add many rows at once!

**Command:**

```sql
INSERT INTO cars (brand, model, year)
VALUES
    ('Volvo', 'p1800', 1968),
    ('BMW', 'M1', 1978),
    ('Toyota', 'Celica', 1975);
```

**Result:**

```
INSERT 0 3
```

**What this means:** 3 rows were added successfully!

---

### View All Data

**Command:**

```sql
SELECT * FROM cars;
```

**Result in SQL Shell:**

```
 brand  |  model  | year 
--------+---------+------
 Ford   | Mustang | 1964
 Volvo  | p1800   | 1968
 BMW    | M1      | 1978
 Toyota | Celica  | 1975
(4 rows)
```

**Result in pgAdmin 4:**

| brand | model | year |
|-------|-------|------|
| Ford | Mustang | 1964 |
| Volvo | p1800 | 1968 |
| BMW | M1 | 1978 |
| Toyota | Celica | 1975 |

---

## SELECT - Fetch Data

Get data from the table.

### Select Specific Columns

**Command:**

```sql
SELECT brand, year FROM cars;
```

**Result:**

```
 brand  | year 
--------+------
 Ford   | 1964
 Volvo  | 1968
 BMW    | 1978
 Toyota | 1975
(4 rows)
```

**What happened:** Only brand and year columns are shown (model is hidden).

---

### Select All Columns

**Command:**

```sql
SELECT * FROM cars;
```

**What `*` means:** All columns

---

## ALTER TABLE - ADD COLUMN

Add a new column to existing table.

### Add Color Column

**Command:**

```sql
ALTER TABLE cars
ADD color VARCHAR(255);
```

**Result:**

```
ALTER TABLE
```

**What this means:** Column added successfully!

---

### View Updated Table Structure

**Command:**

```sql
SELECT * FROM cars;
```

**Result:**

```
 brand  |  model  | year | color 
--------+---------+------+-------
 Ford   | Mustang | 1964 | 
 Volvo  | p1800   | 1968 | 
 BMW    | M1      | 1978 | 
 Toyota | Celica  | 1975 | 
(4 rows)
```

**Notice:** New `color` column exists but is empty (NULL).

---

## UPDATE - Modify Data

Change existing data in the table.

### Update One Row

**Command:**

```sql
UPDATE cars
SET color = 'red'
WHERE brand = 'Volvo';
```

**Result:**

```
UPDATE 1
```

**What this means:** 1 row was updated.

---

### View Updated Data

**Command:**

```sql
SELECT * FROM cars;
```

**Result:**

```
 brand  |  model  | year | color 
--------+---------+------+-------
 Ford   | Mustang | 1964 | 
 BMW    | M1      | 1978 | 
 Toyota | Celica  | 1975 | 
 Volvo  | p1800   | 1968 | red
(4 rows)
```

**Notice:** Only Volvo has color 'red'.

---

### Understanding UPDATE Command

```sql
UPDATE cars
SET color = 'red'
WHERE brand = 'Volvo';
```

| Part | What it means |
|------|---------------|
| `UPDATE` | Command to modify data |
| `cars` | Table name |
| `SET` | Keyword before changes |
| `color = 'red'` | Set color column to 'red' |
| `WHERE` | Condition (which rows to update) |
| `brand = 'Volvo'` | Only rows where brand is Volvo |

---

### Warning: Update Without WHERE

**Dangerous Command:**

```sql
UPDATE cars
SET color = 'red';
```

**Result:**

```
UPDATE 4
```

**What happened:** ALL 4 rows now have color 'red'!

**View result:**

```sql
SELECT * FROM cars;
```

```
 brand  |  model  | year | color 
--------+---------+------+-------
 Ford   | Mustang | 1964 | red
 BMW    | M1      | 1978 | red
 Toyota | Celica  | 1975 | red
 Volvo  | p1800   | 1968 | red
(4 rows)
```

**Lesson:** Always use WHERE clause unless you want to update ALL rows!

---

### Update Multiple Columns

**Command:**

```sql
UPDATE cars
SET color = 'white', year = 1970
WHERE brand = 'Toyota';
```

**Result:**

```
UPDATE 1
```

**View result:**

```sql
SELECT * FROM cars;
```

```
 brand  |  model  | year | color 
--------+---------+------+-------
 Ford   | Mustang | 1964 | red
 BMW    | M1      | 1978 | red
 Volvo  | p1800   | 1968 | red
 Toyota | Celica  | 1970 | white
(4 rows)
```

---

## ALTER TABLE - MODIFY COLUMN

Change the data type of a column.

### Change Year from INT to VARCHAR

**Command:**

```sql
ALTER TABLE cars
ALTER COLUMN year TYPE VARCHAR(4);
```

**Result:**

```
ALTER TABLE
```

**What happened:** Year column now stores text instead of numbers.

**Why would you do this?** Sometimes you need to store years like "2024" as text.

---

## ALTER TABLE - DROP COLUMN

Remove a column from the table.

### Remove Color Column

**Command:**

```sql
ALTER TABLE cars
DROP COLUMN color;
```

**Result:**

```
ALTER TABLE
```

---

### View Updated Table

**Command:**

```sql
SELECT * FROM cars;
```

**Result:**

```
 brand  |  model  | year 
--------+---------+------
 Ford   | Mustang | 1964
 BMW    | M1      | 1978
 Volvo  | p1800   | 1968
 Toyota | Celica  | 1970
(4 rows)
```

**Notice:** Color column is gone!

---

## DELETE - Remove Rows

Delete specific rows from the table.

### Delete One Row

**Command:**

```sql
DELETE FROM cars
WHERE brand = 'Volvo';
```

**Result:**

```
DELETE 1
```

**What this means:** 1 row was deleted.

---

### View Remaining Data

**Command:**

```sql
SELECT * FROM cars;
```

**Result:**

```
 brand  |  model  | year 
--------+---------+------
 Ford   | Mustang | 1964
 BMW    | M1      | 1978
 Toyota | Celica  | 1970
(3 rows)
```

**Notice:** Volvo is gone!

---

### Warning: Delete Without WHERE

**Dangerous Command:**

```sql
DELETE FROM cars;
```

**Result:**

```
DELETE 3
```

**What happened:** ALL rows deleted!

**View result:**

```sql
SELECT * FROM cars;
```

```
 brand | model | year 
-------+-------+------
(0 rows)
```

**Table still exists, but it's empty!**

---

## TRUNCATE TABLE

Delete all rows but keep table structure.

**Command:**

```sql
TRUNCATE TABLE cars;
```

**Result:**

```
TRUNCATE TABLE
```

**What this does:**
- Removes all rows
- Keeps table structure
- Faster than DELETE
- Cannot be undone

---

## DROP TABLE

Delete the entire table (structure + data).

### Delete Cars Table

**Command:**

```sql
DROP TABLE cars;
```

**Result:**

```
DROP TABLE
```

**What this means:** Table completely removed!

---

### Try to View Deleted Table

**Command:**

```sql
SELECT * FROM cars;
```

**Result:**

```
ERROR:  relation "cars" does not exist
LINE 1: SELECT * FROM cars;
                      ^
```

**What this means:** Table doesn't exist anymore!

---

## Operations Summary

| Operation | Command | What it does | Can undo? |
|-----------|---------|--------------|-----------|
| **CREATE TABLE** | CREATE TABLE name (...) | Make new table | Yes (DROP) |
| **INSERT INTO** | INSERT INTO table VALUES (...) | Add rows | Yes (DELETE) |
| **SELECT** | SELECT * FROM table | View data | N/A |
| **UPDATE** | UPDATE table SET ... WHERE ... | Change data | No |
| **DELETE** | DELETE FROM table WHERE ... | Remove rows | No |
| **ALTER TABLE ADD** | ALTER TABLE ADD column | Add column | Yes (DROP COLUMN) |
| **ALTER TABLE DROP** | ALTER TABLE DROP COLUMN | Remove column | No |
| **ALTER TABLE ALTER** | ALTER TABLE ALTER COLUMN | Change column type | No |
| **TRUNCATE** | TRUNCATE TABLE | Remove all rows | No |
| **DROP TABLE** | DROP TABLE | Delete table | No |

---

## Demo Database Setup

Now let's create a complete demo database with multiple tables!

**Why?** To practice more complex queries and relationships.

---

### Table 1: Categories

**Create Table:**

```sql
CREATE TABLE categories (
    category_id SERIAL NOT NULL PRIMARY KEY,
    category_name VARCHAR(255),
    description VARCHAR(255)
);
```

**Result:**

```
CREATE TABLE
```

**Understanding SERIAL:**
- `SERIAL` = Auto-incrementing number
- Starts at 1, increases automatically
- You don't need to provide this value

**Understanding PRIMARY KEY:**
- Unique identifier for each row
- Cannot be NULL
- Cannot have duplicates

---

**Insert Data:**

```sql
INSERT INTO categories (category_name, description)
VALUES
    ('Beverages', 'Soft drinks, coffees, teas, beers, and ales'),
    ('Condiments', 'Sweet and savory sauces, relishes, spreads, and seasonings'),
    ('Confections', 'Desserts, candies, and sweet breads'),
    ('Dairy Products', 'Cheeses'),
    ('Grains/Cereals', 'Breads, crackers, pasta, and cereal'),
    ('Meat/Poultry', 'Prepared meats'),
    ('Produce', 'Dried fruit and bean curd'),
    ('Seafood', 'Seaweed and fish');
```

**Result:**

```
INSERT 0 8
```

**What this means:** 8 categories added!

---

**View Data:**

```sql
SELECT * FROM categories;
```

**Result:**

```
 category_id | category_name  |                    description                     
-------------+----------------+----------------------------------------------------
           1 | Beverages      | Soft drinks, coffees, teas, beers, and ales
           2 | Condiments     | Sweet and savory sauces, relishes, spreads, and...
           3 | Confections    | Desserts, candies, and sweet breads
           4 | Dairy Products | Cheeses
           5 | Grains/Cereals | Breads, crackers, pasta, and cereal
           6 | Meat/Poultry   | Prepared meats
           7 | Produce        | Dried fruit and bean curd
           8 | Seafood        | Seaweed and fish
(8 rows)
```

**Notice:** category_id was generated automatically (1, 2, 3, 4, 5, 6, 7, 8)!

---

### Table 2: Customers

**Create Table:**

```sql
CREATE TABLE customers (
    customer_id SERIAL NOT NULL PRIMARY KEY,
    customer_name VARCHAR(255),
    contact_name VARCHAR(255),
    address VARCHAR(255),
    city VARCHAR(255),
    postal_code VARCHAR(255),
    country VARCHAR(255)
);
```

**Result:**

```
CREATE TABLE
```

---

**Insert Sample Data (first 5 customers):**

```sql
INSERT INTO customers (customer_name, contact_name, address, city, postal_code, country)
VALUES
    ('Alfreds Futterkiste', 'Maria Anders', 'Obere Str. 57', 'Berlin', '12209', 'Germany'),
    ('Ana Trujillo Emparedados y helados', 'Ana Trujillo', 'Avda. de la Constitucion 2222', 'Mexico D.F.', '05021', 'Mexico'),
    ('Antonio Moreno Taquera', 'Antonio Moreno', 'Mataderos 2312', 'Mexico D.F.', '05023', 'Mexico'),
    ('Around the Horn', 'Thomas Hardy', '120 Hanover Sq.', 'London', 'WA1 1DP', 'UK'),
    ('Berglunds snabbkoep', 'Christina Berglund', 'Berguvsvegen 8', 'Lulea', 'S-958 22', 'Sweden');
```

**Result:**

```
INSERT 0 5
```

---

**View Data:**

```sql
SELECT * FROM customers;
```

**Result:**

```
 customer_id |          customer_name           |   contact_name    |            address            |    city    | postal_code | country 
-------------+----------------------------------+-------------------+-------------------------------+------------+-------------+---------
           1 | Alfreds Futterkiste              | Maria Anders      | Obere Str. 57                 | Berlin     | 12209       | Germany
           2 | Ana Trujillo Emparedados y...    | Ana Trujillo      | Avda. de la Constitucion 2222 | Mexico D.F.| 05021       | Mexico
           3 | Antonio Moreno Taquera           | Antonio Moreno    | Mataderos 2312                | Mexico D.F.| 05023       | Mexico
           4 | Around the Horn                  | Thomas Hardy      | 120 Hanover Sq.               | London     | WA1 1DP     | UK
           5 | Berglunds snabbkoep              | Christina Berglund| Berguvsvegen 8                | Lulea      | S-958 22    | Sweden
(5 rows)
```

---

### Table 3: Products

**Create Table:**

```sql
CREATE TABLE products (
    product_id SERIAL NOT NULL PRIMARY KEY,
    product_name VARCHAR(255),
    category_id INT,
    unit VARCHAR(255),
    price DECIMAL(10, 2)
);
```

**Understanding DECIMAL(10, 2):**
- Total 10 digits
- 2 digits after decimal point
- Example: 12345678.99

---

**Insert Sample Data:**

```sql
INSERT INTO products (product_name, category_id, unit, price)
VALUES
    ('Chais', 1, '10 boxes x 20 bags', 18.00),
    ('Chang', 1, '24 - 12 oz bottles', 19.00),
    ('Aniseed Syrup', 2, '12 - 550 ml bottles', 10.00),
    ('Chef Antons Cajun Seasoning', 2, '48 - 6 oz jars', 22.00),
    ('Grandmas Boysenberry Spread', 2, '12 - 8 oz jars', 25.00);
```

**Result:**

```
INSERT 0 5
```

---

**View Data:**

```sql
SELECT * FROM products;
```

**Result:**

```
 product_id |        product_name         | category_id |         unit          | price 
------------+-----------------------------+-------------+-----------------------+-------
          1 | Chais                       |           1 | 10 boxes x 20 bags    | 18.00
          2 | Chang                       |           1 | 24 - 12 oz bottles    | 19.00
          3 | Aniseed Syrup               |           2 | 12 - 550 ml bottles   | 10.00
          4 | Chef Antons Cajun Seasoning |           2 | 48 - 6 oz jars        | 22.00
          5 | Grandmas Boysenberry Spread |           2 | 12 - 8 oz jars        | 25.00
(5 rows)
```

---

### Table 4: Orders

**Create Table:**

```sql
CREATE TABLE orders (
    order_id SERIAL NOT NULL PRIMARY KEY,
    customer_id INT,
    order_date DATE
);
```

---

**Insert Sample Data:**

```sql
INSERT INTO orders (customer_id, order_date)
VALUES
    (1, '2021-07-04'),
    (2, '2021-07-05'),
    (3, '2021-07-08'),
    (4, '2021-07-08'),
    (5, '2021-07-09');
```

**Result:**

```
INSERT 0 5
```

---

**View Data:**

```sql
SELECT * FROM orders;
```

**Result:**

```
 order_id | customer_id | order_date 
----------+-------------+------------
        1 |           1 | 2021-07-04
        2 |           2 | 2021-07-05
        3 |           3 | 2021-07-08
        4 |           4 | 2021-07-08
        5 |           5 | 2021-07-09
(5 rows)
```

---

### Table 5: Order Details

**Create Table:**

```sql
CREATE TABLE order_details (
    order_detail_id SERIAL NOT NULL PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT
);
```

---

**Insert Sample Data:**

```sql
INSERT INTO order_details (order_id, product_id, quantity)
VALUES
    (1, 1, 12),
    (1, 2, 10),
    (2, 3, 9),
    (3, 4, 10),
    (4, 5, 15);
```

**Result:**

```
INSERT 0 5
```

---

**View Data:**

```sql
SELECT * FROM order_details;
```

**Result:**

```
 order_detail_id | order_id | product_id | quantity 
-----------------+----------+------------+----------
               1 |        1 |          1 |       12
               2 |        1 |          2 |       10
               3 |        2 |          3 |        9
               4 |        3 |          4 |       10
               5 |        4 |          5 |       15
(5 rows)
```

---

## Understanding Table Relationships

**How tables connect:**

```
Customers → Orders → Order Details → Products → Categories
```

**Example:**
1. Customer "Alfreds Futterkiste" (customer_id = 1)
2. Placed Order (order_id = 1)
3. Order contains 2 products (order_details)
4. Product "Chais" (product_id = 1) belongs to category "Beverages" (category_id = 1)

---

## Quick Reference: Where to Write Commands

### SQL Shell (psql)

```
postgres=# YOUR_COMMAND_HERE;
```

**After typing command:**
1. Press Enter
2. See result immediately
3. Ready for next command

---

### pgAdmin 4

**Steps:**
1. Open Query Tool
2. Type command in text box
3. Click Play button (▶) or press F5
4. See result in Data Output tab
5. See messages in Messages tab

---

## Common Mistakes & Solutions

| Mistake | Error | Solution |
|---------|-------|----------|
| **Forgot semicolon** | Command doesn't execute | Add `;` at end |
| **Wrong quotes** | Syntax error | Use single quotes `'text'` |
| **Table doesn't exist** | relation "table" does not exist | Check table name spelling |
| **Column doesn't exist** | column "name" does not exist | Check column name spelling |
| **Forgot WHERE** | All rows updated/deleted | Always use WHERE clause |

---

## Key Takeaways

- CREATE TABLE makes new table
- INSERT INTO adds data
- SELECT views data
- UPDATE changes data (use WHERE!)
- DELETE removes rows (use WHERE!)
- ALTER TABLE modifies structure
- DROP TABLE deletes table completely
- SERIAL creates auto-incrementing numbers
- PRIMARY KEY makes column unique
- Always end commands with semicolon `;`
- Text values use single quotes `'text'`
- Numbers don't use quotes
- Use WHERE clause to avoid changing all rows

