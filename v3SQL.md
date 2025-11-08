# **SQL Command Classification System**
## **Complete Memory Framework with Annotated Examples + Function Reference**

---

## **🎯 Core Compression Principle**

**Every SQL command answers ONE question:**
1. **What exists?** → DDL (Structure)
2. **What changed?** → DML (Data)
3. **What do I see?** → DQL (Query)
4. **Who can act?** → DCL (Access)
5. **When does it count?** → TCL (Control)

---

## **📋 QUICK REFERENCE: All SQL Functions by Category**

### **📦 DDL (Data Definition Language) - Structure**
**Context:** Defines and modifies database architecture - tables, columns, indexes, views, schemas

**Commands:**
- CREATE (TABLE, INDEX, VIEW, SCHEMA, DATABASE, SEQUENCE)
- ALTER (TABLE, INDEX, VIEW, SCHEMA, DATABASE)
- DROP (TABLE, INDEX, VIEW, SCHEMA, DATABASE, SEQUENCE)
- TRUNCATE
- RENAME
- COMMENT

**Key Objects:**
- Tables & Columns
- Primary Keys & Foreign Keys
- Indexes (B-tree, Hash, Full-text)
- Views (Regular, Materialized)
- Schemas/Databases
- Sequences/Auto-increment
- Constraints (NOT NULL, UNIQUE, CHECK, DEFAULT)

---

### **📝 DML (Data Manipulation Language) - Data**
**Context:** Modifies actual row data within existing tables

**Commands:**
- INSERT (single, bulk, SELECT INTO)
- UPDATE (single, multiple columns, JOIN updates)
- DELETE (conditional, cascading)
- MERGE (UPSERT - INSERT or UPDATE)
- REPLACE (MySQL - DELETE then INSERT)
- CALL (stored procedures that modify data)
- LOAD DATA (bulk import)
- COPY (PostgreSQL bulk operations)

**Modifiers:**
- SET (for UPDATE)
- VALUES (for INSERT)
- WHERE (for conditional changes)
- RETURNING (PostgreSQL - return modified rows)
- OUTPUT (SQL Server - return modified rows)

---

### **🔍 DQL (Data Query Language) - Query**
**Context:** Retrieves, filters, calculates, and analyzes data without modification

#### **Core Query Commands:**
- SELECT
- FROM
- WHERE
- ORDER BY
- GROUP BY
- HAVING
- LIMIT / TOP / FETCH
- DISTINCT
- OFFSET

#### **Join Operations:**
- INNER JOIN
- LEFT JOIN (LEFT OUTER JOIN)
- RIGHT JOIN (RIGHT OUTER JOIN)
- FULL JOIN (FULL OUTER JOIN)
- CROSS JOIN
- SELF JOIN
- NATURAL JOIN

#### **Set Operations:**
- UNION
- UNION ALL
- INTERSECT
- EXCEPT (MINUS in Oracle)

#### **Subquery Types:**
- Scalar subqueries
- Row subqueries
- Table subqueries
- Correlated subqueries
- EXISTS / NOT EXISTS
- IN / NOT IN
- ANY / ALL

#### **Aggregate Functions:**
- COUNT()
- SUM()
- AVG()
- MAX()
- MIN()
- GROUP_CONCAT() / STRING_AGG()
- STDDEV() / VARIANCE()

#### **Window Functions:**
- ROW_NUMBER()
- RANK()
- DENSE_RANK()
- NTILE()
- LAG()
- LEAD()
- FIRST_VALUE()
- LAST_VALUE()
- PARTITION BY
- OVER()

#### **String Functions:**
- CONCAT() / ||
- UPPER() / LOWER()
- LENGTH() / LEN()
- SUBSTRING() / SUBSTR()
- TRIM() / LTRIM() / RTRIM()
- REPLACE()
- LEFT() / RIGHT()
- POSITION() / INSTR()
- REVERSE()
- REPEAT()
- LPAD() / RPAD()

#### **Numeric Functions:**
- ROUND()
- CEIL() / CEILING()
- FLOOR()
- ABS()
- POWER() / POW()
- SQRT()
- MOD()
- SIGN()
- TRUNCATE()
- RAND() / RANDOM()

