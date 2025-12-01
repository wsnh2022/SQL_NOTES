
## 📊 GROUP BY & AGGREGATION

**What it means**: Organizing rows into groups based on common values, then performing calculations on each group. Like sorting items into buckets and counting/summing what's in each bucket.

### Group By Rules

```sql
SELECT category, SUM(sales)
FROM table
GROUP BY category;
```

- **Grouping Logic**: Categorical columns create logical groups
- **Column Rules**: Non-aggregated columns MUST appear in GROUP BY
- **Compatible Expressions**: Can use expressions in GROUP BY

### Aggregate Functions

**What it means**: Functions that take multiple rows and return a single summary value. They "collapse" many rows into one result per group.

#### Numeric Aggregations

|Function|Purpose|Example|
|---|---|---|
|`SUM()`|Total sum|`SUM(revenue)`|
|`COUNT()`|Count rows|`COUNT(*)` or `COUNT(column)`|
|`AVG()`|Average|`AVG(price)`|
|`MIN()`|Minimum|`MIN(date)`|
|`MAX()`|Maximum|`MAX(salary)`|
#### String Aggregations

**What it means**: Aggregate functions applied to text columns. Can count, find alphabetical extremes, or combine strings from multiple rows into one.

|Function|Purpose|Example|
|---|---|---|
|`COUNT()`|Count non-NULL strings|`COUNT(name)`|
|`COUNT(DISTINCT)`|Unique string count|`COUNT(DISTINCT category)`|
|`MIN()`|First alphabetically|`MIN(name)` → 'Alice'|
|`MAX()`|Last alphabetically|`MAX(name)` → 'Zack'|
|`GROUP_CONCAT()`|Concatenate strings|`GROUP_CONCAT(name, ', ')` → 'Alice, Bob, Carol'|
|`STRING_AGG()`|Concatenate (PostgreSQL)|`STRING_AGG(name, ', ' ORDER BY name)`|
|`LISTAGG()`|Concatenate (Oracle)|`LISTAGG(name, ', ') WITHIN GROUP (ORDER BY name)`|

**String Aggregation Examples**:

```sql
-- Combine all values in a group
SELECT 
  department,
  STRING_AGG(employee_name, ', ') AS employee_list
FROM employees
GROUP BY department;
-- Result: 'Sales' → 'John, Sarah, Mike'

-- With ordering
SELECT 
  product_id,
  STRING_AGG(review_text, ' | ' ORDER BY review_date DESC) AS reviews
FROM reviews
GROUP BY product_id;

-- Count distinct categories
SELECT 
  region,
  COUNT(DISTINCT product_category) AS unique_categories
FROM sales
GROUP BY region;

-- Find first/last customer alphabetically
SELECT 
  store_id,
  MIN(customer_name) AS first_customer,
  MAX(customer_name) AS last_customer
FROM transactions
GROUP BY store_id;
```

**Conditional String Aggregation**:

```sql
-- Concatenate only active users
SELECT 
  team,
  STRING_AGG(
    CASE WHEN status = 'active' THEN username END, 
    ', '
  ) AS active_users
FROM team_members
GROUP BY team;

-- Count specific string patterns
SELECT 
  department,
  COUNT(CASE WHEN email LIKE '%@gmail.com' THEN 1 END) AS gmail_users
FROM employees
GROUP BY department;
```

**Database-Specific String Aggregation**:

```sql
-- MySQL
GROUP_CONCAT(column ORDER BY column SEPARATOR ', ')

-- PostgreSQL
STRING_AGG(column, ', ' ORDER BY column)

-- SQL Server
STRING_AGG(column, ', ') WITHIN GROUP (ORDER BY column)

-- Oracle
LISTAGG(column, ', ') WITHIN GROUP (ORDER BY column)

-- SQLite
GROUP_CONCAT(column, ', ')
```

---

## 🔍 FILTERING DATA

**What it means**: Selecting only the rows that meet specific conditions. Like using a sieve to filter out unwanted data.

### WHERE Clause (Pre-Aggregation)

**What it means**: Filters individual rows BEFORE any grouping or aggregation happens. Tests each row against conditions and keeps only matches.

```sql
WHERE column = value
  AND age > 25
  OR status IN ('active', 'pending')
  AND salary BETWEEN 50000 AND 100000
  AND name LIKE 'J%'
```

**Operators**:

- Comparison: `=`, `!=`, `>`, `<`, `>=`, `<=`
- Logical: `AND`, `OR`, `NOT`
- Membership: `IN (list)`
- Range: `BETWEEN x AND y`
- Pattern: `LIKE '%pattern%'`

