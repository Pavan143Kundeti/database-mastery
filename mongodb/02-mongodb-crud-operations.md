# MongoDB CRUD Operations

## What is CRUD?

**CRUD** stands for:
- **C**reate - Add new data
- **R**ead - View/find data
- **U**pdate - Modify existing data
- **D**elete - Remove data

**Simple Definition:** CRUD is the 4 basic operations you do with any database.

---

## Important Terms (Quick Reference)

| Term | What it means | SQL Equivalent |
|------|---------------|----------------|
| **Database** | Container for collections | Database |
| **Collection** | Group of documents | Table |
| **Document** | Single record (JSON format) | Row |
| **Field** | Single piece of data | Column |
| **_id** | Unique identifier (automatic) | Primary Key |

**Example Document:**
```json
{
  "_id": "507f1f77bcf86cd799439011",
  "title": "My Post",
  "author": "John",
  "likes": 10
}
```

**In this document:**
- `_id`, `title`, `author`, `likes` are **fields**
- The whole thing is a **document**
- It's stored in a **collection** (like "posts")
- The collection is inside a **database** (like "blog")

**Note:** For detailed explanations, see the first file: Introduction to MongoDB

---

## Before You Start

**Make sure:**
1. You're connected to MongoDB (see previous chapter)
2. Your terminal shows the prompt: `test>` or `myFirstDatabase>`

**If not connected:**
```bash
mongosh "your-connection-string" --username your-username
```

---

# Create Operations

## 1. Create Database

### Command: `use`

**Syntax:**
```javascript
use database_name
```

**Where to write:** In mongosh terminal after the `>` prompt

**How to execute:** Type the command and press Enter

---

### Example 1: Create a blog database

**Command:**
```javascript
use blog
```

**Result:**
```
switched to db blog
```

**What happened:** You created (or switched to) a database called "blog"

**Your prompt changes to:**
```
blog>
```

---

### Example 2: Create a shop database

**Command:**
```javascript
use shop
```

**Result:**
```
switched to db shop
```

**Prompt changes to:**
```
shop>
```

---

### Important Notes

| Point | Explanation |
|-------|-------------|
| **Database not created yet** | Database only appears when you add data |
| **Check with `show dbs`** | Empty databases won't show in the list |
| **No error if exists** | If database exists, it just switches to it |

---

## 2. Create Collection

### Two Ways to Create Collection:

| Method | When to Use |
|--------|-------------|
| **Automatic** | Collection created when you insert first document |
| **Manual** | Use `db.createCollection()` to create empty collection |

---

### Method 1: Automatic (Recommended)

**You don't need to create collection manually!** It's created automatically when you insert data.

**Example:**
```javascript
db.posts.insertOne({title: "Hello"})
```

**What happens:** Collection "posts" is created automatically

---

### Method 2: Manual

**Syntax:**
```javascript
db.createCollection("collection_name")
```

**Where to write:** In mongosh terminal

**How to execute:** Type and press Enter

---

### Example: Create posts collection

**Command:**
```javascript
db.createCollection("posts")
```

**Result:**
```
{ ok: 1 }
```

**What it means:** Collection created successfully

---

### Check Collections

**Command:**
```javascript
show collections
```

**Result:**
```
posts
```

**What it means:** You can see all collections in current database

---

## 3. Insert Documents (Add Data)

### Three Methods:

| Method | What it does |
|--------|--------------|
| **insertOne()** | Insert one document |
| **insertMany()** | Insert multiple documents |
| **insert()** | Insert one or many (old method) |

---

### Method 1: insertOne()

**Syntax:**
```javascript
db.collection_name.insertOne({
  field1: value1,
  field2: value2
})
```

**Where to write:** In mongosh terminal

**How to execute:** Type and press Enter

---

### Example 1: Insert one post

**Command:**
```javascript
db.posts.insertOne({
  title: "My First Post",
  author: "John",
  likes: 10
})
```

**Result:**
```
{
  acknowledged: true,
  insertedId: ObjectId("507f1f77bcf86cd799439011")
}
```

