# ER & Relational Model

## What is ER Model?

ER stands for Entity-Relationship. It's a way to design databases before actually creating them.

**Simple Definition:** ER Model is like drawing a map before building a house - it shows what you need and how things connect.

**Three Main Parts:**
- **Entity:** Things you want to store (Student, Course, Teacher)
- **Attribute:** Details about things (Name, Age, ID)
- **Relationship:** How things connect (Student takes Course)

---

## Drawing ER Diagrams

We use shapes to draw ER diagrams:

| Shape | What it means | Example |
|-------|---------------|---------|
| Rectangle | Entity (thing) | Student, Course |
| Oval | Attribute (detail) | Name, Age, ID |
| Diamond | Relationship (connection) | Enrolls in, Teaches |
| Line | Links things together | Connects shapes |

---

## Types of Entities

### 1. Strong Entity
Can exist on its own.
- Has its own unique ID
- Doesn't need other entities
- Example: Student (has Student ID)

### 2. Weak Entity
Cannot exist without another entity.
- Depends on strong entity
- Shown with double rectangle
- Example: Dependent (child of Employee - can't exist without employee)

```
┌─────────┐         ╔═════════╗
│Employee │────────═╣Dependent╠
│(Strong) │         ╚═════════╝
└─────────┘          (Weak)
```

---

## Types of Attributes

### 1. Simple Attribute
Basic information that cannot be divided.
- Example: Age, ID Number

### 2. Composite Attribute
Can be broken into smaller parts.
- Example: Full Name = First Name + Last Name
- Example: Address = Street + City + State

### 3. Multi-valued Attribute
Can have multiple values.
- Shown with double oval
- Example: Phone Numbers (person can have 2-3 numbers)

### 4. Derived Attribute
Calculated from other attributes.
- Shown with dashed oval
- Example: Age (calculated from Date of Birth)

---

## Relationships

### Degree of Relationship

**How many entities are connected:**

| Type | Number of Entities | Example |
|------|-------------------|---------|
| **Unary** | 1 | Person married to Person |
| **Binary** | 2 | Student enrolls in Course |
| **Ternary** | 3 | Doctor treats Patient at Hospital |

---

## Cardinality (How Many)

Shows how many times entities can connect.

### 1. One-to-One (1:1)
One connects to only one.
- Example: One person has one passport
- Example: One husband has one wife

```
Person ────1────◇────1──── Passport
```

### 2. One-to-Many (1:M)
One connects to many.
- Example: One teacher teaches many students
- Example: One department has many employees

```
Department ────1────◇────M──── Employee
```

### 3. Many-to-One (M:1)
Many connect to one.
- Example: Many students have one teacher
- Example: Many employees work in one department

```
Employee ────M────◇────1──── Department
```

### 4. Many-to-Many (M:N)
Many connect to many.
- Example: Many students take many courses
- Example: Many employees work on many projects

```
Student ────M────◇────N──── Course
```

---

## Participation

### Total Participation
Every entity MUST participate.
- Shown with double line
- Example: Every student MUST enroll in a course

### Partial Participation
Entity MAY or MAY NOT participate.
- Shown with single line
- Example: Some courses may have no students

```
Student ════════◇──────── Course
(Total)         (Partial)
```

---

## Relational Model

After drawing ER diagram, we convert it to tables. This is called Relational Model.

**Simple Definition:** Relational Model stores data in tables (like Excel sheets).

### Basic Terms

| Term | What it means | Example |
|------|---------------|---------|
| **Table/Relation** | Where data is stored | Student Table |
| **Row/Tuple** | One record | One student's data |
| **Column/Attribute** | One piece of info | Name, Age, ID |
| **Degree** | Number of columns | 4 columns = Degree 4 |
| **Cardinality** | Number of rows | 100 students = Cardinality 100 |

### Example Table

**Student Table:**

| StudentID | Name | Age | Course |
|-----------|------|-----|--------|
| 1 | Alice | 21 | CS |
| 2 | Bob | 22 | Math |
| 3 | Charlie | 21 | Physics |

- **Degree:** 4 (4 columns)
- **Cardinality:** 3 (3 rows)

---

## Types of Keys

Keys help identify records uniquely.

### 1. Super Key
Any combination that identifies a row uniquely.
- Can have extra columns
- Example: StudentID, (StudentID + Name)

### 2. Candidate Key
Minimum columns needed to identify uniquely.
- No extra columns
- Example: StudentID, Email

### 3. Primary Key
The main key chosen from candidate keys.
- Cannot be NULL
- Must be unique
- Only one per table
- Example: StudentID

### 4. Foreign Key
Links two tables together.
- References primary key of another table
- Can be NULL
- Can have duplicates
- Example: StudentID in Enrollment table

### 5. Composite Key
Multiple columns together form a key.
- Example: (StudentID + CourseID) in Enrollment table

---

## Converting ER to Tables

### Rule 1: Entity → Table
Each entity becomes a table.
- Entity: Student
- Table: Student (StudentID, Name, Age)

### Rule 2: Relationship → Table or Column

**For 1:1 Relationship:**
- Merge into one table

**For 1:M Relationship:**
- Add foreign key to "Many" side

**For M:N Relationship:**
- Create new table with both keys

### Example

**ER Diagram:**
```
Student ────M────◇ Enrolls ◇────N──── Course
```

**Tables:**

**Student Table:**
| StudentID | Name |
|-----------|------|
| 1 | Alice |
| 2 | Bob |

**Course Table:**
| CourseID | CourseName |
|----------|------------|
| 101 | Math |
| 102 | Physics |

**Enrollment Table:**
| StudentID | CourseID | Grade |
|-----------|----------|-------|
| 1 | 101 | A |
| 1 | 102 | B |
| 2 | 101 | A |

---

## Generalization & Specialization

### Generalization
Combining similar entities into one.
- Bottom-up approach
- Example: Student + Teacher → Person

```
    ┌────────┐
    │ Person │ (General)
    └────┬───┘
    ┌────┴────┐
┌───▼───┐ ┌──▼────┐
│Student│ │Teacher│
└───────┘ └───────┘
```

### Specialization
Breaking one entity into specific types.
- Top-down approach
- Example: Employee → Developer, Tester

```
┌────────┐
│Employee│ (General)
└────┬───┘
┌────┴────┐
┌──▼──────┐ ┌──▼────┐
│Developer│ │Tester │
└─────────┘ └───────┘
```

---

## Constraints in DBMS

Rules to keep data correct and consistent.

| Constraint | What it does | Example |
|------------|--------------|---------|
| **PRIMARY KEY** | Makes column unique and not null | StudentID |
| **FOREIGN KEY** | Links to another table | StudentID in Enrollment |
| **UNIQUE** | All values must be different | Email |
| **NOT NULL** | Cannot be empty | Name |
| **CHECK** | Must meet condition | Age > 18 |
| **DEFAULT** | Sets default value | Status = 'Active' |

---

## Schema Design Strategies

### 1. Top-Down
Start with big picture, then add details.
- Begin with main entities
- Then add attributes and relationships

### 2. Bottom-Up
Start with details, then group them.
- Begin with attributes
- Group into entities

### 3. Mixed
Combine both approaches.
- Use top-down for some parts
- Use bottom-up for others

---

## Normalization (Quick Overview)

Making tables better organized.

| Normal Form | Rule | Example |
|-------------|------|---------|
| **1NF** | No repeating groups | One value per cell |
| **2NF** | No partial dependency | All columns depend on full key |
| **3NF** | No transitive dependency | No column depends on non-key column |

---

## Key Takeaways

- ER Model helps design databases visually
- Three main parts: Entity, Attribute, Relationship
- Cardinality shows how many (1:1, 1:M, M:1, M:N)
- Relational Model uses tables to store data
- Keys help identify and link records
- Convert ER diagrams to tables using rules
- Constraints keep data correct
- Normalization organizes data better
