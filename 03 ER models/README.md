# ER Modeling & Design

Welcome to the **ER Modeling & Design** section of the DBMS Interview Preparation Hub. This folder covers the key principles and practices of conceptual database design using Entity–Relationship (ER) diagrams. You’ll learn how to identify entities, model their attributes and relationships, enforce cardinality and participation constraints, distinguish between weak and strong entities, and map the resulting ER design to a relational schema.

---

## 1. Entities, Attributes & Relationships

### 1.1 Entities

* **Definition**: A real-world object or concept with an independent existence that can be distinctly identified (e.g., `Student`, `Course`, `Order`).
* **Types**:

  * **Strong Entity**: Has a primary key that uniquely identifies each instance without relying on other entities (e.g., `Employee(emp_id)`).
  * **Weak Entity**: Depends on a strong entity for its identification; lacks a complete key on its own (e.g., `Dependent` in a family schema).

### 1.2 Attributes

* **Simple (Atomic)**: Indivisible values (e.g., `FirstName VARCHAR(50)`).
* **Composite**: Composed of multiple sub-attributes (e.g., `Address` → `Street`, `City`, `ZIP`).
* **Multi-valued**: Can hold multiple values for a single entity (e.g., `PhoneNumbers`). Represented by a double oval.
* **Derived**: Computed from other attributes (e.g., `Age` derived from `DateOfBirth`). Represented by a dashed oval.

#### Example

```
[Employee]
  • emp_id (PK)
  • name
  • address (Street, City, ZIP)
  • skills {Java, SQL, Python}
  • age (derived)
```

### 1.3 Relationships

* **Definition**: Associations among two or more entities that describe how they interact (e.g., `EnrolledIn` between `Student` and `Course`).
* **Degree**:

  * **Binary**: Between two entities (most common).
  * **Ternary+**: Among three or more entities (use sparingly).

#### Example

```
[Student] ── EnrolledIn ── [Course]
```

---

## 2. Cardinality Constraints & Participation

### 2.1 Cardinality Ratios

Specifies the number of instances of one entity that can or must be associated with each instance of another.

* **One-to-One (1:1)**: e.g., `Person` ↔ `Passport`.
* **One-to-Many (1\:N)**: e.g., `Department` → *has many* → `Employee`.
* **Many-to-One (N:1)**: reverse of 1\:N.
* **Many-to-Many (M\:N)**: e.g., `Student` ↔ *enrolls in* ↔ `Course`.

### 2.2 Participation Constraints

* **Total (Mandatory)**: Every entity instance must participate in the relationship (double line). e.g., every `Employee` must be assigned to a `Department`.
* **Partial (Optional)**: Some instances may not participate (single line). e.g., some `Customers` may not place any `Order` yet.

#### Illustrative ER Snippet

```
[Department] =1──0..* [Employee]
```

* `=1` indicates exactly one department per employee (total or partial participation can be overlaid).
* `0..*` indicates zero or more employees per department.

---

## 3. Weak vs. Strong Entities

### 3.1 Strong Entities

* Have their own primary key.
* Represented by a single rectangle.

### 3.2 Weak Entities

* Do **not** have a complete key; their key is a **partial key** combined with the owning entity’s key.
* Must participate in an identifying relationship with the owner.
* Denoted by a double rectangle; identifying relationship by a double diamond.

#### Example: Dependent Schema

```
[Employee] (PK: emp_id)
  |
  | Identifies (double diamond)
  v
[Dependent] (double rectangle)
  • name (partial key)
  • birthdate
```

The combined key is `(emp_id, name)`.

---

## 4. ER → Relational Mapping

Translating an ER diagram into a set of relational tables:

### 4.1 Mapping Strong Entities

* Each strong entity becomes a table.
* Attributes map to columns; primary key becomes PK constraint.

```sql
CREATE TABLE Employee (
  emp_id     INT PRIMARY KEY,
  name       VARCHAR(100),
  date_of_birth DATE
);
```

### 4.2 Mapping Weak Entities

* Create a table for the weak entity.
* Include owner’s primary key as a foreign key.
* Define a composite primary key `(owner_key, partial_key)`.

```sql
CREATE TABLE Dependent (
  emp_id     INT,
  name       VARCHAR(50),
  birthdate  DATE,
  PRIMARY KEY (emp_id, name),
  FOREIGN KEY (emp_id) REFERENCES Employee(emp_id)
);
```

### 4.3 Mapping Binary Relationships

* **1:1**: Add FK in either table (choose the side with optional participation).
* **1\:N**: Add FK on the N-side referencing the PK of the 1-side.
* **M\:N**: Create a junction table with FKs to both entities; composite PK on those FKs.

```sql
-- 1:N: Department to Employee
ALTER TABLE Employee
ADD COLUMN dept_id INT,
ADD FOREIGN KEY (dept_id) REFERENCES Department(dept_id);

-- M:N: Student and Course
CREATE TABLE EnrollsIn (
  student_id INT,
  course_id  INT,
  grade      CHAR(2),
  PRIMARY KEY (student_id, course_id),
  FOREIGN KEY (student_id) REFERENCES Student(student_id),
  FOREIGN KEY (course_id) REFERENCES Course(course_id)
);
```

### 4.4 Mapping Attributes

* **Composite**: Flatten into separate columns (e.g., `address_street`, `address_city`).
* **Multi-valued**: Create a separate table with a FK back to the owner.
* **Derived**: Do **not** store; compute at query time (e.g., `AGE(CURRENT_DATE, date_of_birth)`).

---

> **Next Steps**: Proceed to the **Normalization** section to refine your relational schema and eliminate redundancy.

---

*Happy ER modeling and best of luck with your DBMS interviews!*