**What it means:**
- `acknowledged: true` - Data inserted successfully
- `insertedId` - Unique ID MongoDB gave to this document

---

### Example 2: Insert with more fields

**Command:**
```javascript
db.posts.insertOne({
  title: "Learning MongoDB",
  author: "Sarah",
  content: "MongoDB is easy to learn",
  likes: 25,
  published: true,
  tags: ["database", "mongodb", "tutorial"]
})
```

**Result:**
```
{
  acknowledged: true,
  insertedId: ObjectId("507f1f77bcf86cd799439012")
}
```

---

### Method 2: insertMany()

**Syntax:**
```javascript
db.collection_name.insertMany([
  {document1},
  {document2},
  {document3}
])
```

**Note:** Use square brackets `[]` to wrap multiple documents

---

### Example: Insert multiple posts

**Command:**
```javascript
db.posts.insertMany([
  {
    title: "Post 1",
    author: "John",
    likes: 5
  },
  {
    title: "Post 2",
    author: "Sarah",
    likes: 15
  },
  {
    title: "Post 3",
    author: "Mike",
    likes: 20
  }
])
```

**Result:**
```
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("507f1f77bcf86cd799439013"),
    '1': ObjectId("507f1f77bcf86cd799439014"),
    '2': ObjectId("507f1f77bcf86cd799439015")
  }
}
```

**What it means:** All 3 documents inserted successfully with their unique IDs

---

### Insert Data Types

| Data Type | Example |
|-----------|---------|
| **String** | `"Hello"` |
| **Number** | `25` or `25.5` |
| **Boolean** | `true` or `false` |
| **Array** | `["tag1", "tag2"]` |
| **Object** | `{name: "John", age: 30}` |
| **Date** | `new Date()` |
| **Null** | `null` |

---

### Example: Insert with different data types

**Command:**
```javascript
db.users.insertOne({
  name: "John Doe",
  age: 30,
  email: "john@example.com",
  active: true,
  hobbies: ["reading", "coding"],
  address: {
    city: "New York",
    country: "USA"
  },
  joinDate: new Date(),
  middleName: null
})
```

---

# Read Operations (Find Data)

## 1. Find All Documents

### Command: `find()`

**Syntax:**
```javascript
db.collection_name.find()
```

**Where to write:** In mongosh terminal

**How to execute:** Type and press Enter

---

### Example: Find all posts

**Command:**
```javascript
db.posts.find()
```

**Result:**
```
[
  {
    _id: ObjectId("507f1f77bcf86cd799439011"),
    title: "My First Post",
    author: "John",
    likes: 10
  },
  {
    _id: ObjectId("507f1f77bcf86cd799439012"),
    title: "Learning MongoDB",
    author: "Sarah",
    likes: 25
  }
]
```

**What it means:** Shows all documents in the posts collection

---

## 2. Find with Pretty Format

### Command: `find().pretty()`

**Note:** In newer versions of mongosh, `find()` already shows pretty format automatically!

**Syntax:**
```javascript
db.collection_name.find().pretty()
```

---

## 3. Find One Document

### Command: `findOne()`

**Syntax:**
```javascript
db.collection_name.findOne()
```

**What it does:** Returns only the first document

---

### Example: Find one post

**Command:**
```javascript
db.posts.findOne()
```

**Result:**
```
{
  _id: ObjectId("507f1f77bcf86cd799439011"),
  title: "My First Post",
  author: "John",
  likes: 10
}
```

---

## 4. Find with Filter (Condition)

### Syntax:
```javascript
db.collection_name.find({field: value})
```

**What it does:** Find documents where field equals value

---

### Example 1: Find posts by author

**Command:**
```javascript
db.posts.find({author: "John"})
```

**Result:**
```
[
  {
    _id: ObjectId("507f1f77bcf86cd799439011"),
    title: "My First Post",
    author: "John",
    likes: 10
  }
]
```

**What it means:** Shows only posts where author is "John"

---

### Example 2: Find posts with specific likes

**Command:**
```javascript
db.posts.find({likes: 25})
```

