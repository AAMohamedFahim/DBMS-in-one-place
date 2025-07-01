# Data Models

Welcome to the **Data Models** section of the DBMS Interview Preparation Hub. In this folder, you'll find an in-depth overview of four core data modeling paradigms:

1. **Hierarchical & Network Models**
2. **Relational Model**
3. **Object‑Oriented & Object‑Relational Models**
4. **Entity–Relationship (ER) Model**

Each model is covered with:

* A conceptual description
* Real-world motivation and use cases
* Strengths & limitations
* A simple illustrative example

---

## 1. Hierarchical & Network Models

### Hierarchical Model

* **Concept**: Organizes data in a tree-like structure (parent–child relationships).
* **Structure**: Each record has a single parent, but a parent can have multiple children.
* **Use Cases**: Early file systems, Windows Registry, XML document structures.

#### Example

```
Company
└─ Department
   ├─ Employee
   └─ Project
```

* **Strengths**:

  * Simple and fast for hierarchy-based queries.
  * Predictable traversal paths.
* **Limitations**:

  * Rigid structure—hard to represent many-to-many relationships.
  * Schema changes require reorganizing the tree.

### Network Model

* **Concept**: Extends the hierarchical model to allow many-to-many relationships via arbitrary graph structures.
* **Structure**: Records are nodes; relationships (called sets) connect nodes in a mesh.
* **Use Cases**: Early telecommunication routing tables, airline reservation systems.

#### Example

```
<Employee> ── works_on ──> <Project>
    ▲                        ▲
    └──────── reports_to ───┘
```

* **Strengths**:

  * Flexible representation of complex relationships.
  * Efficient for interconnected data.
* **Limitations**:

  * Complexity in design and maintenance.
  * Navigational access—queries require procedural traversal logic.

---

## 2. Relational Model

* **Concept**: Represents data as tables (relations) of rows (tuples) and columns (attributes).
* **Key Principles**:

  * **Relation**: A set of tuples sharing the same attributes.
  * **Primary Key**: Uniquely identifies each tuple.
  * **Foreign Key**: Establishes referential integrity between relations.
* **Use Cases**: Dominant model for business applications, OLTP systems, data warehousing.

### Example Schema

```sql
CREATE TABLE Departments (
  dept_id   INT PRIMARY KEY,
  name      VARCHAR(50)
);

CREATE TABLE Employees (
  emp_id    INT PRIMARY KEY,
  name      VARCHAR(100),
  dept_id   INT,
  FOREIGN KEY (dept_id) REFERENCES Departments(dept_id)
);
```

* **Strengths**:

  * Declarative query language (SQL).
  * Strong mathematical foundation.
  * Data independence—physical storage details hidden.
* **Limitations**:

  * Joins can be expensive for very large datasets.
  * Less natural for hierarchical or graph-based data.

---

## 3. Object‑Oriented & Object‑Relational Models

### Object‑Oriented Model

* **Concept**: Integrates database technology with object-oriented programming concepts (classes, objects, inheritance).
* **Features**:

  * Encapsulation of data and behavior.
  * Inheritance, polymorphism, and complex data types.
* **Use Cases**: CAD/CAM, multimedia, engineering applications.

#### Example

```java
class Vehicle {
  int id;
  String model;
}

class Car extends Vehicle {
  int seatingCapacity;
}
```

### Object‑Relational Model

* **Concept**: Hybrid model that adds object-oriented features to the relational paradigm.
* **Features**:

  * Support for complex types, inheritance in tables, user-defined functions and types.
  * Maintains relational integrity while storing richer data structures.
* **Use Cases**: Geographic Information Systems (GIS), complex application domains requiring both structure and flexibility.

#### Example

```sql
CREATE TYPE Address AS (
  street VARCHAR,
  city   VARCHAR,
  zip    VARCHAR
);

CREATE TABLE Customers (
  cust_id INT PRIMARY KEY,
  name    VARCHAR,
  addr    Address
);
```

* **Strengths**:

  * Bridges OO programming and relational storage.
  * Handles complex, nested data naturally.
* **Limitations**:

  * Added complexity in schema design.
  * Varying support across database vendors.

---

## 4. Entity–Relationship (ER) Model

* **Concept**: High-level conceptual modeling tool separating design from implementation.
* **Components**:

  * **Entity**: Real-world object (e.g., Student, Course).
  * **Attribute**: Property of an entity (e.g., StudentID, Name).
  * **Relationship**: Association between entities (e.g., EnrollsIn).
  * **Cardinality**: Specifies how many entities participate (1:1, 1\:N, M\:N).

### Example ER Diagram

```
[Student] ──<EnrolledIn>── [Course]
```

* **Mapping to Relational**:

  * Entities become tables.
  * Relationships become foreign keys or join tables for M\:N.

* **Strengths**:

  * Intuitive visualization for stakeholders.
  * Aids in communication between domain experts and developers.

* **Limitations**:

  * Not directly executable—requires mapping to a logical model.

---

> **Next Steps**: Navigate to the [relational\_model](../relational_model/) folder for deeper dive into SQL, normalization, and advanced relational concepts.

---

*Happy learning and good luck with your DBMS interviews!*
