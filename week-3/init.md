#### Learning Objectives

By the end of this lab, you should be able to:

- Understand what a database transaction is.
- Use START TRANSACTION, COMMIT, and ROLLBACK.
- Understand the purpose of autocommit.
- Use aggregate functions such as COUNT(), SUM(), AVG(), MAX(), and MIN().
- Group records using GROUP BY.
- Filter grouped results using HAVING.

---
#### Part 1: Understanding Transactions

A transaction is a sequence of SQL operations that are treated as one unit of work.

For example, imagine transferring money from one bank account to another. We want all related changes to succeed, or none of them should be saved.

The main transaction commands we will use are:
Starts a transaction.
```sql
START TRANSACTION;
```
Permanently saves the changes.
```sql
COMMIT;
```
Cancels the changes made during the transaction.
```sql
ROLLBACK;
```
---
Part 2: Create a Database for the Transaction Lab

Create a small database that we can use to experiment with transactions.
```sql
CREATE DATABASE stadium;

USE stadium;
```
Create a users table:
```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(100) NOT NULL,
    email VARCHAR(100)
);
```
Insert some initial data:
```sql
INSERT INTO users (id, username, email)
VALUES
(1, 'peter', 'peter@mail.com'),
(2, 'sharon', 'sharon@mail.com'),
(3, 'john', 'john@mail.com');
```
Check the data:

```sql
SELECT * FROM users;
```
---
Part 3: COMMIT — Saving Changes

Suppose Peter changes his email address.

Start a transaction:
```sql
START TRANSACTION;

UPDATE users
SET email = 'peter@gmail.com'
WHERE id = 1;
```
Check the data before committing:

```sql
SELECT * FROM users;
```
You should see the updated email.

Now save the transaction:
```sql
COMMIT;
```
Check the data again:
```sql
SELECT * FROM users;
```
The change has now been permanently saved.

> Think of COMMIT as saying: "I am happy with these changes. Save them."
---
Part 4: ROLLBACK — Undoing Changes

Now let's make a change that we don't want to keep.
```sql
START TRANSACTION;

UPDATE users
SET email = 'wrong@email.com'
WHERE id = 2;
```
Check the data:
```sql
SELECT * FROM users
WHERE id = 2;
```
You will see the new email address.

Now undo the transaction:

```sql
ROLLBACK;
```
Check again:

```sql
SELECT * FROM users
WHERE id = 2;
```
The email should return to the value it had before the transaction.

> Think of ROLLBACK as saying: "I don't want these changes. Undo them."

---

Part 5: Multiple Changes in One Transaction

This is where students can really understand why transactions are useful.

Suppose we want to add a new user and update another user.

```sql
START TRANSACTION;

INSERT INTO users (id, username, email)
VALUES (4, 'mary', 'mary@mail.com');

UPDATE users
SET email = 'sharon@gmail.com'
WHERE id = 2;
```
Check the changes:

```sql
SELECT * FROM users;
```
If everything is correct:

```sql
COMMIT;
```
#### Discussion Question
> What would happen if we ran ROLLBACK instead of COMMIT?
---

#### Part 6: Understanding Autocommit

MySQL normally uses autocommit, meaning individual changes can be committed automatically.

You can turn it off when you want to explicitly control transactions.

```sql
SET autocommit = 0;
```

Now you can control when changes are saved:

```sql
START TRANSACTION;

UPDATE users
SET email = 'new@email.com'
WHERE id = 1;

COMMIT;
```

You can turn autocommit back on with:

```sql
SET autocommit = 1;
```

For beginners, emphasize:

**Autocommit ON:** MySQL automatically commits changes.

**Autocommit OFF:** You control when changes are committed.

---

#### Part 7: Aggregate Functions

Now switch back to the sales database.

```sql
USE sales;
```

Aggregate functions allow us to perform calculations on multiple rows.

The main functions we will use are:

| Function | Purpose |
|---|---|
| `COUNT()` | Counts the number of records |
| `SUM()` | Calculates the total of numeric values |
| `AVG()` | Calculates the average value |
| `MAX()` | Finds the highest value |
| `MIN()` | Finds the lowest value |


**Example 1: COUNT()**

How many products are in the database?

```sql
SELECT COUNT(*) AS total_products
FROM products;
```


**Example 2: AVG()**

What is the average product buying price?

```sql
SELECT AVG(buyPrice) AS average_price
FROM products;
```

```sql
SELECT 
    ROUND(AVG(buyPrice), 2) AS average_price
FROM products;
```

**Example 3: MAX() and MIN()**

Find the most expensive and cheapest products based on buyPrice.

```sql
SELECT 
    MAX(buyPrice) AS highest_price,
    MIN(buyPrice) AS lowest_price
FROM products;
```

---

#### Part 8: SUM() with Calculations

Let's use the orderdetails table.

We want to calculate the total value of all items ordered.

The value of each item is:
```sql
quantityOrdered × priceEach
```

So we can write:
```sql
SELECT 
    SUM(quantityOrdered * priceEach) AS total_sales
FROM orderdetails;
```

This gives us the total sales value across all order details.

---
#### Part 9: GROUP BY

Aggregate functions become even more useful when combined with GROUP BY.

For example, instead of asking:

How many orders are there?

We can ask:

How many orders are there for each status?

```sql
SELECT 
    status,
    COUNT(*) AS total_orders
FROM orders
GROUP BY status;
```
---

#### Part 10: GROUP BY with SUM()

Let's calculate the total value of each order.

```sql
SELECT 
    orderNumber,
    SUM(quantityOrdered * priceEach) AS order_total
FROM orderdetails
GROUP BY orderNumber;
```

We can also sort the results:

```sql
SELECT 
    orderNumber,
    SUM(quantityOrdered * priceEach) AS order_total
FROM orderdetails
GROUP BY orderNumber
ORDER BY order_total DESC;

```

This shows the orders with the highest total value first.
---

#### Part 11: HAVING

> WHERE filters individual rows.

HAVING filters groups created by GROUP BY.

For example, suppose we want to find orders whose total value is greater than 1,000.

```sql
SELECT 
    orderNumber,
    SUM(quantityOrdered * priceEach) AS order_total
FROM orderdetails
GROUP BY orderNumber
HAVING order_total > 1000;
```