#### **Date/Time Functions:**
- NOW() / CURRENT_TIMESTAMP
- CURDATE() / CURRENT_DATE
- CURTIME() / CURRENT_TIME
- DATE()
- TIME()
- YEAR() / MONTH() / DAY()
- HOUR() / MINUTE() / SECOND()
- DATEDIFF()
- DATE_ADD() / DATE_SUB()
- DATE_FORMAT() / TO_CHAR()
- EXTRACT()
- TIMESTAMPDIFF()

#### **Conversion Functions:**
- CAST()
- CONVERT()
- TO_NUMBER()
- TO_DATE()
- TO_CHAR()
- STR()

#### **Conditional Functions:**
- CASE WHEN ... THEN ... ELSE ... END
- IF()
- IFNULL() / ISNULL()
- NULLIF()
- COALESCE()
- NVL() (Oracle)

#### **Advanced Query Features:**
- Common Table Expressions (CTE) - WITH clause
- Recursive CTEs
- LATERAL joins
- Views as subqueries
- Derived tables
- Table expressions

---

### **🔐 DCL (Data Control Language) - Access**
**Context:** Manages user permissions, roles, and database security

**User Management:**
- CREATE USER
- ALTER USER
- DROP USER
- RENAME USER

**Role Management:**
- CREATE ROLE
- ALTER ROLE
- DROP ROLE
- SET ROLE

**Permission Commands:**
- GRANT (assign privileges)
- REVOKE (remove privileges)
- DENY (explicitly block - SQL Server)

**Privilege Types:**
- SELECT
- INSERT
- UPDATE
- DELETE
- EXECUTE (stored procedures/functions)
- CREATE
- ALTER
- DROP
- INDEX
- REFERENCES (foreign keys)
- ALL PRIVILEGES

**Permission Scopes:**
- Database level
- Schema level
- Table level
- Column level
- Stored procedure level
- WITH GRANT OPTION (allow grantee to grant to others)

**Security Views:**
- SHOW GRANTS
- USER_TAB_PRIVS
- INFORMATION_SCHEMA.TABLE_PRIVILEGES

---

### **⚙️ TCL (Transaction Control Language) - Control**
**Context:** Manages transaction boundaries, ensuring data consistency and recoverability

**Transaction Commands:**
- BEGIN TRANSACTION / START TRANSACTION
- COMMIT (save changes)
- ROLLBACK (undo changes)
- SAVEPOINT (create checkpoint)
- ROLLBACK TO SAVEPOINT (partial undo)
- RELEASE SAVEPOINT (remove checkpoint)

**Transaction Configuration:**
- SET TRANSACTION
- SET AUTOCOMMIT (ON/OFF)

**Isolation Levels:**
- READ UNCOMMITTED (lowest isolation)
- READ COMMITTED (default in most DBs)
- REPEATABLE READ
- SERIALIZABLE (highest isolation)

**Lock Management:**
- LOCK TABLES
- UNLOCK TABLES
- SELECT ... FOR UPDATE (row-level lock)
- SELECT ... FOR SHARE (shared lock)

**Transaction Properties (ACID):**
- Atomicity (all or nothing)
- Consistency (valid state transitions)
- Isolation (concurrent transaction separation)
- Durability (permanent after commit)

---

## **🗂️ Category Summary Table**

| Category | Focus | Auto-Commit? | Affects | Primary Use |
|----------|-------|--------------|---------|-------------|
| **DDL** | Structure | ✅ Yes | Schema objects | Define database architecture |
| **DML** | Data | ❌ No* | Table rows | Modify stored data |
| **DQL** | Query | N/A | Nothing (read-only) | Retrieve and calculate information |
| **DCL** | Access | ✅ Yes | User permissions | Control who can do what |
| **TCL** | Control | ✅ COMMIT does | Transaction state | Ensure safe, atomic changes |

*DML can be rolled back when inside explicit transaction

---

## **📦 CATEGORY 1: DDL (Data Definition Language)**
### **Purpose: Define database STRUCTURE**

**Mental Model:** *Architecture blueprint — builds the warehouse*

