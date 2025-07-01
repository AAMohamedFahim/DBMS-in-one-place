# Transaction Management

*A detailed guide on transaction fundamentals, ACID properties, transaction states & lifecycle, scheduling, and serializability concepts.*

---

## Table of Contents

1. [Introduction](#introduction)
2. [ACID Properties](#acid-properties)

   * 2.1 Atomicity
   * 2.2 Consistency
   * 2.3 Isolation
   * 2.4 Durability
3. [Transaction States & Lifecycle](#transaction-states-lifecycle)

   * 3.1 Active
   * 3.2 Partially Committed
   * 3.3 Failed
   * 3.4 Aborted
   * 3.5 Committed
4. [Schedules & Serializability](#schedules-serializability)

   * 4.1 Schedule Definitions (Serial, Non-serial)
   * 4.2 Conflict Serializability
   * 4.3 View Serializability
5. [Conflict vs. View Serializability](#conflict-vs-view-serializability)

   * 5.1 Conflict Serializability Criteria
   * 5.2 View Serializability Criteria
   * 5.3 Comparison and Use Cases
6. [Summary](#summary)
7. [References](#references)

---

## Introduction

Transactions are sequences of database operations that must execute reliably to maintain data integrity. Transaction management ensures that concurrent execution and system failures do not compromise correctness.

---

## ACID Properties

Transactions must satisfy the following properties:

### 2.1 Atomicity

* **Definition**: All operations within a transaction are treated as a single unit; either all succeed, or none are applied.
* **Mechanism**: Use of logs and two-phase commit protocols to rollback on failure.

### 2.2 Consistency

* **Definition**: A transaction transforms the database from one valid state to another, preserving all defined rules (constraints, triggers).
* **Example**: Account balances cannot go negative after a transfer.

### 2.3 Isolation

* **Definition**: Concurrent transactions should not interfere; intermediate states must be invisible to others.
* **Isolation Levels**: Read Uncommitted, Read Committed, Repeatable Read, Serializable.
* **Phenomena**: Dirty reads, non-repeatable reads, phantom reads.

### 2.4 Durability

* **Definition**: Once a transaction is committed, its effects persist, even in case of system crashes.
* **Mechanism**: Write-ahead logging, persistent storage of commit records.

---

## Transaction States & Lifecycle

A transaction progresses through these states:

### 3.1 Active

* Transaction is executing operations.

### 3.2 Partially Committed

* Final operation executed; commit actions are pending.

### 3.3 Failed

* An error or integrity violation occurs.

### 3.4 Aborted

* Effects rolled back; transaction may restart or terminate.

### 3.5 Committed

* All changes are permanently applied; transaction ends successfully.

State transitions follow: Active → Partially Committed → Committed OR Active → Failed → Aborted.

---

## Schedules & Serializability

### 4.1 Schedule Definitions

* **Serial Schedule**: Transactions execute sequentially, one after another.
* **Non-serial Schedule**: Operations of multiple transactions interleave.

### 4.2 Conflict Serializability

* **Definition**: A schedule is conflict-serializable if it can be transformed into a serial schedule by swapping non-conflicting operations.
* **Conflict**: Two operations conflict if they belong to different transactions, operate on the same data item, and at least one is a write.
* **Precedence Graph**: Nodes for transactions; directed edge T\_i → T\_j if T\_i has an operation preceding and conflicting with T\_j's.
* **Criterion**: A schedule is conflict-serializable if and only if its precedence graph is acyclic.

### 4.3 View Serializability

* **Definition**: Two schedules are view-equivalent if they read and write the same data values, preserving initial reads and final writes.
* **Criterion**: A schedule is view-serializable if it is view-equivalent to a serial schedule.

---

## Conflict vs. View Serializability

### 5.1 Conflict Serializability Criteria

* Based solely on pairwise conflicts and precedence graph acyclicity.
* **Strength**: Easier to test; most commonly used.

### 5.2 View Serializability Criteria

* Considers read-from relationships and final writes; more general than conflict.
* **Complexity**: NP-hard to test in general.

### 5.3 Comparison and Use Cases

| Aspect                   | Conflict Serializability       | View Serializability           |
| ------------------------ | ------------------------------ | ------------------------------ |
| Test Complexity          | Polynomial (graph algorithms)  | NP-hard                        |
| Generality               | Subset of view-serializable    | Superset (captures more cases) |
| Practical Implementation | Widely used in DBMS schedulers | Rarely directly implemented    |

---

## Summary

This guide outlined transaction management fundamentals: ACID properties, transaction lifecycle states, schedule types, and the concepts of conflict vs. view serializability. Mastery ensures reliable, concurrent database operations.

---

## References

1. Jim Gray & Andreas Reuter, *Transaction Processing: Concepts and Techniques*.
2. Abiteboul, Hull & Vianu, *Foundations of Databases*.
3. H. Garcia-Molina, J. Ullman, & J. Widom, *Database Systems: The Complete Book*.
4. Gray et al., “The Transaction Concept” in VLDB Proceedings.
