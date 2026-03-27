# Functional Dependencies & Normalization

## What is Functional Dependency?

Functional Dependency (FD) shows how one attribute determines another attribute.

**Simple Definition:** If you know value A, you can find value B. We write it as A → B (A determines B).

**Example:**
- StudentID → StudentName (If you know student ID, you can find student name)
- Email → Password (If you know email, you can find password)

---

## Attribute Closure

Attribute closure finds all attributes that can be determined from a given set of attributes.

**Symbol:** (A)+ means closure of attribute A

**How to Find:**
1. Start with the given attribute
2. Add all attributes it can determine
3. Keep adding until no more can be found

**Example:**

Given FDs:
- A → B
- B → C
- C → D

Find (A)+:
- Start: {A}
- A → B, so add B: {A, B}
- B → C, so add C: {A, B, C}
- C → D, so add D: {A, B, C, D}

**Result:** (A)+ = {A, B, C, D}

---

## Finding Keys Using Attribute Closure

### Super Key
If (X)+ contains all attributes of the table, then X is a super key.

### Candidate Key
If X is a super key and no subset of X is a super key, then X is a candidate key.

**Example:**

Table: Student (StudentID, Name, Phone, State, Country, Age)

FDs:
- StudentID → Name, Phone, State, Country, Age
- State → Country

**Finding Keys:**
- (StudentID)+ = {StudentID, Name, Phone, State, Country, Age} ✓ All attributes
- StudentID is a candidate key (minimal)

---

## Prime and Non-Prime Attributes

| Type | What it means | Example |
|------|---------------|---------|
| **Prime** | Part of any candidate key | StudentID |
| **Non-Prime** | Not part of any candidate key | Name, Age, Phone |

---

## Armstrong's Axioms

Rules to find new functional dependencies from existing ones.

### Primary Rules

| Rule | What it means | Example |
|------|---------------|---------|
| **Reflexivity** | If B ⊆ A, then A → B | {A, B} → A |
| **Augmentation** | If A → B, then AC → BC | If A → B, then AB → BC |
| **Transitivity** | If A → B and B → C, then A → C | A → B, B → C means A → C |

### Secondary Rules

| Rule | What it means | Example |
|------|---------------|---------|
| **Union** | If A → B and A → C, then A → BC | Combine right sides |
| **Decomposition** | If A → BC, then A → B and A → C | Split right side |
| **Composition** | If A → B and X → Y, then AX → BY | Combine both |

**Example:**

Given: A → B, B → C

Using Transitivity: A → C

Using Union: A → BC

---

## Canonical Cover

Minimal set of functional dependencies with no redundancy.

**Simple Definition:** Simplest form of FD set that gives same result.

### Steps to Find Canonical Cover

**Step 1:** Combine FDs with same left side
- A → B and A → C becomes A → BC

**Step 2:** Remove extra attributes
- Check if any attribute can be removed

**Step 3:** Split right side to single attributes
- A → BC becomes A → B and A → C

**Step 4:** Remove redundant FDs
- If an FD can be derived from others, remove it

**Example:**

Given: F = {A → BC, B → C, AB → C}

**Step 1:** Already combined

**Step 2:** In A → BC, C is extra (because A → B and B → C gives A → C)
- Reduce to A → B

**Step 3:** Split if needed (already done)

**Step 4:** AB → C is redundant (can be derived from A → B and B → C)
- Remove it

**Result:** Fc = {A → B, B → C}

---

## The Problem of Redundancy

Redundancy means storing same data multiple times.

**Example Table:**

| StudentID | Name | College | Course | Rank |
|-----------|------|---------|--------|------|
| 100 | Alice | GEU | B.Tech | 1 |
| 101 | Bob | GEU | B.Tech | 1 |
| 102 | Charlie | GEU | B.Tech | 1 |

**Problems:**
- College name repeated 3 times
- Course repeated 3 times
- Rank repeated 3 times

---

## Anomalies (Problems from Redundancy)

### 1. Insertion Anomaly
Cannot insert data without adding extra unrelated data.

**Example:** Cannot add a new college until a student enrolls.

### 2. Deletion Anomaly
Deleting data removes other important information.

**Example:** If last student of a college is deleted, college information is also lost.

### 3. Update Anomaly
Updating data requires changes in multiple places.

**Example:** If college rank changes, must update all rows. If we miss one, data becomes inconsistent.

---

## Normal Forms

Normal forms are rules to organize data and remove redundancy.

### 1NF (First Normal Form)

**Rules:**
- Each cell has only one value (atomic)
- No repeating groups
- Each row is unique

**Example of Violation:**

| StudentID | Name | Phone |
|-----------|------|-------|
| 1 | Alice | 123, 456 |

**Fixed:**

| StudentID | Name | Phone |
|-----------|------|-------|
| 1 | Alice | 123 |
| 1 | Alice | 456 |

---

### 2NF (Second Normal Form)

**Rules:**
- Must be in 1NF
- No partial dependency (non-prime attribute should depend on full primary key)

**Example of Violation:**

Table: Enrollment (StudentID, CourseID, StudentName, CourseName)
- Primary Key: (StudentID, CourseID)
- StudentName depends only on StudentID (partial dependency)

**Fixed:**

