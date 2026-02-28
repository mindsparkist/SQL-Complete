In the world of **relational databases**, the way we communicate with the engine is through structured queries. Below is an expert breakdown of the core concepts you requested, following the specific formatting of **UPPER CASE** for **SQL KEYWORDS** and lowercase for general prose.

---

## SELECTING A DATABASE

Before you can query tables, you must tell the system which database to look into.

* **USE** keyword: This command sets the current database for the session.
* **--** is for comments: Anything after these dashes is ignored by the engine.

```sql
-- select the store database from your uploaded script
USE sql_store; 

```

---

## THE SELECT CLAUSE & ARITHMETIC

The **SELECT** clause determines which columns you want to retrieve.

* **ARITHMETIC EXPRESSIONS**: You can perform math directly in the **SELECT** statement using `+`, `-`, `*`, and `/`.
* **ALIASES (AS)**: Use the **AS** keyword to give a column a temporary, descriptive name.
* **DISTINCT**: Use this to remove duplicate rows from your results.

```sql
-- example: calculate a 10% tax on unit price
SELECT 
    name, 
    unit_price, 
    (unit_price * 1.1) AS price_with_tax, -- arithmetic and alias
    DISTINCT state                        -- distinct values
FROM products;

```

---

## THE WHERE CLAUSE & OPERATORS

The **WHERE** clause is used to filter records. Only rows that meet the specific condition are returned.

### LIST OF COMPARISON OPERATORS

* `>` (greater than)
* `<` (less than)
* `>=` (greater than or equal to)
* `<=` (less than or equal to)
* `=` (equal)
* `!=` or `<>` (not equal)

---

## LOGICAL OPERATORS: AND, OR, NOT

These allow you to combine multiple conditions for complex filtering.

* **AND**: Both conditions must be true.
* **OR**: At least one condition must be true.
* **NOT**: Reverses the logic (returns rows where the condition is false).

```sql
SELECT * FROM orders
WHERE status = 1 AND (order_date > '2019-01-01' OR NOT shipper_id IS NULL);

```

---

## SPECIAL FILTERING OPERATORS

These operators provide more powerful ways to search through data.

| OPERATOR | PURPOSE | EXAMPLE |
| --- | --- | --- |
| **IN** | matches any value in a specified list. | `WHERE state IN ('NY', 'CA', 'TX')` |
| **BETWEEN** | selects values within a specific range (inclusive). | `WHERE points BETWEEN 1000 AND 3000` |
| **LIKE** | searches for a specific pattern using wildcards (`%` for any chars, `_` for one char). | `WHERE first_name LIKE 'b%'` (starts with b) |
| **REGEXP** | uses regular expressions for complex pattern matching. | `WHERE last_name REGEXP 'field |
| **IS NULL** | finds rows with missing data (null values). | `WHERE phone IS NULL` |

---

## ORDER BY: SORTING DATA

The **ORDER BY** clause is used to sort the result set in either ascending (**ASC**) or descending (**DESC**) order. By default, it sorts by **ASC**.

```sql
-- sorted by the clause
SELECT * FROM customers
ORDER BY state DESC, first_name ASC; 

```