### Commands:
```
CREATE   → Build new objects
ALTER    → Modify existing objects
DROP     → Permanently delete objects
TRUNCATE → Empty table (keep structure)
RENAME   → Change object names
```

### **Example Code Block:**
```sql
-- PROBLEM: Need to store employee information in the database
-- SOLUTION: Create a new table with appropriate columns and constraints
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    hire_date DATE,
    salary DECIMAL(10, 2)
);

-- PROBLEM: Table exists but missing a column for department tracking
-- SOLUTION: Add a new column to existing table structure
ALTER TABLE employees 
ADD COLUMN department VARCHAR(50);

-- PROBLEM: Salary column needs to store larger values (wasn't big enough)
-- SOLUTION: Modify the data type to accommodate bigger numbers
ALTER TABLE employees 
MODIFY COLUMN salary DECIMAL(12, 2);

-- PROBLEM: Queries filtering by last_name are too slow
-- SOLUTION: Create an index to speed up searches on last_name column
CREATE INDEX idx_last_name 
ON employees(last_name);

-- PROBLEM: Need a simplified view of employees without exposing salary data
-- SOLUTION: Create a virtual table (view) that shows only specific columns
CREATE VIEW active_employees AS
SELECT employee_id, first_name, last_name, department
FROM employees
WHERE hire_date >= '2023-01-01';

-- PROBLEM: Need to delete all temporary log data but keep table structure
-- SOLUTION: Use TRUNCATE to quickly remove all rows (faster than DELETE)
TRUNCATE TABLE temp_logs;

-- PROBLEM: Table is no longer needed and must be removed from database
-- SOLUTION: Permanently delete the table and all its data
DROP TABLE old_records;

-- PROBLEM: Table name doesn't follow new naming convention
-- SOLUTION: Rename table to match company standards
RENAME TABLE employees TO staff_members;
```

**Key Trait:** Changes are **immediate and auto-committed** (no ROLLBACK)

---

## **📝 CATEGORY 2: DML (Data Manipulation Language)**
### **Purpose: Modify ACTUAL DATA**

**Mental Model:** *Warehouse operations — moving boxes*

### Commands:
```
INSERT → Add new rows
UPDATE → Modify existing rows
DELETE → Remove rows
MERGE  → Conditional INSERT/UPDATE (upsert)
```

### **Example Code Block:**
```sql
-- PROBLEM: New employee joined the company, need to add their record
-- SOLUTION: Insert a single row with all employee details
INSERT INTO employees (employee_id, first_name, last_name, email, hire_date, salary)
VALUES (1, 'John', 'Doe', 'john.doe@company.com', '2024-01-15', 75000.00);

-- PROBLEM: Multiple new hires need to be added at once (batch onboarding)
-- SOLUTION: Insert multiple rows in a single statement for efficiency
INSERT INTO employees (employee_id, first_name, last_name, email, hire_date, salary)
VALUES 
    (2, 'Jane', 'Smith', 'jane.smith@company.com', '2024-02-01', 82000.00),
    (3, 'Bob', 'Johnson', 'bob.j@company.com', '2024-03-10', 68000.00);

-- PROBLEM: Employee got promoted with salary increase and department change
-- SOLUTION: Update multiple columns for a specific employee
UPDATE employees 
SET salary = salary * 1.10,
    department = 'Engineering'
WHERE employee_id = 1;

-- PROBLEM: All Sales employees hired before 2023 deserve a 5% raise
-- SOLUTION: Update multiple rows based on conditional criteria
UPDATE employees 
SET salary = salary * 1.05
WHERE department = 'Sales' AND hire_date < '2023-01-01';

-- PROBLEM: Employee left the company, record must be removed
-- SOLUTION: Delete a specific row using unique identifier
DELETE FROM employees 
WHERE employee_id = 3;

-- PROBLEM: Clean up incomplete records (old employees without department)
-- SOLUTION: Delete multiple rows matching specific conditions
DELETE FROM employees 
WHERE hire_date < '2020-01-01' AND department IS NULL;

-- PROBLEM: Need to sync data from temp table - update if exists, insert if new
-- SOLUTION: Use MERGE to handle both insert and update in one operation (upsert)
MERGE INTO employees AS target
USING temp_employees AS source
ON target.employee_id = source.employee_id
WHEN MATCHED THEN
    UPDATE SET salary = source.salary, department = source.department
WHEN NOT MATCHED THEN
    INSERT (employee_id, first_name, last_name, email, salary)
    VALUES (source.employee_id, source.first_name, source.last_name, 
            source.email, source.salary);
```

