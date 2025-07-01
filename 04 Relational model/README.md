# Relational Model

Welcome to the **Relational Model** section of the DBMS Interview Preparation Hub. This folder dives into the core principles of the relational paradigm, covering the fundamental constructs of tuples, relations, and schemas; various key types; and the essential integrity constraints that keep data valid and consistent.

---

## 1. Tuples, Relations & Schemas

### 1.1 Tuple

* **Definition**: A single row in a relation; an ordered set of attribute values representing one record (e.g., a single `Employee`).
* **Notation**: Usually denoted as `t` or `(v1, v2, …, vn)`.

### 1.2 Relation

* **Definition**: A table with named columns (attributes) and rows (tuples). Mathematically, a relation is a set of tuples over the same attributes.
* **Properties**:

  * **Degree (Arity)**: Number of attributes (columns).
  * **Cardinality**: Number of tuples (rows).
  * **Set Semantics**: No duplicate tuples.
  * **Unordered**: Neither rows nor columns have a fixed order.

#### Example

```
Employees (emp_id, name, dept_id, salary)
----------------------------------------
| 101 | Alice   | 10      | 70000 |
| 102 | Bob     | 20      | 65000 |
| 103 | Charlie | 10      | 72000 |
```

### 1.3 Schema

* **Relation Schema**: Declares a relation’s name and its attributes with types (e.g., `Employee(emp_id INT, name VARCHAR, dept_id INT, salary DECIMAL)`).
* **Database Schema**: A collection of relation schemas along with integrity constraints.
* **Instance**: A snapshot of the database at a given time—i.e., a set of relation instances (tuples) conforming to the schemas.

---

## 2. Keys

Keys uniquely identify tuples and establish relationships across relations.

| **Key Type**  | **Description**                                                                                      | **Notation**                   |
| ------------- | ---------------------------------------------------------------------------------------------------- | ------------------------------ |
| Primary Key   | A minimal set of attributes that uniquely identifies each tuple in a relation.                       | Underlined in schema           |
| Candidate Key | Any attribute or minimal set of attributes that can uniquely identify tuples; one is chosen as PK.   | Multiple possible per relation |
| Superkey      | A set of one or more attributes that uniquely identifies tuples; may contain extra attributes.       | Combination of attributes      |
| Foreign Key   | An attribute in one relation that references the PK of another relation, enforcing referential ties. | Dashed underline or FK(... )   |

#### Example Schema

```sql
CREATE TABLE Department (
  dept_id   INT PRIMARY KEY,
  name      VARCHAR(50)
);

CREATE TABLE Employee (
  emp_id    INT,
  email     VARCHAR(100),
  dept_id   INT,
  PRIMARY KEY (emp_id),         -- Primary Key
  UNIQUE (email),               -- Candidate Key
  FOREIGN KEY (dept_id)         -- Foreign Key
    REFERENCES Department(dept_id)
);
```

* **Superkey vs. Candidate Key**: Every candidate key is a superkey, but not vice versa. E.g., `(emp_id, email)` is a superkey but not minimal.

---

## 3. Integrity Constraints

Integrity constraints enforce correctness, validity, and consistency of data in the database.

### 3.1 Entity Integrity

* **Rule**: No primary key attribute may contain `NULL`. Ensures each tuple can be uniquely identified.
* **Implication**: Every relation with a PK must have a defined, non-null value for that key.

### 3.2 Referential Integrity

* **Rule**: A foreign key must either be `NULL` (if allowed) or match a primary key value in the referenced relation.
* **Implication**: Prevents orphan records. E.g., every `Employee.dept_id` must refer to an existing `Department.dept_id`.

### 3.3 Domain Integrity

* **Rule**: All values for an attribute must be drawn from a predefined domain (type, format, range, possible values).
* **Enforcement**: Via data types, `CHECK` constraints, and `NOT NULL` declarations.

#### Example Constraints

```sql
CREATE TABLE Employee (
  emp_id    INT PRIMARY KEY,
  name      VARCHAR(100) NOT NULL,
  salary    DECIMAL(10,2) CHECK (salary >= 0),
  dept_id   INT,
  FOREIGN KEY (dept_id)
    REFERENCES Department(dept_id)
    ON DELETE SET NULL
);
```

* `NOT NULL` on `name` enforces entity integrity for that attribute.
* `CHECK (salary >= 0)` enforces domain integrity.
* `ON DELETE SET NULL` defines referential action for department deletions.

---

> **Next Steps**: Continue to **SQL & DDL/DML** for hands‑on query writing and schema manipulation!

---

*Master these fundamentals to ensure rigor and precision in your relational database designs!*
