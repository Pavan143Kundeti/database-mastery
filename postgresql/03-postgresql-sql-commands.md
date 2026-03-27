# PostgreSQL SQL Commands

## PostgreSQL Operators

Operators are used in WHERE clause to filter data.

### Comparison Operators

| Operator | Description | Example | Result |
|----------|-------------|---------|--------|
| **=** | Equal to | `WHERE brand = 'Volvo'` | Rows where brand is Volvo |
| **<** | Less than | `WHERE year < 1975` | Rows where year is before 1975 |
| **>** | Greater than | `WHERE year > 1975` | Rows where year is after 1975 |
| **<=** | Less than or equal | `WHERE year <= 1975` | Rows where year is 1975 or before |
| **>=** | Greater than or equal | `WHERE year >= 1975` | Rows where year is 1975 or after |
| **<>** | Not equal to | `WHERE brand <> 'Volvo'` | Rows where brand is not Volvo |
| **!=** | Not equal to | `WHERE brand != 'Volvo'` | Same as <> |

---

### Pattern Matching Operators

| Operator | Description | Case Sensitive | Example |
|----------|-------------|----------------|---------|
| **LIKE** | Pattern matching | Yes | `WHERE model LIKE 'M%'` |
| **ILIKE** | Pattern matching | No | `WHERE model ILIKE 'm%'` |
| **NOT LIKE** | Reverse pattern | Yes | `WHERE brand NOT LIKE 'B%'` |
| **NOT ILIKE** | Reverse pattern | No | `WHERE brand NOT ILIKE 'b%'` |

**Wildcards:**
- `%` = Zero or more characters
- `_` = Exactly one character

---

### Logical Operators

| Operator | Description | Example |
|----------|-------------|---------|
| **AND** | All conditions must be true | `WHERE brand = 'Volvo' AND year = 1968` |
| **OR** | At least one condition true | `WHERE brand = 'Volvo' OR year = 1975` |
| **NOT** | Reverses condition | `WHERE NOT brand = 'Volvo'` |

---

### Range Operators

| Operator | Description | Includes Boundaries | Example |
|----------|-------------|---------------------|---------|
| **IN** | Matches any value in list | N/A | `WHERE brand IN ('Volvo', 'Ford')` |
| **NOT IN** | Doesn't match any in list | N/A | `WHERE brand NOT IN ('Volvo', 'Ford')` |
| **BETWEEN** | Within range | Yes | `WHERE year BETWEEN 1970 AND 1980` |
| **NOT BETWEEN** | Outside range | No | `WHERE year NOT BETWEEN 1970 AND 1980` |

---

### NULL Operators

| Operator | Description | Example |
|----------|-------------|---------|
| **IS NULL** | Value is NULL | `WHERE model IS NULL` |
| **IS NOT NULL** | Value is not NULL | `WHERE model IS NOT NULL` |

---

## Operator Examples

### Equal To (=)

```sql
SELECT * FROM cars WHERE brand = 'Volvo';
```

**Result:**
```
 brand |  model  | year 
-------+---------+------
 Volvo | p1800   | 1968
(1 row)
```

---

### Less Than (<)

```sql
SELECT * FROM cars WHERE year < 1975;
```

**Result:**
```
 brand  |  model  | year 
--------+---------+------
 Ford   | Mustang | 1964
 Volvo  | p1800   | 1968
(2 rows)
```

---

### Greater Than (>)

```sql
SELECT * FROM cars WHERE year > 1975;
```

**Result:**
```
 brand |  model  | year 
-------+---------+------
 BMW   | M1      | 1978
(1 row)
```

---

### LIKE (Case Sensitive)

```sql
SELECT * FROM cars WHERE model LIKE 'M%';
```

**Result:**
```
 brand |  model  | year 
-------+---------+------
 Ford  | Mustang | 1964
 BMW   | M1      | 1978
(2 rows)
```

**Explanation:** Finds models starting with capital 'M'.

---

### ILIKE (Case Insensitive)

