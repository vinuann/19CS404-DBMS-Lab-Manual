# Experiment 3: DML Commands

## AIM
To study and implement DML (Data Manipulation Language) commands.

## THEORY

### 1. INSERT INTO
Used to add records into a relation.
These are three type of INSERT INTO queries which are as
A)Inserting a single record
**Syntax (Single Row):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES (value_1, value_2, ...);
```
**Syntax (Multiple Rows):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES
(value_1, value_2, ...),
(value_3, value_4, ...);
```
**Syntax (Insert from another table):**
```sql
INSERT INTO table_name SELECT * FROM other_table WHERE condition;
```
### 2. UPDATE
Used to modify records in a relation.
Syntax:
```sql
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```
### 3. DELETE
Used to delete records from a relation.
**Syntax (All rows):**
```sql
DELETE FROM table_name;
```
**Syntax (Specific condition):**
```sql
DELETE FROM table_name WHERE condition;
```
### 4. SELECT
Used to retrieve records from a table.
**Syntax:**
```sql
SELECT column1, column2 FROM table_name WHERE condition;
```
**Question 1**
--
Write a SQL query to Delete customers with 'GRADE' 2 and 'CUST_NAME' starting with 'M', and whose 'PAYMENT_AMT' is less than 3000

Sample table: Customer

+-----------+-------------+-------------+--------------+--------------+-------+-------------+-------------+-------------+---------------+--------------+------------+  
|CUST_CODE  | CUST_NAME   | CUST_CITY   | WORKING_AREA | CUST_COUNTRY | GRADE | OPENING_AMT | RECEIVE_AMT | PAYMENT_AMT |OUTSTANDING_AMT| PHONE_NO     | AGENT_CODE |
+-----------+-------------+-------------+--------------+--------------+-------+-------------+-------------+-------------+---------------+--------------+------------+
| C00013    | Holmes      | London      | London       | UK           |     2 |     6000.00 |     5000.00 |     7000.00 |       4000.00 | BBBBBBB      | A003       |
| C00001    | Micheal     | New York    | New York     | USA          |     2 |     3000.00 |     5000.00 |     2000.00 |       6000.00 | CCCCCCC      | A008       |
| C00020    | Albert      | New York    | New York     | USA          |     3 |     5000.00 |     7000.00 |     6000.00

```sql
DELETE FROM customer
WHERE GRADE = 2
AND CUST_NAME LIKE 'M%'
AND PAYMENT_AMT < 3000;
```

**Output:**

<img width="1234" height="237" alt="image" src="https://github.com/user-attachments/assets/415be361-8543-4a18-aacf-074d8b8f395f" />


**Question 2**
---
Write a query to find all the employees whose salary is between 50000 to 100000 from employeeposition table.

EmpID

EmpPosition

DateOfJoining

Salary

1

Manager

01/05/2024

500000

2

Executive

02/05/2024

75000

 

For example:

Result
EmpID       EmpPosition  DateOfJoining  Salary
----------  -----------  -------------  ----------
2           Executive    2024-05-02     75000
3           Manager      2024-05-01     90000
2           Lead         2024-05-02     85000

```sql
SELECT*
FROM employeeposition
WHERE Salary BETWEEN 50000 AND 100000;
```

**Output:**

<img width="786" height="155" alt="image" src="https://github.com/user-attachments/assets/0d567e11-31e8-4e48-b96f-35e48b54f6e4" />



**Question 3**
---
Increase the reorder level by 30% for products from 'Food' category having quantity in stock less than 50% of existing reorder level in the products table
name               type
--------------  ----------
product_id         INT
product_name       VARCHAR(10)
category           VARCHAR(50)
cost_price         DECIMAL(10)
sell_price         DECIMAL(10)
reorder_lvl        INT
quantity              INT
supplier_id           INT
For example:

Test	Result
select changes();
changes()
----------
4


```sql
UPDATE products
SET reorder_lvl = reorder_lvl * 1.30
WHERE category = 'Food'
  AND quantity < reorder_lvl * 0.50;
```

**Output:**

<img width="1260" height="236" alt="image" src="https://github.com/user-attachments/assets/ae7083e6-677c-4794-819b-a85a3a7c7b2c" />


**Question 4**
---
Write a SQL query to calculate the discounted price for products whose original price is between $50 and $150. Return product_id, original_price, discount_percentage, and discounted_price.

Sample table: Products

product_id | original_price | discount_percentage

 ------------+----------------+--------------------- 

101 | 50.00 | 0.10 

102 | 125.00 | 0.15

 103 | 200.00 | 0.20

 

 

For example:

Result
product_id  original_price  discount_percentage  discounted_price
----------  --------------  -------------------  ----------------
101         50.0            0.1                  45.0
102         75.0            0.15                 63.75
103         100.0           0.2                  80.0

```sql
SELECT
    product_id,
    original_price,
    discount_percentage,
    original_price*(1-discount_percentage) AS discounted_price
FROM Products
WHERE original_price BETWEEN 50 AND 150;
```

**Output:**

<img width="1012" height="177" alt="image" src="https://github.com/user-attachments/assets/49e26bd7-e131-4393-8bd7-e801eef0fc01" />


**Question 5**
---
Write a SQL query to delete a doctor from Doctors table whos specialization is 'Cardiology'

Sample table: Doctors

attributes: doctor_id, first_name, last_name, specialization


```sql
DELETE FROM Doctors
WHERE specialization = 'Cardiology';
```

**Output:**

<img width="1013" height="241" alt="image" src="https://github.com/user-attachments/assets/b0ad6855-6ce5-4c21-abf4-fcd0e86af79b" />


**Question 6**
---
Write a SQL query to select patient's names along with their age groups (e.g., 'Under 20', '20-30', '31-40', '41-50', 'Above 50') based on their date of birth. 

