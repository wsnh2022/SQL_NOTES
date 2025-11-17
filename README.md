# Complete SQL Foundations - Non-Negotiable Tier

> This is the minimum viable knowledge base. Master these before moving forward.

---

## Quick Navigation
| Section | Core Concept | Why It Matters |
|---------|--------------|----------------|
| 1-4 | Data Manipulation | Daily CRUD operations |
| 5-7 | Data Retrieval | Complex queries |
| 8-11 | Data Integrity | Prevent errors |
| 12-17 | Data Transformation | Real-world formatting |
| 18-25 | Advanced Operations | Production readiness |

---

# PART 1: DATA MANIPULATION $\color{blue}{Daily\ CRUD\ operations\}$

---

## 1. Core Commands

### 1.1 Basic Data Manipulation - DML (Data Manipulation Language)

<details>
<summary> <strong> SELECT - Retrieve data <strong> </summary>

```sql
SELECT column1, column2 FROM table_name;
SELECT * FROM table_name WHERE condition;
```
</details>

<details>
<summary><strong> INSERT - Add new rows <strong> </summary>

```sql
-- Single row
INSERT INTO users (name, email) VALUES ('John', 'john@example.com');

-- Multiple rows
INSERT INTO users (name, email) VALUES 
  ('Alice', 'alice@example.com'),
  ('Bob', 'bob@example.com');

-- From another query (bulk insert)
INSERT INTO archive_orders 
SELECT * FROM orders WHERE order_date < '2024-01-01';
```
</details>

<details>
<summary> <strong> UPDATE - Modify existing rows <strong> </summary>

```sql
-- Simple update
UPDATE employees SET salary = 60000 WHERE id = 5;

-- Update multiple columns
UPDATE employees SET salary = 65000, department = 'Engineering' WHERE id = 5;

-- Update with JOIN (based on another table)
UPDATE employees e
JOIN departments d ON e.dept_id = d.id
SET e.salary = e.salary * 1.1
WHERE d.name = 'Sales';
```
</details>

<details>
<summary> <strong> DELETE - Remove rows <strong> </summary>

```sql
-- Delete specific rows
DELETE FROM orders WHERE status = 'cancelled';

-- Delete with JOIN (conditional deletion)
DELETE o FROM orders o
JOIN customers c ON o.customer_id = c.id
WHERE c.account_status = 'inactive';
```
</details>

---

### 1.2 Table Structure Commands - DDL (Data Definition Language)

<details>
<summary> <strong> CREATE TABLE - Build new table <strong> </summary>

```sql
CREATE TABLE employees (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE,
  salary DECIMAL(10,2),
  hire_date DATE DEFAULT CURRENT_DATE
);

```
</details>

<details>
<summary> <strong> ALTER TABLE - Modify table structure <strong> </summary>

```sql
-- Add column
ALTER TABLE employees ADD COLUMN phone VARCHAR(20);

-- Drop column
ALTER TABLE employees DROP COLUMN phone;

-- Modify column type
ALTER TABLE employees MODIFY COLUMN salary DECIMAL(12,2);

-- Rename column
ALTER TABLE employees RENAME COLUMN name TO full_name;

-- Add constraint
ALTER TABLE employees ADD CONSTRAINT fk_dept 
  FOREIGN KEY (dept_id) REFERENCES departments(id);

-- Drop constraint
ALTER TABLE employees DROP CONSTRAINT fk_dept;

-- Add index
ALTER TABLE employees ADD INDEX idx_email (email);
```
</details>

<details>
<summary> <strong> DROP TABLE - Delete entire table <strong> </summary>

```sql
DROP TABLE IF EXISTS temp_data;
```
</details>

<details>
<summary> <strong> TRUNCATE - Remove all rows, keep structure <strong> </summary>

```sql
-- Example 1: Clear shopping cart after order completion
TRUNCATE TABLE shopping_cart;
-- Removes all items, cart table still exists, ready for new items

-- Example 2: Reset daily visitor counter
TRUNCATE TABLE daily_visitors;
-- Wipes today's visitor data, table structure remains intact

-- Example 3: Clear temporary upload data
TRUNCATE TABLE temp_file_uploads;
-- Deletes all temporary files info, table ready for new uploads

-- Example 4: Reset game high scores for new season
TRUNCATE TABLE game_scores;
-- All old scores gone, AUTO_INCREMENT back to 1

-- Example 5: Clear email queue after sending
TRUNCATE TABLE email_queue;
-- Removes all sent emails from queue, fresh start

-- Example 6: Clean test database before new testing
TRUNCATE TABLE test_results;
-- Removes all previous test data instantly

-- COMPARISON with DELETE:
-- TRUNCATE TABLE students;        -- Fast, removes ALL, resets ID to 1
-- DELETE FROM students;            -- Slower, removes ALL, keeps last ID
-- DELETE FROM students WHERE age > 18;  -- Only deletes specific rows

-- AFTER TRUNCATE - IDs restart from 1:
TRUNCATE TABLE orders;
INSERT INTO orders (product) VALUES ('Laptop');  -- Gets order_id = 1
INSERT INTO orders (product) VALUES ('Mouse');   -- Gets order_id = 2
```

