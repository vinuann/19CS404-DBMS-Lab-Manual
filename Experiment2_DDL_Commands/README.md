# Experiment 2: DDL Commands

## AIM
To study and implement DDL commands and different types of constraints.

## THEORY

### 1. CREATE
Used to create a new relation (table).

**Syntax:**
```sql
CREATE TABLE (
  field_1 data_type(size),
  field_2 data_type(size),
  ...
);
```
### 2. ALTER
Used to add, modify, drop, or rename fields in an existing relation.
(a) ADD
```sql
ALTER TABLE std ADD (Address CHAR(10));
```
(b) MODIFY
```sql
ALTER TABLE relation_name MODIFY (field_1 new_data_type(size));
```
(c) DROP
```sql
ALTER TABLE relation_name DROP COLUMN field_name;
```
(d) RENAME
```sql
ALTER TABLE relation_name RENAME COLUMN old_field_name TO new_field_name;
```
### 3. DROP TABLE
Used to permanently delete the structure and data of a table.
```sql
DROP TABLE relation_name;
```
### 4. RENAME
Used to rename an existing database object.
```sql
RENAME TABLE old_relation_name TO new_relation_name;
```
### CONSTRAINTS
Constraints are used to specify rules for the data in a table. If there is any violation between the constraint and the data action, the action is aborted by the constraint. It can be specified when the table is created (using CREATE TABLE) or after it is created (using ALTER TABLE).
### 1. NOT NULL
When a column is defined as NOT NULL, it becomes mandatory to enter a value in that column.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) NOT NULL
);
```
### 2. UNIQUE
Ensures that values in a column are unique.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) UNIQUE
);
```
### 3. CHECK
Specifies a condition that each row must satisfy.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) CHECK (logical_expression)
);
```
### 4. PRIMARY KEY
Used to uniquely identify each record in a table.
Properties:
Must contain unique values.
Cannot be null.
Should contain minimal fields.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) PRIMARY KEY
);
```
### 5. FOREIGN KEY
Used to reference the primary key of another table.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size),
  FOREIGN KEY (column_name) REFERENCES other_table(column)
);
```
### 6. DEFAULT
Used to insert a default value into a column if no value is specified.

Syntax:
```sql
CREATE TABLE Table_Name (
  col_name1 data_type,
  col_name2 data_type,
  col_name3 data_type DEFAULT 'default_value'
);
```

**Question 1**
--
Write an SQL query to change the name of the column id to employee_id in the table employee.
For example:

Test	Result
pragma table_info('employee');
cid         name         type        notnull     dflt_value  pk
----------  -----------  ----------  ----------  ----------  ----------
0           employee_id  integer     0                       0
1           salary       number      0                       0

```sql
ALTER TABLE employee
RENAME COLUMN id TO employee_id;
```

**Output:**
<img width="1237" height="145" alt="image" src="https://github.com/user-attachments/assets/f0f31cf4-d924-4f51-92d0-8052e1a77b75" />


**Question 2**
---
Write a SQL query to Add a new column mobilenumber as number in the Student_details table.

Sample table: Student_details

 cid              name             type   notnull     dflt_value  pk
---------------  ---------------  -----  ----------  ----------  ----------
0                RollNo           int    0                       1
1                Name             VARCH  1                       0
2                Gender           TEXT   1                       0
3                Subject          VARCH  0                       0
4                MARKS            INT (  0                       0
For example:

Test	Result
pragma table_info('Student_details');
cid         name        type        notnull     dflt_value  pk
----------  ----------  ----------  ----------  ----------  ----------
0           RollNo      int         0                       1
1           Name        VARCHAR(10  1                       0
2           Gender      TEXT        1                       0
3           Subject     VARCHAR(30  0                       0
4           MARKS       INT (3)     0                       0
5           mobilenumb  number      0                       0

```sql
ALTER TABLE student_details
ADD COLUMN mobilenumber number;
```

**Output:**

<img width="1083" height="213" alt="image" src="https://github.com/user-attachments/assets/38fa5581-1d24-4b2d-bef9-910fdbb01ac1" />


**Question 3**
---
Create a table named Employees with the following constraints:

EmployeeID should be the primary key.
FirstName and LastName should be NOT NULL.
Email should be unique.
Salary should be greater than 0.
DepartmentID should be a foreign key referencing the Departments table.
For example:

Test	Result
-- Attempt to insert a record with NULL FirstName
INSERT INTO Employees (EmployeeID, FirstName, LastName, Email, Salary, DepartmentID)
VALUES (1, NULL, 'Doe', 'john.doe@example.com', 50000, 1);
Error: NOT NULL constraint failed: Employees.FirstName


```sql
CREATE TABLE Employees (
    EmployeeID INTEGER PRIMARY KEY,
    FirstName TEXT NOT NULL,
    LastName TEXT NOT NULL,
    Email TEXT UNIQUE,
    Salary INTEGER CHECK (Salary>0),
    DepartmentID INTEGER,
    FOREIGN KEY (DepartmentID) REFERENCES Departments (DepartmentID)
);
```

**Output:**

<img width="843" height="261" alt="image" src="https://github.com/user-attachments/assets/e4ac7d2b-a11a-4cf0-9f35-c23e6e0f2cd1" />


**Question 4**
---
Create a table named Bonuses with the following constraints:
BonusID as INTEGER should be the primary key.
EmployeeID as INTEGER should be a foreign key referencing Employees(EmployeeID).
BonusAmount as REAL should be greater than 0.
BonusDate as DATE.
Reason as TEXT should not be NULL.
For example:

Test	Result
INSERT INTO Bonuses (BonusID, EmployeeID, BonusAmount, BonusDate, Reason) VALUES (1, 6, 1000.0, '2024-08-01', 'Outstanding performance');
SELECT * FROM Bonuses;
BonusID     EmployeeID  BonusAmount  BonusDate   Reason
----------  ----------  -----------  ----------  -----------------------
1           6           1000.0       20

```sql
CREATE TABLE Bonuses (
    BonusID INTEGER PRIMARY KEY,
    EmployeeID INTEGER,
    BonusAmount REAL CHECK (BonusAmount > 0),
    BonusDate DATE,
    Reason TEXT NOT NULL,
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
);
```

**Output:**

<img width="1109" height="151" alt="image" src="https://github.com/user-attachments/assets/126050ee-488b-4dcd-98a2-171c6ca52db4" />


**Question 5**
---
Insert all books from Out_of_print_books into Books

Table attributes are ISBN, Title, Author, Publisher, YearPublished

For example:

Test	Result
select * from Books;
ISBN            Title           Author              Publisher      YearPublished
--------------  --------------  ------------------  -------------  -------------
978-1234567890  The Lost World  Arthur Conan Doyle  Vintage Books  1912
978-0987654321  Gone with the   Margaret Mitchell   Macmillan      1936
978-1122334455  Moby Dick       Herman Melville     Harper & Brot  1851
```

