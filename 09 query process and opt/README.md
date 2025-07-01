# Query Processing & Optimization

*A detailed guide on SQL query parsing, plan generation, cost estimation, join algorithms, and optimization approaches.*

---

## Table of Contents

1. [Introduction](#introduction)
2. [Parsing & Plan Generation](#parsing--plan-generation)

   * 2.1 Query Parsing and Validation
   * 2.2 Logical Plan Generation
   * 2.3 Physical Plan Generation
3. [Cost Estimation](#cost-estimation)

   * 3.1 Statistics Collection
   * 3.2 Cardinality Estimation
   * 3.3 Cost Models
4. [Join Algorithms](#join-algorithms)

   * 4.1 Nested‑Loop Join
   * 4.2 Sort‑Merge Join
   * 4.3 Hash Join
5. [Heuristics vs. Cost‑based Optimization](#heuristics-vs-cost-based)
6. [Summary](#summary)
7. [References](#references)

---

## Introduction

Query processing and optimization transform an SQL statement into an efficient execution strategy. A relational database system parses the query, generates multiple execution plans, estimates their costs, and selects the best plan based on statistics and cost models.

---

## Parsing & Plan Generation

### 2.1 Query Parsing and Validation

* **Lexical Analysis**: Tokenizes SQL text into keywords, identifiers, literals.
* **Syntax Analysis**: Builds a parse tree according to SQL grammar; rejects invalid syntax.
* **Semantic Analysis**: Checks object existence, data types, privileges, and resolves names.

### 2.2 Logical Plan Generation

* Transforms the parse tree into an algebraic representation (logical query plan).
* Logical operators include **Selection**, **Projection**, **Join**, **Aggregation**, etc.

### 2.3 Physical Plan Generation

* Enumerates possible physical operators for each logical operator (e.g., index scan vs. full table scan).
* Generates alternative execution plans by combining different operator implementations.

---

## Cost Estimation

### 3.1 Statistics Collection

* **Table Statistics**: Number of rows, page counts.
* **Column Statistics**: Distinct values, histograms of value distribution.
* **Index Statistics**: Levels of B‑Tree, clustering factor.

### 3.2 Cardinality Estimation

* Estimates number of rows flowing through each operator based on statistics and predicates.
* Crucial for accurate cost estimation.

### 3.3 Cost Models

* **I/O Cost**: Number of disk pages read/written.
* **CPU Cost**: CPU cycles for comparisons, hashing, sorting.
* **Network Cost**: For distributed databases.
* Total cost = weighted sum of I/O, CPU, and network costs.

---

## Join Algorithms

### 4.1 Nested‑Loop Join

* **Algorithm**: For each row in outer table, scan inner table to find matches.
* **Cost**: O(|R| × |S|) page fetches; can use index on inner to reduce cost.
* **Use Case**: Small tables or highly selective outer predicates.

### 4.2 Sort‑Merge Join

* **Algorithm**: Sort both inputs on join key, then merge by scanning sorted lists.
* **Cost**: Cost of sorting both relations plus linear merge.
* **Use Case**: Both inputs pre-sorted or large inputs requiring sequential access.

### 4.3 Hash Join

* **Algorithm**: Build hash table on smaller relation for join key, then probe with each row of larger relation.
* **Cost**: 2× scan of each relation plus hash table operations.
* **Use Case**: Unsorted inputs; hash table fits in memory.

---

## Heuristics vs. Cost‑based Optimization

| Aspect         | Heuristics                                | Cost‑based                                   |
| -------------- | ----------------------------------------- | -------------------------------------------- |
| Decision Basis | Rule-based (e.g., push selections down)   | Cost model and statistics                    |
| Plan Quality   | Good, but not guaranteed optimal          | Aims for lowest estimated cost               |
| Overhead       | Low planning time                         | Higher planning time due to enumeration      |
| Examples       | Predicate pushdown, join order heuristics | Dynamic programming, branch-and-bound search |

**Hybrid Approaches**:

* Many systems use heuristic pruning to reduce search space, then apply cost estimation on remaining plans.

---

## Summary

Effective query processing relies on thorough parsing, robust cost models, and the selection of optimal join algorithms. Balancing heuristics and cost-based techniques ensures efficient execution in diverse workloads.

---

## References

1. Surajit Chaudhuri, *An Overview of Query Optimization in Relational Systems*.
2. Goetz Graefe, *Query Evaluation Techniques for Large Databases*.
3. PostgreSQL Documentation: Query Planning.
4. H. Garcia-Molina, J. Ullman, & J. Widom, *Database Systems: The Complete Book*.