### HAVING Clause (Post-Aggregation)

**What it means**: Filters groups AFTER aggregation is complete. Used when you need to filter based on calculated summaries (like "groups with total sales > 10000").

```sql
HAVING SUM(sales) > 10000
   AND COUNT(*) >= 5
   AND STRING_AGG(status, ',') LIKE '%active%'
```

- Operates **only** on aggregated results
- Used **after** GROUP BY
- Works with string aggregates too

---

## 🧹 DATA CLEANING

**What it means**: Fixing messy or inconsistent data - handling missing values, correcting data types, removing extra spaces, and standardizing formats.

### NULL Handling

**What it means**: Managing missing or undefined values. NULL represents "no value" or "unknown", and needs special handling to avoid errors.

```sql
COALESCE(column, 'default')          -- First non-null value
NULLIF(column, 'N/A')                 -- NULL if values match
IFNULL(column, 0)                     -- Alternative syntax
```

### Type Conversion

**What it means**: Converting data from one type to another (text to number, date to text, etc.) to ensure compatibility for calculations or comparisons.

```sql
CAST(column AS INTEGER)
CONVERT(VARCHAR, date_column)
```

### String Cleaning

**What it means**: Removing unwanted characters, fixing spacing issues, and replacing text to standardize string data.

```sql
TRIM(column)                          -- Remove spaces
REPLACE(column, 'old', 'new')         -- Replace text
LTRIM(RTRIM(column))                  -- Left + right trim
```

---

## 📅 DATE MANIPULATION

**What it means**: Working with dates and times - adding/subtracting periods, calculating differences between dates, extracting specific components, and formatting dates for analysis.

```sql
DATEADD(DAY, 7, date_column)          -- Add interval
DATEDIFF(DAY, start_date, end_date)   -- Calculate difference
EXTRACT(YEAR FROM date_column)        -- Pull component
DATE_TRUNC('month', date_column)      -- Truncate to month
```

**Common Extractions**: `YEAR`, `MONTH`, `DAY`, `HOUR`, `MINUTE`

---

## 🔤 STRING MANIPULATION

**What it means**: Modifying and transforming text data - combining strings, extracting portions, changing case, measuring length, and pattern matching.

```sql
CONCAT(first_name, ' ', last_name)    -- Combine strings
SUBSTRING(text, 1, 10)                -- Extract characters
LENGTH(column)                         -- String length
LOWER(column) / UPPER(column)         -- Case conversion
REGEXP_REPLACE(col, '[0-9]', '')      -- Regex operations
```

---

## ➗ ARITHMETIC & CALCULATIONS

**What it means**: Performing mathematical operations on numeric data and creating new calculated columns based on existing values.

### Operators

- `+` Addition
- `-` Subtraction
- `*` Multiplication
- `/` Division
- `%` Modulo (remainder)

### Derived Columns

**What it means**: Creating new columns on-the-fly by combining or transforming existing columns. These calculated fields don't exist in the original table.

```sql
SELECT 
  revenue - cost AS profit,
  revenue / NULLIF(cost, 0) AS margin,
  CASE 
    WHEN sales > 1000 THEN 'High'
    WHEN sales > 500 THEN 'Medium'
    ELSE 'Low'
  END AS performance_tier
FROM table;
```

---

## 🔗 JOINS

**What it means**: Combining rows from two or more tables based on related columns. Like matching puzzle pieces to create a complete picture from separate datasets.

|Join Type|Description|Syntax|
|---|---|---|
|`INNER`|Intersection only|`INNER JOIN ON a.id = b.id`|
|`LEFT`|All from left + matches|`LEFT JOIN ON a.id = b.id`|
|`RIGHT`|All from right + matches|`RIGHT JOIN ON a.id = b.id`|
|`FULL`|All from both tables|`FULL OUTER JOIN ON a.id = b.id`|
|`CROSS`|Cartesian product|`CROSS JOIN` (no condition)|

```sql
SELECT a.*, b.column
FROM table_a a
LEFT JOIN table_b b ON a.key = b.key
```

---

## 📦 SUBQUERIES

**What it means**: Queries nested inside other queries. Like solving a problem in steps - the inner query solves one part, and the outer query uses that result.

### Types

```sql
-- Scalar (returns one value)
SELECT (SELECT MAX(price) FROM products) AS max_price;

-- Inline (acts as table)
SELECT * FROM (SELECT * FROM orders WHERE status = 'shipped') sub;

-- Correlated (references outer query)
SELECT * FROM employees e
WHERE salary > (SELECT AVG(salary) FROM employees WHERE dept = e.dept);
```

---