**Key Trait:** Changes are **transactional** (can ROLLBACK before COMMIT)

---

## **🔍 CATEGORY 3: DQL (Data Query Language)**
### **Purpose: READ and COMPUTE data**

**Mental Model:** *Warehouse inspection — just looking, not touching*

### **Example Code Block:**

#### **Basic Retrieval:**
```sql
-- PROBLEM: Need to see names and salaries of all employees
-- SOLUTION: Select specific columns from the table
SELECT first_name, last_name, salary 
FROM employees;

-- PROBLEM: Need to find employees earning more than $70,000
-- SOLUTION: Filter rows using WHERE clause with condition
SELECT * FROM employees 
WHERE salary > 70000;

-- PROBLEM: Need employee list sorted by salary (highest first), then by name
-- SOLUTION: Use ORDER BY with multiple columns (DESC for descending)
SELECT first_name, last_name, salary 
FROM employees 
ORDER BY salary DESC, last_name ASC;

-- PROBLEM: Only need to see the top 5 highest-paid employees
-- SOLUTION: Use LIMIT to restrict number of returned rows
SELECT * FROM employees 
ORDER BY salary DESC 
LIMIT 5;
```

#### **Aggregate Functions:**
```sql
-- PROBLEM: How many employees work at the company?
-- SOLUTION: Count all rows in the table
SELECT COUNT(*) AS total_employees 
FROM employees;

-- PROBLEM: Need salary statistics - average, minimum, and maximum
-- SOLUTION: Use aggregate functions to calculate summary statistics
SELECT 
    AVG(salary) AS avg_salary,
    MIN(salary) AS min_salary,
    MAX(salary) AS max_salary
FROM employees;

-- PROBLEM: What's the total payroll cost for Engineering department?
-- SOLUTION: Sum all salaries for employees in specific department
SELECT SUM(salary) AS total_payroll 
FROM employees 
WHERE department = 'Engineering';
```

#### **Grouping & Filtering:**
```sql
-- PROBLEM: Need employee count and average salary per department
-- SOLUTION: Group rows by department and calculate aggregates for each group
SELECT department, COUNT(*) AS employee_count, AVG(salary) AS avg_salary
FROM employees
GROUP BY department;

-- PROBLEM: Which departments have average salary above $75,000?
-- SOLUTION: Filter grouped results using HAVING (not WHERE - HAVING filters groups)
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 75000;
```

#### **Joins:**
```sql
-- PROBLEM: Need employee names with their department names (data in 2 tables)
-- SOLUTION: Join employees and departments tables on matching department_id
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id;

-- PROBLEM: Show all employees even if they don't have a department assigned
-- SOLUTION: Use LEFT JOIN to keep all employees, show NULL for missing departments
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.department_id;

-- PROBLEM: Need employee, department, and project info (3 tables)
-- SOLUTION: Chain multiple joins to connect related tables
SELECT e.first_name, e.last_name, d.department_name, p.project_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id
LEFT JOIN projects p ON e.employee_id = p.project_lead_id;
```

#### **Combining Queries:**
```sql
-- PROBLEM: Need combined list of Sales employees and active contractors
-- SOLUTION: Use UNION to merge two result sets (removes duplicates)
SELECT first_name, last_name FROM employees WHERE department = 'Sales'
UNION
SELECT first_name, last_name FROM contractors WHERE active = 1;

-- PROBLEM: Need all customer IDs from both years (duplicates OK for counting)
-- SOLUTION: Use UNION ALL to keep duplicate values in combined result
SELECT customer_id FROM orders_2023
UNION ALL
SELECT customer_id FROM orders_2024;
```

