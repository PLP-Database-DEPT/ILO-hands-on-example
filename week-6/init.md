## 🎯 Hands-on Class Activity


### Database Normalization With Real-World Examples

---

## Part 1

### First Normal Form (1NF) 

This normalization level ensures that each column in your data contains only atomic values.
##### Unnormalized Table
```sql
CREATE TABLE Student (
    Student_id INT PRIMARY KEY,
    Student_Email VARCHAR(100),
    Courses_Completed VARCHAR(255)
);

INSERT INTO Student (Student_id, Student_Email, Courses_Completed) VALUES
(235, 'jim@gmail.com', 'Introduction to Python, Intermediate Python'),
(455, 'kelly@yahoo.com', 'Cleaning Data in R'),
(767, 'amy@hotmail.com', 'Machine Learning Toolbox, Deep Learning in Python');
```
##### 📌 Normalized to 1NF

We split the repeating courses into a separate table:
```sql
-- Student table
CREATE TABLE Student (
    Student_id INT PRIMARY KEY,
    Student_Email VARCHAR(100)
);

-- Courses table (atomic values)
CREATE TABLE Student_Courses (
    Course_id INT AUTO_INCREMENT PRIMARY KEY,
    Student_id INT,
    Course_Name VARCHAR(100),
    FOREIGN KEY (Student_id) REFERENCES Student(Student_id)
);

-- Insert data
INSERT INTO Student (Student_id, Student_Email) VALUES
(235, 'jim@gmail.com'),
(455, 'kelly@yahoo.com'),
(767, 'amy@hotmail.com');

INSERT INTO Student_Courses (Student_id, Course_Name) VALUES
(235, 'Introduction to Python'),
(235, 'Intermediate Python'),
(455, 'Cleaning Data in R'),
(767, 'Machine Learning Toolbox'),
(767, 'Deep Learning in Python');
```
---
## Part 2
 ### Second Normal Form (2NF)

Eliminates partial dependencies by ensuring that **non-key attributes** depend only on the **primary key**.

What this means, in essence, is that there should be a direct relationship between each column and the primary key, and not between other columns.
##### Unnormalized (violates 2NF)

```sql
CREATE TABLE Student_Course_Progress (
    Student_id INT,
    Course_id INT,
    Instructor_id INT,
    Instructor VARCHAR(100),
    Progress DECIMAL(3,2),
    PRIMARY KEY (Student_id, Course_id)
);

INSERT INTO Student_Course_Progress (Student_id, Course_id, Instructor_id, Instructor, Progress) VALUES
(235, 2001, 560, 'Nick Carchedi', 0.55),
(455, 2345, 658, 'Ginger Grant', 0.10),
(767, 6584, 999, 'Chester Ismay', 1.00);
```
##### 📌 Normalized to 2NF
We separate **Instructor details** into their own table, since they depend only on **Instructor_id**, not the full (Student_id, Course_id) key.
```sql
-- Student table
CREATE TABLE Student (
    Student_id INT PRIMARY KEY
);

-- Course table
CREATE TABLE Course (
    Course_id INT PRIMARY KEY
);

-- Instructor table (atomic, no redundancy)
CREATE TABLE Instructor (
    Instructor_id INT PRIMARY KEY,
    Instructor_Name VARCHAR(100)
);

-- Student progress table (depends fully on Student + Course)
CREATE TABLE Student_Course_Progress (
    Student_id INT,
    Course_id INT,
    Progress DECIMAL(3,2),
    PRIMARY KEY (Student_id, Course_id),
    FOREIGN KEY (Student_id) REFERENCES Student(Student_id),
    FOREIGN KEY (Course_id) REFERENCES Course(Course_id)
);

-- Course-Instructor relationship (each course has an instructor)
CREATE TABLE Course_Instructor (
    Course_id INT PRIMARY KEY,
    Instructor_id INT,
    FOREIGN KEY (Course_id) REFERENCES Course(Course_id),
    FOREIGN KEY (Instructor_id) REFERENCES Instructor(Instructor_id)
);

-- Insert data
-- Insert Students
INSERT INTO Student (Student_id) VALUES
(235),
(455),
(767);

-- Insert Courses
INSERT INTO Course (Course_id) VALUES
(2001),
(2345),
(6584);

-- Insert Instructors
INSERT INTO Instructor (Instructor_id, Instructor_Name) VALUES
(560, 'Nick Carchedi'),
(658, 'Ginger Grant'),
(999, 'Chester Ismay');

-- Insert Student Progress
INSERT INTO Student_Course_Progress (Student_id, Course_id, Progress) VALUES
(235, 2001, 0.55),
(455, 2345, 0.10),
(767, 6584, 1.00);

-- Insert Course–Instructor relationship
INSERT INTO Course_Instructor (Course_id, Instructor_id) VALUES
(2001, 560),
(2345, 658),
(6584, 999);
```
---
## Part 3
### Third Normal Form (3NF)

Removes **transitive dependencies** by ensuring that non-key attributes depend only on the primary key. 

This level of normalization builds on 2NF.

**transitive dependencies** is when non-key attributes depending on other non-key attributes.
##### Unnormalized (violates 3NF)
```sql
CREATE TABLE Course_Info (
    Course_id INT PRIMARY KEY,
    Instructor_id INT,
    Instructor_Name VARCHAR(100),
    Tech VARCHAR(50)
);

INSERT INTO Course_Info (Course_id, Instructor_id, Instructor_Name, Tech) VALUES
(2001, 560, 'Nick Carchedi', 'Python'),
(2345, 658, 'Ginger Grant', 'SQL'),
(6584, 999, 'Chester Ismay', 'R');
```
##### 📌 Normalize to 3NF
We separate:
- Instructor details into their own table.
- Course details into their own table.
- Keep Course_Instructor as a relationship table.
```sql
-- Instructors table (no redundancy)
CREATE TABLE Instructor (
    Instructor_id INT PRIMARY KEY,
    Instructor_Name VARCHAR(100)
);

-- Courses table (each course belongs to one tech)
CREATE TABLE Course (
    Course_id INT PRIMARY KEY,
    Tech VARCHAR(50)
);

-- Relationship: which instructor teaches which course
CREATE TABLE Course_Instructor (
    Course_id INT PRIMARY KEY,
    Instructor_id INT,
    FOREIGN KEY (Course_id) REFERENCES Course(Course_id),
    FOREIGN KEY (Instructor_id) REFERENCES Instructor(Instructor_id)
);

-- Insert Instructors
INSERT INTO Instructor (Instructor_id, Instructor_Name) VALUES
(560, 'Nick Carchedi'),
(658, 'Ginger Grant'),
(999, 'Chester Ismay');

-- Insert Courses
INSERT INTO Course (Course_id, Tech) VALUES
(2001, 'Python'),
(2345, 'SQL'),
(6584, 'R');

-- Assign Instructors to Courses
INSERT INTO Course_Instructor (Course_id, Instructor_id) VALUES
(2001, 560),
(2345, 658),
(6584, 999);
```