```sql
SELECT * FROM cars WHERE model ILIKE 'm%';
```

**Result:**
```
 brand |  model  | year 
-------+---------+------
 Ford  | Mustang | 1964
 BMW   | M1      | 1978
(2 rows)
```

**Explanation:** Finds models starting with 'm' or 'M'.

---

### AND Operator

```sql
SELECT * FROM cars WHERE brand = 'Volvo' AND year = 1968;
```

**Result:**
```
 brand |  model  | year 
-------+---------+------
 Volvo | p1800   | 1968
(1 row)
```

---

### OR Operator

```sql
SELECT * FROM cars WHERE brand = 'Volvo' OR year = 1975;
```

**Result:**
```
 brand  |  model  | year 
--------+---------+------
 Volvo  | p1800   | 1968
 Toyota | Celica  | 1975
(2 rows)
```

---

### IN Operator

```sql
SELECT * FROM cars WHERE brand IN ('Volvo', 'Mercedes', 'Ford');
```

**Result:**
```
 brand |  model  | year 
-------+---------+------
 Ford  | Mustang | 1964
 Volvo | p1800   | 1968
(2 rows)
```

---

### BETWEEN Operator

```sql
SELECT * FROM cars WHERE year BETWEEN 1970 AND 1980;
```

**Result:**
```
 brand  |  model  | year 
--------+---------+------
 BMW    | M1      | 1978
 Toyota | Celica  | 1975
(2 rows)
```

**Note:** Includes 1970 and 1980.

---

### IS NULL

```sql
SELECT * FROM cars WHERE model IS NULL;
```

**Result:**
```
 brand | model | year 
-------+-------+------
(0 rows)
```

---

## SELECT Statement

Retrieves data from database.

### SELECT Specific Columns

**Syntax:**
```sql
SELECT column1, column2 FROM table_name;
```

**Example:**
```sql
SELECT customer_name, country FROM customers;
```

**Result:**
```
          customer_name           | country 
----------------------------------+---------
 Alfreds Futterkiste              | Germany
 Ana Trujillo Emparedados y...    | Mexico
 Antonio Moreno Taquera           | Mexico
(3 rows)
```

---

### SELECT All Columns

**Syntax:**
```sql
SELECT * FROM table_name;
```

**Example:**
```sql
SELECT * FROM customers;
```

**Result:** All columns and all rows.

---

## SELECT DISTINCT

Returns only unique values.

### Syntax

```sql
SELECT DISTINCT column_name FROM table_name;
```

### Example

```sql
SELECT DISTINCT country FROM customers;
```

**Result:**
```
  country   
------------
 Germany
 Mexico
 UK
 Sweden
(4 rows)
```

---

### COUNT with DISTINCT

```sql
SELECT COUNT(DISTINCT country) FROM customers;
```

**Result:**
```
 count 
-------
    21
(1 row)
```

**Explanation:** Returns number of unique countries.

---

## WHERE Clause

Filters records based on conditions.

### Syntax

```sql
SELECT column1, column2 FROM table_name
WHERE condition;
```

### Example: Text Value

```sql
SELECT * FROM customers WHERE city = 'London';
```

**Important:** Use single quotes for text values.

---

### Example: Numeric Value

```sql
SELECT * FROM customers WHERE customer_id = 19;
```

**Important:** No quotes for numbers.

---

### Example: Greater Than

```sql
SELECT * FROM customers WHERE customer_id > 80;
```

---

## ORDER BY

Sorts results in ascending or descending order.

### Syntax

```sql
SELECT column1, column2 FROM table_name
ORDER BY column_name ASC|DESC;
```

**Default:** ASC (ascending)

---

### ORDER BY Examples

| Command | Description | Result |
|---------|-------------|--------|
| `ORDER BY price` | Sort by price (ascending) | Lowest to highest |
| `ORDER BY price DESC` | Sort by price (descending) | Highest to lowest |
| `ORDER BY product_name` | Sort alphabetically | A to Z |
| `ORDER BY product_name DESC` | Sort reverse alphabetically | Z to A |

