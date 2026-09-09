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
---

## Part 2: DDL — Altering the Table

Suppose the school wants to store each student's email address.

Use **ALTER TABLE** to add a new column:
```sql
ALTER TABLE students
ADD email VARCHAR(100);
```
Check the table again:
```sql
DESCRIBE students;
```
You should now see the email column.

**Rename a Column**

**For example, to rename city to location:**
```sql
ALTER TABLE students
RENAME COLUMN city TO location;
```
**Remove a Column**

**If the email column is no longer required:**
```sql
ALTER TABLE students
DROP COLUMN email;
```
---

## Part 3: DML — Inserting Data

DML stands for **Data Manipulation Language**.

Common DML commands include:

- INSERT
- UPDATE
- DELETE

**Let's add some students to our table.**

```sql
INSERT INTO students
(first_name, last_name, age, gender, course, marks, location)
VALUES
('John', 'Kamau', 21, 'Male', 'Database', 85.50, 'Nairobi'),
('Mary', 'Wanjiku', 22, 'Female', 'Python', 92.00, 'Nakuru'),
('Peter', 'Otieno', 20, 'Male', 'Database', 67.50, 'Kisumu'),
('Aisha', 'Mohamed', 23, 'Female', 'Cybersecurity', 78.00, 'Mombasa'),
('Brian', 'Ochieng', 19, 'Male', 'Python', 55.00, 'Nairobi'),
('Grace', 'Njeri', 21, 'Female', 'Database', 88.50, 'Nairobi'),
('David', 'Mwangi', 24, 'Male', 'Cloud Computing', 73.00, 'Eldoret'),
('Faith', 'Akinyi', 20, 'Female', 'Python', 95.00, 'Kisumu'),
('Kevin', 'Maina', 22, 'Male', 'Cybersecurity', 62.50, 'Nakuru'),
('Lucy', 'Atieno', 23, 'Female', 'Database', 81.00, 'Mombasa');
```
Check the data:

```sql
SELECT * FROM students;
```
---
## Part 4: DML — Updating Data

**Suppose John's marks were entered incorrectly and should be 90.**

```sql
UPDATE students
SET marks = 90
WHERE student_id = 1;
```
**Verify the change:**

```sql
SELECT * FROM students
WHERE student_id = 1;
```
---

## Part 5: DML — Deleting Data

**Suppose we want to remove student with ID 5.**

```sql
DELETE FROM students
WHERE student_id = 5;
```
Check whether the student has been removed:
```sql
SELECT * FROM students;
```

Important

**_Always be careful with DELETE._**

This:
```sql
DELETE FROM students;
```
will delete all records from the table.

---
## Part 6: WHERE — Filtering Data

The **WHERE clause** allows us to retrieve records that meet a specific condition.

**Example 1:** Students from Nairobi
```sql
SELECT *
FROM students
WHERE location = 'Nairobi';
```
**Example 2:** Students Taking Database
```sql
SELECT *
FROM students
WHERE course = 'Database';
```
**Example 3:** Students with Marks Greater Than 80
```sql
SELECT *
FROM students
WHERE marks > 80;
```
**Example 4:** Students Under 21
```sql
SELECT *
FROM students
WHERE age < 21;
```
---
## Part 7: Comparison Operators

**Students with marks greater than or equal to 70:**
```sql
SELECT *
FROM students
WHERE marks >= 70;
```
**Students with marks less than 60:**
```sql
SELECT *
FROM students
WHERE marks < 60;
```
**Students who are not from Nairobi:**
```sql
SELECT *
FROM students
WHERE location <> 'Nairobi';
```
**Students who are exactly 22 years old:**
```sql
SELECT *
FROM students
WHERE age = 22;
```
---
## Part 8: Logical Operators with WHERE

- AND
- OR
- NOT

#### AND

Both conditions must be true.

**Find students from Nairobi who scored more than 80:**
```sql
SELECT *
FROM students
WHERE location = 'Nairobi'
AND marks > 80;
```
#### OR

At least one condition must be true.

**Find students from Nairobi or Mombasa:**
```sql
SELECT *
FROM students
WHERE location = 'Nairobi'
OR location = 'Mombasa';
```

#### NOT

Exclude a condition.

**Find students who are not taking Python:**
```sql
SELECT *
FROM students
WHERE NOT course = 'Python';
```
---
## Part 9: Other Useful WHERE Operators

#### BETWEEN

**Find students whose marks are between 70 and 90:**
```sql
SELECT *
FROM students
WHERE marks BETWEEN 70 AND 90;
```
#### IN

**Find students from Nairobi, Kisumu, or Mombasa:**

```sql
SELECT *
FROM students
WHERE location IN ('Nairobi', 'Kisumu', 'Mombasa');
```

#### LIKE

**Find students whose first name starts with J:**
```sql
SELECT *
FROM students
WHERE first_name LIKE 'J%';
```

**Find students whose last name ends with i:**

```sql
SELECT *
FROM students
WHERE last_name LIKE '%i';
```

**Find students whose first name contains a:**

```sql
SELECT *
FROM students
WHERE first_name LIKE '%a%';
```
---
## Part 10: ORDER BY — Sorting Results

**ORDER BY** is used to sort query results.

**Sort Marks from Lowest to Highest**
```sql
SELECT *
FROM students
ORDER BY marks ASC;
```
>ASC means ascending order

**Sort Marks from Highest to Lowest**

```sql
SELECT *
FROM students
ORDER BY marks DESC;
```
>DESC means descending order.

**Sort by Age**
```sql
SELECT *
FROM students
ORDER BY age ASC;
```
---

## Part 11: Combining WHERE and ORDER BY

**Find students taking Database and sort them from highest to lowest marks:**

```sql
SELECT *
FROM students
WHERE course = 'Database'
ORDER BY marks DESC;
```

**Find students from Nairobi who scored at least 70, then sort them alphabetically:**

```sql
SELECT *
FROM students
WHERE location = 'Nairobi'
AND marks >= 70
ORDER BY first_name ASC;
```

**Find Python students and display them from the youngest to the oldest:**

```sql
SELECT *
FROM students
WHERE course = 'Python'
ORDER BY age ASC;
```
