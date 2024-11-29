
# SQL Concepts Summary 🚀

### **1. Optimizing Slow-Running Queries**
- **Indexes**: Create indexes on columns used in `JOIN`, `WHERE`, and `ORDER BY` to speed up data retrieval.
- **Query Simplification**: Use `EXPLAIN` to analyze query execution plans and remove unnecessary joins.
- **Proper Data Types**: Use appropriate data types to avoid excess memory usage.

---

### **2. Deadlocks Detection and Resolution**
- **Deadlocks**: Occur when two queries block each other waiting for resources.
- **Detection**: Use database logs or tools like `sys.dm_tran_locks` in SQL Server.
- **Resolution**: 
  - Avoid circular dependencies.
  - Use consistent ordering of resources.
  - Enable retries for failed transactions.

---

### **3. Window Functions (ROW_NUMBER, RANK, DENSE_RANK)**
- **ROW_NUMBER**: Assigns a unique rank to each row, even for duplicate values.
- **RANK**: Assigns the same rank to duplicate values but skips ranks for duplicates.
- **DENSE_RANK**: Similar to `RANK` but doesn’t skip ranks for duplicates.
- **Example**:
  ```sql
  SELECT name, department, salary,
         ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS row_num,
         RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank_num,
         DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rank_num
  FROM employees;
  ```

---

### **4. Second Highest Salary in Each Department**
- **CTE Approach**:
  ```sql
  WITH CTE_Employees AS (
      SELECT department, salary,
             ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS rnk
      FROM employees
  )
  SELECT department, salary
  FROM CTE_Employees
  WHERE rnk = 2;
  ```
- **Alternate Subquery**:
  ```sql
  SELECT department, salary
  FROM (
      SELECT department, salary,
             ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS rnk
      FROM employees
  ) AS Temp
  WHERE rnk = 2;
  ```

---

### **5. ACID Properties**
- **Atomicity**: Transaction is all or nothing.
- **Consistency**: Database remains valid after the transaction.
- **Isolation**: One transaction doesn’t interfere with another.
- **Durability**: Once committed, data is permanently saved.

---

### **6. OFFSET Not Working?**
- Ensure `ORDER BY` is included when using `OFFSET`.
- Correct Syntax Example (SQL Server):
  ```sql
  SELECT * FROM employees
  ORDER BY salary DESC
  OFFSET 5 ROWS FETCH NEXT 5 ROWS ONLY;
  ```
