In database management, performing **DML (Data Manipulation Language)** operations effectively is a core skill for maintaining data integrity. Below is an expert breakdown of how to populate and duplicate data in **MYSQL**.

---

## 2. INSERTING A ROW

To add a single record, you use the **INSERT INTO** statement. You can either specify the column names or provide values for all columns in the order they were defined in the schema.

* **EXPLICIT COLUMN LIST**: It is best practice to list the columns you are targeting. This prevents your code from breaking if the table structure changes (e.g., adding a new nullable column later).
* **DEFAULT & NULL**: For columns that are **AUTO_INCREMENT** or have a **DEFAULT** value, you can use the **DEFAULT** keyword or omit them from the column list.

```sql
-- example: adding a new payment method
INSERT INTO payment_methods (name)
VALUES ('Apple Pay');

```

---

## 3. INSERTING MULTIPLE ROWS

You can insert multiple records in a single statement by separating the value sets with commas. This is significantly faster than executing multiple single-row insert commands.

```sql
-- example: adding multiple shippers at once
INSERT INTO shippers (name)
VALUES 
    ('Shipper A'),
    ('Shipper B'),
    ('Shipper C');

```

---

## 4. INSERTING HIERARCHICAL ROWS (`LAST_INSERT_ID`)

When dealing with parent-child relationships (like an **Order** and its **Order Items**), you need the ID generated for the parent row to insert the children.

* **LAST_INSERT_ID()**: This built-in function returns the ID of the last row successfully inserted in the current session.

```sql
-- example: creating an order and its items
INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2026-03-13', 1);

-- use the generated order_id for the child records
INSERT INTO order_items (order_id, product_id, quantity, unit_price)
VALUES 
    (LAST_INSERT_ID(), 1, 2, 10.50),
    (LAST_INSERT_ID(), 2, 1, 5.00);

```

---

## 5. CREATING A COPY OF A TABLE

There are two primary ways to duplicate data, depending on whether you want the table structure (indexes and constraints) or just the data itself.

### A. COPYING DATA ONLY (SELECT INTO)

You can create a new table and populate it with the results of a query in one step.

* **CRITICAL NOTE**: This method does **not** copy the Primary Key, Indexes, or Auto-increment attributes.

```sql
-- example: creating an archive of old invoices
CREATE TABLE invoices_archived AS
SELECT * FROM invoices
WHERE payment_date IS NOT NULL;

```

### B. TRUNCATING AND RE-INSERTING

If the table already exists and you want to refresh it with data from another table:

```sql
-- empty the target table first
TRUNCATE TABLE invoices_archived;

-- insert from the source table
INSERT INTO invoices_archived
SELECT * FROM invoices
WHERE payment_date IS NOT NULL;

```

---

## SUMMARY OF DML RULES

* **CONSTRAINTS**: Ensure that when you insert into tables like `invoices` or `payments`, the `client_id` already exists in the `clients` table, or the operation will fail due to **Foreign Key Constraints**.
* **TRUNCATE VS DELETE**: Use **TRUNCATE** to completely wipe a table and reset the auto-increment counter; use **DELETE** to remove specific rows based on a condition.
