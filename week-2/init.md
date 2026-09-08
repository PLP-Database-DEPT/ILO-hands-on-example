## SQL Hands-On Lab: Managing and Querying a Student Database

Lab Objective

By the end of this lab, you should be able to:

1. Create a database and table using DDL.
2. Modify a table using ALTER TABLE.
3. Insert, update, and delete records using DML.
4. Retrieve data using SELECT.
5. Filter records using WHERE.
6. Use comparison and logical operators with WHERE.
7. Sort results using ORDER BY

---
#### Creating the Database and Table
DDL stands for Data Definition Language. It is used to define and modify the structure of database objects.

Common DDL commands include:

- CREATE
- ALTER
- DROP
- TRUNCATE

#### Step 1: Create the Database
```sql
CREATE DATABASE school;
```
Select the database:

```sql
USE school;
```
#### Step 2: Create the Students Table
```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    age INT,
    gender VARCHAR(10),
    course VARCHAR(50),
    marks DECIMAL(5,2),
    city VARCHAR(50)
);
```
You can also use:
```sql
SHOW TABLES;
```