## 📝 CTEs (Common Table Expressions)

**What it means**: Temporary named result sets that exist only during query execution. Makes complex queries more readable by breaking them into logical, reusable pieces. Think of them as "query variables".

```sql
WITH sales_summary AS (
  SELECT 
    product_id,
    SUM(amount) AS total_sales
  FROM orders
  GROUP BY product_id
)
SELECT * FROM sales_summary WHERE total_sales > 1000;
```

**Recursive CTE** (hierarchical data):

**What it means**: CTEs that reference themselves, used for hierarchical data like organizational charts, family trees, or nested categories.

```sql
WITH RECURSIVE hierarchy AS (
  SELECT id, name, manager_id, 1 AS level
  FROM employees WHERE manager_id IS NULL
  UNION ALL
  SELECT e.id, e.name, e.manager_id, h.level + 1
  FROM employees e JOIN hierarchy h ON e.manager_id = h.id
)
SELECT * FROM hierarchy;
```

---

## 🪟 WINDOW FUNCTIONS

**What it means**: Perform calculations across rows related to the current row WITHOUT collapsing them into groups. Like having a sliding window that looks at surrounding rows while keeping all original rows intact.

```sql
SELECT 
  column,
  ROW_NUMBER() OVER (PARTITION BY category ORDER BY date) AS row_num,
  RANK() OVER (ORDER BY sales DESC) AS sales_rank,
  SUM(amount) OVER (PARTITION BY region) AS region_total,
  LAG(price, 1) OVER (ORDER BY date) AS prev_price,
  LEAD(price, 1) OVER (ORDER BY date) AS next_price
FROM table;
```

**Components**:

- `OVER()`: Window specification
- `PARTITION BY`: Logical grouping within window
- `ORDER BY`: Ordering within partition

**Functions**: `ROW_NUMBER`, `RANK`, `DENSE_RANK`, `LAG`, `LEAD`, `FIRST_VALUE`, `LAST_VALUE`

---

## 🔀 SET OPERATIONS

**What it means**: Combining results from multiple queries vertically (stacking rows). Like mathematical set operations - union, intersection, and difference.

```sql
SELECT column FROM table1
UNION          -- Distinct values
SELECT column FROM table2;

SELECT column FROM table1
UNION ALL      -- All values (with duplicates)
SELECT column FROM table2;

SELECT column FROM table1
INTERSECT      -- Common values
SELECT column FROM table2;

SELECT column FROM table1
EXCEPT         -- Values in table1 not in table2
SELECT column FROM table2;
```

**Rule**: Column count and data types must match

---

## 🏗️ DATA MODELING

**What it means**: Designing how data is structured and organized in a database. Involves defining tables, relationships, and rules to ensure data integrity and efficient querying.

### Normalization

**What it means**: The process of organizing data to reduce redundancy and improve integrity by splitting data into related tables following specific rules.

- **1NF**: Atomic values, no repeating groups
- **2NF**: No partial dependencies on composite keys
- **3NF**: No transitive dependencies

### Keys

**What it means**: Special columns that establish identity and relationships between tables.

- **Primary Key**: Unique identifier (NOT NULL, UNIQUE)
- **Foreign Key**: References primary key in another table

### Schema Design

**What it means**: The blueprint of how tables connect and organize. Star schema is optimized for analytical queries with a central fact table surrounded by dimension tables.

```
Star Schema:
┌─────────────┐
│  Fact Table │ (transactions, metrics)
│  - fact_id  │
│  - dim1_fk ─┼──→ Dimension Tables
│  - dim2_fk ─┼──→ (attributes, descriptions)
│  - measure  │
└─────────────┘
```

**Components**:

- **Fact Tables**: Quantitative data, foreign keys
- **Dimension Tables**: Descriptive attributes
- **Star Schema**: One central fact table, denormalized dimensions

---

## 💡 Quick Tips

✅ **Best Practices**:

- Use aliases for readability: `AS`
- Always specify join conditions
- Use CTEs for complex queries (better than nested subqueries)
- Index foreign keys and WHERE clause columns
- Use `EXPLAIN` to analyze query performance
- Check string aggregate syntax for your specific database

⚠️ **Common Pitfalls**:

- Forgetting GROUP BY for non-aggregated columns
- Using WHERE instead of HAVING for aggregates
- Division by zero (use `NULLIF`)
- Cartesian joins (missing join conditions)
- Database-specific string aggregation functions vary

🎯 **String Aggregation Tips**:

- Always specify delimiter/separator explicitly
- Use ORDER BY within aggregation for consistent results
- Consider string length limits (varies by database)
- NULL values are typically ignored in string aggregations