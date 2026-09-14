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
