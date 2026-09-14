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
