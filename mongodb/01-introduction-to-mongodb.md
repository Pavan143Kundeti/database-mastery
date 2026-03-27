# Introduction to MongoDB

## What is MongoDB?

MongoDB is a document database that stores data in flexible documents instead of tables.

**Simple Definition:** MongoDB is a NoSQL database that stores data like JSON objects, making it easy to work with.

---

## SQL vs MongoDB

### SQL Databases (Relational)

**How they work:**
- Store data in separate tables
- Need to join tables to get complete data
- Fixed structure (schema)

**Example:**
```
Customers Table    Orders Table
+----+------+     +----+------------+
| ID | Name |     | ID | CustomerID |
+----+------+     +----+------------+
| 1  | John |     | 1  | 1          |
+----+------+     +----+------------+
```

---

### MongoDB (Document Database)

**How it works:**
- Store all related data together in one document
- No need to join data
- Flexible structure

**Example:**
```json
{
  "id": 1,
  "name": "John",
  "orders": [
    { "id": 1, "product": "Laptop" }
  ]
}
```

---

## Key Differences

| Feature | SQL Database | MongoDB |
|---------|--------------|---------|
| **Data Storage** | Tables (rows & columns) | Documents (JSON-like) |
| **Structure** | Fixed schema | Flexible schema |
| **Terminology** | Tables | Collections |
| **Terminology** | Rows | Documents |
| **Terminology** | Columns | Fields |
| **Relationships** | Join tables | Embed data |
| **Best For** | Complex transactions | Flexible data |

---

## MongoDB Terminology

| SQL Term | MongoDB Term | What it means |
|----------|--------------|---------------|
| **Database** | Database | Container for collections |
| **Table** | Collection | Group of documents |
| **Row** | Document | Single record |
| **Column** | Field | Single piece of data |

---

## Why Use MongoDB?

| Benefit | Description |
|---------|-------------|
| **Fast Reading** | All related data in one place |
| **Flexible** | Can change structure anytime |
| **Easy to Scale** | Handles large amounts of data |
| **JSON-like** | Easy for developers to use |
| **No Complex Joins** | Data already together |

---

## Local vs Cloud Database

### Local Database

**What it means:** Install MongoDB on your own computer.

**Pros:**
- Full control
- Free to use
- No internet needed

**Cons:**
- You manage everything
- You handle updates
- You handle backups

---

### Cloud Database (MongoDB Atlas)

**What it means:** MongoDB hosted on the internet.

**Pros:**
- Easy to set up
- Automatic backups
- Automatic updates
- Access from anywhere

**Cons:**
- Need internet connection
- Free tier has limits

**We'll use MongoDB Atlas in this tutorial because it's easier!**

---

# Setting Up MongoDB Atlas

## Step 1: Create Account

