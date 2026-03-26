# Database Design & Security

This comprehensive guide covers database design principles including ER modeling, normalization, keys, and security aspects including SQL injection prevention, encryption, and backup strategies.

---

## Table of Contents
1. [Entity-Relationship (ER) Model](#entity-relationship-er-model)
2. [Database Normalization](#database-normalization)
3. [Keys in Relational Model](#keys-in-relational-model)
4. [SQL Injection](#sql-injection)
5. [SQL Data Encryption](#sql-data-encryption)
6. [SQL Backup](#sql-backup)
7. [Object-Relational Mapping (ORM)](#object-relational-mapping-orm)

---

## Entity-Relationship (ER) Model

The ER Model is a conceptual model for designing databases. It represents the logical structure including entities, attributes, and relationships.

**Key Concepts:**
- **Entity:** An object stored as data (e.g., Student, Course, Company)
- **Attribute:** Properties that describe an entity (e.g., StudentID, CourseName)
- **Relationship:** A connection between entities (e.g., Student enrolls in Course)

---

### Components of ER Diagram

The graphical representation of the ER Model is called an Entity-Relationship Diagram (ERD).

```
┌─────────────────────────────────────────────────────────┐
│            ER Model Components                           │
└─────────────────────────────────────────────────────────┘
                            |
        ┌───────────────────┼───────────────────┐
        |                   |                   |
    ┌───▼───┐          ┌────▼────┐        ┌────▼────┐
    │Entity │          │Attribute│        │Relation │
    └───────┘          └─────────┘        └─────────┘
```

---

### ER Model in Database Design Process

**Steps for Designing a Database:**

1. **Gather Requirements:** Ask questions to understand functional and data needs
2. **Create Logical Design:** Use ER model to represent conceptual design
3. **Physical Database Design:** Focus on indexing and optimization
4. **External Design:** Create views for different user perspectives

---

### Why Use ER Diagrams?

| Benefit | Description |
|---------|-------------|
| **Easy Conversion** | ER diagrams easily convert into relations (tables) |
| **Real-World Modeling** | Models objects intuitively |
| **No Technical Knowledge Required** | Understandable without DBMS expertise |
| **Visual Clarity** | Makes complex systems easier to understand |

---

### Symbols Used in ER Model

| Symbol | Representation | Description |
|--------|----------------|-------------|
| Rectangle | Entity | Represents an entity type |
| Oval | Attribute | Represents an attribute |
| Diamond | Relationship | Represents a relationship |
| Line | Link | Connects entities to attributes/relationships |
| Double Rectangle | Weak Entity | Entity dependent on another |
| Double Diamond | Identifying Relationship | Relationship for weak entity |
| Underlined Oval | Key Attribute | Primary key attribute |

---

## Entity

An Entity represents a real-world object, concept, or thing about which data is stored.

**Examples of Entities:**
- Real-World Objects: Person, Car, Employee
- Concepts: Course, Event, Reservation
- Things: Product, Document, Device

### Entity Set

An **entity** refers to an individual object of an entity type. The collection of all entities of a particular type is called an **entity set**.

**Example:** E1 is an entity of type "Student," and the group of all students forms the entity set.

**Note:** ER diagrams represent entity sets (structure), not individual entities (data rows).

---

### Types of Entity

#### 1. Strong Entity

A Strong Entity has a key attribute that uniquely identifies each instance.

**Characteristics:**
- Has a primary key
- Does not depend on other entities
- Represented by a single rectangle

**Example:** Student, Employee, Product

#### 2. Weak Entity

A Weak Entity cannot be uniquely identified by its own attributes alone.

**Characteristics:**
- Depends on a strong entity (identifying entity)
- Represented by a double rectangle
- Participation is always total
- Identifying relationship shown by double diamond

**Example:**
- Dependents (Parents, Children, Spouse) of an Employee
- Dependents can't exist without the employee
- Employee is the strong entity, Dependent is the weak entity

```
┌─────────────┐         ╔═════════╗         ┌─────────────┐
│  Employee   │────────═╣ has     ╠═────────│  Dependent  │
│ (Strong)    │         ╚═════════╝         │  (Weak)     │
└─────────────┘                             └─────────────┘
```

---

## Attributes in ER Model

Attributes are properties that define the entity type.

**Example:** For Student entity: Roll_No, Name, DOB, Age, Address, Mobile_No

### Types of Attributes

#### 1. Key Attribute

Uniquely identifies each entity in the entity set.

**Symbol:** Oval with underline

**Example:** Roll_No for Student

```
    ┌─────────┐
    │Roll_No  │  (underlined)
    └────┬────┘
         │
    ┌────▼────┐
    │ Student │
    └─────────┘
```

#### 2. Composite Attribute

An attribute composed of many other attributes.

**Symbol:** Oval comprising of ovals

**Example:** Address = Street + City + State + Country

```
         ┌─────────┐
         │ Address │
         └────┬────┘
      ┌───────┼───────┐
      │       │       │
  ┌───▼──┐ ┌──▼──┐ ┌─▼────┐
  │Street│ │City │ │State │
  └──────┘ └─────┘ └──────┘
```

#### 3. Multivalued Attribute

An attribute with more than one value for a given entity.

**Symbol:** Double oval

**Example:** Phone_No (can be multiple for one student)

```
    ╔═════════╗
    ║Phone_No ║
    ╚════╤════╝
         │
    ┌────▼────┐
    │ Student │
    └─────────┘
```

#### 4. Derived Attribute

An attribute that can be derived from other attributes.

**Symbol:** Dashed oval

**Example:** Age (derived from DOB)

```
    ┌ ─ ─ ─ ─ ┐
      Age
    └ ─ ─┬─ ─ ┘
         │
    ┌────▼────┐
    │ Student │
    └─────────┘
```

---

### Complete Entity Example

**Student Entity with All Attributes:**

```
    ┌─────────┐         ┌──────┐         ┌ ─ ─ ─ ┐
    │Roll_No  │         │Name  │           Age
    └────┬────┘         └───┬──┘         └ ─┬─ ─ ┘
         │                  │                │
         │     ┌────────────▼────────────┐   │
         └─────┤       Student           ├───┘
               └────────────┬────────────┘
                           │
                      ┌────▼────┐
                      │ Address │
                      └────┬────┘
                   ┌───────┼───────┐
                   │       │       │
               ┌───▼──┐ ┌──▼──┐ ┌─▼────┐
               │Street│ │City │ │State │
               └──────┘ └─────┘ └──────┘
```

---

## Relationship Type and Relationship Set

A **Relationship Type** represents the association between entity types.

**Example:** 'Enrolled in' is a relationship between Student and Course

**Symbol:** Diamond connecting entities with lines

### Relationship Set

A set of relationships of the same type.

**Example:**
- S1 enrolled in C2
- S2 enrolled in C1
- S3 enrolled in C3

```
┌─────────┐         ┌──────────┐         ┌────────┐
│Student  │────────◇│Enrolled  │◇────────│Course  │
│         │         │   in     │         │        │
└─────────┘         └──────────┘         └────────┘
```

---

### Degree of a Relationship Set

The number of entity sets participating in a relationship.

#### 1. Unary/Recursive Relationship

Only ONE entity set participating.

**Example:** One person is married to only one person

```
        ┌──────────┐
    ┌───┤  Person  │◄───┐
    │   └──────────┘    │
    │                   │
    └───◇ married to ◇──┘
```

#### 2. Binary Relationship

TWO entity sets participating.

**Example:** Student enrolled in Course

```
┌─────────┐         ┌────────┐
│Student  │────────◇│Course  │
└─────────┘         └────────┘
```

#### 3. Ternary Relationship

THREE entity sets participating.

**Example:** Doctor treats Patient at Hospital

```
┌────────┐
│ Doctor │────┐
└────────┘    │
              ▼
          ┌───────┐
          │Treats │
          └───────┘
              ▲    ▲
              │    │
┌─────────┐   │    │   ┌──────────┐
│ Patient │───┘    └───│ Hospital │
└─────────┘            └──────────┘
```

#### 4. N-ary Relationship

N entity sets participating.

---

## Cardinality in ER Model

The maximum number of times an entity can participate in a relationship set.

### 1. One-to-One (1:1)

Each entity in each set can participate only once.

**Example:** One person has one passport, one passport belongs to one person

```
┌────────┐         ┌──────────┐         ┌──────────┐
│ Person │────1────◇│  has     │◇────1───│ Passport │
└────────┘         └──────────┘         └──────────┘
```

**Set Representation:**
```
Person = {P1, P2, P3}
Passport = {Pass1, Pass2, Pass3}
Relationship = {(P1, Pass1), (P2, Pass2), (P3, Pass3)}
```

---

### 2. One-to-Many (1:M)

One entity can be associated with multiple entities.

**Example:** One Department has many Doctors

```
┌────────────┐         ┌──────────┐         ┌────────┐
│ Department │────1────◇│   has    │◇────M───│ Doctor │
└────────────┘         └──────────┘         └────────┘
```

**Set Representation:**
```
Department = {D1}
Doctor = {Doc1, Doc2, Doc3}
Relationship = {(D1, Doc1), (D1, Doc2), (D1, Doc3)}
```

---

### 3. Many-to-One (M:1)

Multiple entities can be associated with one entity.

**Example:** Many surgeries performed by one surgeon

```
┌─────────┐         ┌──────────┐         ┌─────────┐
│ Surgery │────M────◇│performed │◇────1───│ Surgeon │
└─────────┘         │    by    │         └─────────┘
                    └──────────┘
```

**Set Representation:**
```
Surgery = {S1, S2, S3}
Surgeon = {Surg1}
Relationship = {(S1, Surg1), (S2, Surg1), (S3, Surg1)}
```

---

### 4. Many-to-Many (M:N)

Entities in all sets can participate more than once.

**Example:** Employees work on multiple projects, projects have multiple employees

```
┌──────────┐         ┌──────────┐         ┌─────────┐
│ Employee │────M────◇│ works on │◇────N───│ Project │
└──────────┘         └──────────┘         └─────────┘
```

**Set Representation:**
```
Employee = {E1, E2, E3}
Project = {P1, P2, P3}
Relationship = {(E1, P1), (E1, P2), (E2, P2), (E2, P3), (E3, P1)}
```

---

## Participation Constraint

Defines whether all entities must participate in a relationship.

### Total Participation

Each entity MUST participate in the relationship.

**Symbol:** Double line

**Example:** Every student must enroll in a course

```
┌─────────┐         ┌──────────┐         ┌────────┐
│ Student │════════◇│ Enrolled │◇────────│ Course │
└─────────┘         │    in    │         └────────┘
                    └──────────┘
```

### Partial Participation

Entity MAY or MAY NOT participate.

**Symbol:** Single line

**Example:** Some courses may not have any students enrolled

**Set Representation:**
```
Student = {S1, S2, S3}  (all participate)
Course = {C1, C2, C3, C4}  (C4 has no students)
Relationship = {(S1, C1), (S2, C2), (S3, C3)}
```

---

## How to Draw an ER Diagram

**Step-by-Step Process:**

1. **Identify Entities:** Find all entities and represent them in rectangles
2. **Identify Relationships:** Find relationships and represent with diamonds
3. **Add Attributes:** Attach attributes using ovals
4. **Define Primary Keys:** Assign and underline primary keys
5. **Remove Redundancies:** Eliminate unnecessary entities/relationships
6. **Review for Clarity:** Ensure diagram is clear and effective

**Example ER Diagram:**

```
    ┌─────────┐
    │Roll_No  │ (underlined - primary key)
    └────┬────┘
         │
    ┌────▼────┐         ┌──────────┐         ┌────────┐
    │ Student │════════◇│ Enrolled │◇────────│ Course │
    └────┬────┘         │    in    │         └───┬────┘
         │              └──────────┘             │
    ┌────▼────┐                             ┌───▼────┐
    │  Name   │                             │Course  │
    └─────────┘                             │  ID    │
                                            └────────┘
```

---

## Database Normalization

Normalization is the process of organizing database attributes to reduce or eliminate data redundancy.

**Why Normalize?**
- Reduces data redundancy
- Eliminates insertion, update, and deletion anomalies
- Improves data consistency
- Makes database more efficient

---

### Before and After Normalization

**Before Normalization:**

| EmpID | Name | Department | Dept_Location |
|-------|------|------------|---------------|
| 1 | Nick Wise | HR | New York |
| 2 | Lily Case | HR | New York |
| 3 | John Doe | IT | Boston |

**Problems:**
- Data redundancy (HR location repeated)
- Update anomaly (changing HR location requires multiple updates)
- Deletion anomaly (deleting all HR employees loses department info)
- Insertion anomaly (can't add new department without employee)

**After Normalization:**

**Employee Table:**

| EmpID | Name | DeptID |
|-------|------|--------|
| 1 | Nick Wise | 1 |
| 2 | Lily Case | 1 |
| 3 | John Doe | 2 |

**Department Table:**

| DeptID | Department | Location |
|--------|------------|----------|
| 1 | HR | New York |
| 2 | IT | Boston |

---

### Need of Normalization

Normalization eliminates three types of anomalies:

#### 1. Insertion Anomalies

Cannot insert data because required fields are missing.

**Example:** Can't add a new department without an employee.

#### 2. Deletion Anomalies

Deleting a record unintentionally loses other data.

**Example:** Deleting all IT employees loses IT department information.

#### 3. Update Anomalies

Modifying data causes inconsistencies.

**Example:** Updating HR location in one row but not others causes inconsistency.

---

### Features of Normalization

| Feature | Description |
|---------|-------------|
| **Elimination of Redundancy** | Reduces repeated data |
| **Data Consistency** | Ensures accurate and consistent data |
| **Simplified Management** | Easier to manage and update data |
| **Improved Design** | Better overall database structure |
| **Avoiding Anomalies** | Prevents insertion, update, deletion issues |
| **Standardization** | Consistent and uniform data storage |

---

## Normal Forms in DBMS

Normal forms are standards for organizing database tables to reduce redundancy.

### 1. First Normal Form (1NF)

**Rule:** Every attribute must be single-valued (atomic).

**Before 1NF:**

| StudentID | Name | Courses |
|-----------|------|---------|
| 1 | Alice | Math, Science |
| 2 | Bob | English |

**After 1NF:**

| StudentID | Name | Course |
|-----------|------|--------|
| 1 | Alice | Math |
| 1 | Alice | Science |
| 2 | Bob | English |

---

### 2. Second Normal Form (2NF)

**Rules:**
- Must be in 1NF
- Every non-primary-key attribute must be fully functionally dependent on the primary key

**Before 2NF:**

| StudentID | CourseID | StudentName | CourseName |
|-----------|----------|-------------|------------|
| 1 | 101 | Alice | Math |
| 1 | 102 | Alice | Science |

**Problem:** StudentName depends only on StudentID, not on the composite key (StudentID, CourseID)

**After 2NF:**

**Student Table:**

| StudentID | StudentName |
|-----------|-------------|
| 1 | Alice |

**Enrollment Table:**

| StudentID | CourseID | CourseName |
|-----------|----------|------------|
| 1 | 101 | Math |
| 1 | 102 | Science |

---

### 3. Third Normal Form (3NF)

**Rules:**
- Must be in 2NF
- No transitive dependency (non-prime attributes should not depend on other non-prime attributes)

**Before 3NF:**

| StudentID | Name | ZipCode | City |
|-----------|------|---------|------|
| 1 | Alice | 10001 | NYC |
| 2 | Bob | 10001 | NYC |

**Problem:** City depends on ZipCode, not directly on StudentID (transitive dependency)

**After 3NF:**

**Student Table:**

| StudentID | Name | ZipCode |
|-----------|------|---------|
| 1 | Alice | 10001 |
| 2 | Bob | 10001 |

**ZipCode Table:**

| ZipCode | City |
|---------|------|
| 10001 | NYC |

---

### 4. Boyce-Codd Normal Form (BCNF)

**Rules:**
- Must be in 3NF
- For every functional dependency X → Y, X should be a super key

**Stricter version of 3NF**

---

### 5. Fourth Normal Form (4NF)

**Rules:**
- Must be in BCNF
- No multi-valued dependencies

---

### 6. Fifth Normal Form (5NF)

**Rules:**
- Must be in 4NF
- Cannot be further decomposed without loss

---

## Keys in Relational Model

Keys are fundamental elements that ensure uniqueness, data integrity, and efficient data access.

**Why Keys are Important:**
- Ensure uniqueness of records
- Prevent data duplication
- Maintain data consistency
- Enable efficient data retrieval
- Create relationships between tables

---

### Types of Database Keys

```
┌─────────────────────────────────────────────────────────┐
│                    Database Keys                         │
└─────────────────────────────────────────────────────────┘
                            |
        ┌───────────────────┼───────────────────┐
        |                   |                   |
    ┌───▼───────┐      ┌────▼────┐        ┌────▼────────┐
    │Super Key  │      │Candidate│        │Primary Key  │
    └───────────┘      │   Key   │        └─────────────┘
                       └────┬────┘
                            |
                    ┌───────┴────────┐
                    |                |
            ┌───────▼──────┐  ┌──────▼────────┐
            │Alternate Key │  │Foreign Key    │
            └──────────────┘  └───────────────┘
```

---

### 1. Super Key

A set of one or more attributes that can uniquely identify a tuple.

**Characteristics:**
- May include extra attributes not necessary for uniqueness
- Supports NULL values
- Can be a combination of multiple attributes

**Example: STUDENT Table**

| STUD_NO | SNAME | ADDRESS | PHONE |
|---------|-------|---------|-------|
| 1 | Alice | NYC | 1234567890 |
| 2 | Bob | LA | 2345678901 |
| 3 | Charlie | Boston | 3456789012 |

**Super Keys:**
- STUD_NO
- (STUD_NO, SNAME)
- (STUD_NO, PHONE)
- (STUD_NO, SNAME, ADDRESS)

---

### 2. Candidate Key

A minimal super key with no extra attributes.

**Characteristics:**
- Minimal set of attributes for unique identification
- No repeated data
- Must contain unique values
- Every table must have at least one candidate key
- Table can have multiple candidate keys

**Example:**

For STUDENT table:
- STUD_NO (candidate key)
- PHONE (candidate key if unique)

For STUDENT_COURSE table:

| STUD_NO | COURSE_NO | GRADE |
|---------|-----------|-------|
| 1 | 101 | A |
| 1 | 102 | B |
| 2 | 101 | A |

**Composite Candidate Key:** {STUD_NO, COURSE_NO}

---

### 3. Primary Key

A candidate key chosen to uniquely identify each record.

**Characteristics:**
- Uniquely identifies every tuple
- Cannot be NULL
- May be single-column or composite
- Only one primary key per table
- Database organizes data using primary key

**Example:**

```sql
CREATE TABLE Student (
    STUD_NO INT PRIMARY KEY,
    SNAME VARCHAR(50),
    ADDRESS VARCHAR(100),
    PHONE VARCHAR(15)
);
```

**STUDENT Table:**

| STUD_NO | SNAME | ADDRESS | PHONE |
|---------|-------|---------|-------|
| 1 | Alice | NYC | 1234567890 |
| 2 | Bob | LA | 2345678901 |
| 3 | Charlie | Boston | 3456789012 |

---

### 4. Alternate Key

Any candidate key not chosen as the primary key.

**Characteristics:**
- Also called secondary key
- Can uniquely identify records
- Not the primary key

**Example:**

If STUD_NO is the primary key, then PHONE is an alternate key.

```
Candidate Keys: {STUD_NO, PHONE}
Primary Key: STUD_NO
Alternate Key: PHONE
```

---

### 5. Foreign Key

An attribute in one table that refers to the primary key in another table.

**Characteristics:**
- Creates relationships between tables
- Can contain duplicate values
- May be NULL
- Acts as cross-reference between tables

**Example:**

**STUDENT Table:**

| STUD_NO | SNAME | ADDRESS |
|---------|-------|---------|
| 1 | Alice | NYC |
| 2 | Bob | LA |
| 3 | Charlie | Boston |

**STUDENT_COURSE Table:**

| STUD_NO | COURSE_NO | GRADE |
|---------|-----------|-------|
| 1 | 101 | A |
| 1 | 102 | B |
| 2 | 101 | A |

STUD_NO in STUDENT_COURSE is a foreign key referencing STUD_NO in STUDENT.

```
┌─────────────┐         ┌──────────────────┐
│   STUDENT   │         │ STUDENT_COURSE   │
├─────────────┤         ├──────────────────┤
│ STUD_NO (PK)│◄────────│ STUD_NO (FK)     │
│ SNAME       │         │ COURSE_NO        │
│ ADDRESS     │         │ GRADE            │
└─────────────┘         └──────────────────┘
```

---

### 6. Composite Key

A combination of two or more attributes used together as a primary key.

**Characteristics:**
- Acts as primary key when single column isn't enough
- Different combinations give different accuracy

**Example:**

In STUDENT_COURSE table, {STUD_NO, COURSE_NO} forms a composite key.

```sql
CREATE TABLE STUDENT_COURSE (
    STUD_NO INT,
    COURSE_NO INT,
    GRADE CHAR(1),
    PRIMARY KEY (STUD_NO, COURSE_NO)
);
```

---

### Key Relationships Diagram

```
┌─────────────────────────────────────────────────────────┐
│              Key Relationships                           │
└─────────────────────────────────────────────────────────┘

Super Key (Largest set)
    │
    ├─── Candidate Key (Minimal super key)
    │        │
    │        ├─── Primary Key (Chosen candidate key)
    │        │
    │        └─── Alternate Key (Other candidate keys)
    │
    └─── Foreign Key (References primary key in another table)
```

---

## SQL Injection

SQL Injection is a security vulnerability where attackers insert malicious SQL code through user input to manipulate database queries.

**Why It's Dangerous:**
- Access sensitive data
- Modify or delete database contents
- Bypass authentication
- Take control of the system

**Real-World Example:** Capital One Data Breach (2019) - Over 100 million customer records leaked due to SQL injection vulnerability.

---

### SQL Injection Security Levels

#### 1. Low Security

No input filtering or sanitization.

```php
$id = $_GET['id'];
$query = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
```

**Vulnerable to:**
- `'` - Breaks the query, reveals vulnerability
- `1' OR '1'='1` - Returns all users
- `1' UNION SELECT user, password FROM users--` - Fetches hidden data

---

#### 2. Medium Security

Basic sanitization using `addslashes()`.

```php
$id = addslashes($_GET['id']);
$query = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
```

**Bypass:**
- Simple `'` won't work (becomes `\'`)
- Numeric injection still works: `1 OR 1=1`

---

#### 3. High Security

Uses prepared statements (parameterized queries).

```php
$stmt = $pdo->prepare("SELECT first_name, last_name FROM users WHERE user_id = ?");
$stmt->execute([$id]);
```

**Result:** All injection attempts fail. Input treated as data, not SQL code.

---

### Types of SQL Injection

#### 1. Error-Based SQL Injection

Attacker causes database errors to reveal information.

**How It Works:**

1. **Identify Vulnerable Input:** Find input field that interacts with database
2. **Inject Malicious Payload:** Enter special character like `'`
3. **Analyze Error:** Database returns detailed error message
4. **Refine Attack:** Use information to extract more data

**Example:**

**Step 1:** Enter valid ID
```
Input: 1
Query: SELECT * FROM users WHERE id = '1'
Result: User details displayed
```

**Step 2:** Break the query
```
Input: '
Query: SELECT * FROM users WHERE id = '''
Result: SQL error message
```

**Error Message:**
```
You have an error in your SQL syntax; check the manual that corresponds 
to your MySQL server version for the right syntax to use near ''' at line 1
```

**What Attacker Learns:**
- Database type (MySQL)
- Query structure
- Column information

---

#### 2. Union-Based SQL Injection

Uses UNION operator to combine results from multiple SELECT statements.

**Requirements:**
- Both queries must have same number of columns
- Columns must have similar data types
- Columns must be in same order

**Example:**

**Step 1:** Find number of columns
```
Input: 1 ORDER BY 1  (works)
Input: 1 ORDER BY 2  (works)
Input: 1 ORDER BY 3  (works)
Input: 1 ORDER BY 4  (error - Unknown column '4')
```

**Result:** Query has 3 columns

**Step 2:** Use UNION to extract data
```
Input: 1 UNION SELECT username, password, email FROM users--
Query: SELECT id, name, email FROM users WHERE id = '1' 
       UNION SELECT username, password, email FROM users--
```

**Result:** Attacker gets usernames and passwords

---

#### 3. Blind SQL Injection

Attacker cannot see query results directly, infers information from behavior.

**Two Types:**

**A. Boolean-Based Blind SQLi**

Observes true/false responses.

```
Input: admin' AND 1=1; --
Result: Page loads normally (true)

Input: admin' AND 1=2; --
Result: Page shows error (false)
```

**B. Time-Based Blind SQLi**

Uses time delays to infer information.

```
Input: admin' AND SLEEP(5); --
Result: Page takes 5 seconds to load (condition true)
```

**Example Attack:**

```sql
-- Check if first character of password is 'a'
admin' AND IF(SUBSTRING(password,1,1)='a', SLEEP(5), 0); --

-- If page delays 5 seconds, first character is 'a'
-- Repeat for each character to extract full password
```

---

### Impact of SQL Injection Attacks

| Impact | Description |
|--------|-------------|
| **Unauthorized Access** | Retrieve personal, financial, or confidential data |
| **Data Integrity Issues** | Modify, delete, or corrupt critical data |
| **Privilege Escalation** | Bypass authentication, gain admin privileges |
| **Service Downtime** | Overload server, cause crashes |
| **Reputation Damage** | Loss of customer trust |

---

### Preventing SQL Injection Attacks

#### 1. Use Prepared Statements and Parameterized Queries

**Best Practice:** Treat user input as data, not SQL code.

**PHP Example (MySQLi):**
```php
$stmt = $conn->prepare("SELECT * FROM users WHERE username = ? AND password = ?");
$stmt->bind_param("ss", $username, $password);
$stmt->execute();
```

**Python Example:**
```python
cursor.execute("SELECT * FROM users WHERE username = ? AND password = ?", 
               (username, password))
```

---

#### 2. Employ Stored Procedures

Predefined SQL queries stored in the database.

```sql
CREATE PROCEDURE GetUserByUsername (IN username VARCHAR(50))
BEGIN
    SELECT * FROM users WHERE username = username;
END;
```

**Call Procedure:**
```sql
CALL GetUserByUsername('alice');
```

---

#### 3. Whitelist Input Validation

Allow only specific characters and patterns.

```php
// Only allow alphanumeric characters
if (preg_match('/^[a-zA-Z0-9]+$/', $username)) {
    // Process input
} else {
    // Reject input
}
```

---

#### 4. Use ORM Frameworks

Object-Relational Mapping frameworks handle query generation automatically.

**Examples:**
- Hibernate (Java)
- Entity Framework (.NET)
- Django ORM (Python)
- Sequelize (Node.js)

---

#### 5. Restrict Database Privileges

Grant minimum required permissions.

```sql
-- Create user with limited permissions
CREATE USER 'app_user'@'localhost' IDENTIFIED BY 'password';

-- Grant only SELECT and INSERT
GRANT SELECT, INSERT ON database_name.* TO 'app_user'@'localhost';

-- Revoke dangerous permissions
REVOKE DROP, ALTER ON database_name.* FROM 'app_user'@'localhost';
```

---

#### 6. Error Handling

Don't display detailed error messages to users.

**Bad Practice:**
```php
die("Error: " . mysqli_error($conn));
```

**Good Practice:**
```php
// Log error internally
error_log("Database error: " . mysqli_error($conn));

// Show generic message to user
die("An error occurred. Please try again later.");
```

---

## SQL Data Encryption

SQL Data Encryption protects sensitive database information by converting it into an unreadable format.

**Why Encrypt Data?**
- Secure personal and confidential data
- Protect data at rest and in transit
- Ensure only authorized users can access data
- Prevent data misuse during security breaches

---

### Types of SQL Data Encryption

#### 1. Transparent Data Encryption (TDE)

Encrypts entire database including data and log files.

**Characteristics:**
- Operates at file level
- Automatic encryption/decryption
- No application changes required
- Uses symmetric key

**Steps to Implement TDE:**

**Step 1: Create Database Master Key**
```sql
USE dba;
GO
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'ABC@123';
GO
```

**Step 2: Create Certificate**
```sql
USE dba;
GO
CREATE CERTIFICATE TDE_Certificate
    WITH SUBJECT = 'Certificate for TDE';
GO
```

**Step 3: Create Encryption Key**
```sql
USE dba;
GO
CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_256
ENCRYPTION BY SERVER CERTIFICATE TDE_Certificate;
```

**Step 4: Enable Encryption**
```sql
ALTER DATABASE dba SET ENCRYPTION ON;
```

---

#### 2. Column-Level Encryption (CLE)

Encrypts specific columns within a table.

**Characteristics:**
- Selective encryption
- Useful for mixed sensitive/non-sensitive data
- Uses asymmetric key

**Example Setup:**

```sql
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(30) NOT NULL,
    RollNumber VARCHAR(10) NOT NULL
);

INSERT INTO Student VALUES
(10, 'James', '1234'),
(20, 'Olivia', '4321'),
(30, 'William', '4554'),
(40, 'Emma', '7896');
```

**Steps to Implement CLE:**

**Step 1: Create Master Key**
```sql
USE Student;
GO
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '123@4321';
```

**Step 2: Create Certificate**
```sql
USE Student;
GO
CREATE CERTIFICATE Certificate_test
WITH SUBJECT = 'Protect my data';
GO
```

**Step 3: Create Symmetric Key**
```sql
CREATE SYMMETRIC KEY SymKey_test
WITH ALGORITHM = AES_256
ENCRYPTION BY CERTIFICATE Certificate_test;
```

**Step 4: Encrypt Column**
```sql
ALTER TABLE Student ADD RollNumber_encrypt VARBINARY(MAX);

OPEN SYMMETRIC KEY SymKey_test
DECRYPTION BY CERTIFICATE Certificate_test;

UPDATE Student
SET RollNumber_encrypt = EncryptByKey(Key_GUID('SymKey_test'), RollNumber);

CLOSE SYMMETRIC KEY SymKey_test;
```

**Result:**

| StudentID | StudentName | RollNumber | RollNumber_encrypt |
|-----------|-------------|------------|--------------------|
| 10 | James | 1234 | 0x01000000D0... |
| 20 | Olivia | 4321 | 0x01000000E1... |

---

### Benefits of SQL Data Encryption

| Benefit | Description |
|---------|-------------|
| **Data Protection** | Secure from unauthorized access |
| **Enhanced Security** | Reduces risk of data breaches |
| **Data Integrity** | Maintains accuracy and consistency |
| **Selective Encryption** | Encrypt only critical data |
| **Compliance** | Meets regulatory requirements (GDPR, HIPAA) |

---

## SQL Backup

A backup is a copy of database data stored in a different location for recovery in case of data loss.

**Why Backup?**
- Protect against data loss
- Recover from hardware failures
- Restore after accidental deletions
- Maintain business continuity

---

### Backup Methods

#### 1. Using SQL Server Management Studio (SSMS)

GUI-based backup creation.

#### 2. Using Transact-SQL

Command-line backup creation.

---

### Backup Syntax

```sql
BACKUP DATABASE databasename
TO backup_device [ ]
[ WITH with_options [] ];
```

**Parameters:**
- `databasename`: Database to backup
- `backup_device`: DISK or TAPE
- `with_options`: Additional options

---

### Common Backup Options

| Option | Description |
|--------|-------------|
| **COMPRESSION / NO_COMPRESSION** | Enable/disable backup compression |
| **DESCRIPTION** | Describe backup (max 255 characters) |
| **NAME** | Name of backup set (max 128 characters) |
| **FORMAT** | Overwrite existing media |
| **MEDIANAME** | Media name |
| **MEDIADESCRIPTION** | Media description |

---

### Types of SQL Server Backups

#### 1. Full Database Backup

Complete backup of entire database.

```sql
BACKUP DATABASE DatabaseName
TO DISK = 'C:\DatabaseName.BAK'
GO
```

**Example:**
```sql
BACKUP DATABASE GeeksDB
TO DISK = 'D:\Backup\GeeksDB.bak'
WITH FORMAT,
MEDIANAME = 'GeeksDBBackup',
NAME = 'Full Backup of GeeksDB';
GO
```

---

#### 2. Differential Backup

Backs up only changes since last full backup.

```sql
BACKUP DATABASE DatabaseName
TO DISK = 'C:\DatabaseName.BAK'
WITH DIFFERENTIAL
GO
```

**Use Case:** Faster than full backup, useful for frequent backups.

---

#### 3. File-Level Backup

Backs up specific database files.

```sql
BACKUP DATABASE DatabaseName
FILE = 'DatabaseName'
TO DISK = 'C:\DatabaseName_DatabaseName.FILE'
GO
```

---

#### 4. Filegroup Backup

Backs up specific filegroups.

```sql
BACKUP DATABASE DatabaseName
FILEGROUP = 'ReadOnly'
TO DISK = 'C:\DatabaseName.FLG'
GO
```

---

#### 5. Multiple Disk Files Backup

Splits backup across multiple files.

```sql
BACKUP DATABASE DatabaseName
TO DISK = 'C:\DatabaseName_1.BAK',
   DISK = 'D:\DatabaseName_2.BAK',
   DISK = 'E:\DatabaseName_3.BAK'
GO
```

**Benefit:** Creates smaller files instead of one large file.

---

#### 6. Password-Protected Backup

Creates backup with password protection.

```sql
BACKUP DATABASE DatabaseName
TO DISK = 'C:\DatabaseName_1.BAK'
WITH PASSWORD = 'Q!W@E#R$'
GO
```

**Note:** Password required when restoring.

---

#### 7. Backup with Progress Stats

Shows backup progress.

```sql
BACKUP DATABASE DatabaseName
TO DISK = 'C:\DatabaseName_1.BAK'
WITH STATS
GO
```

**Custom Progress Interval:**
```sql
BACKUP DATABASE DatabaseName
TO DISK = 'C:\DatabaseName_1.BAK'
WITH STATS = 2  -- Show progress every 2%
GO
```

---

#### 8. Backup with Description

Adds description to backup.

```sql
BACKUP DATABASE DatabaseName
TO DISK = 'C:\DatabaseName_1.BAK'
WITH DESCRIPTION = 'Full backup for DatabaseName'
GO
```

**Use Case:** Helps identify backup contents during restore.

---

#### 9. Mirrored Backup

Creates multiple backup copies.

```sql
BACKUP DATABASE DatabaseName
TO DISK = 'C:\DatabaseName_1.BAK'
MIRROR TO DISK = 'D:\DatabaseName_mirror.BAK'
WITH FORMAT
GO
```

**Benefit:** Multiple copies in different locations for redundancy.

---

#### 10. Multiple Options Backup

Combines multiple backup options.

```sql
BACKUP DATABASE DatabaseName
TO DISK = 'C:\DatabaseName_1.BAK'
MIRROR TO DISK = 'D:\DatabaseName_mirror.BAK'
WITH FORMAT, STATS, PASSWORD = 'Q!W@E#R$'
GO
```

---

### Complete Backup Example

```sql
-- Step 1: Create Database
CREATE DATABASE GeeksDB;
GO

-- Step 2: Use Database
USE GeeksDB;
GO

-- Step 3: Create Backup
BACKUP DATABASE GeeksDB
TO DISK = 'D:\Backup\GeeksDB.bak'
WITH FORMAT,
MEDIANAME = 'GeeksDBBackup',
NAME = 'Full Backup of GeeksDB';
GO
```

**Output:**
```
Processed 160 pages for database 'GeeksDB', file 'GeeksDB' on file 1.
Processed 2 pages for database 'GeeksDB', file 'GeeksDB_log' on file 1.
BACKUP DATABASE successfully processed 162 pages in 0.123 seconds (10.267 MB/sec).
```

---

### Backup Best Practices

| Practice | Description |
|----------|-------------|
| **Regular Schedule** | Automate backups daily/weekly |
| **Multiple Locations** | Store backups in different locations |
| **Test Restores** | Regularly test backup restoration |
| **Compression** | Use compression to save space |
| **Encryption** | Encrypt sensitive backups |
| **Retention Policy** | Define how long to keep backups |
| **Documentation** | Document backup procedures |

---

## Object-Relational Mapping (ORM)

ORM is a technique that bridges object-oriented programming and relational databases.

**What is ORM?**
- Converts data between incompatible systems
- Maps objects to database tables
- Simplifies database interactions
- Reduces manual SQL code

---

### Object-Relational Database Management System (ORDBMS)

ORDBMS enhances relational databases with object-oriented principles.

**Features:**
- Handles complex data types
- Supports encapsulation
- Implements inheritance
- Combines relational and object-oriented capabilities

---

### Key ORM Concepts

#### 1. Entities

Objects or classes in code that map to database tables.

**Example:**
```python
# Python class (Entity)
class Student:
    def __init__(self, id, name, age):
        self.id = id
        self.name = name
        self.age = age
```

**Maps to:**
```sql
-- Database table
CREATE TABLE Student (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT
);
```

---

#### 2. Relationships

Define how entities relate to each other.

**Types:**
- One-to-One
- One-to-Many
- Many-to-One
- Many-to-Many

**Example:**
```python
class Student:
    courses = []  # Many-to-Many relationship

class Course:
    students = []  # Many-to-Many relationship
```

---

#### 3. Persistence

Ability to store data beyond application lifetime.

**ORM ensures:**
- Data persists in database
- Available after application restart
- Secure and accessible

---

### How ORM Works

#### 1. Entity Mapping

Map classes to database tables.

**Code:**
```python
class Employee:
    id = IntegerField(primary_key=True)
    name = StringField(max_length=50)
    salary = DecimalField()
```

**Database:**
```sql
CREATE TABLE Employee (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    salary DECIMAL(10,2)
);
```

---

#### 2. Relationship Mapping

Define relationships between entities.

**One-to-Many Example:**
```python
class Department:
    id = IntegerField(primary_key=True)
    name = StringField()
    employees = OneToMany('Employee')

class Employee:
    id = IntegerField(primary_key=True)
    name = StringField()
    department = ForeignKey('Department')
```

---

#### 3. Data Type Mapping

Convert between programming language and database types.

| Python Type | SQL Type |
|-------------|----------|
| int | INT |
| str | VARCHAR |
| float | DECIMAL |
| datetime | DATETIME |
| bool | BOOLEAN |

---

#### 4. CRUD Operations

ORM simplifies Create, Read, Update, Delete operations.

**Without ORM:**
```python
cursor.execute("INSERT INTO Employee (name, salary) VALUES (?, ?)", 
               ('Alice', 50000))
```

**With ORM:**
```python
employee = Employee(name='Alice', salary=50000)
employee.save()
```

---

#### 5. Query Language

ORM provides object-oriented query syntax.

**Example (Django ORM):**
```python
# Get all employees with salary > 50000
employees = Employee.objects.filter(salary__gt=50000)

# Get employee by ID
employee = Employee.objects.get(id=1)

# Update employee
employee.salary = 60000
employee.save()

# Delete employee
employee.delete()
```

**Equivalent SQL:**
```sql
-- Get all employees with salary > 50000
SELECT * FROM Employee WHERE salary > 50000;

-- Get employee by ID
SELECT * FROM Employee WHERE id = 1;

-- Update employee
UPDATE Employee SET salary = 60000 WHERE id = 1;

-- Delete employee
DELETE FROM Employee WHERE id = 1;
```

---

### Benefits of ORM

| Benefit | Description |
|---------|-------------|
| **Abstraction** | Hides SQL complexity from developers |
| **Portability** | Switch databases with minimal code changes |
| **Code Reusability** | Use same codebase with different databases |
| **Maintenance** | Easier to maintain and update |
| **Scalability** | Optimize without major code changes |
| **Productivity** | Faster development time |
| **Type Safety** | Compile-time error checking |

---

### Popular ORM Frameworks

| Language | ORM Framework | Description |
|----------|---------------|-------------|
| **Java** | Hibernate | Most popular Java ORM |
| **Python** | Django ORM | Built into Django framework |
| **Python** | SQLAlchemy | Flexible Python ORM |
| **.NET** | Entity Framework | Microsoft's ORM for .NET |
| **Node.js** | Sequelize | Promise-based Node.js ORM |
| **Ruby** | Active Record | Rails ORM |
| **PHP** | Doctrine | PHP ORM |

---

### ORM Example: Python Django

**Define Model:**
```python
from django.db import models

class Student(models.Model):
    student_id = models.IntegerField(primary_key=True)
    name = models.CharField(max_length=50)
    age = models.IntegerField()
    email = models.EmailField()
    
    def __str__(self):
        return self.name
```

**Create Record:**
```python
student = Student(student_id=1, name='Alice', age=20, email='alice@example.com')
student.save()
```

**Read Records:**
```python
# Get all students
all_students = Student.objects.all()

# Get specific student
student = Student.objects.get(student_id=1)

# Filter students
young_students = Student.objects.filter(age__lt=21)
```

**Update Record:**
```python
student = Student.objects.get(student_id=1)
student.age = 21
student.save()
```

**Delete Record:**
```python
student = Student.objects.get(student_id=1)
student.delete()
```

---

### ORM vs Raw SQL

| Aspect | ORM | Raw SQL |
|--------|-----|---------|
| **Syntax** | Object-oriented | SQL statements |
| **Learning Curve** | Easier for OOP developers | Requires SQL knowledge |
| **Portability** | High (database-agnostic) | Low (database-specific) |
| **Performance** | May be slower for complex queries | Optimized for performance |
| **Maintenance** | Easier to maintain | More code to maintain |
| **Flexibility** | Limited for complex queries | Full control |

---

### When to Use ORM

**Use ORM When:**
- Building standard CRUD applications
- Need database portability
- Want faster development
- Team familiar with OOP
- Maintenance is priority

**Use Raw SQL When:**
- Complex queries required
- Performance is critical
- Need fine-grained control
- Working with legacy databases
- Specific database features needed

---

## Quick Reference Tables

### ER Model Components

| Component | Symbol | Description |
|-----------|--------|-------------|
| Entity | Rectangle | Real-world object |
| Attribute | Oval | Property of entity |
| Relationship | Diamond | Connection between entities |
| Key Attribute | Underlined Oval | Primary key |
| Weak Entity | Double Rectangle | Dependent entity |

---

### Normal Forms Summary

| Normal Form | Rule | Example |
|-------------|------|---------|
| **1NF** | Atomic values | No multi-valued attributes |
| **2NF** | 1NF + No partial dependency | All attributes depend on full key |
| **3NF** | 2NF + No transitive dependency | No non-key depends on non-key |
| **BCNF** | 3NF + X is super key | Stricter version of 3NF |
| **4NF** | BCNF + No multi-valued dependency | Eliminate multi-valued dependencies |
| **5NF** | 4NF + No join dependency | Cannot be further decomposed |

---

### Database Keys Summary

| Key Type | Uniqueness | NULL Allowed | Count per Table |
|----------|------------|--------------|-----------------|
| Super Key | Yes | Yes | Multiple |
| Candidate Key | Yes | No | Multiple |
| Primary Key | Yes | No | One |
| Alternate Key | Yes | No | Multiple |
| Foreign Key | No | Yes | Multiple |
| Composite Key | Yes | No | Multiple |

---

### SQL Injection Prevention

| Method | Effectiveness | Implementation |
|--------|---------------|----------------|
| Prepared Statements | High | Use parameterized queries |
| Stored Procedures | High | Predefined SQL in database |
| Input Validation | Medium | Whitelist allowed characters |
| ORM Frameworks | High | Automatic query generation |
| Least Privilege | Medium | Restrict database permissions |
| Error Handling | Low | Hide detailed errors |

---

## Key Takeaways

### Database Design:
- ER Model visualizes database structure
- Entities represent real-world objects
- Relationships connect entities
- Attributes describe entity properties
- Normalization reduces redundancy
- Keys ensure data integrity

### Security:
- SQL Injection is a major threat
- Use prepared statements always
- Encrypt sensitive data (TDE, CLE)
- Regular backups are essential
- Implement least privilege principle
- Validate and sanitize all inputs

### ORM:
- Bridges OOP and relational databases
- Simplifies database interactions
- Improves code maintainability
- Provides database portability
- Reduces manual SQL code
- Choose based on project needs