**Real-World Scenarios:**
- 🛒 Empty shopping cart after checkout
- 📊 Reset daily statistics table
- 🗑️ Clear temporary/staging data
- 🎮 New game season - reset leaderboard
- 📧 Clear processed email queue
- 🧪 Reset test environment data

**Important Notes:**
- ✅ Keeps table structure (columns, indexes)
- ✅ Very fast operation
- ✅ Resets AUTO_INCREMENT to 1
- ❌ Cannot use WHERE condition
- ❌ Deletes ALL rows only
- ⚠️ Cannot be undone - use carefully!

</details>

<details>
<summary> <strong> RENAME TABLE - Change table name <strong> </summary>

```sql
-- Single table rename
RENAME TABLE old_employees TO employees;

-- Multiple table rename
RENAME TABLE old_table1 TO new_table1,
             old_table2 TO new_table2;
```
</details>

<details>
<summary> <strong> CREATE INDEX - Manual index creation <strong> </summary>

```sql
-- Single column index
CREATE INDEX idx_name ON employees(name);

-- Composite index
CREATE INDEX idx_name_dept ON employees(name, dept_id);

-- Unique index
CREATE UNIQUE INDEX idx_email ON employees(email);

-- Full-text index
CREATE FULLTEXT INDEX idx_description ON products(description);
```
</details>

<details>
<summary> <strong> DROP INDEX - Remove index <strong> </summary>

```sql
DROP INDEX idx_name ON employees;
```
</details>

<details>
<summary> <strong> CREATE VIEW - Create virtual table <strong> </summary>

```sql
CREATE VIEW active_employees AS
SELECT id, name, email, salary
FROM employees
WHERE status = 'active';

-- Create or replace view
CREATE OR REPLACE VIEW high_earners AS
SELECT name, salary
FROM employees
WHERE salary > 100000;
```
</details>

<details>
<summary> <strong> DROP VIEW - Remove view <strong> </summary>

```sql
DROP VIEW IF EXISTS active_employees;
```
</details>

<details>
<summary> <strong> CREATE TEMPORARY TABLE - Create session-only table <strong> </summary>

```sql
CREATE TEMPORARY TABLE temp_results (
  id INT,
  total DECIMAL(10,2)
);
-- Automatically dropped when session ends
```
</details>

<details>
<summary> <strong> COMMENT - Add table/column comments <strong> </summary>

```sql
-- Add comment to table
ALTER TABLE employees COMMENT = 'Employee master data';

-- Add comment to column
ALTER TABLE employees 
MODIFY COLUMN salary DECIMAL(10,2) COMMENT 'Monthly salary in USD';

-- Add comment during table creation
CREATE TABLE products (
  id INT PRIMARY KEY COMMENT 'Product ID',
  name VARCHAR(100) COMMENT 'Product name'
) COMMENT = 'Product catalog';
```
</details>

<details>
<summary> <strong> CONSTRAINTS - Data integrity rules <strong> </summary>

```sql
-- Primary Key
ALTER TABLE employees ADD PRIMARY KEY (id);

-- Foreign Key
ALTER TABLE orders ADD CONSTRAINT fk_customer
  FOREIGN KEY (customer_id) REFERENCES customers(id)
  ON DELETE CASCADE
  ON UPDATE CASCADE;

-- Unique Constraint
ALTER TABLE employees ADD CONSTRAINT uq_email UNIQUE (email);

-- Check Constraint (MySQL 8.0.16+)
ALTER TABLE employees ADD CONSTRAINT chk_salary 
  CHECK (salary > 0);

-- Not Null
ALTER TABLE employees MODIFY COLUMN name VARCHAR(100) NOT NULL;

-- Default Value
ALTER TABLE employees ALTER COLUMN status SET DEFAULT 'active';
```
</details>

<details>
<summary> <strong> AUTO_INCREMENT - Manage auto-increment values <strong> </summary>

```sql
-- Reset auto-increment value
ALTER TABLE employees AUTO_INCREMENT = 1000;

-- View current auto-increment value
SHOW TABLE STATUS LIKE 'employees';
```
</details> 

---
### 1.3 Metadata/Information Commands (Not technically DDL):

<details>
<summary> <strong> DESCRIBE / SHOW COLUMNS - View table structure <strong> </summary>

```sql
-- Show table structure
DESCRIBE employees;
DESC employees;  -- shorthand

-- Alternative syntax
SHOW COLUMNS FROM employees;

-- Show detailed column info
SHOW FULL COLUMNS FROM employees;
```
</details>

<details>
<summary> <strong> SHOW TABLES - List all tables <strong> </summary>

```sql
-- Show all tables in current database
SHOW TABLES;

-- Show tables matching pattern
SHOW TABLES LIKE 'emp%';

-- Show tables with full information
SHOW FULL TABLES;
```
</details>

<details>
<summary> <strong> SHOW CREATE TABLE - View table creation SQL <strong> </summary>

```sql
SHOW CREATE TABLE employees;
```
</details>