---

### Example: Ascending

```sql
SELECT * FROM products ORDER BY price;
```

**Result:**
```
 product_id | product_name | price 
------------+--------------+-------
          3 | Aniseed Syrup| 10.00
          1 | Chais        | 18.00
          2 | Chang        | 19.00
(3 rows)
```

---

### Example: Descending

```sql
SELECT * FROM products ORDER BY price DESC;
```

**Result:**
```
 product_id | product_name | price 
------------+--------------+-------
          2 | Chang        | 19.00
          1 | Chais        | 18.00
          3 | Aniseed Syrup| 10.00
(3 rows)
```

---

## LIMIT

Limits number of records returned.

### Syntax

```sql
SELECT column_name FROM table_name
LIMIT number;
```

### Example

```sql
SELECT * FROM customers LIMIT 20;
```

**Result:** First 20 records only.

---

### LIMIT with OFFSET

Skip records before returning results.

**Syntax:**
```sql
SELECT column_name FROM table_name
LIMIT number OFFSET skip_number;
```

**Example:**
```sql
SELECT * FROM customers LIMIT 20 OFFSET 40;
```

**Explanation:** Skip first 40 records, return next 20.

**Note:** First record is 0, so OFFSET 40 starts at record 41.

---

## Aggregate Functions

Perform calculations on data.

### Aggregate Functions Overview

| Function | Description | Returns | Example |
|----------|-------------|---------|---------|
| **MIN()** | Smallest value | Single value | `MIN(price)` |
| **MAX()** | Largest value | Single value | `MAX(price)` |
| **COUNT()** | Number of rows | Single number | `COUNT(*)` |
| **SUM()** | Total sum | Single number | `SUM(quantity)` |
| **AVG()** | Average value | Single number | `AVG(price)` |

---

### MIN Function

Returns smallest value.

**Syntax:**
```sql
SELECT MIN(column_name) FROM table_name;
```

**Example:**
```sql
SELECT MIN(price) FROM products;
```

**Result:**
```
  min  
-------
 10.00
(1 row)
```

---

### MAX Function

Returns largest value.

**Syntax:**
```sql
SELECT MAX(column_name) FROM table_name;
```

**Example:**
```sql
SELECT MAX(price) FROM products;
```

**Result:**
```
  max   
--------
 263.50
(1 row)
```

---

### COUNT Function

Returns number of rows.

**Syntax:**
```sql
SELECT COUNT(column_name) FROM table_name;
```

**Example:**
```sql
SELECT COUNT(customer_id) FROM customers;
```

**Result:**
```
 count 
-------
    91
(1 row)
```

**Note:** NULL values are not counted.

---

### COUNT with WHERE

```sql
SELECT COUNT(customer_id) FROM customers
WHERE city = 'London';
```

**Result:**
```
 count 
-------
     6
(1 row)
```

---

### SUM Function

Returns total sum.

**Syntax:**
```sql
SELECT SUM(column_name) FROM table_name;
```

**Example:**
```sql
SELECT SUM(quantity) FROM order_details;
```

**Result:**
```
  sum  
-------
 12743
(1 row)
```

---

### AVG Function

Returns average value.

**Syntax:**
```sql
SELECT AVG(column_name) FROM table_name;
```

**Example:**
```sql
SELECT AVG(price) FROM products;
```

**Result:**
```
        avg         
--------------------
 28.8663636363636364
(1 row)
```

---

### Set Column Name with AS

Give custom name to result column.

**Example:**
```sql
SELECT MIN(price) AS lowest_price FROM products;
```

**Result:**
```
 lowest_price 
--------------
        10.00
(1 row)
```

---

### Aggregate Functions Comparison