#### **Subqueries:**
```sql
-- PROBLEM: Find employees earning more than company average
-- SOLUTION: Use subquery to calculate average, then compare in WHERE clause
SELECT first_name, last_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- PROBLEM: Get departments with above-average salaries (complex filtering)
-- SOLUTION: Use subquery in FROM clause to create temporary result set
SELECT dept, avg_sal
FROM (
    SELECT department AS dept, AVG(salary) AS avg_sal
    FROM employees
    GROUP BY department
) AS dept_stats
WHERE avg_sal > 70000;
```

#### **Scalar Functions:**
```sql
-- PROBLEM: Need full names, uppercase emails, and name length for mailing labels
-- SOLUTION: Use string functions to manipulate and format text data
SELECT 
    CONCAT(first_name, ' ', last_name) AS full_name,
    UPPER(email) AS email_upper,
    LENGTH(last_name) AS name_length
FROM employees;

-- PROBLEM: Calculate employment duration and format hire dates nicely
-- SOLUTION: Use date functions to calculate differences and format dates
SELECT 
    first_name,
    hire_date,
    DATEDIFF(CURDATE(), hire_date) AS days_employed,
    DATE_FORMAT(hire_date, '%M %d, %Y') AS formatted_date
FROM employees;

-- PROBLEM: Project salary with 10% raise and calculate monthly pay
-- SOLUTION: Use numeric functions for calculations and rounding
SELECT 
    salary,
    ROUND(salary * 1.10, 2) AS salary_with_raise,
    FLOOR(salary / 12) AS monthly_salary
FROM employees;

-- PROBLEM: Categorize employees into salary brackets for reporting
-- SOLUTION: Use CASE statement to create conditional categories
SELECT 
    first_name,
    last_name,
    salary,
    CASE 
        WHEN salary > 100000 THEN 'High'
        WHEN salary > 70000 THEN 'Medium'
        ELSE 'Entry'
    END AS salary_bracket
FROM employees;
```

**Key Trait:** **Read-only** — never modifies data

---

## **🔐 CATEGORY 4: DCL (Data Control Language)**
### **Purpose: Manage USER PERMISSIONS**

**Mental Model:** *Security office — who gets warehouse keys*

### Commands:
```
GRANT  → Give permissions
REVOKE → Remove permissions
DENY   → Explicitly block access (SQL Server)
```

### **Example Code Block:**
```sql
-- PROBLEM: New data analyst needs database access
-- SOLUTION: Create a new user account with password authentication
CREATE USER 'analyst_user'@'localhost' 
IDENTIFIED BY 'secure_password123';

-- PROBLEM: Need reusable permission sets for similar users (not assigning individually)
-- SOLUTION: Create roles to group permissions logically
CREATE ROLE data_analyst;
CREATE ROLE developer;

-- PROBLEM: Analyst needs to read employee data but not modify it
-- SOLUTION: Grant only SELECT permission on specific table
GRANT SELECT ON company_db.employees TO 'analyst_user'@'localhost';

-- PROBLEM: Developer needs to read, add, and update orders (but not delete)
-- SOLUTION: Grant multiple specific permissions in one statement
GRANT SELECT, INSERT, UPDATE ON company_db.orders TO developer;

-- PROBLEM: Admin user needs full control over products table
-- SOLUTION: Grant all possible permissions (SELECT, INSERT, UPDATE, DELETE, etc.)
GRANT ALL PRIVILEGES ON company_db.products TO 'admin_user'@'localhost';

-- PROBLEM: All data analysts should have same read-only access to database
-- SOLUTION: Grant permissions to role instead of individual users
GRANT SELECT ON company_db.* TO data_analyst;

-- PROBLEM: User is a data analyst and should get analyst permissions
-- SOLUTION: Assign role to user (user inherits all role permissions)
GRANT data_analyst TO 'analyst_user'@'localhost';

-- PROBLEM: Manager needs to grant permissions to their team members
-- SOLUTION: Grant permission WITH GRANT OPTION so they can grant to others
GRANT SELECT ON company_db.employees TO 'manager_user'@'localhost' 
WITH GRANT OPTION;

-- PROBLEM: Developer shouldn't be able to modify orders anymore (role change)
-- SOLUTION: Revoke specific permissions while keeping others
REVOKE INSERT, UPDATE ON company_db.orders FROM developer;

-- PROBLEM: Temporary contractor's access should be completely removed
-- SOLUTION: Revoke all permissions at once
REVOKE ALL PRIVILEGES ON company_db.* FROM 'temp_user'@'localhost';

-- PROBLEM: Interns should never be able to delete employee records (safety rule)
-- SOLUTION: Use DENY to explicitly block permission (SQL Server)
DENY DELETE ON employees TO intern_role;

-- PROBLEM: Former employee's account must be removed from system
-- SOLUTION: Drop user to completely remove their database access
DROP USER 'old_user'@'localhost';

-- PROBLEM: Need to verify what permissions a user currently has (audit)
-- SOLUTION: Display all granted permissions for specific user
SHOW GRANTS FOR 'analyst_user'@'localhost';
```