---

### 1.4 Index Management

<details>
<summary> <strong> CREATE INDEX - Manual index creation <strong> </summary>

```sql
CREATE INDEX idx_email ON users(email);
CREATE INDEX idx_name_dept ON employees(name, department);
```
</details>

<details>
<summary> <strong> DROP INDEX - Remove index <strong> </summary>

```sql
DROP INDEX idx_email ON users;
```
</details>

---

### 1.5 Views (Virtual Tables)

<details>
<summary> <strong> CREATE VIEW - Create virtual table <strong> </summary>

```sql
CREATE VIEW active_customers AS
SELECT id, name, email FROM customers WHERE status = 'active';

-- Use like regular table
SELECT * FROM active_customers WHERE name LIKE 'A%';
```
</details>

> **💡 Pro tip:** Views don't store data - they're saved queries that run when accessed.

---

## 2. Query Clause Order

**$\color{ProcessBlue}{Write\ order\ (syntax):}$**
```sql
SELECT DISTINCT column        -- specify columns; DISTINCT removes duplicates
FROM table                    -- identify source table
JOIN other_table ON condition -- combine tables based on condition
WHERE filter                  -- filter individual rows before grouping
GROUP BY column               -- aggregate rows into groups
HAVING aggregate_filter       -- filter groups after aggregation
ORDER BY column               -- sort final results (ASC default, DESC for reverse)
LIMIT n OFFSET m;             -- skip m rows, return n rows
```

**$\color{ProcessBlue}{Execution\ order\ (engine\ processing):}$**
```
FROM → ON → JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```

**Key Differences:**
- **WHERE** runs before **SELECT** → can't use column aliases in WHERE
- **HAVING** runs after **GROUP BY** → filters aggregated groups, uses aggregate functions
- **ORDER BY** runs after **SELECT** → can use column aliases
- **DISTINCT** happens after **SELECT**, before sorting
- **Aggregate functions** (COUNT, SUM, AVG, MAX, MIN) only in SELECT/HAVING, not WHERE

> **Memory trick:** "From Where Groups Have Selected Orders Limited"

**Why this matters:** Execution order determines where aliases work and when aggregations are available—preventing logic errors.

---

## 3. Operators

**Comparison:**
`=` `!=` `>` `<` `>=` `<=`

**Logical:**
`AND` `OR` `NOT`

**Filtering:**
- `IN (value1, value2)` - matches any value in list
- `BETWEEN x AND y` - inclusive range
- `LIKE 'pattern%'` - pattern matching (% = wildcard)
- `IS NULL` / `IS NOT NULL` - null checks

**Operator precedence (high to low):**
Missing details — complete, precise SQL logical operator precedence (from highest to lowest):

1. **Parentheses `()`** — force explicit evaluation order.
2. **Arithmetic operators** `*, /, +, -` — handled before logical/comparison.
3. **Comparison operators** `=, >, <, >=, <=, <>, !=, IS, LIKE, IN, BETWEEN` — evaluated left to right.
4. **NOT** — unary logical negation, applies to next condition only.
5. **AND** — joins conditions; evaluated before OR.
6. **OR** — lowest logical precedence.

> **⚠️ Warning:** parentheses override all. Always use parentheses for clarity, even when not strictly needed.

---

## 4. Aggregation

**Functions:**
`COUNT()` `SUM()` `AVG()` `MIN()` `MAX()`

**Critical distinction:**
- `WHERE` filters **rows** before aggregation
- `HAVING` filters **aggregated results** after grouping

```sql
-- WHERE filters individual rows
SELECT department, AVG(salary)
FROM employees
WHERE salary > 50000  -- row filter
GROUP BY department
HAVING AVG(salary) > 80000;  -- aggregate filter
```

---

# PART 2: DATA RETRIEVAL $\color{blue}{Complex\ queries\}$

---

## 5. Joins

| Type | Logic |
|------|-------|
| `INNER JOIN` | Intersection only (matching rows) |
| `LEFT JOIN` | All left + matches from right |
| `RIGHT JOIN` | All right + matches from left |
| `FULL JOIN` | Union (all rows from both) |
| `CROSS JOIN` | Cartesian product (every combination) |

**Mental model:** INNER = strict overlap, LEFT/RIGHT = one side dominant, FULL = everything, CROSS = multiplication.

> **⚡ Performance:** INNER JOIN is typically fastest, CROSS JOIN most expensive. Filter early with WHERE to reduce rows before joining.

---

## 6. Subqueries

A query nested inside another query. Inner query executes first, outer query uses its result.