| Function | Use Case | Example Query | Example Result |
|----------|----------|---------------|----------------|
| **MIN()** | Find cheapest product | `SELECT MIN(price) FROM products` | 10.00 |
| **MAX()** | Find most expensive | `SELECT MAX(price) FROM products` | 263.50 |
| **COUNT()** | Count total customers | `SELECT COUNT(*) FROM customers` | 91 |
| **SUM()** | Total quantity ordered | `SELECT SUM(quantity) FROM order_details` | 12743 |
| **AVG()** | Average price | `SELECT AVG(price) FROM products` | 28.87 |

---

## LIKE Operator

Pattern matching for text.

### Wildcards

| Wildcard | Meaning | Example | Matches |
|----------|---------|---------|---------|
| **%** | Zero or more characters | `'a%'` | apple, ant, a |
| **_** | Exactly one character | `'a_'` | an, at (not apple) |

---

### LIKE Examples

| Pattern | Description | Example |
|---------|-------------|---------|
| `'a%'` | Starts with 'a' | apple, ant |
| `'%a'` | Ends with 'a' | pizza, pasta |
| `'%or%'` | Contains 'or' | fork, morning |
| `'_r%'` | Second letter is 'r' | brown, fruit |
| `'a__%'` | Starts with 'a', at least 3 chars | apple, ant |

---

### Example: Starts With

```sql
SELECT * FROM products WHERE product_name LIKE 'C%';
```

**Result:**
```
 product_id | product_name | price 
------------+--------------+-------
          1 | Chais        | 18.00
          2 | Chang        | 19.00
(2 rows)
```

---

### Example: Contains

```sql
SELECT * FROM products WHERE product_name LIKE '%Syrup%';
```

**Result:**
```
 product_id |  product_name  | price 
------------+----------------+-------
          3 | Aniseed Syrup  | 10.00
(1 row)
```

---

## IN Operator

Matches any value in a list.

### Syntax

```sql
SELECT column_name FROM table_name
WHERE column_name IN (value1, value2, ...);
```

### Example

```sql
SELECT * FROM customers
WHERE country IN ('Germany', 'France', 'UK');
```

**Result:** All customers from these 3 countries.

---

### NOT IN

```sql
SELECT * FROM customers
WHERE country NOT IN ('Germany', 'France', 'UK');
```

**Result:** All customers NOT from these countries.

---

## BETWEEN Operator

Selects values within a range.

### Syntax

```sql
SELECT column_name FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

### Example: Numbers

```sql
SELECT * FROM products
WHERE price BETWEEN 10 AND 20;
```

**Result:** Products with price from 10 to 20 (inclusive).

---

### Example: Dates

```sql
SELECT * FROM orders
WHERE order_date BETWEEN '2021-07-01' AND '2021-07-31';
```

**Result:** Orders in July 2021.

---

### NOT BETWEEN

```sql
SELECT * FROM products
WHERE price NOT BETWEEN 10 AND 20;
```

**Result:** Products with price less than 10 or greater than 20.

---

## AS (Aliases)

Give temporary names to columns or tables.

### Column Alias

**Syntax:**
```sql
SELECT column_name AS alias_name FROM table_name;
```

**Example:**
```sql
SELECT customer_name AS customer, city AS location
FROM customers;
```

**Result:**
```
       customer        | location 
-----------------------+----------
 Alfreds Futterkiste   | Berlin
(1 row)
```

---

### Table Alias

**Syntax:**
```sql
SELECT column_name FROM table_name AS alias_name;
```

**Example:**
```sql
SELECT c.customer_name, o.order_id
FROM customers AS c, orders AS o
WHERE c.customer_id = o.customer_id;
```

---

## JOIN Operations

Combine rows from two or more tables.

### Types of Joins

| Join Type | Description | Returns |
|-----------|-------------|---------|
| **INNER JOIN** | Matching records from both tables | Only matches |
| **LEFT JOIN** | All from left + matching from right | All left + matches |
| **RIGHT JOIN** | All from right + matching from left | All right + matches |
| **FULL JOIN** | All records from both tables | Everything |
| **CROSS JOIN** | All possible combinations | Cartesian product |

---

### INNER JOIN

Returns only matching records.

**Syntax:**
```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