**What to do:**
1. Go to [MongoDB Atlas website](https://www.mongodb.com/cloud/atlas)
2. Click "Try Free" or "Sign Up"
3. Enter your email and password
4. Verify your email

**Time needed:** 2-3 minutes

---

## Step 2: Create a Cluster

**What is a Cluster?** A cluster is your database server in the cloud.

### Steps:

**1. Choose Plan:**
- Select "Shared" (Free tier)
- Click "Create"

**2. Choose Provider:**
- AWS, Google Cloud, or Azure (any is fine)
- Choose region closest to you
- Click "Create Cluster"

**3. Wait:**
- Cluster creation takes 3-5 minutes
- You'll see a progress bar

---

## Step 3: Set Up Database Access (Create User)

**Why?** You need a username and password to connect to your database.

### Steps:

**1. Go to Database Access:**
- Click "Database Access" in left menu
- Click "Add New Database User"

**2. Create User:**
- Choose "Password" authentication
- Enter username: `myuser` (or any name you want)
- Enter password: `mypassword123` (or any password)
- **Write these down! You'll need them later!**

**3. Set Privileges:**
- Select "Read and write to any database"
- Click "Add User"

---

## Step 4: Set Up Network Access (Add IP Address)

**Why?** MongoDB Atlas blocks all connections by default for security.

### Steps:

**1. Go to Network Access:**
- Click "Network Access" in left menu
- Click "Add IP Address"

**2. Allow Your Computer:**
- Click "Add Current IP Address" (easiest option)
- OR click "Allow Access from Anywhere" (0.0.0.0/0) for testing

**3. Confirm:**
- Click "Confirm"
- Wait for status to become "Active" (green)

---

## Step 5: Install MongoDB Shell (mongosh)

**What is mongosh?** A command-line tool to interact with MongoDB.

### Installation Steps:

**Windows:**
1. Go to [MongoDB Shell Download](https://www.mongodb.com/try/download/shell)
2. Download Windows version
3. Run the installer
4. Follow installation wizard

**Mac:**
```bash
brew install mongosh
```

**Linux:**
```bash
# Follow official instructions for your distribution
```

---

### Verify Installation

**Open your terminal and type:**
```bash
mongosh --version
```

**You should see:**
```
1.3.1
```
(or whatever the latest version is)

**If you see a version number, installation successful!**

---

## Step 6: Get Connection String

**What is a Connection String?** A special URL that tells mongosh how to connect to your database.

### Steps:

**1. Go to Database:**
- Click "Database" in left menu
- Click "Connect" button on your cluster

**2. Choose Connection Method:**
- Click "Connect with the MongoDB Shell"

**3. Copy Connection String:**
- You'll see something like this:

```
mongosh "mongodb+srv://cluster0.ex4ht.mongodb.net/myFirstDatabase" --apiVersion 1 --username YOUR_USER_NAME
```

**4. Save it somewhere!**

---

## Step 7: Connect to Database

**Now let's connect to your database!**

### Steps:

**1. Open Terminal:**
- Windows: Command Prompt or PowerShell
- Mac/Linux: Terminal

**2. Paste Connection String:**
- Paste the connection string you copied
- Replace `YOUR_USER_NAME` with your actual username
- Press Enter

**Example:**
```bash
mongosh "mongodb+srv://cluster0.ex4ht.mongodb.net/myFirstDatabase" --apiVersion 1 --username myuser
```

**3. Enter Password:**
- You'll be prompted: `Enter password:`
- Type your password (you won't see it as you type)
- Press Enter

**4. Connected!**

You should see:
```
Current Mongosh Log ID: 507f1f77bcf86cd799439011
Connecting to: mongodb+srv://cluster0.ex4ht.mongodb.net/myFirstDatabase
Using MongoDB: 5.0.0
Using Mongosh: 1.3.1

myFirstDatabase>
```

**The `myFirstDatabase>` prompt means you're connected!**

---

## Understanding the Prompt

```
myFirstDatabase>
```

| Part | Meaning |
|------|---------|
| `myFirstDatabase` | Current database name |
| `>` | Ready for your command |

---

## Basic Commands to Try

### Check Current Database

**Command:**
```javascript
db
```

**Result:**
```
myFirstDatabase
```

**What it means:** You're currently in the "myFirstDatabase" database.

---

### Show All Databases

**Command:**
```javascript
show dbs
```

**Result:**
```
admin   40.00 KiB
config  12.00 KiB
local   72.00 KiB
```

**What it means:** These are the default databases. Your "myFirstDatabase" isn't shown because it's empty.

**Important:** Empty databases don't appear in the list!

---

### Create or Switch Database

**Command:**
```javascript
use blog
```

**Result:**
```
switched to db blog
```

**What happened:** You created (or switched to) a database called "blog".

**Your prompt changes to:**
```
blog>
```

---

## MongoDB Query API

**What is it?** The way you interact with your data in MongoDB.

### Two Main Ways:

| Method | What it does |
|--------|--------------|
| **CRUD Operations** | Create, Read, Update, Delete data |
| **Aggregation Pipelines** | Complex data analysis and transformations |

---

### What You Can Do:

| Operation | Description |
|-----------|-------------|
| **Queries** | Find and filter data |
| **Transformations** | Change data format |
| **Joins** | Combine data from different collections |
| **Search** | Full-text search |
| **Indexing** | Speed up queries |
| **Analysis** | Time series, graphs, geospatial |

---

## Key Concepts to Remember

### 1. Database

**What it is:** Container for collections.

**Example:** `blog`, `shop`, `users`

---

### 2. Collection

**What it is:** Group of documents (like a table in SQL).

**Example:** `posts`, `products`, `customers`

---

### 3. Document

**What it is:** Single record in JSON format (like a row in SQL).

**Example:**
```json
{
  "_id": "507f1f77bcf86cd799439011",
  "title": "My First Post",
  "author": "John",
  "likes": 10
}
```

---

### 4. Field

**What it is:** Single piece of data (like a column in SQL).

**Example:** `title`, `author`, `likes`

---

## Important Rules

| Rule | Description |
|------|-------------|
| **Empty databases don't exist** | Database created when you add data |
| **Empty collections don't exist** | Collection created when you add documents |
| **_id field is automatic** | MongoDB adds unique ID to every document |
| **Flexible schema** | Documents in same collection can have different fields |

---

## Common Questions

### Q1: Do I need to create a database first?

**Answer:** No! Just use `use database_name` and it will be created when you add data.

---

### Q2: Do I need to create a collection first?

**Answer:** No! It will be created automatically when you insert a document.

---

### Q3: Can I change the structure later?

**Answer:** Yes! MongoDB is flexible. You can add or remove fields anytime.

---

### Q4: What is the _id field?

**Answer:** A unique identifier MongoDB automatically adds to every document. Like a primary key in SQL.

---

### Q5: Can I use MongoDB without internet?

**Answer:** Yes, if you install MongoDB locally. But MongoDB Atlas requires internet.

---

### Q6: Is MongoDB free?

**Answer:** Yes! MongoDB Community Edition is free. MongoDB Atlas has a free tier.

---

## What's Next?

Now that you're connected, you'll learn to:

1. **Create** databases and collections
2. **Insert** documents (add data)
3. **Find** documents (read data)
4. **Update** documents (modify data)
5. **Delete** documents (remove data)
6. **Query** with operators
7. **Aggregate** data for analysis

---

## Quick Reference

### Connection

```bash
# Connect to MongoDB Atlas
mongosh "your-connection-string" --username your-username

# Check current database
db

# Show all databases
show dbs

# Switch/create database
use database_name

# Show collections in current database
show collections
```

---

## Troubleshooting

### Problem: Can't connect

**Solutions:**
1. Check your internet connection
2. Verify username and password
3. Check if IP address is whitelisted
4. Make sure cluster is running (not paused)

---

### Problem: "myFirstDatabase" doesn't show in `show dbs`

**Solution:** This is normal! Empty databases don't appear. Add some data and it will show up.

---

### Problem: Forgot password

**Solution:**
1. Go to MongoDB Atlas dashboard
2. Click "Database Access"
3. Click "Edit" on your user
4. Set new password

---

## Key Takeaways

- MongoDB stores data in flexible documents (JSON-like)
- Collections are like tables, documents are like rows
- No need to define structure beforehand
- MongoDB Atlas is cloud-hosted (easier than local)
- mongosh is the command-line tool to interact with MongoDB
- Empty databases and collections don't appear until they have data
- Every document gets an automatic _id field
- Use `use database_name` to create or switch databases