**Result:**
```
[
  {
    _id: ObjectId("507f1f77bcf86cd799439012"),
    title: "Learning MongoDB",
    author: "Sarah",
    likes: 25
  }
]
```

---

### Example 3: Find with multiple conditions

**Command:**
```javascript
db.posts.find({
  author: "John",
  likes: 10
})
```

**What it means:** Find posts where author is "John" AND likes is 10

---

## 5. Find Specific Fields (Projection)

### Syntax:
```javascript
db.collection_name.find(
  {filter},
  {field1: 1, field2: 1}
)
```

**What it means:**
- `1` means include this field
- `0` means exclude this field

---

### Example 1: Show only title and author

**Command:**
```javascript
db.posts.find(
  {},
  {title: 1, author: 1}
)
```

**Result:**
```
[
  {
    _id: ObjectId("507f1f77bcf86cd799439011"),
    title: "My First Post",
    author: "John"
  },
  {
    _id: ObjectId("507f1f77bcf86cd799439012"),
    title: "Learning MongoDB",
    author: "Sarah"
  }
]
```

**Note:** `_id` is always included unless you explicitly exclude it

---

### Example 2: Exclude _id field

**Command:**
```javascript
db.posts.find(
  {},
  {title: 1, author: 1, _id: 0}
)
```

**Result:**
```
[
  {
    title: "My First Post",
    author: "John"
  },
  {
    title: "Learning MongoDB",
    author: "Sarah"
  }
]
```

---

## 6. Count Documents

### Command: `countDocuments()`

**Syntax:**
```javascript
db.collection_name.countDocuments()
```

---

### Example 1: Count all posts

**Command:**
```javascript
db.posts.countDocuments()
```

**Result:**
```
5
```

**What it means:** There are 5 documents in posts collection

---

### Example 2: Count with filter

**Command:**
```javascript
db.posts.countDocuments({author: "John"})
```

**Result:**
```
2
```

**What it means:** There are 2 posts by John

---

## 7. Limit Results

### Command: `limit()`

**Syntax:**
```javascript
db.collection_name.find().limit(number)
```

---

### Example: Show only 2 posts

**Command:**
```javascript
db.posts.find().limit(2)
```

**Result:**
```
[
  {
    _id: ObjectId("507f1f77bcf86cd799439011"),
    title: "My First Post",
    author: "John",
    likes: 10
  },
  {
    _id: ObjectId("507f1f77bcf86cd799439012"),
    title: "Learning MongoDB",
    author: "Sarah",
    likes: 25
  }
]
```

---

## 8. Sort Results

### Command: `sort()`

**Syntax:**
```javascript
db.collection_name.find().sort({field: 1})
```

**What it means:**
- `1` = Ascending order (A to Z, 0 to 9)
- `-1` = Descending order (Z to A, 9 to 0)

---

### Example 1: Sort by likes (ascending)

**Command:**
```javascript
db.posts.find().sort({likes: 1})
```

**Result:** Posts sorted from lowest to highest likes

---

### Example 2: Sort by likes (descending)

**Command:**
```javascript
db.posts.find().sort({likes: -1})
```

**Result:** Posts sorted from highest to lowest likes

---

### Example 3: Sort by title alphabetically

**Command:**
```javascript
db.posts.find().sort({title: 1})
```

**Result:** Posts sorted A to Z by title

---

# Update Operations

## 1. Update One Document

### Command: `updateOne()`

**Syntax:**
```javascript
db.collection_name.updateOne(
  {filter},
  {$set: {field: new_value}}
)
```

**Where to write:** In mongosh terminal

**How to execute:** Type and press Enter

---

### Example 1: Update likes

**Command:**
```javascript
db.posts.updateOne(
  {title: "My First Post"},
  {$set: {likes: 50}}
)
```

**Result:**
```
{
  acknowledged: true,
  matchedCount: 1,
  modifiedCount: 1
}
```

**What it means:**
- `matchedCount: 1` - Found 1 document matching the filter
- `modifiedCount: 1` - Updated 1 document

---

### Example 2: Update multiple fields