**Student Table:**
| StudentID | StudentName |
|-----------|-------------|
| 1 | Alice |

**Course Table:**
| CourseID | CourseName |
|----------|------------|
| 101 | Math |

**Enrollment Table:**
| StudentID | CourseID |
|-----------|----------|
| 1 | 101 |

---

### 3NF (Third Normal Form)

**Rules:**
- Must be in 2NF
- No transitive dependency (non-prime should not depend on non-prime)

**Example of Violation:**

Table: Student (StudentID, State, Country)
- StudentID → State
- State → Country (transitive dependency)

**Fixed:**

**Student Table:**
| StudentID | State |
|-----------|-------|
| 1 | CA |

**State Table:**
| State | Country |
|-------|---------|
| CA | USA |

---

### BCNF (Boyce-Codd Normal Form)

**Rules:**
- Must be in 3NF
- For every FD (A → B), A must be a super key

**Example of Violation:**

Table: Class (StudentID, Course, Instructor)
- FD: Course → Instructor
- But Course is not a super key

**Fixed:**

**Course Table:**
| Course | Instructor |
|--------|------------|
| Math | Mr. Smith |

**Enrollment Table:**
| StudentID | Course |
|-----------|--------|
| 1 | Math |

---

### 4NF (Fourth Normal Form)

**Rules:**
- Must be in BCNF
- No multi-valued dependencies

**Example of Violation:**

Table: Student (StudentID, Language, Hobby)
- One student can have multiple languages
- One student can have multiple hobbies
- Languages and hobbies are independent

**Fixed:**

**Student_Language:**
| StudentID | Language |
|-----------|----------|
| 1 | English |
| 1 | Spanish |

**Student_Hobby:**
| StudentID | Hobby |
|-----------|-------|
| 1 | Reading |
| 1 | Gaming |

---

### 5NF (Fifth Normal Form)

**Rules:**
- Must be in 4NF
- No join dependencies

**Simple Definition:** Table cannot be split into smaller tables without losing information.

---

## Normal Forms Summary

| Normal Form | Main Rule | What it removes |
|-------------|-----------|-----------------|
| **1NF** | Atomic values | Repeating groups |
| **2NF** | No partial dependency | Partial dependencies |
| **3NF** | No transitive dependency | Transitive dependencies |
| **BCNF** | Determinant must be super key | All anomalies |
| **4NF** | No multi-valued dependency | Multi-valued dependencies |
| **5NF** | No join dependency | Join dependencies |

**Hierarchy:**
```
1NF ⊂ 2NF ⊂ 3NF ⊂ BCNF ⊂ 4NF ⊂ 5NF
```

---

## Lossless Join Decomposition

When we split a table into two tables, we should be able to get back original table by joining them.

**Rules for Lossless Join:**

If R is split into R1 and R2:

1. R1 ∪ R2 = R (all attributes covered)
2. R1 ∩ R2 ≠ Φ (common attribute exists)
3. Common attribute must be key in at least one table

**Example:**

R (A, B, C, D) with FD: A → BC

Split into:
- R1 (A, B, C)
- R2 (A, D)

**Check:**
1. (ABC) ∪ (AD) = (ABCD) ✓
2. (ABC) ∩ (AD) = A ✓
3. A is key in R1 ✓

**Result:** Lossless join decomposition

---

## Dependency Preserving Decomposition

All functional dependencies should be preserved after decomposition.

**Example:**

R (A, B, C, D) with FD: A → BC

Split into:
- R1 (A, B, C)
- R2 (A, D)

**Check:**
- FD A → BC is in R1 ✓

**Result:** Dependency preserving

---

## Denormalization

Adding redundancy back to improve performance.

**Why Denormalize:**
- Reduce number of joins
- Faster queries
- Better read performance

**When to Use:**
- Read-heavy applications
- Reporting systems
- Data warehouses

**Example:**

**Normalized:**

**Student Table:**
| StudentID | Name |
|-----------|------|
| 1 | Alice |

**Class Table:**
| ClassID | ClassName | Teacher |
|---------|-----------|---------|
| 101 | Math | Mr. Smith |

**Enrollment Table:**
| StudentID | ClassID |
|-----------|---------|
| 1 | 101 |

**Denormalized:**

| StudentID | Name | ClassName | Teacher |
|-----------|------|-----------|---------|
| 1 | Alice | Math | Mr. Smith |

---

## Normalization vs Denormalization

| Feature | Normalization | Denormalization |
|---------|---------------|-----------------|
| **Redundancy** | No redundancy | Some redundancy |
| **Performance** | Slower reads | Faster reads |
| **Updates** | Easy | Complex |
| **Storage** | Less space | More space |
| **Joins** | Many joins | Few joins |
| **Use Case** | Transactional systems | Reporting systems |

---

## Key Takeaways

- Functional dependency shows how attributes relate (A → B)
- Attribute closure finds all attributes determined by a set
- Armstrong's axioms help derive new FDs
- Canonical cover is minimal set of FDs
- Redundancy causes insertion, deletion, and update anomalies
- Normal forms (1NF to 5NF) remove redundancy step by step
- Lossless join means no data loss when splitting tables
- Dependency preserving means all FDs are maintained
- Denormalization adds redundancy for better performance
- Use normalization for data integrity, denormalization for speed

