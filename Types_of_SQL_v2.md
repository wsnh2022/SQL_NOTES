Here is the **final optimized version** — complete structure, full syntax, clear headings, and a rephrased **memory compression logic** at the end.
This version is formatted for long-term recall and spaced memorization.

---

### **1. DDL — Data Definition Language**

Defines and modifies database structure (tables, schemas, columns, constraints).

```sql
-- CREATE TABLE: defines a new table and its structure.
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(30),
    salary DECIMAL(10,2)
);
-- Avoid mistake: forgetting PRIMARY KEY or using spaces/special characters in column names.
```

```sql
-- ALTER TABLE: modifies an existing table’s structure.
ALTER TABLE employees
ADD COLUMN hire_date DATE;
-- Avoid mistake: not specifying the correct keyword (ADD/MODIFY/DROP) or altering wrong column name.
```

```sql
-- DROP TABLE: permanently deletes a table and all its data.
DROP TABLE employees;
-- Avoid mistake: running this without confirming; cannot be undone once committed.
```

```sql
-- TRUNCATE TABLE: removes all rows but keeps table structure.
TRUNCATE TABLE employees;
-- Avoid mistake: using TRUNCATE when you meant DELETE; TRUNCATE cannot use WHERE clause.
```

```sql
-- RENAME TABLE: renames an existing table.
RENAME TABLE employees TO staff;
-- Avoid mistake: referencing the old table name afterward without updating dependent queries.
```

---

### **2. DML — Data Manipulation Language**

Handles data stored inside tables (insert, modify, delete).

```sql
-- INSERT INTO: adds new rows of data into a table.
INSERT INTO employees (id, name, department, salary)
VALUES (1, 'John Doe', 'Sales', 55000.00);
-- Avoid mistake: mismatching column count and VALUES count; always align them properly.
```

```sql
-- UPDATE: modifies existing data in one or more rows.
UPDATE employees
SET salary = 60000.00
WHERE id = 1;
-- Avoid mistake: missing WHERE clause will update all rows unintentionally.
```

```sql
-- DELETE FROM: removes specific rows from a table.
DELETE FROM employees
WHERE id = 1;
-- Avoid mistake: omitting WHERE clause deletes entire table contents.
```

---

### **3. DQL — Data Query Language**

Retrieves data for analysis and reporting (read-only).

```sql
-- SELECT: retrieves specific data from one or more tables.
SELECT id, name, department, salary
FROM employees
WHERE department = 'Sales';
-- Avoid mistake: using SELECT * in production queries; always specify needed columns.
```

---

### **4. DCL — Data Control Language**

Controls access, permissions, and security over database objects.

```sql
-- GRANT: gives users permissions to perform specific actions.
GRANT SELECT, INSERT ON employees TO user1;
-- Avoid mistake: granting privileges without verifying user’s role or scope.
```

```sql
-- REVOKE: removes previously granted permissions.
REVOKE INSERT ON employees FROM user1;
-- Avoid mistake: forgetting to revoke access for removed users.
```

---

### **5. TCL — Transaction Control Language**

Manages logical units of work to ensure data integrity.

```sql
-- BEGIN TRANSACTION: starts a transaction that groups multiple operations.
BEGIN TRANSACTION;
UPDATE employees SET salary = salary * 1.1 WHERE department = 'Sales';
COMMIT;
-- Avoid mistake: forgetting COMMIT leaves transaction unfinalized, locking resources.
```

```sql
-- ROLLBACK: undoes changes made during the current transaction.
ROLLBACK;
-- Avoid mistake: trying to rollback after a COMMIT—rollback only works before commit.
```

```sql
-- SAVEPOINT: sets a temporary checkpoint in a transaction.
SAVEPOINT before_raise;
-- Avoid mistake: forgetting to name savepoints clearly; reuse makes rollback confusing.
```

```sql
-- ROLLBACK TO: reverts to a specific savepoint within the same transaction.
ROLLBACK TO before_raise;
-- Avoid mistake: rolling back to a non-existent savepoint causes an error.
```

---

### **Compression Logic for Recall**

For long-term retention and quick mental mapping:

**DDL → Structure** → CREATE TABLE, ALTER TABLE, DROP TABLE, TRUNCATE TABLE, RENAME TABLE
**DML → Data** → INSERT, UPDATE, DELETE
**DQL → Query** → SELECT
**DCL → Access** → GRANT, REVOKE
**TCL → Control** → COMMIT, ROLLBACK, SAVEPOINT

Each SQL category answers one question:
*Structure defines, Data changes, Query reads, Access secures, Control ensures consistency.*
