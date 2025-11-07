# **Complete SQL Language Types — With Function-Level Explanation and Engine Support**

---

## **1. DDL – Data Definition Language**

Defines or modifies the **structure of a table**, not the data inside it.
Tables are made up of **columns (fields)** and **rows (records)** — DDL shapes that framework.

**Functions:**

* **CREATE** – Builds a new table or database structure.

  ```sql
  CREATE TABLE employees (
      id INT,
      name VARCHAR(50),
      salary DECIMAL(10,2)
  );
  ```

  → Creates columns `id`, `name`, and `salary` where rows (records) will be stored.

* **ALTER** – Modifies existing columns or adds new ones.

  ```sql
  ALTER TABLE employees ADD department VARCHAR(50);
  ```

  → Adds a new column (field) for department without touching existing rows.

* **DROP** – Deletes the entire table structure, removing all its rows and columns permanently.

  ```sql
  DROP TABLE employees;
  ```

* **TRUNCATE** – Deletes all rows (records) but keeps the table’s columns (structure).

  ```sql
  TRUNCATE TABLE employees;
  ```

* **RENAME** – Changes the table name without altering rows or columns.

  ```sql
  RENAME TABLE employees TO staff;
  ```

**Supported by:**
MySQL, PostgreSQL, SQL Server, Oracle (fully); SQLite partially supports `ALTER`.

---

## **2. DML – Data Manipulation Language**

Handles the **actual data inside tables** — that means working directly with **rows and their column values**.

**Functions:**

* **SELECT** – Reads specific columns and rows.

  ```sql
  SELECT name, salary FROM employees WHERE department = 'Sales';
  ```

  → Retrieves certain fields (columns) from matching records (rows).

* **INSERT** – Adds new rows (records) into the table.

  ```sql
  INSERT INTO employees (id, name, salary, department)
  VALUES (1, 'John', 55000, 'Sales');
  ```

* **UPDATE** – Modifies column values in existing rows.

  ```sql
  UPDATE employees SET salary = 60000 WHERE id = 1;
  ```

* **DELETE** – Removes rows while keeping the column structure intact.

  ```sql
  DELETE FROM employees WHERE id = 1;
  ```

**Supported by:**
All SQL engines — fully standardized behavior.

---

### **3. DCL – Data Control Language**

Controls **who can view, change, or manage** tables and their data.

**Functions:**

* **GRANT** – Gives a user access to read or modify certain columns or rows.

  ```sql
  GRANT SELECT, UPDATE ON employees TO user1;
  ```

* **REVOKE** – Removes previously given access rights.

  ```sql
  REVOKE UPDATE ON employees FROM user1;
  ```

**Supported by:**
MySQL, PostgreSQL, SQL Server, Oracle.
**Not supported in SQLite** (permissions handled by the file system).

---

## **4. TCL – Transaction Control Language**

Manages **transactions**, which are groups of DML changes on rows/columns treated as one logical operation — either all succeed or all fail.

**Functions:**

* **COMMIT** – Saves all changes made to rows permanently.

  ```sql
  COMMIT;
  ```

* **ROLLBACK** – Reverts uncommitted changes to rows and restores previous column values.

  ```sql
  ROLLBACK;
  ```

* **SAVEPOINT** – Creates a checkpoint within a transaction so you can undo up to that point.

  ```sql
  SAVEPOINT sp1;
  UPDATE employees SET salary = 90000 WHERE id = 5;
  ROLLBACK TO sp1;
  ```

* **SET TRANSACTION** – Defines transaction properties like read/write mode.

  ```sql
  SET TRANSACTION READ WRITE;
  ```

**Supported by:**
MySQL (requires manual transaction mode), PostgreSQL, SQL Server, Oracle (full), SQLite (transaction applies to entire database file).

---

## **5. DQL – Data Query Language**

Used only for **reading data** — it doesn’t alter rows or columns.

**Function:**

* **SELECT** – Retrieves data from one or more tables, allowing filters, sorting, and aggregation.

  ```sql
  SELECT department, AVG(salary)
  FROM employees
  GROUP BY department
  HAVING AVG(salary) > 50000;
  ```

  → Reads column data (`department`, `salary`) and processes rows to produce summarized results.

**Supported by:**
Every SQL engine identically.

---

### **Summary**

