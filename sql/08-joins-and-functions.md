# SQL Joins & Functions

This comprehensive guide covers SQL Joins for combining data from multiple tables and SQL Functions for manipulating dates and strings.

---

## Table of Contents
1. [SQL Joins](#sql-joins)
   - [INNER JOIN](#1-inner-join)
   - [LEFT JOIN](#2-left-join)
   - [RIGHT JOIN](#3-right-join)
   - [FULL JOIN](#4-full-join)
   - [NATURAL JOIN](#5-natural-join)
   - [CROSS JOIN](#6-cross-join)
2. [SQL Date Functions](#sql-date-functions)
3. [SQL String Functions](#sql-string-functions)

---

## SQL Joins

SQL Joins combine data from two or more tables based on a related column.

**Why Use Joins?**
- Retrieve connected data stored across multiple tables
- Match records using common columns
- Improve data analysis by combining related information
- Create meaningful result sets from separate tables

---

## Types of SQL Joins

```
┌─────────────────────────────────────────────────────────┐
│                    Types of SQL Joins                    │
└─────────────────────────────────────────────────────────┘

1. INNER JOIN    - Returns matching rows from both tables
2. LEFT JOIN     - Returns all from left + matching from right
3. RIGHT JOIN    - Returns all from right + matching from left
4. FULL JOIN     - Returns all rows from both tables
5. NATURAL JOIN  - Auto-joins on columns with same name
6. CROSS JOIN    - Returns all combinations (Cartesian product)
```

---

### Example Setup: Student and StudentCourse Tables

```sql
-- Create Student table
CREATE TABLE Student (
    ROLL_NO INT PRIMARY KEY,
    NAME VARCHAR(50),
    ADDRESS VARCHAR(100),
    PHONE VARCHAR(15),
    AGE INT
);

INSERT INTO Student VALUES
(1, 'Ram', 'Delhi', '9455123451', 18),
(2, 'Ramesh', 'Gurgaon', '9652431543', 18),
(3, 'Sujit', 'Rohtak', '9156253131', 20),
(4, 'Suresh', 'Delhi', '9156768971', 18);

-- Create StudentCourse table
CREATE TABLE StudentCourse (
    COURSE_ID INT PRIMARY KEY,
    ROLL_NO INT
);

INSERT INTO StudentCourse VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 5);
```

**Student Table:**

| ROLL_NO | NAME | ADDRESS | PHONE | AGE |
|---------|------|---------|-------|-----|
| 1 | Ram | Delhi | 9455123451 | 18 |
| 2 | Ramesh | Gurgaon | 9652431543 | 18 |
| 3 | Sujit | Rohtak | 9156253131 | 20 |
| 4 | Suresh | Delhi | 9156768971 | 18 |

**StudentCourse Table:**

| COURSE_ID | ROLL_NO |
|-----------|---------|
| 1 | 1 |
| 2 | 2 |
| 3 | 3 |
| 4 | 5 |

---

## 1. INNER JOIN

INNER JOIN retrieves rows where matching values exist in both tables.

**What It Does:**
- Combines records based on a related column
- Returns only matching rows from both tables
- Excludes non-matching data from the result set
- Ensures accurate data relationships between tables

### Syntax:

```sql
SELECT table1.column1, table1.column2, table2.column1, ...
FROM table1
INNER JOIN table2
ON table1.matching_column = table2.matching_column;
```

**Note:** You can write `JOIN` instead of `INNER JOIN`. They are the same.

### Visual Representation:

```
        Table A              Table B
    ┌─────────────┐      ┌─────────────┐
    │             │      │             │
    │      ┌──────┼──────┼──────┐      │
    │      │  ●●● │      │ ●●●  │      │
    │      └──────┼──────┼──────┘      │
    │             │      │             │
    └─────────────┘      └─────────────┘
         Only matching rows (●●●) are returned
```

### Example:

Show names and age of students enrolled in courses:

```sql
SELECT StudentCourse.COURSE_ID, Student.NAME, Student.AGE
FROM Student
INNER JOIN StudentCourse
ON Student.ROLL_NO = StudentCourse.ROLL_NO;
```

### Output:

| COURSE_ID | NAME | AGE |
|-----------|------|-----|
| 1 | Ram | 18 |
| 2 | Ramesh | 18 |
| 3 | Sujit | 20 |

**Explanation:** Only students with matching ROLL_NO in both tables are shown. Suresh (ROLL_NO 4) is excluded because he has no course.

---

## 2. LEFT JOIN

LEFT JOIN retrieves all rows from the left table and matching rows from the right table.

**What It Does:**
- Returns all records from the left table
- Shows matching data from the right table
- Displays NULL values where no match exists in the right table
- Also known as LEFT OUTER JOIN

### Syntax:

```sql
SELECT table1.column1, table1.column2, table2.column1, ...
FROM table1
LEFT JOIN table2
ON table1.matching_column = table2.matching_column;
```

**Note:** `LEFT JOIN` and `LEFT OUTER JOIN` are the same.

### Visual Representation:

```
        Table A              Table B
    ┌─────────────┐      ┌─────────────┐
    │             │      │             │
    │ ●●●  ┌──────┼──────┼──────┐      │
    │ ●●●  │  ●●● │      │ ●●●  │      │
    │ ●●●  └──────┼──────┼──────┘      │
    │             │      │             │
    └─────────────┘      └─────────────┘
    All from left (●●●) + matching from right
```

### Example:

Retrieve all students and their courses (if enrolled):

```sql
SELECT Student.NAME, StudentCourse.COURSE_ID
FROM Student
LEFT JOIN StudentCourse
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```

### Output:

| NAME | COURSE_ID |
|------|-----------|
| Ram | 1 |
| Ramesh | 2 |
| Sujit | 3 |
| Suresh | NULL |

**Explanation:** All students are shown. Suresh has NULL for COURSE_ID because he's not enrolled in any course.

---

## 3. RIGHT JOIN

RIGHT JOIN retrieves all rows from the right table and matching rows from the left table.

**What It Does:**
- Returns all records from the right-side table
- Shows matching data from the left-side table
- Displays NULL values where no match exists in the left table
- Also known as RIGHT OUTER JOIN

### Syntax:

```sql
SELECT table1.column1, table1.column2, table2.column1, ...
FROM table1
RIGHT JOIN table2
ON table1.matching_column = table2.matching_column;
```

**Note:** `RIGHT JOIN` and `RIGHT OUTER JOIN` are the same.

### Visual Representation:

```
        Table A              Table B
    ┌─────────────┐      ┌─────────────┐
    │             │      │             │
    │      ┌──────┼──────┼──────┐ ●●●  │
    │      │  ●●● │      │ ●●●  │ ●●●  │
    │      └──────┼──────┼──────┘ ●●●  │
    │             │      │             │
    └─────────────┘      └─────────────┘
    Matching from left + all from right (●●●)
```

### Example:

Retrieve all courses and student names (if enrolled):

```sql
SELECT Student.NAME, StudentCourse.COURSE_ID
FROM Student
RIGHT JOIN StudentCourse
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```

### Output:

| NAME | COURSE_ID |
|------|-----------|
| Ram | 1 |
| Ramesh | 2 |
| Sujit | 3 |
| NULL | 4 |

**Explanation:** All courses are shown. Course 4 has NULL for NAME because ROLL_NO 5 doesn't exist in Student table.

---

## 4. FULL JOIN

FULL JOIN combines the results of both LEFT JOIN and RIGHT JOIN.

**What It Does:**
- Returns all rows from both tables
- Shows matching records from each table
- Displays NULL values where no match exists in either table
- Provides complete data from both sides of the join

### Syntax:

```sql
SELECT table1.column1, table1.column2, table2.column1, ...
FROM table1
FULL JOIN table2
ON table1.matching_column = table2.matching_column;
```

### Visual Representation:

```
        Table A              Table B
    ┌─────────────┐      ┌─────────────┐
    │             │      │             │
    │ ●●●  ┌──────┼──────┼──────┐ ●●●  │
    │ ●●●  │  ●●● │      │ ●●●  │ ●●●  │
    │ ●●●  └──────┼──────┼──────┘ ●●●  │
    │             │      │             │
    └─────────────┘      └─────────────┘
    All rows from both tables (●●●)
```

### Example:

Return all students and all courses:

```sql
SELECT Student.NAME, StudentCourse.COURSE_ID
FROM Student
FULL JOIN StudentCourse
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```

### Output:

| NAME | COURSE_ID |
|------|-----------|
| Ram | 1 |
| Ramesh | 2 |
| Sujit | 3 |
| Suresh | NULL |
| NULL | 4 |

**Explanation:** All students and all courses are shown. NULL appears where there's no match.

---

## 5. NATURAL JOIN

NATURAL JOIN automatically joins tables based on columns with the same name and data type.

**What It Does:**
- Joins tables using common columns with the same name
- Returns only rows where values in those columns match
- The common column appears only once in the result
- No need to specify the join condition

### Example Setup: Employee and Department Tables

```sql
CREATE TABLE Employee (
    Emp_id INT PRIMARY KEY,
    Emp_name VARCHAR(50),
    Dept_id INT
);

INSERT INTO Employee VALUES
(1, 'Alice', 101),
(2, 'Bob', 102),
(3, 'Charlie', 103);

CREATE TABLE Department (
    Dept_id INT PRIMARY KEY,
    Dept_name VARCHAR(50)
);

INSERT INTO Department VALUES
(101, 'HR'),
(102, 'IT'),
(103, 'Sales');
```

**Employee Table:**

| Emp_id | Emp_name | Dept_id |
|--------|----------|---------|
| 1 | Alice | 101 |
| 2 | Bob | 102 |
| 3 | Charlie | 103 |

**Department Table:**

| Dept_id | Dept_name |
|---------|-----------|
| 101 | HR |
| 102 | IT |
| 103 | Sales |

### Example:

Find all employees and their departments:

```sql
SELECT Emp_name, Dept_name
FROM Employee
NATURAL JOIN Department;
```

### Output:

| Emp_name | Dept_name |
|----------|-----------|
| Alice | HR |
| Bob | IT |
| Charlie | Sales |

**Explanation:** Tables are joined on Dept_id (common column). No need to specify ON condition.

---

## 6. CROSS JOIN

CROSS JOIN creates a Cartesian product of two tables, returning every possible combination of rows.

**What It Does:**
- Returns every possible combination of rows from both tables
- Does not use a join condition
- Total rows = (rows in table1) × (rows in table2)
- Useful for generating all combinations of data

### Syntax:

```sql
SELECT *
FROM table1
CROSS JOIN table2;
```

### Visual Representation:

```
Table A (2 rows)  ×  Table B (2 rows)  =  4 combinations

    A1  ×  B1  =  A1-B1
    A1  ×  B2  =  A1-B2
    A2  ×  B1  =  A2-B1
    A2  ×  B2  =  A2-B2
```

### Example Setup: Customer and Orders Tables

```sql
CREATE TABLE Customer (
    CustomerID INT,
    CustomerName VARCHAR(50)
);

INSERT INTO Customer VALUES
(1, 'John'),
(2, 'Jane');

CREATE TABLE Orders (
    OrderID INT,
    OrderDate DATE
);

INSERT INTO Orders VALUES
(101, '2024-01-01'),
(102, '2024-01-02');
```

**Customer Table:**

| CustomerID | CustomerName |
|------------|--------------|
| 1 | John |
| 2 | Jane |

**Orders Table:**

| OrderID | OrderDate |
|---------|-----------|
| 101 | 2024-01-01 |
| 102 | 2024-01-02 |

### Example:

```sql
SELECT *
FROM Customer
CROSS JOIN Orders;
```

### Output:

| CustomerID | CustomerName | OrderID | OrderDate |
|------------|--------------|---------|-----------|
| 1 | John | 101 | 2024-01-01 |
| 1 | John | 102 | 2024-01-02 |
| 2 | Jane | 101 | 2024-01-01 |
| 2 | Jane | 102 | 2024-01-02 |

**Explanation:** Every customer is combined with every order. 2 customers × 2 orders = 4 rows.

---

## SQL Joins Comparison Table

| Join Type | Returns | Use Case |
|-----------|---------|----------|
| **INNER JOIN** | Only matching rows from both tables | Get related data that exists in both tables |
| **LEFT JOIN** | All from left + matching from right | Get all records from main table with optional related data |
| **RIGHT JOIN** | All from right + matching from left | Get all records from secondary table with optional related data |
| **FULL JOIN** | All rows from both tables | Get complete data from both tables |
| **NATURAL JOIN** | Matching rows on same-named columns | Auto-join on common columns |
| **CROSS JOIN** | All combinations of rows | Generate all possible combinations |

---

## SQL Date Functions

SQL Date Functions handle, modify, and analyze date/time values in a database.

**Why Use Date Functions?**
- Extract specific parts of a date (year, month, day)
- Format dates for user-friendly display
- Track trends, deadlines, or schedules
- Calculate date differences and intervals

---

### Example Setup: Sales Table

```sql
CREATE TABLE sales (
    sale_id INT PRIMARY KEY,
    product_name VARCHAR(50),
    sale_date DATE
);

INSERT INTO sales VALUES
(1, 'Laptop', '2024-08-01'),
(2, 'Mouse', '2024-08-05'),
(3, 'Keyboard', '2024-08-10');
```

**Sales Table:**

| sale_id | product_name | sale_date |
|---------|--------------|-----------|
| 1 | Laptop | 2024-08-01 |
| 2 | Mouse | 2024-08-05 |
| 3 | Keyboard | 2024-08-10 |

---

### 1. NOW()

Returns the current date and time.

```sql
SELECT NOW() AS current_datetime;
```

**Output:**
```
2026-03-27 14:30:45
```

**Use Case:** Capture exact event moments, transaction timestamps, logging.

---

### 2. CURDATE()

Returns today's date in YYYY-MM-DD format.

```sql
SELECT CURDATE() AS current_date;
```

**Output:**
```
2026-03-27
```

**Use Case:** Get current date for reporting or filtering records.

---

### 3. CURTIME()

Returns the current time in HH:MM:SS format.

```sql
SELECT CURTIME() AS current_time;
```

**Output:**
```
14:30:45
```

**Use Case:** Time-based operations, scheduling, time comparisons.

---

### 4. DATE()

Extracts only the date from a datetime value.

```sql
SELECT sale_id, product_name, DATE(sale_date) AS sale_date_only
FROM sales;
```

**Output:**

| sale_id | product_name | sale_date_only |
|---------|--------------|----------------|
| 1 | Laptop | 2024-08-01 |
| 2 | Mouse | 2024-08-05 |
| 3 | Keyboard | 2024-08-10 |

**Use Case:** Date-only comparisons, ignoring time component.

---

### 5. EXTRACT()

Retrieves a specific part of a date (year, month, day).

```sql
SELECT sale_id, product_name, EXTRACT(YEAR FROM sale_date) AS sale_year
FROM sales;
```

**Output:**

| sale_id | product_name | sale_year |
|---------|--------------|-----------|
| 1 | Laptop | 2024 |
| 2 | Mouse | 2024 |
| 3 | Keyboard | 2024 |

**Other Options:**
- `EXTRACT(MONTH FROM date)`
- `EXTRACT(DAY FROM date)`
- `EXTRACT(HOUR FROM datetime)`

**Use Case:** Grouping or filtering by year, month, or day.

---

### 6. DATE_ADD()

Adds a time interval to a date.

```sql
SELECT sale_id, product_name,
DATE_ADD(sale_date, INTERVAL 7 DAY) AS sale_date_plus_7_days
FROM sales;
```

**Output:**

| sale_id | product_name | sale_date_plus_7_days |
|---------|--------------|----------------------|
| 1 | Laptop | 2024-08-08 |
| 2 | Mouse | 2024-08-12 |
| 3 | Keyboard | 2024-08-17 |

**Intervals:** DAY, MONTH, YEAR, HOUR, MINUTE, SECOND

**Use Case:** Calculate future dates, deadlines, expiration dates.

---

### 7. DATE_SUB()

Subtracts a time interval from a date.

```sql
SELECT sale_id, product_name,
DATE_SUB(sale_date, INTERVAL 3 DAY) AS sale_date_minus_3_days
FROM sales;
```

**Output:**

| sale_id | product_name | sale_date_minus_3_days |
|---------|--------------|------------------------|
| 1 | Laptop | 2024-07-29 |
| 2 | Mouse | 2024-08-02 |
| 3 | Keyboard | 2024-08-07 |

**Use Case:** Calculate past dates, retrospective analysis.

---

### 8. DATEDIFF()

Returns the number of days between two dates.

```sql
SELECT sale_id, product_name, sale_date,
DATEDIFF('2024-08-15', sale_date) AS days_until_aug15
FROM sales;
```

**Output:**

| sale_id | product_name | sale_date | days_until_aug15 |
|---------|--------------|-----------|------------------|
| 1 | Laptop | 2024-08-01 | 14 |
| 2 | Mouse | 2024-08-05 | 10 |
| 3 | Keyboard | 2024-08-10 | 5 |

**Use Case:** Calculate durations, deadlines, overdue periods.

---

### 9. DATE_FORMAT()

Formats a date using a specified pattern.

```sql
SELECT sale_id, product_name,
DATE_FORMAT(sale_date, '%W, %M %d, %Y') AS formatted_sale_date
FROM sales;
```

**Output:**

| sale_id | product_name | formatted_sale_date |
|---------|--------------|---------------------|
| 1 | Laptop | Thursday, August 01, 2024 |
| 2 | Mouse | Monday, August 05, 2024 |
| 3 | Keyboard | Saturday, August 10, 2024 |

**Common Format Codes:**
- `%Y` - Year (4 digits)
- `%m` - Month (01-12)
- `%d` - Day (01-31)
- `%W` - Weekday name
- `%M` - Month name

**Use Case:** User-friendly date display in reports.

---

### 10. ADDDATE()

Adds days to a date (simpler than DATE_ADD).

```sql
SELECT sale_id, product_name,
ADDDATE(sale_date, 10) AS sale_date_plus_10_days
FROM sales;
```

**Output:**

| sale_id | product_name | sale_date_plus_10_days |
|---------|--------------|------------------------|
| 1 | Laptop | 2024-08-11 |
| 2 | Mouse | 2024-08-15 |
| 3 | Keyboard | 2024-08-20 |

---

### 11. ADDTIME()

Adds time to a time or datetime value.

```sql
SELECT sale_id, product_name,
ADDTIME('10:30:00', '02:30:00') AS sale_time_plus_2hrs_30min
FROM sales;
```

**Output:**

| sale_id | product_name | sale_time_plus_2hrs_30min |
|---------|--------------|---------------------------|
| 1 | Laptop | 13:00:00 |
| 2 | Mouse | 13:00:00 |
| 3 | Keyboard | 13:00:00 |

**Use Case:** Adjust times by adding hours, minutes, or seconds.

---

## SQL String Functions

SQL String Functions manipulate and format text data efficiently.

**Why Use String Functions?**
- Handle names, addresses, and text-based data
- Clean, compare, and extract meaningful information
- Organize, analyze, and improve data quality

---

### 1. CONCAT()

Combines two or more strings into one.

```sql
SELECT CONCAT('John', ' ', 'Doe') AS FullName;
```

**Output:**
```
John Doe
```

**Use Case:** Merge first and last names, combine address fields.

---

### 2. CHAR_LENGTH() / CHARACTER_LENGTH()

Returns the length of a string in characters.

```sql
SELECT CHAR_LENGTH('Hello') AS StringLength;
```

**Output:**
```
5
```

**Use Case:** Validate text length, check password requirements.

---

### 3. UPPER() and LOWER()

Convert text to uppercase or lowercase.

```sql
SELECT UPPER('hello') AS UpperCase;
SELECT LOWER('HELLO') AS LowerCase;
```

**Output:**
```
HELLO
hello
```

**Use Case:** Normalize text for case-insensitive comparisons.

---

### 4. LENGTH()

Returns the length of a string in bytes.

```sql
SELECT LENGTH('Hello') AS LengthInBytes;
```

**Output:**
```
5
```

**Use Case:** Work with multi-byte character sets.

---

### 5. REPLACE()

Replaces occurrences of a substring with another substring.

```sql
SELECT REPLACE('Hello World', 'World', 'SQL') AS UpdatedString;
```

**Output:**
```
Hello SQL
```

**Use Case:** Clean data, fix formatting errors, replace invalid characters.

---

### 6. SUBSTRING() / SUBSTR()

Extracts a substring from a string.

```sql
SELECT SUBSTRING('Hello World', 1, 5) AS SubStringExample;
```

**Output:**
```
Hello
```

**Syntax:** `SUBSTRING(string, start_position, length)`

**Use Case:** Extract domain from email, get first N characters.

---

### 7. LEFT() and RIGHT()

Extract characters from left or right side of a string.

```sql
SELECT LEFT('Hello World', 5) AS LeftString;
SELECT RIGHT('Hello World', 5) AS RightString;
```

**Output:**
```
Hello
World
```

**Use Case:** Truncate strings for display, extract prefixes/suffixes.

---

### 8. INSTR()

Finds the position of the first occurrence of a substring.

```sql
SELECT INSTR('Hello World', 'World') AS SubstringPosition;
```

**Output:**
```
7
```

**Returns:** Position (1-based index), or 0 if not found.

**Use Case:** Locate specific characters or substrings.

---

### 9. TRIM()

Removes leading and trailing spaces (or specified characters).

```sql
SELECT TRIM(' ' FROM '  Hello World  ') AS TrimmedString;
```

**Output:**
```
Hello World
```

**Use Case:** Clean user inputs, remove extra spaces.

---

### 10. REVERSE()

Reverses the characters in a string.

```sql
SELECT REVERSE('Hello') AS ReversedString;
```

**Output:**
```
olleH
```

**Use Case:** Pattern matching, password validation.

---

## Advanced String Functions

### 11. ASCII()

Returns the ASCII value of a character.

```sql
SELECT ASCII('t');
```

**Output:**
```
116
```

---

### 12. CONCAT_WS()

Concatenates strings with a separator.

```sql
SELECT CONCAT_WS('_', 'geeks', 'for', 'geeks');
```

**Output:**
```
geeks_for_geeks
```

**Use Case:** Join values with custom separator.

---

### 13. FIND_IN_SET()

Returns the position of a value in a comma-separated list.

```sql
SELECT FIND_IN_SET('b', 'a,b,c,d,e,f');
```

**Output:**
```
2
```

---

### 14. FORMAT()

Formats a number as a string.

```sql
SELECT FORMAT(0.981 * 100, 2) AS PercentageOutput;
```

**Output:**
```
98.10
```

---

### 15. LCASE()

Converts string to lowercase (same as LOWER).

```sql
SELECT LCASE('GeeksFor Geeks To Learn');
```

**Output:**
```
geeksforgeeks to learn
```

---

### 16. LOCATE()

Finds the position of a substring.

```sql
SELECT LOCATE('for', 'geeksforgeeks', 1);
```

**Output:**
```
6
```

---

### 17. LPAD()

Pads a string to a certain length by adding characters to the left.

```sql
SELECT LPAD('geeks', 8, '0');
```

**Output:**
```
000geeks
```

**Use Case:** Format data to fixed length, add leading zeros.

---

### 18. MID()

Extracts a substring (similar to SUBSTRING).

```sql
SELECT MID('geeksforgeeks', 6, 2);
```

**Output:**
```
fo
```

---

### 19. POSITION()

Finds the position of a character in a string.

```sql
SELECT POSITION('e' IN 'geeksforgeeks');
```

**Output:**
```
2
```

---

### 20. REPEAT()

Repeats a string a specified number of times.

```sql
SELECT REPEAT('geeks', 2);
```

**Output:**
```
geeksgeeks
```

---

### 21. RPAD()

Pads the right side of a string.

```sql
SELECT RPAD('geeks', 8, '0');
```

**Output:**
```
geeks000
```

---

### 22. RTRIM()

Removes trailing characters from the right side.

```sql
SELECT RTRIM('geeksxyxzyyy', 'xyz');
```

**Output:**
```
geeks
```

---

### 23. SPACE()

Generates a string of spaces.

```sql
SELECT SPACE(7);
```

**Output:**
```
'       '
```

---

### 24. STRCMP()

Compares two strings lexicographically.

```sql
SELECT STRCMP('google.com', 'geeksforgeeks.com');
```

**Output:**
```
1
```

**Returns:**
- `0` if strings are equal
- Negative if string1 < string2
- Positive if string1 > string2

---

## Quick Reference Tables

### Date Functions Summary

| Function | Purpose | Example |
|----------|---------|---------|
| NOW() | Current date and time | `2026-03-27 14:30:45` |
| CURDATE() | Current date | `2026-03-27` |
| CURTIME() | Current time | `14:30:45` |
| DATE() | Extract date part | `2024-08-01` |
| EXTRACT() | Get year/month/day | `2024` |
| DATE_ADD() | Add interval to date | `2024-08-08` |
| DATE_SUB() | Subtract interval | `2024-07-29` |
| DATEDIFF() | Days between dates | `14` |
| DATE_FORMAT() | Format date | `Thursday, August 01, 2024` |

### String Functions Summary

| Function | Purpose | Example |
|----------|---------|---------|
| CONCAT() | Combine strings | `John Doe` |
| CHAR_LENGTH() | String length | `5` |
| UPPER() / LOWER() | Change case | `HELLO` / `hello` |
| REPLACE() | Replace substring | `Hello SQL` |
| SUBSTRING() | Extract substring | `Hello` |
| LEFT() / RIGHT() | Extract from sides | `Hello` / `World` |
| TRIM() | Remove spaces | `Hello World` |
| REVERSE() | Reverse string | `olleH` |

---

## Key Takeaways

### Joins:
- INNER JOIN returns only matching rows
- LEFT JOIN returns all from left table
- RIGHT JOIN returns all from right table
- FULL JOIN returns all from both tables
- NATURAL JOIN auto-joins on same-named columns
- CROSS JOIN returns all combinations

### Date Functions:
- Use NOW() for current timestamp
- Use CURDATE() for current date only
- Use DATE_ADD() and DATE_SUB() for date calculations
- Use DATEDIFF() to find days between dates
- Use DATE_FORMAT() for user-friendly display

### String Functions:
- Use CONCAT() to combine strings
- Use UPPER()/LOWER() for case conversion
- Use SUBSTRING() to extract parts of strings
- Use TRIM() to remove extra spaces
- Use REPLACE() to clean and fix data
