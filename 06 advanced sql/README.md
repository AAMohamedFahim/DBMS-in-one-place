# Advanced SQL Features

Welcome to the **Advanced SQL Features** section of the DBMS Interview Preparation Hub. This folder explores powerful SQL capabilities that go beyond basic querying—allowing you to perform complex analyses, automate data operations, and manage transaction logic.

You will learn about:

* **Window Functions**
* **Common Table Expressions (CTEs) & Recursive Queries**
* **Triggers, Stored Procedures & Functions**
* **Cursors & Transactions**

---

## 1. Window Functions

### 1.1 Concept

* **Definition**: Functions that perform calculations across a set of table rows related to the current row, without collapsing them into a single output row (unlike aggregates).
* **Use Cases**: Ranking, running totals, moving averages, percentiles.

### 1.2 Syntax Structure

```sql
FUNCTION() OVER (
  [PARTITION BY partition_expression]
  [ORDER BY order_expression]
  [frame_clause]
)
```

### 1.3 Common Examples

* **ROW\_NUMBER()**: Assigns a unique sequential integer to rows within a partition.

```sql
SELECT emp_id, dept_id,
  ROW_NUMBER() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rn
FROM Employee;
```

* **RANK(), DENSE\_RANK()**: Ranking functions that handle ties differently.

* **SUM(), AVG()** running totals:

```sql
SELECT order_date,
  SUM(amount) OVER (ORDER BY order_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM Orders;
```

---

## 2. CTEs (WITH) & Recursive Queries

### 2.1 CTEs

* **Definition**: Temporary named result sets defined within a `WITH` clause, improving readability and modularity.
* **Syntax**:

```sql
WITH cte_name (col1, col2, ...) AS (
  SELECT ...
)
SELECT * FROM cte_name;
```

* **Use Case**: Breaking complex queries into logical steps.

### 2.2 Recursive CTEs

* **Definition**: CTEs that refer to themselves to perform hierarchical or iterative queries.

* **Structure**:

  1. **Anchor member**: Base result set.
  2. **Recursive member**: References the CTE name.

* **Syntax**:

```sql
WITH RECURSIVE cte_name AS (
  -- Anchor
  SELECT ...
  UNION ALL
  -- Recursive step
  SELECT ... FROM cte_name JOIN ...
)
SELECT * FROM cte_name;
```

* **Example**: Employee managerial hierarchy.

```sql
WITH RECURSIVE OrgChart(emp_id, manager_id, level) AS (
  SELECT emp_id, manager_id, 1 AS level
  FROM Employee
  WHERE manager_id IS NULL
  UNION ALL
  SELECT e.emp_id, e.manager_id, oc.level + 1
  FROM Employee e
  JOIN OrgChart oc ON e.manager_id = oc.emp_id
)
SELECT * FROM OrgChart;
```

---

## 3. Triggers, Stored Procedures & Functions

### 3.1 Triggers

* **Definition**: Automatically executed code in response to specified table events (INSERT, UPDATE, DELETE).
* **Use Cases**: Auditing, validation, cascading updates.
* **Syntax**:

```sql
CREATE TRIGGER trigger_name
  {BEFORE | AFTER} {INSERT | UPDATE | DELETE}
  ON table_name
  FOR EACH ROW
BEGIN
  -- procedural code (SQL or PL/pgSQL)
END;
```

### 3.2 Stored Procedures

* **Definition**: Named, parameterized sets of statements stored in the database and invoked explicitly.
* **Features**: Control-of-flow statements, transaction control.
* **Syntax (MySQL example)**:

```sql
DELIMITER $$
CREATE PROCEDURE increase_salaries(p_dept INT, p_pct DECIMAL)
BEGIN
  UPDATE Employee
  SET salary = salary * (1 + p_pct)
  WHERE dept_id = p_dept;
END$$
DELIMITER ;
```

### 3.3 Functions

* **Definition**: Similar to procedures but return a single value and can be used in SQL expressions.
* **Example (PostgreSQL)**:

```sql
CREATE FUNCTION calculate_tax(amount DECIMAL) RETURNS DECIMAL AS $$
BEGIN
  RETURN amount * 0.1;
END;
$$ LANGUAGE plpgsql;
```

---

## 4. Cursors & Transactions

### 4.1 Cursors

* **Definition**: Database objects that allow row-by-row processing of query results.
* **Use Cases**: Complex business logic, row-level operations not achievable in set-based SQL.
* **Syntax (PL/pgSQL)**:

```sql
DECLARE cur CURSOR FOR SELECT emp_id, salary FROM Employee;
OPEN cur;
FETCH NEXT FROM cur INTO v_emp, v_sal;
-- loop and process
CLOSE cur;
```

### 4.2 Transactions

* **Definition**: Logical units of work that must be entirely committed or rolled back to maintain data integrity.
* **ACID Properties**: Atomicity, Consistency, Isolation, Durability.
* **Control**:

  * `BEGIN` or `START TRANSACTION`
  * `COMMIT` to save changes
  * `ROLLBACK` to undo
* **Example**:

```sql
BEGIN;
  UPDATE Account SET balance = balance - 100 WHERE id = 1;
  UPDATE Account SET balance = balance + 100 WHERE id = 2;
  COMMIT;
```

---

> **Next Steps**: Head to **Performance & Optimization** or **Distributed Databases** to broaden your DBMS expertise.

---

*Leverage these advanced SQL features to write efficient, maintainable, and powerful database routines!*