| Category | Purpose             | Works On               | Core Commands                         | Example Effect                | Engine Support             |
| -------- | ------------------- | ---------------------- | ------------------------------------- | ----------------------------- | -------------------------- |
| **DDL**  | Define structure    | Columns & tables       | CREATE, ALTER, DROP, TRUNCATE, RENAME | Build or reshape table layout | All (SQLite limited ALTER) |
| **DML**  | Manipulate data     | Rows & columns         | SELECT, INSERT, UPDATE, DELETE        | Add, modify, or remove data   | Fully supported            |
| **DCL**  | Control access      | Tables, columns, users | GRANT, REVOKE                         | Manage user permissions       | All except SQLite          |
| **TCL**  | Manage transactions | Row changes in memory  | COMMIT, ROLLBACK, SAVEPOINT           | Group multiple DML actions    | All (behavior differs)     |
| **DQL**  | Retrieve data       | Columns & rows         | SELECT                                | Query and analyze data        | Universal                  |

This version aligns SQL language categories with the **row (record) and column (field)** model — showing exactly **what each command changes, reads, or controls** inside a relational table.

---

**SQL Core Recall Drill — Foundational Memory Questions**
*(Answer these from memory without checking notes. They test total internalization of SQL’s five language categories.)*

---

### **Part 1 — Category Identification**

1. Which SQL category defines how tables and columns are structured?
2. Which category modifies actual data stored in rows?
3. Which category grants or removes access rights to users?
4. Which category ensures transactions either fully succeed or fully revert?
5. Which category focuses purely on retrieving data without changing it?

<details>
<summary> <strong> SQL Core Recall Drill — Answer Key <strong> </summary>

---

1. **DDL (Data Definition Language)**
2. **DML (Data Manipulation Language)**
3. **DCL (Data Control Language)**
4. **TCL (Transaction Control Language)**
5. **DQL (Data Query Language)**

---
</details>

---

### **Part 2 — Command Mapping**

6. `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME` belong to which SQL language?
7. `SELECT`, `INSERT`, `UPDATE`, `DELETE` belong to which category?
8. `GRANT` and `REVOKE` belong to which category?
9. `COMMIT`, `ROLLBACK`, `SAVEPOINT` belong to which category?
10. Which single command defines the entire DQL category?

<details>
<summary> <strong> Command Mapping — Answer Key <strong> </summary>

6. **DDL**
7. **DML**
8. **DCL**
9. **TCL**
10. **SELECT**

---
</details>

---

### **Part 3 — Row/Column Understanding**

11. Which commands modify column definitions rather than data in rows?
12. Which commands remove rows but retain column structure?
13. Which command permanently removes both rows and columns?
14. Which command restores data in rows to their previous values before commit?
15. Which command is used to create a checkpoint before modifying multiple rows?


<details>
<summary> <strong> Row/Column Understanding — Answer Key <strong> </summary>

11. **ALTER** — modifies column structure (DDL).
12. **DELETE** or **TRUNCATE** — remove rows, keep structure.
13. **DROP** — removes both structure and data.
14. **ROLLBACK** — undoes row changes.
15. **SAVEPOINT** — sets a reversible checkpoint.

</details>

---

### **Part 4 — Behavioral Logic**

16. What happens to a table after a `TRUNCATE` command — structure or data?
17. Why is `ROLLBACK` considered safer than deleting rows directly?
18. Why can’t SQLite use DCL commands like `GRANT` or `REVOKE`?
19. What makes DQL unique among all SQL types?
20. Why must `COMMIT` be used after `INSERT` or `UPDATE` when autocommit is off?

<details>
<summary> <strong> Behavioral Logic — Answer Key <strong> </summary>

16. `TRUNCATE` removes **all rows (records)**, keeps **columns (fields)** and table structure.
17. `ROLLBACK` reverts uncommitted changes, preserving data integrity; direct deletes are irreversible once committed.
18. SQLite has **no internal user system** — permissions depend on file-level OS control, so DCL isn’t implemented.
19. DQL doesn’t modify anything; it **reads and returns data** only.
20. Without `COMMIT`, data changes stay **temporary** and vanish if the session ends or a rollback occurs.

</details>

---

### **Part 5 — Scenario Recall**

21. You create a new column in a table — which category did you use?
22. You change the department name of one employee — which category?
23. You let a new user read data but not modify it — which category?
24. You undo a set of updates before saving them — which category?
25. You extract the average salary of each department — which category?

<details>
<summary> <strong> Scenario Recall — Answer Key <strong> </summary>

21. **DDL** (`ALTER TABLE`)
22. **DML** (`UPDATE`)
23. **DCL** (`GRANT`)
24. **TCL** (`ROLLBACK`)
25. **DQL** (`SELECT` with `GROUP BY`)

</details>
---

Mastery check:
If you can answer **all 25 questions instantly**, you’ve internalized SQL’s structure at the cognitive level necessary to proceed into **joins, normalization, indexing, and query optimization** without confusion.

If all 25 answers come instantly — without second-guessing — you’ve anchored SQL’s mental model:

* **DDL → structure**
* **DML → data**
* **DCL → access**
* **TCL → control**
* **DQL → query**

That’s the irreversible pivot point between **memorization** and **operational fluency**.