**Scalar subquery (returns single value):**
```sql
SELECT name FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

**Table subquery (returns multiple rows):**
```sql
SELECT * FROM employees
WHERE department_id IN (SELECT id FROM departments WHERE location = 'NYC');
```

**Correlated subquery (references outer query):**
```sql
SELECT e1.name FROM employees e1
WHERE salary > (SELECT AVG(salary) FROM employees e2 WHERE e2.dept = e1.dept);
```

---

## 7. Aliases

`AS` creates temporary names for columns or tables.

**Uses:**
- Readability: `SELECT first_name AS name`
- Calculated fields: `SELECT price * quantity AS total`
- Self-joins: `FROM employees e1 JOIN employees e2`

---

# PART 3: DATA INTEGRITY $\color{blue}{Prevent\ errors\}$

---

## 8. NULL Handling

`NULL` = unknown/missing value.

> **❌ Never use `= NULL` or `!= NULL`**

**Correct:**
- `IS NULL`
- `IS NOT NULL`

**Why:** NULL isn't a value—it's the absence of value. Standard comparison operators don't work.

**Critical:** Any arithmetic with NULL returns NULL:
- `5 + NULL = NULL`
- `NULL * 10 = NULL`

Use `COALESCE(column, 0)` to substitute NULL with a default value.

---

## 9. Constraints

Enforce data integrity rules:

- `PRIMARY KEY` - unique identifier, cannot be NULL
- `FOREIGN KEY` - references another table's primary key
- `UNIQUE` - no duplicates allowed
- `NOT NULL` - value required
- `DEFAULT` - fallback value if none provided
- `CHECK` - custom validation rule

---

## 10. Data Types

**Numeric:**
- `INT` - whole numbers
- `DECIMAL(p,s)` - fixed precision (use for money)

**Text:**
- `VARCHAR(n)` - variable length, max n characters
- `TEXT` - large text blocks

**Temporal:**
- `DATE` - calendar date only
- `DATETIME` - date + time

**Other:**
- `BOOLEAN` - true/false

**Choosing the right type:**

| ❌ Wrong | ✅ Right |
|----------|----------|
| `VARCHAR(255)` for everything | `VARCHAR(50)` for names |
| `FLOAT` for prices | `DECIMAL(10,2)` for prices |
| `VARCHAR` for IDs | `INT` or `BIGINT` for IDs |

---

## 11. Index Basics

**What:** Data structure that speeds up row retrieval.

**Trade-off:**
- ✅ Faster `SELECT` queries
- ❌ Slower `INSERT`/`UPDATE`/`DELETE`

**When to index:**
- Columns in WHERE clauses
- Columns in JOIN conditions
- Columns in ORDER BY (if frequently used)

**When NOT to index:**
- Small tables (<1000 rows)
- Columns with low cardinality (few unique values like gender)
- Columns updated frequently

> **💡 Automatic:** Primary keys auto-create an index.

---

# PART 4: DATA TRANSFORMATION $\color{blue}{Real-world\ formatting\}$

---

## 12. Common Query Patterns

**Top N results:**
```sql
SELECT * FROM products
ORDER BY sales DESC
LIMIT 10;
```

**Remove duplicates:**
```sql
SELECT DISTINCT category FROM products;
```

**Count all rows:**
```sql
SELECT COUNT(*) FROM orders;
```

---

## 13. DISTINCT vs GROUP BY

Both remove duplicates, but different use cases:

```sql
-- DISTINCT: Simple deduplication
SELECT DISTINCT city FROM customers;

-- GROUP BY: Deduplication + aggregation
SELECT city, COUNT(*) FROM customers GROUP BY city;
```

**Key insight:** `GROUP BY` is required when mixing aggregates with non-aggregated columns.

---

## 14. ORDER BY Behavior

**Default:** Ascending (`ASC`)  
**Explicit descending:** `DESC`

**NULL handling varies by database:**
- Some systems: NULLs sort first
- Others: NULLs sort last
- Control with: `NULLS FIRST` / `NULLS LAST` (if supported)

**Multi-column sorting:**
```sql
ORDER BY department ASC, salary DESC
-- Sorts by department first, then salary within each department
```

---

## 15. String Functions (Essential Subset)

<details>
<summary> <strong> CONCAT() - Combine strings <strong> </summary>

```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM employees;
-- Result: 'John Doe'
```
</details>

<details>
<summary> <strong> UPPER() / LOWER() - Case conversion <strong> </summary>

```sql
SELECT UPPER(email) FROM users;  -- 'JOHN@EXAMPLE.COM'
SELECT LOWER(email) FROM users;  -- 'john@example.com'
```

</details>

<details>
<summary> <strong> LENGTH() - String length <strong> </summary>

```sql
SELECT name, LENGTH(name) AS name_length FROM products;
-- 'Laptop' → 6
```
</details>


<details>
<summary> <strong> TRIM() - Remove whitespace <strong> </summary>

```sql
SELECT TRIM(name) FROM customers;  -- '  John  ' → 'John'
SELECT LTRIM(name) FROM customers; -- '  John' → 'John'
SELECT RTRIM(name) FROM customers; -- 'John  ' → 'John'
```

</details>

<details>
<summary> <strong> SUBSTRING() - Extract portion of string <strong> </summary>

```sql
SELECT SUBSTRING(phone, 1, 3) AS area_code FROM contacts;
-- '555-1234' → '555'

