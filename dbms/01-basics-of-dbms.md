# Basics of DBMS

## What is DBMS?

DBMS stands for Database Management System. It's software that helps us store and manage data.

**Simple Definition:** DBMS is like a smart filing system for your computer that keeps all your data organized and safe.

**What it does:**
- Stores data in an organized way
- Helps you find data quickly
- Keeps data safe and secure
- Lets many people use data at the same time

---

## Why Do We Need DBMS?

Before DBMS, people used simple files to store data. This caused many problems:

| Problem | What it means |
|---------|---------------|
| **Data Redundancy** | Same data saved many times |
| **Inconsistency** | Different files have different information |
| **Hard to Access** | Finding data takes too much time |
| **No Security** | Anyone can see or change data |
| **Single User** | Only one person can use it at a time |
| **No Backup** | If data is lost, it's gone forever |

**Example:** A school storing student data in different files (one for grades, one for attendance) - same student name appears everywhere, wasting space.

---

## Parts of DBMS

DBMS has 6 main parts:

### 1. Hardware
Physical things you can touch.
- Computer, hard disk, keyboard, monitor
- Stores and processes data

### 2. Software
Programs that run the database.
- MySQL, Oracle, PostgreSQL
- Helps you talk to the database

### 3. Data
The actual information you store.
- Student names, ages, marks
- Product prices, customer details

### 4. Procedures
Rules for using the database.
- How to login
- How to backup data
- How to keep data safe

### 5. Database Language
Commands to work with data.
- SQL (most common)
- Used to create, read, update, delete data

### 6. People
Users who work with the database.
- **Admin:** Manages the database
- **Developer:** Builds apps using database
- **End User:** Uses the apps (students, customers)

---

## Types of DBMS

### 1. Relational DBMS (RDBMS)
Stores data in tables (rows and columns).
- Most popular type
- Uses SQL language
- Examples: MySQL, Oracle, PostgreSQL

**Think of it like:** Excel spreadsheet with multiple sheets connected to each other.

### 2. NoSQL DBMS
Stores data in flexible formats (not tables).
- Good for big data
- Fast and scalable
- Examples: MongoDB, Redis

**Think of it like:** A big storage box where you can put anything in any format.

### 3. Hierarchical Database
Data organized like a family tree.
- Parent-child relationship
- Example: IBM IMS

**Think of it like:** Company organization chart.

### 4. Network Database
Data connected like a web.
- More flexible than hierarchical
- Example: IDS

### 5. Cloud Database
Database stored on the internet.
- Access from anywhere
- Automatic backup
- Examples: Amazon RDS, Google Cloud SQL

---

## Database Languages

### 1. DDL (Data Definition Language)
Creates and modifies database structure.
- CREATE - Make new table
- ALTER - Change table
- DROP - Delete table

### 2. DML (Data Manipulation Language)
Works with data inside tables.
- INSERT - Add data
- UPDATE - Change data
- DELETE - Remove data

### 3. DCL (Data Control Language)
Controls who can access data.
- GRANT - Give permission
- REVOKE - Take away permission

### 4. TCL (Transaction Control Language)
Manages changes to data.
- COMMIT - Save changes
- ROLLBACK - Undo changes

### 5. DQL (Data Query Language)
Gets data from database.
- SELECT - Fetch data

---

## Where is DBMS Used?

| Area | How it's used |
|------|---------------|
| **Banking** | Store account details, transactions |
| **E-commerce** | Track products, orders, customers |
| **Healthcare** | Keep patient records |
| **Education** | Manage student grades, attendance |
| **Social Media** | Store user profiles, posts |

---

## DBMS Architecture Types

### 1-Tier Architecture
Everything on one computer.
- User, database, application - all in one place
- Example: MS Excel
- Good for: Personal use
- Bad for: Multiple users

```
┌─────────────────┐
│   Your Computer │
│  ┌───────────┐  │
│  │ Database  │  │
│  └───────────┘  │
└─────────────────┘
```

### 2-Tier Architecture
Client and Server separated.
- Client: Your computer (front-end)
- Server: Database computer (back-end)
- Example: Library management system
- Good for: Small organizations

```
┌─────────┐         ┌──────────┐
│ Client  │────────▶│  Server  │
│ (You)   │◀────────│(Database)│
└─────────┘         └──────────┘
```

### 3-Tier Architecture
Three separate layers.
- Client: User interface
- Application Server: Business logic
- Database Server: Data storage
- Example: E-commerce website
- Good for: Large applications

```
┌────────┐    ┌────────────┐    ┌──────────┐
│ Client │───▶│Application │───▶│ Database │
│        │◀───│   Server   │◀───│  Server  │
└────────┘    └────────────┘    └──────────┘
```

---

## File System vs DBMS

| Feature | File System | DBMS |
|---------|-------------|------|
| **Data Storage** | Files and folders | Tables |
| **Data Redundancy** | Yes (duplicate data) | No |
| **Security** | Low | High |
| **Multiple Users** | No | Yes |
| **Backup** | Manual | Automatic |
| **Cost** | Cheap | Expensive |
| **Speed** | Slow for big data | Fast |

**Simple Example:**
- **File System:** Like keeping papers in different folders - hard to find, easy to lose
- **DBMS:** Like a library with a catalog - easy to find, well organized

---

## Key Takeaways

- DBMS is software that manages data
- It solves problems of old file systems
- Has 6 main parts: Hardware, Software, Data, Procedures, Language, People
- Different types for different needs (RDBMS, NoSQL, etc.)
- Used everywhere: banks, schools, hospitals, websites
- Three architecture types: 1-Tier, 2-Tier, 3-Tier
- Better than file systems in every way