INSERT INTO Books (ISBN, Title, Author, Publisher, YearPublished)
SELECT ISBN, Title, Author, Publisher, YearPublished
FROM Out_of_print_books;
```

**Output:**
<img width="1228" height="156" alt="image" src="https://github.com/user-attachments/assets/292266ad-806a-42fd-a3c3-0864d5af7af1" />


**Question 6**
---
Create a table named Attendance with the following constraints:
AttendanceID as INTEGER should be the primary key.
EmployeeID as INTEGER should be a foreign key referencing Employees(EmployeeID).
AttendanceDate as DATE.
Status as TEXT should be one of 'Present', 'Absent', 'Leave'.
For example:

Test	Result
INSERT INTO Attendance (AttendanceID, EmployeeID, AttendanceDate, Status) VALUES (1, 1, '2024-08-01', 'Present');
SELECT * FROM Attendance;
AttendanceID  EmployeeID  AttendanceDate  Status
------------  ----------  --------------  ----------
1             1           2024-08-01      Present

```sql
CREATE TABLE Attendance (
    AttendanceID INTEGER PRIMARY KEY,
    EmployeeID INTEGER,
    AttendanceDate DATE,
    Status TEXT CHECK (Status IN ('Present', 'Absent', 'Leave')),
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
);
```

**Output:**

<img width="834" height="147" alt="image" src="https://github.com/user-attachments/assets/bbe5a283-e2e0-456b-89f4-5090226073ac" />


**Question 7**
---
Insert the following students into the Student_details table:
RollNo      Name        Gender      Subject     MARKS
----------  ----------  ----------  ----------  ----------
202            Ella King         F           Chemistry   87
203            James Bond   M          Literature    78

 

 

For example:

Test	Result
SELECT * FROM Student_details;
RollNo      Name        Gender      Subject     MARKS
----------  ----------  ----------  ----------  ----------
202         Ella King   F           Chemistry   87
203         James Bond  M           Literature  78


```sql
INSERT INTO Student_details (RollNo, Name,Gender,Subject, MARKS)
VALUES
(202,'Ella King','F','Chemistry ',  87),
(203 ,'James Bond',   'M ',         'Literature',    78);
```

**Output:**

<img width="1193" height="162" alt="image" src="https://github.com/user-attachments/assets/ca6aba22-d8b8-4818-aab3-09ff261e8f57" />


**Question 8**
---
In the Student_details table, insert a student record where some fields are NULL, another record where all fields are filled without any NULL values, and a third record where some fields are filled, and others are left as NULL.

RollNo      Name            Gender      Subject      MARKS
----------  ------------    ----------  ----------   ----------
205         Olivia Green    F
207         Liam Smith      M           Mathematics  85
208         Sophia Johnson  F           Science
For example:

Test	Result
select * from Student_details;
RollNo      Name          Gender      Subject     MARKS
----------  ------------  ----------  ----------  ----------
205         Olivia Green  F
207         Liam Smith    M           Mathematic  85
208         Sophia Johns  F           Science

```sql
INSERT INTO Student_details (RollNo, Name, Gender, Subject, MARKS)
VALUES
(205, 'Olivia Green', 'F', NULL, NULL),
(207, 'Liam Smith', 'M', 'Mathematics', 85),
(208, 'Sophia Johnson', 'F', 'Science', NULL);
```

**Output:**

<img width="1226" height="177" alt="image" src="https://github.com/user-attachments/assets/84af70c4-b03f-4ff9-b705-c0e272048dbb" />


**Question 9**
---
Create a table named Department with the following constraints:
DepartmentID as INTEGER should be the primary key.
DepartmentName as TEXT should be unique and not NULL.
Location as TEXT.
For example:

Test	Result
INSERT INTO Department (DepartmentID, DepartmentName, Location) VALUES (1, 'Human Resources', 'New York');
select * from Department;
DepartmentID  DepartmentName   Location
------------  ---------------  ----------
1             Human Resources  New York

```sql
CREATE TABLE Department(
    DepartmentID INTEGER PRIMARY KEY,
    DepartmentName TEXT UNIQUE NOT NULL,
    Location TEXT
);
```

**Output:**

<img width="912" height="156" alt="image" src="https://github.com/user-attachments/assets/c25c57c7-c8ea-4139-b660-2d719207dbc6" />


**Question 10**
---
Create a table named Locations with the following columns:

LocationID as INTEGER
LocationName as TEXT
Address as TEXT
For example:

Test	Result
pragma table_info('Locations');
cid       name             type        notnull     dflt_value  pk
--------  ---------------  ----------  ----------  ----------  ----------
0         LocationID       INTEGER     0                       0
1         LocationName     TEXT        0                       0
2         Address          TEXT        0                       0


```sql
CREATE TABLE Locations(
    LocationID INTEGER,
    LocationName TEXT,
    Address TEXT
);
```

**Output:**

<img width="1137" height="230" alt="image" src="https://github.com/user-attachments/assets/68a539e1-f139-45fa-bd58-8acd6201e4ef" />



## RESULT
Thus, the SQL queries to implement different types of constraints and DDL commands have been executed successfully.