SELECT SUBSTRING(email, 1, POSITION('@' IN email) - 1) AS username FROM users;
-- 'john@example.com' → 'john'
```

</details>

---

## 16. Date Functions (Basic Operations)

<details>
<summary> <strong> NOW() / CURRENT_DATE - Current date/time <strong> </summary>

```sql
SELECT NOW();           -- '2024-11-06 14:30:45'
SELECT CURRENT_DATE;    -- '2024-11-06'
SELECT CURRENT_TIME;    -- '14:30:45'
```

</details>

<details>
<summary> <strong> DATE_ADD() / DATE_SUB() - Date arithmetic <strong> </summary>

```sql
-- Add 7 days
SELECT DATE_ADD(order_date, INTERVAL 7 DAY) AS delivery_date FROM orders;

-- Subtract 1 month
SELECT DATE_SUB(NOW(), INTERVAL 1 MONTH) AS last_month;

-- Add 2 years
SELECT DATE_ADD(hire_date, INTERVAL 2 YEAR) FROM employees;
```

</details>

<details>
<summary> <strong> DATEDIFF() - Days between dates <strong> </summary>

```sql
SELECT DATEDIFF(NOW(), order_date) AS days_since_order FROM orders;
-- Returns: 45 (days difference)

SELECT DATEDIFF('2024-12-31', '2024-01-01') AS days_in_year;
-- Returns: 365
```

</details>

<details>
<summary> <strong> EXTRACT() - Get year/month/day component <strong> </summary>

```sql
SELECT EXTRACT(YEAR FROM order_date) AS order_year FROM orders;
-- '2024-03-15' → 2024

SELECT EXTRACT(MONTH FROM hire_date) AS hire_month FROM employees;
-- '2024-03-15' → 3

SELECT EXTRACT(DAY FROM created_at) AS day_of_month FROM posts;
-- '2024-03-15' → 15
```

</details>

<details>
<summary> <strong> Common date filtering patterns <strong> </summary>

```sql
-- Orders from last 30 days
SELECT * FROM orders 
WHERE order_date >= DATE_SUB(NOW(), INTERVAL 30 DAY);

-- Orders from current year
SELECT * FROM orders 
WHERE EXTRACT(YEAR FROM order_date) = EXTRACT(YEAR FROM NOW());

-- Orders from specific month
SELECT * FROM orders 
WHERE order_date >= '2024-03-01' AND order_date < '2024-04-01';
```
</details>


> **📊 Stat:** Time-based filtering is in ~70% of business queries.

---

## 17. CASE Statements

SQL's if-then-else logic:

**Searched CASE (most flexible):**
```sql
SELECT name,
  CASE 
    WHEN salary < 50000 THEN 'Junior'
    WHEN salary < 100000 THEN 'Mid'
    ELSE 'Senior'
  END AS level
FROM employees;
```

**Simple CASE (cleaner for equality checks):**
```sql
SELECT 
  CASE status
    WHEN 'pending' THEN 'In Progress'
    WHEN 'done' THEN 'Complete'
  END AS status_label
FROM tasks;
```
---

# PART 5: ADVANCED OPERATIONS $\color{blue}{Production\ readiness\}$

---

## 18. Transaction Basics

```sql
BEGIN TRANSACTION;
  UPDATE accounts SET balance = balance - 100 WHERE id = 1;
  UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT; -- or ROLLBACK if error
```

**ACID properties:**
- **A**tomicity: All or nothing
- **C**onsistency: Valid state always
- **I**solation: Concurrent transactions don't interfere
- **D**urability: Committed changes persist

---

## 19. UNION vs UNION ALL

Combine results from multiple queries:

- `UNION` - removes duplicates (slower)
- `UNION ALL` - keeps duplicates (faster)

```sql
SELECT name FROM customers
UNION
SELECT name FROM suppliers;
```

**Requirement:** Same number of columns, compatible data types.

> **⚠️ Performance:** `UNION` must sort and deduplicate. Use `UNION ALL` unless duplicates are truly problematic.

---

## 20. Wildcards in LIKE

- `%` - matches zero or more characters
- `_` - matches exactly one character

```sql
WHERE name LIKE 'A%'    -- Starts with A
WHERE name LIKE '%son'  -- Ends with son
WHERE name LIKE 'J_n'   -- Jon, Jan, Jin (but not John)
```

---

## 21. Implicit vs Explicit Joins

**Explicit (modern, preferred):**
```sql
SELECT * FROM orders
JOIN customers ON orders.customer_id = customers.id;
```

**Implicit (old-style, avoid):**
```sql
SELECT * FROM orders, customers
WHERE orders.customer_id = customers.id;
```

---

## 22. Alias Best Practices

**When aliases are required:**
- Self-joins: `FROM employees e1 JOIN employees e2`
- Multiple tables with same column names
- Subqueries in FROM: `FROM (SELECT ...) AS subquery`

**Style guide:**
- Table aliases: short (e1, e2, o, c)
- Column aliases: descriptive (total_revenue, customer_name)

---

## 23. COUNT Nuances

- `COUNT(*)` - counts all rows (including NULLs)
- `COUNT(column)` - counts non-NULL values in that column
- `COUNT(DISTINCT column)` - counts unique non-NULL values

```sql
SELECT 
  COUNT(*) AS total_rows,
  COUNT(email) AS rows_with_email,
  COUNT(DISTINCT email) AS unique_emails
