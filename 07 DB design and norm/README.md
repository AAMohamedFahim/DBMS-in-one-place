# Database Design & Normalization

*A comprehensive guide covering key concepts in database design, functional dependencies, normal forms (1NF through 5NF & BCNF), multi-valued dependencies, and denormalization trade-offs.*

---

## Table of Contents

1. [Introduction](#introduction)
2. [Functional Dependencies](#functional-dependencies)

   * 2.1 Definition and Notation
   * 2.2 Examples
   * 2.3 Trivial and Non-trivial Dependencies
3. [Normal Forms](#normal-forms)

   * 3.1 First Normal Form (1NF)
   * 3.2 Second Normal Form (2NF)
   * 3.3 Third Normal Form (3NF)
   * 3.4 Boyce–Codd Normal Form (BCNF)
   * 3.5 Fourth Normal Form (4NF)
   * 3.6 Fifth Normal Form (5NF)
4. [Multi‐valued Dependencies (MVDs)](#multi-valued-dependencies)
5. [Denormalization Trade‐offs](#denormalization-trade-offs)
6. [Summary](#summary)
7. [References](#references)

---

## Introduction

Database normalization is the process of organizing data in a relational database to reduce redundancy and improve data integrity. This involves decomposing tables based on functional and multi-valued dependencies into smaller, well-structured relations. Conversely, denormalization may be applied to improve performance by introducing controlled redundancy. This guide explores these concepts in depth.

---

## Functional Dependencies

### 2.1 Definition and Notation

A **Functional Dependency (FD)** is a constraint between two sets of attributes in a relation. Denoted as $X \rightarrow Y$, it means that if two tuples (rows) agree on the attributes in set $X$, they must also agree on the attributes in set $Y$.

*Formally*: For relation schema $R$, $X, Y \subseteq R$, $X \rightarrow Y$ holds if for all pairs of tuples $t_1, t_2$ in any instance of $R$, whenever $t_1[X] = t_2[X]$, then $t_1[Y] = t_2[Y]$.

### 2.2 Examples

* In a **Student** table, `StudentID → Name` means each `StudentID` uniquely determines a `Name`.
* In an **Order** table, `(OrderID, ProductID) → Quantity` means the quantity of a specific product in a particular order is uniquely determined by the combination.

### 2.3 Trivial and Non-trivial Dependencies

* **Trivial FD**: $X → Y$ is trivial if $Y \subseteq X$.
* **Non-trivial FD**: $X → Y$ is non-trivial if $Y$ is not a subset of $X$, and **fully non-trivial** if $X ∩ Y = ∅$.

**Key Operations**:

* **Closure $X^+$**: The set of all attributes functionally determined by $X$.
* **Minimal Cover**: A minimal set of FDs equivalent to the original set, with no redundant dependencies or attributes.

---

## Normal Forms

Normalization is guided by normal forms, each eliminating certain types of anomalies.

### 3.1 First Normal Form (1NF)

* **Requirement**: Every attribute domain contains only atomic (indivisible) values; no repeating groups or arrays.
* **Violation Example**: `Orders(OrderID, Products[])` where `Products` is an array.
* **Normalization**: Split into `Orders` and `OrderItems(OrderID, ProductID)`.

### 3.2 Second Normal Form (2NF)

* **Requirement**: Be in 1NF plus every non-key attribute must be fully functionally dependent on the **whole** primary key (no partial dependency).
* **Violation Example**: In `OrderItems(OrderID, ProductID, ProductName)`, `ProductName` depends only on `ProductID`, not the composite key.
* **Normalization**: Move `ProductName` to `Products(ProductID, ProductName)`.

### 3.3 Third Normal Form (3NF)

* **Requirement**: Be in 2NF plus no transitive dependencies: non-key attributes cannot depend on other non-key attributes.
* **Violation Example**: `Employees(EmpID, DeptID, DeptName)` where `DeptName` depends on `DeptID`.
* **Normalization**: Split into `Employees(EmpID, DeptID)` and `Departments(DeptID, DeptName)`.

### 3.4 Boyce–Codd Normal Form (BCNF)

* **Requirement**: For every non-trivial FD $X → Y$, $X$ must be a superkey of the relation.
* **When Required**: Some relations in 3NF may still violate BCNF.
* **Normalization**: Decompose relation until all FDs have determinants that are keys.

### 3.5 Fourth Normal Form (4NF)

* **Requirement**: Be in BCNF and have no non-trivial multi-valued dependencies.
* **Definition**: $X →→ Y$ (multi-valued dependency) means each $X$ value is associated with multiple independent $Y$ values.
* **Violation Example**: `MusicianGenres(Musician, Genre, Instrument)` where genres and instruments are independent lists.
* **Normalization**: Split into `MusicianGenres(Musician, Genre)` and `MusicianInstruments(Musician, Instrument)`.

### 3.6 Fifth Normal Form (5NF)

* **Requirement**: Be in 4NF and decompose join dependencies: no lossless decomposition based on join dependencies other than key-based.
* **When Needed**: Rare, for complex interrelated data.
* **Example**: A relation capturing suppliers, parts, and projects that must satisfy certain combination constraints.

---

## Multi‐valued Dependencies (MVDs)

* **Definition**: A multi‐valued dependency $X →→ Y$ holds if, for each value of $X$, the set of values of $Y$ is independent of other attributes.
* **Use Case**: Modeling independent one-to-many relationships within a single table.
* **Normalization**: Eliminated in 4NF by separating into distinct relation for each independent set.

**Example**:

```
ProjectSkills(ProjectID, Developer, Skill)
```

If developers can have multiple skills and projects require multiple developers independently, split into:

* `ProjectDevelopers(ProjectID, Developer)`
* `DeveloperSkills(Developer, Skill)`

---

## Denormalization Trade‐offs

While normalization reduces redundancy, it can impact performance with multiple joins. Denormalization deliberately introduces redundancy to optimize read-heavy operations.

**Common Denormalization Techniques**:

* **Precomputed Joins**: Store join results in a materialized view or summary table.
* **Duplicate Columns**: Add frequently accessed attributes from related tables.
* **Aggregated Data**: Store pre-aggregated counts or sums.

**Trade‐offs**:

| Benefit                       | Drawback                                  |
| ----------------------------- | ----------------------------------------- |
| Faster read/query performance | Increased storage and data redundancy     |
| Simplified queries            | Potential for data update anomalies       |
| Reduced join complexity       | More complex write operations/maintenance |

**Best Practices**:

* Apply selectively on hotspots identified by profiling.
* Maintain data consistency via triggers or ETL processes.
* Balance between read and write workloads.

---

## Summary

This guide covered the fundamentals of database normalization, from functional dependencies through the fifth normal form, including BCNF and multi-valued dependencies, and weighed the trade-offs of denormalization. Proper design ensures data integrity, scalability, and maintainability.

---

## References

1. C. J. Date, "An Introduction to Database Systems", 8th Edition.
2. R. Elmasri & S. Navathe, "Fundamentals of Database Systems".
3. H. Garcia-Molina, J. Ullman, & J. Widom, "Database Systems: The Complete Book".
4. ISO/IEC 9075: SQL Standard.
5. Online resources: Wikipedia entries on Normalization and Functional Dependency.
