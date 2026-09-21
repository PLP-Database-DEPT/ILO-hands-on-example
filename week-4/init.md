## Hands-on Class Activity
###  Speeding up Queries with Indexes

---
### Part 1
First, try running this query without an index:
```sql
SELECT 
    employeeNumber, 
    lastName, 
    firstName
FROM
    employees
WHERE
    jobTitle = 'Sales Rep';
```
Use the **CREATE INDEX** statement to create a new index for a table.

Now create an index to make the search faster:
```sql
CREATE INDEX idxTitle ON employees(jobTitle);
```
Check if the index was created:
```sql
SHOW INDEXES FROM employees;
```
Test the performance plan:
```sql
EXPLAIN SELECT 
    employeeNumber, 
    lastName, 
    firstName
FROM
    employees
WHERE
    jobTitle = 'Sales Rep';
```
---
### Part 2:
### MySQL DROP INDEX statement
Use the **DROP INDEX** statement to remove an existing index.
```sql
DROP INDEX idxTitle ON employees;
```
---
### Part 3:
### MySQL CREATE USER statement
To create a new user in the MySQL database, you use the **CREATE USER** statement.

Let’s create a new user:
```sql
CREATE USER jontefresh@localhost IDENTIFIED BY '1234';
```
To show the users on the current MySQL Server:
```sql
SELECT user FROM mysql.user;
```
---
### Part 4:
### Change MySQL User Password
To change the password of the a user use the **ALTER** … IDENTIFIED BY statement
```sql
ALTER USER jontefresh@localhost IDENTIFIED BY 'abcd'; -- new password
```
###  MySQL RENAME USER statement
The **RENAME USER** statement allows you to rename one or more existing user accounts.
```sql
RENAME USER 'jontefresh'@'localhost' TO 'john'@'localhost';
```
###  MySQL DROP USER statement
To remove a user account from the MySQL Server, you use the **DROP USER** statement as follows:
```sql
DROP USER john@localhost;
```
###  MySQL GRANT statement
You use the **GRANT** statement to assign one or more privileges to a user account.

**Global Privileges**

To grant all privileges in all databases in the current database server
```sql
GRANT ALL ON *.* TO jontefresh@localhost;
```
To show the privileges assigned to jontefresh
```sql
SHOW GRANTS FOR jontefresh@localhost;
```
**Database Privileges**

Apply to all objects within a specific database (all tables, views, etc. in that database).
```sql
-- create a new user
CREATE USER mercy@localhost IDENTIFIED BY '1234';
-- giving access to one database only
GRANT SELECT, INSERT, UPDATE, DELETE ON salesdb.* TO 'mercy'@'localhost';
```
**Table Privileges**

Apply to specific tables within a database.
```sql
-- create a new user
CREATE USER gerald@localhost IDENTIFIED BY '1234';
-- giving access to only one table inside the database.
GRANT SELECT, INSERT ON salesdb.customers TO 'gerald'@'localhost';
```
### 🎭 MySQL Roles

MySQL database server may have multiple users with the same set of privileges.

A **role** is essentially a named collection of privileges.
#### Roles in the Coffeehouse
Your coffeehouse has different teams:
- 👩‍💻 Developers (dev)
- 📊 Accounts team (accounts)
- ☕ Baristas (barista)
```sql
CREATE DATABASE coffeehouse;
USE coffeehouse;
CREATE TABLE customers(
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(255) NOT NULL, 
    last_name VARCHAR(255) NOT NULL, 
    phone VARCHAR(15) NOT NULL,
    email VARCHAR(255) UNIQUE,
    amount DECIMAL(10,2) CHECK(amount>0)
);
INSERT INTO customers(first_name,last_name,phone,email,amount)
VALUES
('James','brown','123456','james@mail.com',300),
('Michael','white','123789','white@mail.com',150),
('Mary','smith','123789','smith@mail.com',150),
('Louis','peters','123789','peters@mail.com',1100),
('Brenda','audrey','123789','audrey@mail.com',1200);
SELECT * FROM customers;
```
**Let’s create roles:**
```sql
CREATE ROLE dev, accounts, barista,interns;
```
**Grant privileges to each role:**

The following statement grants all privileges to dev role:
```sql
GRANT ALL ON coffeehouse.* TO dev;
```
The following statement grants **INSERT**, **UPDATE**, and **DELETE** privileges to accounts role
```sql
GRANT INSERT, UPDATE, DELETE ON coffeehouse.* TO accounts;
```
The following statement grants **SELECT** privilege to barista role
```sql
GRANT SELECT ON cofeehouse.* TO barista;
```
### Assigning roles to user accounts
```sql
-- developer user 
CREATE USER henry@localhost IDENTIFIED BY 'pass';

-- read access user
CREATE USER wayne@localhost IDENTIFIED BY 'pass';
CREATE USER testuser@localhost IDENTIFIED BY 'pass';     

-- read/write users
CREATE USER brenda@localhost IDENTIFIED BY '1234';   
CREATE USER ann@localhost IDENTIFIED BY '1234';
```
To verify the role assignments:
```sql
SHOW GRANTS FOR henry@localhost;
```
**Assigning Permission to users using ROLES**
```sql
GRANT dev TO henry@localhost;
GRANT  accounts TO brenda@localhost;
GRANT barista TO wayne@localhost;
```
**Setting default roles**

To specify which roles should be active each time a user account connects to the database server, you can use the SET DEFAULT ROLE statement.
```sql
SET DEFAULT ROLE ALL TO testuser@localhost;
```
**Revoking privileges from roles**
```sql
REVOKE INSERT, UPDATE, DELETE  ON crm.* FROM accounts;
```
**Removing roles**
To delete one or more roles, you use the DROP ROLE statement:
```sql
DROP ROLE barista;
```
### MySQL REVOKE statement

The **REVOKE** statement revokes one or more privileges from a user account.
```sql
REVOKE ALL, GRANT OPTION FROM jontefresh@localhost;
```
#### revoke privileges from a user
```sql
-- create a new user 
CREATE USER tamara@localhost IDENTIFIED BY '1234';
-- grant privilege
GRANT SELECT, UPDATE, INSERT ON salesdb.* TO tamara@localhost;
-- display the granted privileges
SHOW GRANTS FOR tamara@localhost;
-- revoke privilege
REVOKE INSERT, UPDATE ON salesdb.* FROM  tamara@localhost;
```