**Key Trait:** Controls **access rights**, not data content

---

## **⚙️ CATEGORY 5: TCL (Transaction Control Language)**
### **Purpose: Control TRANSACTION FLOW**

**Mental Model:** *Save points in a video game — commit or reload*

### Commands:
```
BEGIN/START TRANSACTION → Start transaction block
COMMIT                  → Save all changes permanently
ROLLBACK                → Undo all changes since BEGIN
SAVEPOINT               → Create restore point within transaction
```

### **Example Code Block:**

#### **Basic Transaction:**
```sql
-- PROBLEM: Money transfer between accounts must complete fully or not at all
-- SOLUTION: Use transaction to ensure both updates succeed or both fail (atomicity)
START TRANSACTION;

    -- Deduct $500 from account 101
    UPDATE accounts 
    SET balance = balance - 500 
    WHERE account_id = 101;
    
    -- Add $500 to account 102
    UPDATE accounts 
    SET balance = balance + 500 
    WHERE account_id = 102;
    
    -- Verify both balances updated correctly
    SELECT balance FROM accounts WHERE account_id IN (101, 102);

-- If both updates succeeded, make changes permanent
COMMIT;
```

#### **Transaction with Error Handling:**
```sql
-- PROBLEM: Order processing involves multiple steps - all must succeed together
-- SOLUTION: Wrap operations in transaction, rollback if any step fails
START TRANSACTION;

    -- Record the order
    INSERT INTO orders (order_id, customer_id, total) 
    VALUES (5001, 250, 1500.00);
    
    -- Reduce inventory quantity
    UPDATE inventory 
    SET quantity = quantity - 5 
    WHERE product_id = 42;
    
    -- If inventory update fails (not enough stock), undo the order insert too

-- If any error occurred, undo all changes
ROLLBACK;

-- Otherwise, save both the order and inventory change
-- COMMIT;
```

#### **Savepoints:**
```sql
-- PROBLEM: Complex process with multiple stages - want to undo only part if needed
-- SOLUTION: Use savepoints to create "checkpoints" within transaction
START TRANSACTION;

    -- Stage 1: Log process start
    INSERT INTO audit_log (action, timestamp) 
    VALUES ('Process started', NOW());
    
    SAVEPOINT after_log;
    
    -- Stage 2: Apply salary increases (might need to undo this)
    UPDATE employees SET salary = salary * 1.10 
    WHERE department = 'Sales';
    
    SAVEPOINT after_salary_update;
    
    -- Stage 3: Clean up temp data
    DELETE FROM temp_data WHERE processed = 1;
    
    -- Oops! Realized the delete was wrong, but salary update is good
    -- Undo only the delete (back to checkpoint), keep salary changes
    ROLLBACK TO SAVEPOINT after_salary_update;
    
    -- Continue with final logging
    INSERT INTO audit_log (action, timestamp) 
    VALUES ('Salaries updated', NOW());

-- Save everything except the rolled-back delete operation
COMMIT;
```