**Example:**
```sql
SELECT customers.customer_name, orders.order_id
FROM customers
INNER JOIN orders
ON customers.customer_id = orders.customer_id;
```

**Result:**
```
     customer_name      | order_id 
------------------------+----------
 Alfreds Futterkiste    |        1
 Ana Trujillo Emparedados|       2
(2 rows)
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
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```

**Example:**
```sql
SELECT customers.customer_name, orders.order_id
FROM customers
LEFT JOIN orders
ON customers.customer_id = orders.customer_id;
```

**Result:** All customers, even those without orders (order_id will be NULL).

**Visual:**
```
Table1    Table2
  A         A     → Returns A (match)
  B         C     → Returns B with NULL
```

---

### RIGHT JOIN

Returns all from right table + matching from left.

**Syntax:**
```sql
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;
```

**Example:**
```sql
SELECT customers.customer_name, orders.order_id
FROM customers
RIGHT JOIN orders
ON customers.customer_id = orders.customer_id;
```

**Result:** All orders, even if customer doesn't exist.

---

### FULL JOIN

Returns all records from both tables.

**Syntax:**
```sql
SELECT columns
FROM table1
FULL JOIN table2
ON table1.column = table2.column;
```

**Example:**
```sql
SELECT customers.customer_name, orders.order_id
FROM customers
FULL JOIN orders
ON customers.customer_id = orders.customer_id;
```

**Result:** All customers and all orders, with NULLs where no match.

---

### CROSS JOIN

Returns all possible combinations.

**Syntax:**
```sql
SELECT columns
FROM table1
CROSS JOIN table2;
```

**Example:**
```sql
SELECT customers.customer_name, products.product_name
FROM customers
CROSS JOIN products;
```

**Result:** If 5 customers and 10 products = 50 rows.

---

### Join Comparison

| Join Type | Left Table | Right Table | Result |
|-----------|------------|-------------|--------|
| **INNER** | Only matches | Only matches | Matches only |
| **LEFT** | All rows | Only matches | All left + matches |
| **RIGHT** | Only matches | All rows | All right + matches |
| **FULL** | All rows | All rows | Everything |
| **CROSS** | All rows | All rows | All combinations |

---

## UNION

Combines result sets from multiple SELECT statements.

### Syntax

```sql
SELECT column_name FROM table1
UNION
SELECT column_name FROM table2;
```

### Example

```sql
SELECT city FROM customers
UNION
SELECT city FROM suppliers
ORDER BY city;
```

**Result:** Unique cities from both tables.

---

### UNION vs UNION ALL

| Feature | UNION | UNION ALL |
|---------|-------|-----------|
| **Duplicates** | Removes duplicates | Keeps duplicates |
| **Speed** | Slower | Faster |
| **Use** | Unique values only | All values |

**Example:**
```sql
SELECT city FROM customers
UNION ALL
SELECT city FROM suppliers;
```

---

## GROUP BY

Groups rows with same values.

### Syntax

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name;
```

### Example

```sql
SELECT country, COUNT(*) AS customer_count
FROM customers
GROUP BY country;
```

**Result:**
```
  country   | customer_count 
------------+----------------
 Germany    |             11
 Mexico     |              5
 UK         |              7