Note: Consider current date as '2023-12-30' while calculating age.

Table: Patients

name                  type
--------------------  ----------
patient_id            INT
first_name            VARCHAR(50
last_name             VARCHAR(50
date_of_birth         DATE
admission_date        DATE
discharge_date        DATE
doctor_id             INT
For example:

Result
first_name  last_name   AgeGroup
----------  ----------  ----------
Alice       Williams    41-50
Bob         Miller      20-30
Charlie     Davis       Above 50


```sql
SELECT
    first_name,
    last_name,
    CASE
        WHEN (strftime('%Y', '2023-12-30') - strftime('%Y', date_of_birth)
              - (strftime('%m-%d', '2023-12-30') < strftime('%m-%d', date_of_birth))) < 20
            THEN 'Under 20'
        WHEN (strftime('%Y', '2023-12-30') - strftime('%Y', date_of_birth)
              - (strftime('%m-%d', '2023-12-30') < strftime('%m-%d', date_of_birth))) BETWEEN 20 AND 30
            THEN '20-30'
        WHEN (strftime('%Y', '2023-12-30') - strftime('%Y', date_of_birth)
              - (strftime('%m-%d', '2023-12-30') < strftime('%m-%d', date_of_birth))) BETWEEN 31 AND 40
            THEN '31-40'
        WHEN (strftime('%Y', '2023-12-30') - strftime('%Y', date_of_birth)
              - (strftime('%m-%d', '2023-12-30') < strftime('%m-%d', date_of_birth))) BETWEEN 41 AND 50
            THEN '41-50'
        ELSE 'Above 50'
    END AS AgeGroup
FROM Patients;
```

**Output:**
<img width="607" height="273" alt="image" src="https://github.com/user-attachments/assets/06762d59-40e6-4c61-ba11-3821f166b78b" />


**Question 7**
---
Write a SQL query to label rows in the Calculations table as 'Even' if value1 is even, otherwise 'Odd'.

cid         name        type        notnull     dflt_value  pk
----------  ----------  ----------  ----------  ----------  ----------
0           id          INTEGER     0                       1
1           value1      REAL        0                       0
2           value2      REAL        0                       0
3           base        INTEGER     0                       0
4           exponent    INTEGER     0                       0
5           number      REAL        0                       0
6           decimal     REAL        0                       0
 

For example:

Result
id          value1      parity
----------  ----------  ----------
1           -87.65      Odd
2           45.78       Odd
3           89.99       Odd
4           -0.005      Even


```sql
SELECT
    id,
    value1,
    CASE
        WHEN CAST(value1 AS INTEGER) % 2 = 0 THEN 'Even'
        ELSE 'Odd'
    END AS parity
FROM Calculations;
```

**Output:**
<img width="589" height="312" alt="image" src="https://github.com/user-attachments/assets/f33a318b-1598-4225-a511-1be3fd9f0940" />


**Question 8**
---
Write a SQL query to find all employees who were hired in the year 2022 from emp table.

cid         name        type        
----------  ----------  ---------- 
0           empno       INT         
1           ename       VARCHAR(100)
2           job         VARCHAR(50)
3           mgr         INT        
4           hiredate    DATE        
5           sal         DECIMAL(10,2)  
6           comm        DECIMAL(10,2)  
7           deptno      INT         
For example:

Result
empno       ename       job         mgr         hiredate    sal         comm        deptno
----------  ----------  ----------  ----------  ----------  ----------  ----------  ----------
7369        SMITH       CLERK       7902        2022-08-22  800                     20
7499        ALLEN       SALESMAN    7698        2022-08-22  1600        300         30
7521        WARD        SALESMAN    7698        2022-08-22  1250        500         30
7900        JAMES       CLERK       7698        2022-08-22  950                     30
7902        FORD        ANALYST     7566        2022-08-22  3000                    20
7934        MILLER      CLERK       7782        2022-08-22  1300                    10

```sql
SELECT *
FROM emp
WHERE strftime('%Y' , hiredate)='2022';
```

**Output:**

<img width="1279" height="206" alt="image" src="https://github.com/user-attachments/assets/b9344ea9-9d5c-4880-9974-3cdcc11c91da" />


**Question 9**
---
 Write a query to retrieve the first four characters of  EmpLname from the EmployeeInfo table.

EmployeeInfo Table

EmpID

EmpFname

EmpLname

Department

Project

Address

DOB

Gender

1

Sanjay

Mehra

HR

P1

Hyderabad(HYD)

01/12/1976

M

2

Ananya

Mishra

Admin

P2

Delhi(DEL)

02/05/1968

F

```sql
SELECT SUBSTR(EmpLname, 1, 4)
FROM EmployeeInfo;
```

**Output:**

<img width="453" height="199" alt="image" src="https://github.com/user-attachments/assets/9fae7215-2570-45ca-a9d8-55d6d83c8759" />


**Question 10**
---
Write a SQL query to calculate the absolute value of the value1 column from the Calculations table.

cid         name        type        notnull     dflt_value  pk
----------  ----------  ----------  ----------  ----------  ----------
0           id          INTEGER     0                       1
1           value1      REAL        0                       0
2           value2      REAL        0                       0
3           base        INTEGER     0                       0
4           exponent    INTEGER     0                       0
5           number      REAL        0                       0
6           decimal     REAL        0                       0

```sql
SELECT
    id,
    value1,
    ABS(value1) AS absolute_value
FROM Calculations;
```

**Output:**

<img width="638" height="167" alt="image" src="https://github.com/user-attachments/assets/e5cd91d3-3c05-425d-8046-ee514d2f41fe" />


## RESULT
Thus, the SQL queries to implement DML commands have been executed successfully.
