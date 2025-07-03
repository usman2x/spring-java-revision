# Indexing Cheat Sheet

## 1. **Why Use Indexes?**

| Benefit             | Description                                  |
| ------------------- | -------------------------------------------- |
| 🔍 Faster Searches  | Indexes speed up `WHERE`, `JOIN`, `ORDER BY` |
| 📉 Less I/O         | Reduces full table scans                     |
| 📊 Sorting/Grouping | Speeds up `ORDER BY`, `GROUP BY`, aggregates |
| 🔗 Faster Joins     | Indexed keys are faster for lookups          |


## 2. **When to Add Indexes**

Add indexes when your column is:

| Use Case                                    | Example                                |
| ------------------------------------------- | -------------------------------------- |
| Frequently used in `WHERE`                  | `WHERE account_id = ?`                 |
| Used in `JOIN` conditions                   | `JOIN ON customer_id`                  |
| Used in `ORDER BY` or `GROUP BY`            | `ORDER BY transaction_date`            |
| Used in `UNIQUE`, `FOREIGN KEY` constraints | `account_id`, `user_id`                |
| Frequently filtered with high selectivity   | e.g., `email`, `SSN`, `transaction_id` |


## When NOT to Use Indexes

| Reason                    | Example                                   |
| ------------------------- | ----------------------------------------- |
| Low cardinality values    | e.g., `status: SUCCESS/FAILED`, `country` |
| Columns that change often | frequent updates → index maintenance cost |
| Columns not queried       | don't index unused data                   |
| Large batch inserts       | disable indexes temporarily               |


## 3. **Types of Indexes**

| Type                 | Use Case                                          |
| -------------------- | ------------------------------------------------- |
| **B-Tree (default)** | General purpose, sorted lookup                    |
| **Hash**             | Fast equality (`=`) only, not `range`             |
| **GIN / GiST**       | For full-text search, arrays, JSON                |
| **Composite**        | Multiple columns (order matters!)                 |
| **Partial Index**    | Only some rows (e.g., `WHERE is_active = true`)   |
| **Covering Index**   | Includes all needed columns → avoids table access |


## 4. **What is Cardinality?**

> **Cardinality** = # of distinct values in a column

| Type             | Description                      | Index Friendly?    |
| ---------------- | -------------------------------- | ------------------ |
| High cardinality | Each value is mostly unique      | ✅ Great for index  |
| Medium           | Reasonably distinct              | ✅ Worth evaluating |
| Low cardinality  | Few distinct values (e.g., 2–10) | ❌ Usually avoid    |

### How to Check Cardinality (SQL)

```sql
SELECT COUNT(*) AS total_rows, COUNT(DISTINCT column_name) AS unique_values
FROM table_name;
```

Then compute:

```sql
SELECT COUNT(DISTINCT column_name) * 1.0 / COUNT(*) AS cardinality_ratio
FROM table_name;
```

| Ratio   | Meaning              |
| ------- | -------------------- |
| \~1.0   | High cardinality     |
| 0.1–0.5 | Medium               |
| < 0.05  | Low (avoid indexing) |



##  5. **Performance Evaluation & Tuning**

### A. PostgreSQL: Check Index Usage

```sql
SELECT relname AS "Table", indexrelname AS "Index", idx_scan AS "Used"
FROM pg_stat_user_indexes
ORDER BY idx_scan ASC;
```

⛔ `idx_scan = 0` = unused index


### B. Query Plan with EXPLAIN

```sql
EXPLAIN ANALYZE
SELECT * FROM transactions WHERE account_id = 123;
```

Look for:

* `Index Scan` ✅ (good)
* `Seq Scan` ❌ (bad for large tables)

---

### C. Measure Index Size

```sql
SELECT pg_size_pretty(pg_relation_size('index_name'));
```

Track index bloat and unused space.

---

###  D. Optimize Index Strategy

| Strategy              | Tip                                                                                                           |
| --------------------- | ------------------------------------------------------------------------------------------------------------- |
| Composite Index       | Order matters: `CREATE INDEX ON users (last_name, first_name)` will only help if `last_name` is queried first |
| Partial Index         | Use if only indexing subset of rows                                                                           |
| Index Only Scan       | Ensure indexed columns cover the query (`SELECT only indexed columns`)                                        |
| Remove Unused Indexes | They waste space and slow writes                                                                              |
| Test Before Add       | Benchmark query before & after indexing using `EXPLAIN ANALYZE`                                               |

---

## Pro Tips

| Scenario                   | Index Strategy                          |
| -------------------------- | --------------------------------------- |
| `WHERE email = ?`          | ✅ Create index on `email`               |
| `WHERE status = 'PENDING'` | ❌ Avoid (low cardinality)               |
| `JOIN ON customer_id`      | ✅ Index `customer_id` on both tables    |
| `ORDER BY created_at DESC` | ✅ Index on `created_at`                 |
| `SELECT COUNT(*)`          | ❌ Index won't help much unless filtered |
| Full text search           | Use `GIN` index on `to_tsvector(...)`   |

---

### **1. Will B-Tree index work for full-text search?**

B-Tree indexes work only for prefix-based string matches like `LIKE 'abc%'`, where the string starts with a known value. However, they do **not** help with `LIKE '%abc%'` or full-text pattern searches that require tokenization and ranking. For proper full-text search, use a `FULLTEXT` index in MySQL or a `GIN` index with `to_tsvector()` in PostgreSQL.

```sql
-- PostgreSQL full-text index
CREATE INDEX idx_post_content ON posts USING GIN (to_tsvector('english', post_content));
```

---

### **2. How do you decide applying composite indexing? Is composite index better than two separate indexes?**

Composite indexes are ideal when multiple columns are queried together in the same `WHERE`, `JOIN`, or `ORDER BY` clause. A composite index like `(column1, column2)` helps if the query filters on `column1` or both columns together. However, it won’t help if only `column2` is used. Two separate indexes might work better if both columns are used independently across different queries.

```sql
CREATE INDEX idx_user_city ON users (country, city);
-- Efficient for: WHERE country = 'PK' AND city = 'Lahore'
```

---

### **3. If a column has almost unique values across rows but is not used in filtering/searching, should it be indexed?**

No. Even if the column has high cardinality (mostly unique values), it should **not** be indexed unless it is actually involved in filtering, joining, or sorting in queries. Indexing it would consume disk space and slow down writes, without offering any performance benefit.

---

### **4. How can I remove useless indexes?**

You can identify unused indexes by checking system statistics. In PostgreSQL, use `pg_stat_user_indexes` and look for `idx_scan = 0` to find indexes that were never used. In MySQL, monitor `SHOW INDEX` and review query patterns. Once confirmed, remove the unused index using a `DROP INDEX` statement.

```sql
-- PostgreSQL
SELECT * FROM pg_stat_user_indexes WHERE idx_scan = 0;

-- Drop index
DROP INDEX idx_unused ON transactions;
```

---

### **5. How do you fine-tune indexes?**

Fine-tuning involves analyzing queries using `EXPLAIN ANALYZE` to ensure indexes are being used effectively. Prioritize indexing columns used in `WHERE`, `JOIN`, or `ORDER BY` with high selectivity. Consider composite indexes for multi-column filters, and avoid indexing low-cardinality or frequently updated columns. Regularly monitor index size, remove redundant indexes, and use covering indexes or partial indexes for complex scenarios.

```sql
EXPLAIN ANALYZE SELECT * FROM orders WHERE customer_id = 101;

SELECT pg_size_pretty(pg_relation_size('idx_orders_customer_id'));
```


