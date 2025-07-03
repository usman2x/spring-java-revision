# FAQs Query Execution Flow – Quick Reference

### **Q1: What is the logical execution order of a basic SELECT query?**

**A:** `FROM → WHERE → SELECT → ORDER BY → LIMIT`

### **Q2: In a query with GROUP BY and HAVING, which clause is executed first?**
**A:** `GROUP BY` runs before `HAVING`, and both run after `WHERE`.

### **Q3: When does a JOIN query evaluate its ON condition?**
**A:** The `JOIN` happens first, then the `ON` condition filters matching rows before `WHERE`.

### **Q4: Are subqueries always executed before the main query?**
**A:** Only **non-correlated** subqueries are; **correlated** subqueries execute once per outer row.

### **Q5: What is the execution order in a query using a subquery in the FROM clause?**
**A:** The subquery is executed first and treated as a temporary table by the outer query.