**Command:**
```javascript
db.posts.updateOne(
  {title: "My First Post"},
  {$set: {
    likes: 100,
    author: "John Doe",
    published: true
  }}
)
```

---

### Example 3: Add new field

**Command:**
```javascript
db.posts.updateOne(
  {title: "My First Post"},
  {$set: {category: "Tutorial"}}
)
```

**What happens:** Adds a new field "category" to the document

---

## 2. Update Many Documents

### Command: `updateMany()`

**Syntax:**
```javascript
db.collection_name.updateMany(
  {filter},
  {$set: {field: new_value}}
)
```

---

### Example: Update all posts by John

**Command:**
```javascript
db.posts.updateMany(
  {author: "John"},
  {$set: {verified: true}}
)
```

**Result:**
```
{
  acknowledged: true,
  matchedCount: 3,
  modifiedCount: 3
}
```

**What it means:** Updated 3 documents where author is "John"

---

## 3. Replace Document

### Command: `replaceOne()`

**Syntax:**
```javascript
db.collection_name.replaceOne(
  {filter},
  {new_document}
)
```

**Warning:** This replaces the entire document (except _id)

---

### Example: Replace entire document

**Command:**
```javascript
db.posts.replaceOne(
  {title: "My First Post"},
  {
    title: "Updated Post",
    author: "Jane",
    likes: 0
  }
)
```

**What happens:** Old document completely replaced with new one

---

## Update Operators

| Operator | What it does | Example |
|----------|--------------|---------|
| **$set** | Set field value | `{$set: {likes: 10}}` |
| **$inc** | Increase number | `{$inc: {likes: 1}}` |
| **$mul** | Multiply number | `{$mul: {likes: 2}}` |
| **$rename** | Rename field | `{$rename: {likes: "reactions"}}` |
| **$unset** | Remove field | `{$unset: {likes: ""}}` |
| **$min** | Update if new value is less | `{$min: {likes: 5}}` |
| **$max** | Update if new value is more | `{$max: {likes: 100}}` |

---

### Example 1: Increase likes by 1

**Command:**
```javascript
db.posts.updateOne(
  {title: "My First Post"},
  {$inc: {likes: 1}}
)
```

**What happens:** Likes increased by 1 (10 becomes 11)

---

### Example 2: Multiply likes by 2

**Command:**
```javascript
db.posts.updateOne(
  {title: "My First Post"},
  {$mul: {likes: 2}}
)
```

**What happens:** Likes multiplied by 2 (10 becomes 20)

---

### Example 3: Remove a field

**Command:**
```javascript
db.posts.updateOne(
  {title: "My First Post"},
  {$unset: {published: ""}}
)
```

**What happens:** "published" field removed from document

---

# Delete Operations

## 1. Delete One Document

### Command: `deleteOne()`

**Syntax:**
```javascript
db.collection_name.deleteOne({filter})
```

**Where to write:** In mongosh terminal

**How to execute:** Type and press Enter

---

### Example: Delete one post

**Command:**
```javascript
db.posts.deleteOne({title: "My First Post"})
```

**Result:**
```
{
  acknowledged: true,
  deletedCount: 1
}
```

**What it means:** 1 document deleted successfully

---

## 2. Delete Many Documents

### Command: `deleteMany()`

**Syntax:**
```javascript
db.collection_name.deleteMany({filter})
```

---

### Example 1: Delete all posts by John

**Command:**
```javascript
db.posts.deleteMany({author: "John"})
```

**Result:**
```
{
  acknowledged: true,
  deletedCount: 3
}
```

**What it means:** 3 documents deleted

---

### Example 2: Delete all documents

**Command:**
```javascript
db.posts.deleteMany({})
```

**Warning:** This deletes ALL documents in the collection!

**Result:**
```
{
  acknowledged: true,
  deletedCount: 10
}
```

---

## 3. Drop Collection

### Command: `drop()`

**Syntax:**
```javascript
db.collection_name.drop()
```

**What it does:** Deletes the entire collection

---

### Example: Drop posts collection

**Command:**
```javascript
db.posts.drop()
```

**Result:**
```
true
```

**What it means:** Collection deleted successfully

