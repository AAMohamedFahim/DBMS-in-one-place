# SQL & DDL/DML

Welcome to the **SQL & DDL/DML** section of the DBMS Interview Preparation Hub. This folder provides an extensive guide to leveraging SQL for defining, manipulating, and optimizing relational data structures. You will learn about:

* **DDL (Data Definition Language)**: Defining and modifying database structures.
* **DML (Data Manipulation Language)**: Inserting, updating, deleting, and querying data.
* **Joins**: Combining data across multiple tables.
* **Subqueries**: Nested queries to solve complex retrieval tasks.
* **Set Operations**: Merging result sets in various ways.
* **Views & Index‑Only Scans**: Creating virtual tables and optimizing read performance.

---

## 1. DDL: CREATE, ALTER, DROP

### 1.1 CREATE

* **Purpose**: Define new database objects (tables, views, indexes, schemas).
* **Key Syntax**:

  ```sql
  CREATE TABLE TableName (
    column1 datatype [constraints],
    column2 datatype [constraints],
    ...
  );
  ```
* **Example**:

  ```sql
  CREATE TABLE Department (
    dept_id   INT PRIMARY KEY,
    name      VARCHAR(50) NOT NULL
  );
  ```

### 1.2 ALTER

* **Purpose**: Modify existing objects without losing data.
* **Common Operations**:

  * `ADD COLUMN`
  * `DROP COLUMN`
  * `ALTER COLUMN` (change data type or constraints)
* **Example**:

  ```sql
  ALTER TABLE Employee
    ADD COLUMN hire_date DATE;

  ALTER TABLE Employee
    ALTER COLUMN name SET NOT NULL;
  ```

### 1.3 DROP

* **Purpose**: Remove database objects permanently.
* **Caution**: Cascading drops may remove dependent objects.
* **Example**:

  ```sql
  DROP TABLE IF EXISTS TempData;
  DROP VIEW DepartmentSummary;
  ```

---

## 2. DML: SELECT, INSERT, UPDATE, DELETE

### 2.1 SELECT

* **Purpose**: Retrieve data with filtering, aggregation, and sorting.
* **Basic Syntax**:

  ```sql
  SELECT column_list
  FROM table_list
  [WHERE conditions]
  [GROUP BY grouping_columns]
  [HAVING filter_on_aggregates]
  [ORDER BY sort_columns];
  ```
* **Example**:

  ```sql
  -- Retrieve employees in dept 10, ordered by salary
  SELECT emp_id, name, salary
  FROM Employee
  WHERE dept_id = 10
  ORDER BY salary DESC;
  ```

### 2.2 INSERT

* **Purpose**: Add new tuples to a table.
* **Syntax**:

  ```sql
  INSERT INTO table_name (col1, col2, ...)
  VALUES (val1, val2, ...);
  ```
* **Example**:

  ```sql
  INSERT INTO Department (dept_id, name)
  VALUES (30, 'Marketing');
  ```

### 2.3 UPDATE

* **Purpose**: Modify existing tuples.
* **Syntax**:

  ```sql
  UPDATE table_name
  SET column1 = value1 [, column2 = value2, ...]
  [WHERE conditions];
  ```
* **Example**:

  ```sql
  UPDATE Employee
  SET salary = salary * 1.05
  WHERE performance_rating = 'A';
  ```

### 2.4 DELETE

* **Purpose**: Remove tuples from a table.
* **Syntax**:

  ```sql
  DELETE FROM table_name
  [WHERE conditions];
  ```
* **Example**:

  ```sql
  DELETE FROM Employee
  WHERE hire_date < '2020-01-01';
  ```

---

## 3. Joins: Inner, Outer, Cross, Self

### 3.1 Inner Join

* **Definition**: Returns rows with matching values in both tables.
* **Syntax**:

  ```sql
  SELECT a.*, b.*
  FROM A INNER JOIN B
    ON a.key = b.key;
  ```
* **Use Case**: Find employees and their departments.

### 3.2 Outer Joins

* **Left Outer**: All rows from left table plus matching right rows.
* **Right Outer**: All rows from right table plus matching left rows.
* **Full Outer**: All rows when there is a match in either table.
* **Syntax (Left)**:

  ```sql
  SELECT e.name, d.name
  FROM Employee e
  LEFT JOIN Department d
    ON e.dept_id = d.dept_id;
  ```

### 3.3 Cross Join

* **Definition**: Cartesian product of two tables.
* **Syntax**:

  ```sql
  SELECT *
  FROM A CROSS JOIN B;
  ```
* **Caution**: Results in `|A| * |B|` rows—use carefully.

### 3.4 Self Join

* **Definition**: A table joined with itself to relate rows within the same table.
* **Syntax**:

  ```sql
  SELECT e1.name AS manager, e2.name AS report
  FROM Employee e1
  JOIN Employee e2
    ON e1.emp_id = e2.manager_id;
  ```

---

## 4. Subqueries: Correlated & Uncorrelated

### 4.1 Uncorrelated Subquery

* **Definition**: Independent of outer query; executed once.
* **Example**:

  ```sql
  SELECT name
  FROM Employee
  WHERE salary > (
    SELECT AVG(salary) FROM Employee
  );
  ```

### 4.2 Correlated Subquery

* **Definition**: References columns from the outer query; executes per row.
* **Example**:

  ```sql
  SELECT e.name, e.salary
  FROM Employee e
  WHERE e.salary > (
    SELECT AVG(salary)
    FROM Employee
    WHERE dept_id = e.dept_id
  );
  ```

---

## 5. Set Operations: UNION, INTERSECT, EXCEPT

* **UNION**: Combine result sets and remove duplicates.
* **UNION ALL**: Include duplicates.
* **INTERSECT**: Return only rows common to both sets.
* **EXCEPT (MINUS in some systems)**: Return rows in the first set not in the second.

**Example**:

```sql
-- Employees in dept 10 or 20
SELECT emp_id FROM Employee WHERE dept_id = 10
UNION
SELECT emp_id FROM Employee WHERE dept_id = 20;
```

---

## 6. Views & Index‑Only Scans

### 6.1 Views

* **Definition**: Virtual tables defined by a stored query.
* **Purpose**: Simplify complex queries, enforce security, present tailored data.
* **Syntax**:

  ```sql
  CREATE VIEW DeptSalary AS
    SELECT d.name AS dept_name, AVG(e.salary) AS avg_salary
    FROM Employee e
    JOIN Department d ON e.dept_id = d.dept_id
    GROUP BY d.name;
  ```
* **Usage**:

  ```sql
  SELECT * FROM DeptSalary WHERE avg_salary > 70000;
  ```

### 6.2 Index‑Only Scans

* **Concept**: The database engine reads exclusively from an index without touching the table.
* **Prerequisite**: The index must cover all columns requested by the query.
* **Benefit**: Significantly faster reads for covered queries.
* **Example**: If you have an index on `(dept_id, salary)`, a query on these columns may be satisfied by the index alone.

---

> **Next Steps**: Explore **Advanced SQL Features** for window functions, recursive CTEs, and performance tuning.

---

*Master these SQL DDL/DML concepts to build, query, and optimize robust relational databases!*