#### **Transaction Isolation Levels:**
```sql
-- PROBLEM: Prevent reading uncommitted data from other concurrent transactions
-- SOLUTION: Set isolation level to control what transaction can "see"
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

START TRANSACTION;
    -- This query won't see changes from other uncommitted transactions
    SELECT * FROM accounts WHERE account_id = 101;
COMMIT;

-- Other isolation levels for different concurrency problems:

-- Allow reading uncommitted changes (fastest, least safe - "dirty reads")
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

-- Ensure same query returns same data if run multiple times in transaction
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

-- Complete isolation - transaction runs as if it's the only one (slowest, safest)
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

#### **Real-World Example (Bank Transfer):**
```sql
-- PROBLEM: Bank transfer must be safe, atomic, and prevent negative balances
-- SOLUTION: Complete transaction with validation, savepoints, and error handling
START TRANSACTION;

    -- Lock source account and check balance (prevent concurrent modifications)
    SELECT balance INTO @source_balance 
    FROM accounts 
    WHERE account_id = 101 FOR UPDATE;
    
    -- If @source_balance < 1000.00, then ROLLBACK (insufficient funds)
    -- Otherwise continue...
    
    -- Stage 1: Deduct from source account
    UPDATE accounts 
    SET balance = balance - 1000.00 
    WHERE account_id = 101;
    
    -- Create savepoint after deduction (can restore if destination fails)
    SAVEPOINT after_deduction;
    
    -- Stage 2: Add to destination account
    UPDATE accounts 
    SET balance = balance + 1000.00 
    WHERE account_id = 102;
    
    -- Stage 3: Record transaction in audit log
    INSERT INTO transaction_log (from_account, to_account, amount, timestamp)
    VALUES (101, 102, 1000.00, NOW());
    
    -- If all operations succeeded, make permanent
    COMMIT;
    
    -- If destination account doesn't exist, could do:
    -- ROLLBACK TO SAVEPOINT after_deduction;
    -- Then handle error and decide whether to COMMIT or ROLLBACK entirely
```

**Key Trait:** Ensures **ACID properties** (Atomicity, Consistency, Isolation, Durability)

---

## **🧠 Memory Anchors**

### **Visual Mnemonic (S-D-Q-A-C):**
```
Structure → DDL → "Build the warehouse"     → CREATE TABLE employees
Data      → DML → "Move the boxes"          → INSERT INTO employees
Query     → DQL → "Inspect the boxes"       → SELECT * FROM employees
Access    → DCL → "Guard the doors"         → GRANT SELECT ON employees
Control   → TCL → "Save or undo moves"      → COMMIT / ROLLBACK
```

### **Decision Tree:**
```
Am I changing WHAT EXISTS?        → DDL    → CREATE, ALTER, DROP
Am I changing DATA IN TABLES?     → DML    → INSERT, UPDATE, DELETE
Am I just READING/CALCULATING?    → DQL    → SELECT, JOIN, COUNT
Am I managing WHO CAN DO WHAT?    → DCL    → GRANT, REVOKE
Am I controlling WHEN IT'S FINAL? → TCL    → COMMIT, ROLLBACK
```

---

## **✅ Quick Reference Table**

| Category | Example | What Problem Does It Solve? | Can Rollback? |
|----------|---------|---------------------------|---------------|
| DDL      | CREATE TABLE | Need to define database structure | ❌ No (auto-commit) |
| DML      | INSERT INTO | Need to add/change/remove data | ✅ Yes (in transaction) |
| DQL      | SELECT FROM | Need to retrieve or calculate information | N/A (read-only) |
| DCL      | GRANT SELECT | Need to control who can access what | ❌ No (auto-commit) |
| TCL      | COMMIT | Need to ensure changes are safe and atomic | ✅ Yes (that's its purpose) |

---

## **🎓 Practice Exercise**

**Categorize these scenarios:**
1. *"The app is too slow when searching users by email"* → **DDL** (CREATE INDEX)
2. *"Customer changed their shipping address"* → **DML** (UPDATE)
3. *"Need to generate monthly sales report"* → **DQL** (SELECT with GROUP BY)
4. *"New intern shouldn't see salary information"* → **DCL** (REVOKE or DENY)
5. *"Payment processing failed halfway - need to undo"* → **TCL** (ROLLBACK)

---

**This complete framework helps you understand WHY each SQL command exists, WHEN to use it, and HOW they all fit into the five fundamental categories!**