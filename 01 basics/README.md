# 1. Basic Concepts & Architecture

Welcome to the **Basic Concepts & Architecture** module. This folder dives deep into foundational DBMS ideas—you’ll build a solid base before moving on to advanced topics.

## 1.1 Data vs. Information

* **Data**: Raw, unprocessed facts and figures (e.g., `['Alice', '25', 'Engineer']`).
* **Information**: Data that has been processed, organized, or structured to make it meaningful (e.g., “Alice, a 25‑year‑old engineer”).
* **Key Points**:

  * Data becomes information when context and purpose are applied.
  * Information supports decision‑making; data alone does not.
  * Example: Sensor readings (data) → Daily temperature report (information).

## 1.2 DBMS vs. File System

| Aspect              | File System                        | DBMS                                   |
| ------------------- | ---------------------------------- | -------------------------------------- |
| Data Model          | None or application‑specific       | Well‑defined relational/object model   |
| Redundancy          | High, manual control               | Low, managed via integrity constraints |
| Data Independence   | Low — apps tied to file structures | High — logical/physical independence   |
| Concurrency Control | Manual locking                     | Built‑in transaction management        |
| Recovery            | Limited, often manual backups      | Automated logging & recovery           |
| Security            | OS‑level permissions only          | Fine‑grained access control & auditing |

## 1.3 Three‑schema Architecture

1. **Internal Level** (Physical Schema)

   * Describes how data is physically stored (files, indexes).
   * Optimizes storage and access paths.
2. **Conceptual Level** (Logical Schema)

   * Global view of the database for the community of users.
   * Defines entities, relationships, constraints.
3. **External Level** (View Level)

   * Individual user or application views.
   * Subsets of the conceptual schema tailored to specific needs.

**Benefits**:

* Separates user applications from physical data storage.
* Facilitates data independence—changes at one level don’t affect others.

## 1.4 Data Independence

* **Logical Data Independence**:

  * Ability to change conceptual schema without altering external schemas or applications.
  * Example: Adding a new attribute to a table shouldn’t break existing queries.
* **Physical Data Independence**:

  * Ability to change internal schema without modifying conceptual schema.
  * Example: Switching from a heap file to a clustered index.

**Why It Matters**:

* Enhances maintainability and scalability.
* Reduces cost and effort for evolving systems.

---

> Next: Move on to [Data Models](../data_models/) for an in‑depth look at various DB structures.