(3 rows)
```

---

### GROUP BY with ORDER BY

```sql
SELECT country, COUNT(*) AS customer_count
FROM customers
GROUP BY country
ORDER BY customer_count DESC;
```

**Result:** Countries sorted by customer count (highest first).

---

## HAVING

Filters groups (used with GROUP BY).

### Syntax

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

### Example

```sql
SELECT country, COUNT(*) AS customer_count
FROM customers
GROUP BY country
HAVING COUNT(*) > 5;
```

**Result:** Only countries with more than 5 customers.

---

### WHERE vs HAVING

| Feature | WHERE | HAVING |
|---------|-------|--------|
| **Filters** | Individual rows | Groups |
| **Used with** | Any query | GROUP BY |
| **When applied** | Before grouping | After grouping |
| **Can use aggregates** | No | Yes |

**Example:**
```sql
SELECT country, COUNT(*) AS customer_count
FROM customers
WHERE city != 'London'
GROUP BY country
HAVING COUNT(*) > 3;
```

---

## EXISTS

Tests if subquery returns any records.

### Syntax

```sql
SELECT column_name
FROM table_name
WHERE EXISTS (SELECT column_name FROM table_name WHERE condition);
```

### Example

```sql
SELECT customer_name
FROM customers
WHERE EXISTS (
    SELECT order_id FROM orders
    WHERE orders.customer_id = customers.customer_id
);
```

**Result:** Customers who have placed at least one order.

---

## ANY and ALL

Compare value to set of values.

### ANY Operator

True if ANY subquery value meets condition.

**Syntax:**
```sql
SELECT column_name
FROM table_name
WHERE column_name operator ANY (subquery);
```

**Example:**
```sql
SELECT product_name
FROM products
WHERE price > ANY (SELECT price FROM products WHERE category_id = 1);
```

---

### ALL Operator

True if ALL subquery values meet condition.

**Syntax:**
```sql
SELECT column_name
FROM table_name
WHERE column_name operator ALL (subquery);
```

**Example:**
```sql
SELECT product_name
FROM products
WHERE price > ALL (SELECT price FROM products WHERE category_id = 1);
```

---

### ANY vs ALL

| Operator | Condition | Example |
|----------|-----------|---------|
| **ANY** | At least one value matches | `price > ANY (10, 20, 30)` means > 10 |
| **ALL** | All values match | `price > ALL (10, 20, 30)` means > 30 |

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

### Example

```sql
SELECT product_name, price,
CASE
    WHEN price < 20 THEN 'Cheap'
    WHEN price BETWEEN 20 AND 50 THEN 'Medium'
    ELSE 'Expensive'
END AS price_category
FROM products;
```

**Result:**
```
   product_name    | price | price_category 
-------------------+-------+----------------
 Chais             | 18.00 | Cheap
 Chang             | 19.00 | Cheap
 Aniseed Syrup     | 10.00 | Cheap
 Chef Antons...    | 22.00 | Medium
(4 rows)
```

---

### CASE with ORDER BY

```sql
SELECT product_name, price
FROM products
ORDER BY
CASE
    WHEN price < 20 THEN 1
    WHEN price < 50 THEN 2
    ELSE 3
END;
```

**Result:** Products sorted by price category.

---

## Quick Reference Summary

### Basic Queries

| Command | Purpose | Example |
|---------|---------|---------|
| **SELECT** | Get data | `SELECT * FROM table` |
| **WHERE** | Filter rows | `WHERE price > 10` |
| **ORDER BY** | Sort results | `ORDER BY price DESC` |
| **LIMIT** | Limit results | `LIMIT 10` |

---

### Aggregate Functions

| Function | Purpose | Example |
|----------|---------|---------|
| **COUNT()** | Count rows | `COUNT(*)` |
| **SUM()** | Total sum | `SUM(quantity)` |
| **AVG()** | Average | `AVG(price)` |
| **MIN()** | Minimum | `MIN(price)` |
| **MAX()** | Maximum | `MAX(price)` |

---

### Joins

| Join | Returns |
|------|---------|
| **INNER** | Matches only |
| **LEFT** | All left + matches |
| **RIGHT** | All right + matches |
| **FULL** | Everything |
| **CROSS** | All combinations |

---

## Key Takeaways

- Use WHERE to filter rows before grouping
- Use HAVING to filter groups after grouping
- LIKE is case-sensitive, ILIKE is case-insensitive
- BETWEEN includes boundary values
- Aggregate functions work on multiple rows, return single value
- INNER JOIN returns only matches
- LEFT JOIN returns all from left table
- GROUP BY groups rows with same values
- CASE creates conditional logic
- Always use single quotes for text values
- No quotes for numeric values

