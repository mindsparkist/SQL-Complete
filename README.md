In relational algebra, **JOINS** are the primary mechanism for combining data from different tables based on a related column between them.

---

## 1. INNER JOIN CONCEPTS

The **INNER JOIN** is the most common type. It returns only the rows where there is a match in both tables.

* **BASIC CONCEPT**: If a record in the left table does not have a matching record in the right table, it is excluded from the result set.
* **SQL KEYWORD**: You can use **INNER JOIN** or just **JOIN** (which defaults to **INNER**).

```sql
-- Join clients and invoices from your sql_invoicing database
USE sql_invoicing;

SELECT invoice_id, number, name
FROM invoices
JOIN clients ON invoices.client_id = clients.client_id;

```

---

## 2. JOINING ACROSS MULTIPLE DATABASES

In **MYSQL**, you can join tables located in different databases on the same server by prefixing the table name with the database name.

```sql
-- Join products from sql_inventory with order_items in sql_store
SELECT *
FROM sql_store.order_items oi
JOIN sql_inventory.products p 
    ON oi.product_id = p.product_id;

```

---

## 3. JOINING MULTIPLE TABLES

To link more than two tables, you simply chain the **JOIN** clauses.

```sql
-- Join 3 tables: payments, clients, and payment_methods
USE sql_invoicing;

SELECT 
    p.date,
    p.amount,
    c.name AS client_name,
    pm.name AS payment_method
FROM payments p
JOIN clients c ON p.client_id = c.client_id
JOIN payment_methods pm ON p.payment_method = pm.payment_method_id;

```

---

## 4. COMPOUND JOIN CONDITIONS (COMPOSITE KEYS)

A **COMPOSITE KEY** is a primary key that consists of more than one column. To join such tables, you must use a **COMPOUND JOIN** condition with the **AND** operator.

```sql
-- From your sql_store database: order_items has a composite primary key (order_id, product_id)
SELECT *
FROM order_items oi
JOIN order_item_notes oin
    ON oi.order_id = oin.order_id
    AND oi.product_id = oin.product_id;

```

---

## 5. IMPLICIT JOIN SYNTAX (OLD SCHOOL)

Though not recommended for modern development, you can join tables using the **WHERE** clause. If you forget the **WHERE** clause, you get a **CROSS JOIN** (Cartesian product).

```sql
-- Implicit Join (Avoid this in professional code)
SELECT *
FROM orders o, customers c
WHERE o.customer_id = c.customer_id;

```

---

## 6. OUTER JOINS (LEFT / RIGHT)

An **OUTER JOIN** returns all records from one table and the matched records from the other. If there is no match, the result is **NULL** on the side that lacks the data.

* **LEFT JOIN**: Returns **ALL** records from the left table and matching records from the right.
* **RIGHT JOIN**: Returns **ALL** records from the right table and matching records from the left.

```sql
-- Find ALL customers, even those who have never placed an order
USE sql_store;

SELECT c.first_name, o.order_id
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id;

```

---

## 7. OUTER JOINS BETWEEN MULTIPLE TABLES

When joining multiple tables with **OUTER JOINS**, it is best practice to stick to **LEFT JOIN** throughout the query to maintain readability and avoid confusing the engine's optimizer.

```sql
-- Get all customers, their orders, and the shipper (if they have one)
SELECT 
    c.first_name,
    o.order_id,
    sh.name AS shipper
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
LEFT JOIN shippers sh ON o.shipper_id = sh.shipper_id;

```
In advanced SQL development, mastering how to combine data from the same table or across different record sets is essential for complex reporting and data analysis.

---

## 9. SELF OUTER JOINS

A **SELF JOIN** occurs when you join a table with itself. This is common for hierarchical data (like an organizational chart). When you use an **OUTER JOIN** (specifically a **LEFT JOIN**) in a self-join, you ensure that the "top-level" records (those without a parent/manager) are not excluded from the results.

```sql
-- example: showing all employees and their managers from your sql_hr database
USE sql_hr;

SELECT 
    e.first_name AS employee,
    m.first_name AS manager
FROM employees e
LEFT JOIN employees m 
    ON e.reports_to = m.employee_id;
-- using LEFT JOIN ensures the CEO (who reports to NULL) still appears

```

---

## 10. THE USING CLAUSE

When the column names used for joining are **identical** in both tables, you can use the **USING** keyword to simplify your query. It replaces the `ON tableA.id = tableB.id` syntax.

```sql
-- joining customers and orders where both have 'customer_id'
USE sql_store;

SELECT 
    o.order_id,
    c.first_name
FROM orders o
JOIN customers c USING (customer_id);

```

---

## 11. NATURAL JOIN (NOT RECOMMENDED)

A **NATURAL JOIN** is a join that happens "automatically" based on columns with the same name.

* **WHY IT IS NOT RECOMMENDED**: It is dangerous because the database engine makes assumptions. If someone adds a column to one table with the same name as a column in another (even if they aren't related), the query will break or produce incorrect data. **Expert advice:** Always be explicit with your join conditions.

```sql
-- dangerous: the engine guesses the join columns
SELECT *
FROM orders o
NATURAL JOIN customers c;

```

---

## 12. CROSS JOINS

A **CROSS JOIN** produces a **Cartesian Product**, meaning every row from the first table is joined with every row from the second table.

* **USE CASE**: Useful for generating combinations, such as matching every color with every size in a clothing store.

```sql
-- explicit syntax
SELECT s.name AS shipper, p.name AS product
FROM shippers s
CROSS JOIN products p;

-- implicit syntax
SELECT s.name, p.name
FROM shippers s, products p;

```

---

## 13. UNIONS

While **JOINS** combine columns horizontally, a **UNION** combines rows vertically. This allows you to stack the results of multiple queries into one result set.

### RULES FOR UNIONS:

1. Both queries must return the same **number** of columns.
2. The columns must have **compatible** data types.
3. The column names of the **first** query determine the header names.

```sql
-- example: labeling customers based on their loyalty points
USE sql_store;

SELECT first_name, points, 'active' AS status
FROM customers
WHERE points >= 2000
UNION
SELECT first_name, points, 'archived' AS status
FROM customers
WHERE points < 2000
ORDER BY first_name; 
-- notice ORDER BY is applied to the final combined set

```
