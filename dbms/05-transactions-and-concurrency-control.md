# Transactions & Concurrency Control

## What is a Transaction?

A transaction is a set of operations that work together as one unit.

**Simple Definition:** A transaction is like a task that must be completed fully or not at all.

**Example:** Transferring money from Account A to Account B
- Step 1: Deduct $100 from Account A
- Step 2: Add $100 to Account B

Both steps must happen together. If one fails, both should be cancelled.

---

## ACID Properties

ACID ensures transactions are reliable and safe.

| Property | What it means |
|----------|---------------|
| **A**tomicity | All or nothing |
| **C**onsistency | Data stays correct |
| **I**solation | Transactions don't interfere |
| **D**urability | Changes are permanent |

---

### 1. Atomicity (All or Nothing)

Transaction either completes fully or doesn't happen at all.

**Two Outcomes:**
- **Commit:** Transaction succeeds, changes are saved
- **Rollback:** Transaction fails, all changes are undone

**Example:**

Transfer $100 from X to Y:
- X = 500, Y = 200
- Deduct from X: X = 400
- **System crashes here**
- Add to Y: (doesn't happen)

**Without Atomicity:** X = 400, Y = 200 (Money lost!)

**With Atomicity:** Transaction rolls back, X = 500, Y = 200 (Safe!)

---

### 2. Consistency (Data Stays Correct)

Database must follow all rules before and after transaction.

**Rules include:**
- Primary keys must be unique
- Foreign keys must match
- Check constraints must be satisfied
- Total balance should remain same

**Example:**

Before transaction: X = 500, Y = 200, Total = 700
After transaction: X = 400, Y = 300, Total = 700 ✓

If total changes, consistency is violated.

---

### 3. Isolation (Transactions Don't Interfere)

Each transaction runs independently without affecting others.

**Problems Without Isolation:**

| Problem | What happens |
|---------|--------------|
| **Dirty Read** | Reading uncommitted data |
| **Non-Repeatable Read** | Data changes between two reads |
| **Phantom Read** | New rows appear during transaction |

**Example:**

Transaction T1: Transfer $50 from X to Y
- X = 500, Y = 500
- Read Y = 500
- X = 450 (deduct 50)
- Y = 550 (add 50)

Transaction T2: Calculate total
- Reads X = 500, Y = 500
- Total = 1000

**Without Isolation:** T2 might read X = 450, Y = 500 = 950 (Wrong!)

**With Isolation:** T2 waits for T1 to finish, then reads X = 450, Y = 550 = 1000 ✓

---

### 4. Durability (Changes Are Permanent)

Once transaction commits, changes are saved forever, even if system crashes.

**How it works:**
- Changes written to disk (non-volatile storage)
- System can recover after crash
- Committed data never lost

**Example:**

Transfer $100 from A to B
- Transaction completes
- System commits changes
- **Power failure happens**
- After restart: Transfer is still there ✓

---

## ACID Responsibility

| Property | Who maintains it |
|----------|------------------|
| **Atomicity** | Transaction Manager |
| **Consistency** | Application Programmer |
| **Isolation** | Concurrency Control Manager |
| **Durability** | Recovery Manager |

---

## Types of Schedules

Schedule is the order in which operations of transactions are executed.

### 1. Serial Schedule

Transactions execute one after another (no overlap).

**Example:**

```
T1: R(A), W(A), R(B), W(B)
T2: R(A), W(A), R(B), W(B)

Serial Schedule:
T1 completes first, then T2 starts
```

**Advantages:**
- Always consistent
- No conflicts
- Simple

**Disadvantages:**
- Slow (no concurrency)
- Poor performance

---

### 2. Non-Serial Schedule

Transactions execute in interleaved manner (operations mixed).

**Example:**

```
T1: R(A), W(A), R(B), W(B)
T2: R(A), W(A), R(B), W(B)

Non-Serial Schedule:
T1: R(A), W(A)
T2: R(A), W(A)
T1: R(B), W(B)
T2: R(B), W(B)
```

**Advantages:**
- Better performance
- Higher concurrency

**Disadvantages:**
- May cause conflicts
- Needs checking for correctness

---

## Serializable Schedules

Non-serial schedule that gives same result as serial schedule.

### 1. Conflict Serializable

Can be converted to serial schedule by swapping non-conflicting operations.

**Two operations conflict if:**
- They belong to different transactions
- They operate on same data item
- At least one is a write operation

**Example:**

```
T1: R(A), W(A)
T2: R(B), W(B)

Can swap because they work on different data (A and B)
```

---

### 2. View Serializable

Schedule where:
- Transactions read same initial values
- Transactions read values written by same transactions
- Final write is by same transaction

**Note:** Conflict Serializable ⊂ View Serializable

---

## Recoverable Schedules

### 1. Recoverable Schedule

Transaction commits only after all transactions it depends on have committed.

**Rule:** If T2 reads value written by T1, then T2 must commit after T1.

**Example:**

```
T1: W(A)
T2: R(A)
T1: Commit
T2: Commit
```

This is recoverable because T2 commits after T1.

---

### 2. Cascading Schedule

Failure of one transaction causes other transactions to abort.

**Problem:** If T1 aborts, T2 must also abort (because T2 read uncommitted data from T1).

---

### 3. Cascadeless Schedule

Transaction only reads committed data.

**Rule:** T2 can read value written by T1 only after T1 commits.

**Example:**

```
T1: W(A)
T1: Commit
T2: R(A)
T2: Commit
```

**Advantage:** Prevents cascading aborts.

---

### 4. Strict Schedule

Stronger than cascadeless. Transaction cannot read or write data written by another transaction until that transaction commits.

**Example:**

```
T1: W(A)
T1: Commit
T2: R(A), W(A)
T2: Commit
```

**Advantages:**
- Prevents dirty reads
- Prevents dirty writes

---

### 5. Non-Recoverable Schedule

Transaction commits after reading uncommitted data.

**Example:**

```
T1: W(A)
T2: R(A)
T2: Commit
T1: Abort
```

**Problem:** T2 committed using wrong data from T1. This is invalid!

**Note:** Must be avoided.

---

## Schedule Hierarchy

```
Serial ⊂ Strict ⊂ Cascadeless ⊂ Recoverable
```

**Meaning:**
- All serial schedules are strict
- All strict schedules are cascadeless
- All cascadeless schedules are recoverable

---

## Concurrency Control

Managing multiple transactions running at the same time.

**Why Needed:**
- Prevent conflicts
- Maintain data consistency
- Avoid incorrect updates

---

## Concurrency Problems

| Problem | What happens | Example |
|---------|--------------|---------|
| **Dirty Read** | Reading uncommitted data | T2 reads data written by T1 before T1 commits |
| **Lost Update** | One update overwrites another | T1 and T2 both update same data, one is lost |
| **Inconsistent Read** | Data changes between reads | T1 reads X twice, gets different values |

---

## Concurrency Control Protocols

### 1. Lock-Based Protocol

Uses locks to control access to data.

**Types of Locks:**

| Lock Type | What it does | Who can use |
|-----------|--------------|-------------|
| **Shared Lock (S)** | Read only | Multiple transactions |
| **Exclusive Lock (X)** | Read and write | Only one transaction |

**Two-Phase Locking (2PL):**

**Phase 1 (Growing):** Transaction acquires locks, cannot release any

**Phase 2 (Shrinking):** Transaction releases locks, cannot acquire any

**Advantage:** Guarantees serializability

---

### 2. Timestamp-Based Protocol

Each transaction gets a timestamp (start time).

**Rules:**
- Older transaction has priority
- If conflict occurs, younger transaction may be aborted
- No locks needed

**Advantage:** No deadlocks

---

## Deadlock

Deadlock occurs when two or more transactions wait for each other forever.

**Example:**

```
T1: Locks A, wants B
T2: Locks B, wants A

Both wait forever!
```

---

## Conditions for Deadlock

All four must be true:

| Condition | What it means |
|-----------|---------------|
| **Mutual Exclusion** | Only one transaction can hold resource |
| **Hold and Wait** | Transaction holds resource and waits for more |
| **No Preemption** | Resource cannot be forcibly taken |
| **Circular Wait** | T1 waits for T2, T2 waits for T1 |

---

## Handling Deadlocks

### 1. Deadlock Prevention

Prevent deadlock from happening.

**Methods:**
- Access resources in same order
- Use timeouts
- Limit number of resources

---

### 2. Deadlock Avoidance

Check before granting resource if it can cause deadlock.

**Wait-Die Scheme (Non-preemptive):**
- Older transaction waits
- Younger transaction dies (aborted)

**Example:**
- T1 (older) wants resource held by T2 → T1 waits
- T2 (younger) wants resource held by T1 → T2 dies

**Wound-Wait Scheme (Preemptive):**
- Older transaction wounds (kills) younger
- Younger transaction waits

**Example:**
- T1 (older) wants resource held by T2 → T2 is killed
- T2 (younger) wants resource held by T1 → T2 waits

---

### 3. Deadlock Detection

Let deadlock happen, then detect and resolve.

**Method:** Wait-For Graph
- Nodes = Transactions
- Edges = Waiting relationships
- If cycle exists → Deadlock found

**Action:** Abort one transaction to break cycle.

---

## Database Recovery

Restoring database to correct state after failure.

**Types of Failures:**
- System crash
- Power failure
- Disk failure
- Software bugs

---

## Recovery Techniques

### 1. Log-Based Recovery

Keep a log file that records all changes.

**Log contains:**
- Transaction start
- Read/Write operations
- Transaction commit/abort

**Recovery Actions:**

| Action | What it does | When used |
|--------|--------------|-----------|
| **Undo** | Reverse changes | For uncommitted transactions |
| **Redo** | Reapply changes | For committed transactions |

**Example:**

After crash:
- Undo: Transactions that didn't commit
- Redo: Transactions that committed

---

### 2. Checkpoint

Saves current state periodically to speed up recovery.

**What happens at checkpoint:**
- All log records written to disk
- All modified data written to disk
- Checkpoint record added to log

**Advantage:** Recovery starts from last checkpoint, not from beginning.

---

### 3. Shadow Paging

Keeps two versions of database pages:
- **Shadow page:** Original version
- **Current page:** Modified version

**On commit:** Current becomes shadow

**On abort:** Discard current, keep shadow

**Advantage:** No undo/redo needed

**Disadvantage:** Storage overhead

---

### 4. Backup and Restore

Keep backup copies of database.

**Types:**

| Backup Type | What it saves |
|-------------|---------------|
| **Full Backup** | Complete database |
| **Differential Backup** | Changes since last full backup |
| **Transaction Log Backup** | All transactions |

**Recovery:** Restore backup + Apply transaction logs

---

## Key Takeaways

- ACID properties ensure reliable transactions
- Atomicity: All or nothing
- Consistency: Data stays correct
- Isolation: Transactions don't interfere
- Durability: Changes are permanent
- Serial schedules are safe but slow
- Non-serial schedules are fast but need checking
- Recoverable schedules prevent data loss
- Cascadeless schedules prevent cascading aborts
- Strict schedules are strongest
- Concurrency control prevents conflicts
- Locks and timestamps manage concurrent access
- Deadlock occurs when transactions wait forever
- Recovery techniques restore database after failure
- Checkpoints speed up recovery