---

## 4. Drop Database

### Command: `dropDatabase()`

**Syntax:**
```javascript
db.dropDatabase()
```

**What it does:** Deletes the current database

---

### Example: Drop current database

**Step 1:** Make sure you're in the right database
```javascript
db
```

**Step 2:** Drop it
```javascript
db.dropDatabase()
```

**Result:**
```
{ ok: 1, dropped: 'blog' }
```

**What it means:** Database "blog" deleted successfully

---

# Quick Reference

## Create Operations

| Command | What it does |
|---------|--------------|
| `use db_name` | Create/switch database |
| `db.createCollection("name")` | Create collection |
| `db.coll.insertOne({})` | Insert one document |
| `db.coll.insertMany([{}, {}])` | Insert many documents |

---

## Read Operations

| Command | What it does |
|---------|--------------|
| `db.coll.find()` | Find all documents |
| `db.coll.findOne()` | Find one document |
| `db.coll.find({field: value})` | Find with filter |
| `db.coll.find({}, {field: 1})` | Find specific fields |
| `db.coll.countDocuments()` | Count documents |
| `db.coll.find().limit(5)` | Limit results |
| `db.coll.find().sort({field: 1})` | Sort results |

---

## Update Operations

| Command | What it does |
|---------|--------------|
| `db.coll.updateOne({}, {$set: {}})` | Update one document |
| `db.coll.updateMany({}, {$set: {}})` | Update many documents |
| `db.coll.replaceOne({}, {})` | Replace entire document |

---

## Delete Operations

| Command | What it does |
|---------|--------------|
| `db.coll.deleteOne({})` | Delete one document |
| `db.coll.deleteMany({})` | Delete many documents |
| `db.coll.drop()` | Drop collection |
| `db.dropDatabase()` | Drop database |

---

## Common Questions

### Q1: What happens if I insert without creating collection?

**Answer:** Collection is created automatically. No need to create it first!

---

### Q2: Can I update a field that doesn't exist?

**Answer:** Yes! Using `$set` will add the field if it doesn't exist.

---

### Q3: What's the difference between updateOne and updateMany?

**Answer:**
- `updateOne()` - Updates only the first matching document
- `updateMany()` - Updates all matching documents

---

### Q4: How do I delete all documents but keep the collection?

**Answer:** Use `db.collection.deleteMany({})`

---

### Q5: What's the difference between deleteMany({}) and drop()?

**Answer:**
- `deleteMany({})` - Deletes all documents, collection still exists
- `drop()` - Deletes the entire collection

---

### Q6: Can I undo a delete operation?

**Answer:** No! There's no undo in MongoDB. Always be careful with delete operations.

---

## Practice Exercise

Try these commands in order:

```javascript
// 1. Create database
use practice

// 2. Insert one document
db.students.insertOne({
  name: "Alice",
  age: 20,
  grade: "A"
})

// 3. Insert many documents
db.students.insertMany([
  {name: "Bob", age: 22, grade: "B"},
  {name: "Charlie", age: 21, grade: "A"},
  {name: "David", age: 23, grade: "C"}
])

// 4. Find all students
db.students.find()

// 5. Find students with grade A
db.students.find({grade: "A"})

// 6. Count total students
db.students.countDocuments()

// 7. Update Bob's grade
db.students.updateOne(
  {name: "Bob"},
  {$set: {grade: "A"}}
)

// 8. Increase all ages by 1
db.students.updateMany(
  {},
  {$inc: {age: 1}}
)

// 9. Delete one student
db.students.deleteOne({name: "David"})

// 10. Find all students again
db.students.find()
```

---

## Key Takeaways

- CRUD = Create, Read, Update, Delete
- Collections are created automatically when you insert data
- `find()` returns all documents, `findOne()` returns first document
- Use `$set` to update fields
- Use `$inc` to increase numbers
- `deleteOne()` deletes first match, `deleteMany()` deletes all matches
- Always double-check before deleting data
- Empty filter `{}` means "all documents"

---

**Next:** Learn about Query Operators and Aggregation Pipelines!