FROM customers;
```

---

## 24. Common Beginner Mistakes

### Logic Errors

<details>
<summary><strong>❌ Missing GROUP BY with aggregates</strong></summary>

```sql
-- Wrong
SELECT name, COUNT(*) FROM employees;

-- Right
SELECT name, COUNT(*) FROM employees GROUP BY name;
```

</details>

<details>
<summary><strong>❌ Using WHERE instead of HAVING for aggregates</strong></summary>

```sql
-- Wrong
SELECT department, AVG(salary)
FROM employees
WHERE AVG(salary) > 50000
GROUP BY department;

-- Right
SELECT department, AVG(salary)
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

</details>

<details>
<summary><strong>❌ Wrong NULL comparison</strong></summary>

```sql
-- Wrong
WHERE column = NULL

-- Right
WHERE column IS NULL
```
</details>

### Performance Killers

<details>
<summary><strong>❌ Forgetting to filter before joining</strong></summary>

```sql
-- Slow: joins millions of rows then filters
SELECT * FROM orders o
JOIN customers c ON o.customer_id = c.id
WHERE o.order_date > '2024-01-01';

-- Fast: filters first, then joins
SELECT * FROM orders o
JOIN customers c ON o.customer_id = c.id
WHERE o.order_date > '2024-01-01'  -- Filter applied early
```

</details>

<details>
<summary><strong>❌ Using SELECT * in production</strong></summary>

```sql
-- Bad: breaks when columns change, wastes bandwidth
SELECT * FROM users;

-- Good: explicit columns
SELECT id, name, email FROM users;
```

</details>

<details>
<summary><strong>❌ No LIMIT in exploratory queries</strong></summary>

```sql
-- Dangerous: might return millions of rows
SELECT * FROM logs;

-- Safe: limit results
SELECT * FROM logs LIMIT 100;
```

</details>

<details>
<summary><strong>❌ Using functions on indexed columns</strong></summary>

```sql
-- Breaks index
WHERE YEAR(created_at) = 2024;

-- Uses index
WHERE created_at >= '2024-01-01' AND created_at < '2025-01-01';
```

</details>
### Data Issues

<details>
<summary><strong>❌ Comparing dates as strings</strong></summary>

```sql
-- Wrong: string comparison
WHERE date_column > '2024-1-1'  -- Fails for Feb-Dec

-- Right: proper date format
WHERE date_column > '2024-01-01'
```

</details>


<details>
<summary><strong>❌ Forgetting table prefixes in multi-table queries</strong></summary>

```sql
-- Ambiguous
SELECT name FROM customers c
JOIN orders o ON c.id = o.customer_id;

-- Clear
SELECT c.name FROM customers c
JOIN orders o ON c.id = o.customer_id;
```

</details>

<details>
<summary><strong>❌ Not handling empty result sets</strong></summary>

```sql
-- Dangerous: causes app crash if no rows
SELECT id FROM users WHERE email = 'test@example.com';

-- Safe: handle NULL case
SELECT COALESCE(MAX(id), 0) FROM users WHERE email = 'test@example.com';
```

</details>

### Join Mistakes


<details>
<summary><strong>❌ Joining without ON clause (accidental CROSS JOIN) </strong></summary>

```sql
-- Wrong: cartesian product
SELECT * FROM orders, customers;

-- Right: explicit condition
SELECT * FROM orders o
JOIN customers c ON o.customer_id = c.id;
```

</details>

<details>
<summary><strong>❌ Using UNION when UNION ALL suffices</strong></summary>

```sql
-- Slow: unnecessary deduplication
SELECT id FROM table1
UNION
SELECT id FROM table2;

-- Fast: when duplicates don't matter
SELECT id FROM table1
UNION ALL
SELECT id FROM table2;
```

</details>

### Subquery Errors

<details>
<summary><strong>❌ Subquery returns multiple rows when expecting one</strong></summary>

```sql
-- Fails if subquery returns >1 row
WHERE salary > (SELECT salary FROM employees WHERE department = 'Sales');

-- Safe: use aggregate
WHERE salary > (SELECT AVG(salary) FROM employees WHERE department = 'Sales');
```

</details>

---

## 25. Core Commands - Quick Reference

### Data Manipulation
```sql
-- SELECT
SELECT column FROM table WHERE condition;

-- INSERT single
INSERT INTO table (col1, col2) VALUES (val1, val2);

-- INSERT multiple
INSERT INTO table (col1, col2) VALUES (v1, v2), (v3, v4);

-- INSERT from query
INSERT INTO archive SELECT * FROM orders WHERE date < '2024-01-01';

-- UPDATE simple
UPDATE table SET col1 = val1 WHERE condition;

-- UPDATE with JOIN
UPDATE table1 t1
JOIN table2 t2 ON t1.id = t2.id
SET t1.col = val WHERE condition;

-- DELETE
DELETE FROM table WHERE condition;

-- DELETE with JOIN
DELETE t1 FROM table1 t1
JOIN table2 t2 ON t1.id = t2.id
WHERE condition;
```

