# Storage & Indexing

*An in-depth guide on file organization methods, index structures (B‑Tree, Hash, Bitmap), clustered vs. non‑clustered indexes, and index design strategies.*

---

## Table of Contents

1. [Introduction](#introduction)
2. [File Organization](#file-organization)

   * 2.1 Heap Files
   * 2.2 Sorted Files
3. [Index Types](#index-types)

   * 3.1 B‑Tree Indexes
   * 3.2 Hash Indexes
   * 3.3 Bitmap Indexes
4. [Clustered vs. Non‑clustered Indexes](#clustered-vs-non-clustered)
5. [Index Design Strategies](#index-design-strategies)
6. [Summary](#summary)
7. [References](#references)

---

## Introduction

Efficient storage and retrieval are critical for database performance. File organization determines how records are physically laid out on disk, while indexes accelerate query processing by providing quick access paths. Understanding these techniques and strategies helps design high-performance database systems.

---

## File Organization

### 2.1 Heap Files

* **Definition**: Unordered collection of records. New records are placed in the first available free space.
* **Advantages**:

  * Simple to implement.
  * Fast insertion.
* **Disadvantages**:

  * Full table scans required for most queries.
  * Poor space utilization over time.

### 2.2 Sorted Files

* **Definition**: Records are stored in sorted order based on one or more attributes (the sort key).
* **Advantages**:

  * Efficient range queries using binary search.
  * Eliminates full scans for sorted attributes.
* **Disadvantages**:

  * Expensive insertions/deletions (shifting records).
  * Requires periodic reorganization.

---

## Index Types

### 3.1 B‑Tree Indexes

* **Structure**: Balanced tree with nodes containing search keys and pointers.
* **Properties**:

  * Height-balanced: all leaf nodes at same depth.
  * Supports range scans and point queries.
* **Use Cases**: General-purpose indexing for primary and secondary keys.

### 3.2 Hash Indexes

* **Structure**: Hash table mapping key values to bucket addresses.
* **Properties**:

  * Constant-time (O(1)) lookup for equality queries.
  * No support for range queries.
* **Use Cases**: Equality-based lookups on large tables.

### 3.3 Bitmap Indexes

* **Structure**: Bitmaps representing presence of key values in record positions.
* **Properties**:

  * Compact for low-cardinality attributes.
  * Very efficient for combining multiple predicates via bitwise operations.
* **Use Cases**: Data warehouses, OLAP, columns with few distinct values (e.g., gender, status).

---

## Clustered vs. Non‑clustered Indexes

| Feature          | Clustered Index                    | Non‑clustered Index                         |
| ---------------- | ---------------------------------- | ------------------------------------------- |
| Physical Order   | Table rows sorted on disk by key   | Separate structure; table order unchanged   |
| Number per Table | At most one                        | Multiple allowed                            |
| Lookup Speed     | Faster range and ordered retrieval | Slightly slower due to extra pointer lookup |
| Use Case         | Primary key, range queries         | Secondary attributes, covering indexes      |

---

## Index Design Strategies

1. **Selectivity**:

   * Choose indexes on columns with high selectivity (many distinct values).
2. **Composite Indexes**:

   * Order columns in the index to match query predicates (most selective first).
3. **Covering Indexes**:

   * Include all columns needed by a query so that lookups avoid accessing table data.
4. **Index Maintenance**:

   * Balance read vs. write overhead; avoid over-indexing on write-heavy tables.
5. **Monitoring and Tuning**:

   * Use query execution plans and index usage statistics to identify missing or unused indexes.

---

## Summary

This guide covered storage layouts (heap vs. sorted), index structures (B‑Tree, hash, bitmap), the differences between clustered and non‑clustered indexes, and essential design strategies to optimize database performance.

---

## References

1. R. Elmasri & S. Navathe, *Fundamentals of Database Systems*.
2. C. J. Date, *An Introduction to Database Systems*.
3. H. Garcia-Molina, J. Ullman, & J. Widom, *Database Systems: The Complete Book*.
4. PostgreSQL Documentation: Index Types.
