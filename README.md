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