### Table Operations
```sql
-- CREATE
CREATE TABLE name (
  id INT PRIMARY KEY,
  col VARCHAR(50) NOT NULL
);

-- ALTER - add column
ALTER TABLE name ADD COLUMN col TYPE;

-- ALTER - drop column
ALTER TABLE name DROP COLUMN col;

-- DROP
DROP TABLE IF EXISTS name;

-- TRUNCATE
TRUNCATE TABLE name;
```

### Index & View
```sql
-- CREATE INDEX
CREATE INDEX idx_name ON table(column);

-- DROP INDEX
DROP INDEX idx_name ON table;

-- CREATE VIEW
CREATE VIEW view_name AS SELECT ... ;
```

---

## Hands-On Practice Problems

**Problem 1:** Find customers who have never placed an order.  
**Hint:** Use LEFT JOIN + IS NULL

**Problem 2:** Get the top 3 highest-paid employees per department.  
**Hint:** Use subquery with GROUP BY or window functions

**Problem 3:** Calculate running total of sales by date.  
**Hint:** Self-join with date comparison (or window functions if allowed)

**Problem 4:** Find duplicate email addresses.  
**Hint:** GROUP BY + HAVING COUNT(*) > 1

**Problem 5:** List products with above-average prices in their category.  
**Hint:** Correlated subquery or JOIN with aggregated subquery

---

## Complete Memorization Test

### Questions

1. What's the execution order difference from write order?
2. `WHERE` vs `HAVING` - which filters what?
3. What's the output of `LEFT JOIN` if right table has no match?
4. Why can't you use `column = NULL`?
5. Name all 5 aggregate functions.
6. When do you NEED `GROUP BY` vs when can you use `DISTINCT`?
7. What does `UNION` do that `UNION ALL` doesn't?
8. Write a `CASE` statement that categorizes numbers as "low" (<10) or "high".
9. What's the difference between `COMMIT` and `ROLLBACK`?
10. Does `%` in `LIKE` match zero characters or must it match at least one?
11. What does `COUNT(*)` count that `COUNT(column)` doesn't?
12. Name 3 situations where you should NOT create an index.

### Core Commands Syntax Quiz

Write the syntax from memory (no peeking!):

13. INSERT multiple rows at once
14. UPDATE a table using data from another table (with JOIN)
15. DELETE rows based on condition from another table (with JOIN)
16. Add a new column to existing table
17. Create an index on multiple columns
18. Create a view from a SELECT query
19. INSERT data from a SELECT query result
20. TRUNCATE vs DELETE - when to use which?

---

<details>
<summary><b>Click to reveal answers</b></summary>

### Answers 1-12

1. **Execution order:** FROM first (loads data), WHERE filters rows, GROUP BY aggregates, HAVING filters groups, SELECT picks columns, ORDER BY sorts, LIMIT restricts output
2. **WHERE** filters individual rows before grouping; **HAVING** filters aggregated results after grouping
3. LEFT JOIN returns all left rows + **NULL** for unmatched right columns
4. NULL represents "unknown," not a value. Use `IS NULL` or `IS NOT NULL`
5. `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`
6. Use **DISTINCT** for simple deduplication; use **GROUP BY** when you need aggregation or mixing aggregates with non-aggregated columns
7. **UNION** removes duplicates (sorts/deduplicates), **UNION ALL** keeps all rows (faster)
8. `CASE WHEN number < 10 THEN 'low' ELSE 'high' END`
9. **COMMIT** saves all changes permanently; **ROLLBACK** undoes all changes since BEGIN TRANSACTION
10. `%` matches **zero or more** characters (including zero)
11. `COUNT(*)` counts rows with NULL values; `COUNT(column)` only counts non-NULL values
12. Don't index: small tables (<1000 rows), low-cardinality columns (e.g., gender), frequently updated columns

### Answers 13-20

13. `INSERT INTO table (col1, col2) VALUES (v1, v2), (v3, v4);`
14. `UPDATE t1 JOIN t2 ON t1.id = t2.id SET t1.col = value WHERE condition;`
15. `DELETE t1 FROM table1 t1 JOIN table2 t2 ON t1.id = t2.id WHERE condition;`
16. `ALTER TABLE name ADD COLUMN col_name TYPE;`
17. `CREATE INDEX idx_name ON table(col1, col2);`
18. `CREATE VIEW view_name AS SELECT columns FROM table WHERE condition;`
19. `INSERT INTO target_table SELECT columns FROM source_table WHERE condition;`
20. **TRUNCATE:** Faster, resets auto-increment, can't be rolled back, removes all rows. **DELETE:** Slower, can be selective with WHERE, can be rolled back, row-by-row operation

</details>

---

## Mastery Checkpoint

### ✅ You're ready for intermediate SQL when you can:

- Write queries with joins, subqueries, and aggregations without reference
- Explain execution order and predict query results
- Debug common errors (NULL comparisons, GROUP BY violations)
- Choose appropriate data types and constraints
- Use CASE, string functions, and date operations fluently
- Understand when to use indexes and transactions
- Write optimized queries (UNION ALL vs UNION, filtering before joins)
- Recall all core command syntax patterns from memory

### 🚀 Next Level Topics

After mastering these foundations, progress to:
- CTEs (Common Table Expressions) - `WITH` clauses
- Window Functions - `ROW_NUMBER()`, `RANK()`, `LAG()`, `LEAD()`
- Query Optimization - EXPLAIN plans, query profiling
- Advanced Indexing - Composite indexes, covering indexes
- Normalization - 1NF, 2NF, 3NF, denormalization strategies
- Stored Procedures & Functions
- Triggers & Events

---

## Study Recommendations

### 📅 3-Week Intensive Path (3 hours/day)

**Total Time Investment:** 63 hours over 21 days

---

**Week 1: Foundation Building (21 hours)**

*Day 1-2:* Section 1 (Core Commands) - 6 hours
- Hour 1: Read and understand all syntax
- Hour 2-3: Practice INSERT, UPDATE, DELETE variations
- Hour 4-5: Practice ALTER TABLE, CREATE INDEX, Views
- Hour 6: Build a sample database with all commands

*Day 3:* Sections 2-3 (Query Order & Operators) - 3 hours
- Hour 1: Memorize execution order
- Hour 2: Practice all operators with real queries
- Hour 3: Write queries explaining execution flow

*Day 4-5:* Section 4-5 (Aggregation & Joins) - 6 hours
- Hour 1-2: Master WHERE vs HAVING with examples
- Hour 3-4: Practice all 5 JOIN types extensively
- Hour 5-6: Combine aggregations with joins

*Day 6-7:* Sections 6-7 (Subqueries & Aliases) - 6 hours
- Hour 1-2: Scalar and table subqueries
- Hour 3-4: Correlated subqueries (tricky!)
- Hour 5-6: Complex queries with aliases and subqueries

---

**Week 2: Data Integrity & Transformation (21 hours)**

*Day 8:* Sections 8-9 (NULL & Constraints) - 3 hours
- Hour 1: NULL handling patterns
- Hour 2: Practice with COALESCE, NULLIF
- Hour 3: Design tables with proper constraints

*Day 9:* Sections 10-11 (Data Types & Indexes) - 3 hours
- Hour 1: Choose appropriate types for scenarios
- Hour 2: Index strategy planning
- Hour 3: Measure query performance with/without indexes

*Day 10-11:* Sections 12-14 (Patterns, DISTINCT, ORDER BY) - 6 hours
- Hour 1-2: Common patterns practice
- Hour 3-4: DISTINCT vs GROUP BY scenarios
- Hour 5-6: Complex sorting with NULLs

*Day 12-14:* Sections 15-17 (Functions & CASE) - 9 hours
- Hour 1-3: String functions for data cleaning
- Hour 4-6: Date calculations and time-based queries
- Hour 7-9: CASE statement variations and nested logic

---

**Week 3: Advanced Operations & Mastery (21 hours)**

*Day 15-16:* Sections 18-20 (Transactions, UNION, Wildcards) - 6 hours
- Hour 1-2: Transaction scenarios and ACID properties
- Hour 3-4: UNION vs UNION ALL performance testing
- Hour 5-6: Pattern matching with LIKE wildcards

*Day 17:* Sections 21-23 (Joins, Aliases, COUNT) - 3 hours
- Hour 1: Join syntax comparison
- Hour 2: Alias best practices in complex queries
- Hour 3: COUNT variations and edge cases

*Day 18:* Section 24-25 (Mistakes & Quick Reference) - 3 hours
- Hour 1: Review all common mistakes
- Hour 2: Debug intentionally broken queries
- Hour 3: Speed drill on core command syntax

*Day 19-20:* Practice Problems - 6 hours
- Hour 1-2: Problems 1-2
- Hour 3-4: Problems 3-4
- Hour 5-6: Problem 5 + create your own variations

*Day 21:* Final Assessment - 3 hours
- Hour 1: Complete memorization test (all 20 questions)
- Hour 2: Build mini-project from scratch
- Hour 3: Review weak areas and plan next steps

---

### 🎯 Daily 3-Hour Practice Routine

**Hour 1 - Learn (Deep Study)**
- Read section thoroughly
- Take notes in your own words
- Understand WHY, not just HOW

**Hour 2 - Practice (Active Coding)**
- Write 8-10 queries from scratch
- Experiment with variations
- Break things intentionally and fix them

**Hour 3 - Apply (Problem Solving)**
- Solve real-world scenarios
- Combine multiple concepts
- Optimize and refactor queries

### ✅ Validation Checklist

- [ ] Can write all core commands without reference
- [ ] Understand and explain execution order
- [ ] Score 100% on memorization test
- [ ] Complete all 5 practice problems
- [ ] Can identify and fix all listed mistakes
- [ ] Built at least one small database project

---

> **Remember:** SQL mastery comes from repetition. Write queries daily, make mistakes, debug them, and iterate. This guide is your reference—return to it often!
