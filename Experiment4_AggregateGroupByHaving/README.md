# Experiment 4: Aggregate Functions, Group By and Having Clause

## AIM
To study and implement aggregate functions, GROUP BY, and HAVING clause with suitable examples.

## THEORY

### Aggregate Functions
These perform calculations on a set of values and return a single value.

- **MIN()** – Smallest value  
- **MAX()** – Largest value  
- **COUNT()** – Number of rows  
- **SUM()** – Total of values  
- **AVG()** – Average of values

**Syntax:**
```sql
SELECT AGG_FUNC(column_name) FROM table_name WHERE condition;
```
### GROUP BY
Groups records with the same values in specified columns.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name;
```
### HAVING
Filters the grouped records based on aggregate conditions.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

**Question 1**
--
What is the total number of appointments scheduled for each day?

Table: Appointments

name                 type
-------------------  ----------
AppointmentID        INTEGER
PatientID            INTEGER
DoctorID             INTEGER
AppointmentDateTime  DATETIME
Purpose              TEXT
Status               TEXT
 

For example:

Result
AppointmentDate  TotalAppointments
---------------  -----------------
2024-02-16       4
2024-02-18       1
2024-02-20       1
2024-02-21       1
2024-02-22       1
2024-02-23       2


```sql
select date(AppointmentDateTime) as "AppointmentDate",
count(*) as TotalAppointments
from Appointments
group by date(AppointmentDateTime)
order by date(AppointmentDateTime);
```

**Output:**

<img width="941" height="737" alt="image" src="https://github.com/user-attachments/assets/59c9a501-fcb7-43b3-b19b-2e0adb6c472d" />


**Question 2**
---
How many patients are covered by each insurance company?

Sample table:Insurance Table

name               type
-----------------  ----------
InsuranceID        INTEGER
PatientID          INTEGER
InsuranceCompany   TEXT
PolicyNumber       TEXT
PolicyHolder       TEXT
ValidityPeriod     TEXT
For example:

Result
InsuranceCompany   TotalPatients
-----------------  -------------
DEF Insurance      1
GHI Insurance      1
GLM Insurance      2
JKL Insurance      1
MNO Insurance      3
PQR Insurance      1
YZA Insurance      1


```sql
select "InsuranceCompany",
count("PatientID") as TotalPatients
from "Insurance"
group by "InsuranceCompany";
```

**Output:**
<img width="850" height="737" alt="image" src="https://github.com/user-attachments/assets/a01853de-2dcc-48a4-b6dc-a2598e859f55" />


**Question 3**
---
Write a SQL query to calculate total purchase amount of all orders. Return total purchase amount.

Sample table: orders

ord_no      purch_amt   ord_date    customer_id  salesman_id

----------  ----------  ----------  -----------  -----------

70001       150.5       2012-10-05  3005         5002

70009       270.65      2012-09-10  3001         5005

70002       65.26       2012-10-05  3002         5001

For example:

Result
TOTAL
----------
17541.18


```sql
select SUM(purch_amt) as TOTAL from orders;
```

**Output:**

<img width="494" height="394" alt="image" src="https://github.com/user-attachments/assets/e45d3563-f6ed-4bda-8db4-167cc300c4f5" />


**Question 4**
---
Write a SQL query to find the total amount of fruits with a unit type of 'LB'.

Note: Inventory attribute contains amount of fruits

Table: fruits

name        type
----------  ----------
id          INTEGER
name        TEXT
unit        TEXT
inventory   INTEGER
price       REAL
 

For example:

Result
total
----------
225

```sql
select SUM(inventory) as total
from fruits
where unit='LB';
```

**Output:**

<img width="477" height="384" alt="image" src="https://github.com/user-attachments/assets/88011488-e77f-4ab9-b83b-83dba65314ff" />


**Question 5**
---
Write a SQL query to Calculate the average email length (in characters) for people who lives in Mumbai city

Table: customer

name        type
----------  ----------
id          INTEGER
name        TEXT   
city        TEXT
email       TEXT
phone       INTEGER
For example:

Result
avg_email_length_below_30
-------------------------
14.0

```sql
select AVG(LENGTH(email)) as avg_email_length_below_30
from customer
where city='Mumbai';
```

**Output:**

<img width="712" height="393" alt="image" src="https://github.com/user-attachments/assets/d2fcac51-072a-495a-b2a0-7b53b2dff7c3" />


**Question 6**
---
Write a SQL query to find  how many employees work in California?

Table: employee

name        type
----------  ----------
id          INTEGER
name        TEXT
age         INTEGER
city        TEXT
income      INTEGER
 

For example:

Result
employees_in_california
-----------------------
2


```sql
select count(*) as employees_in_california from employee where city='California';
```

**Output:**
<img width="721" height="397" alt="image" src="https://github.com/user-attachments/assets/617888d2-c2bc-4770-9ec9-700b42bdee79" />

**Question 7**
---
Which cities (addresses) in the "customer1" table have an average salary lesser than Rs. 15000

Sample table: customer1



For example:

Result
address     AVG(salary)
----------  -----------
Ahmedabad   2000.0
Bhopal      8500.0
Delhi       1500.0
Hyderabad   4500.0
Indore      10000.0
Kota        2000.0
Mumbai      6500.0


```sql
select address,AVG(salary)
from customer1
group by address
having AVG(salary)<15000;
```

**Output:**
<img width="640" height="679" alt="image" src="https://github.com/user-attachments/assets/7105c244-4c29-4bbe-b610-539935812c6d" />


**Question 8**
---
-- Paste Question 8 here

```sql
-- Paste your SQL code below for Question 8
```

**Output:**

![Output8](output.png)

**Question 9**
---
Write the SQL query that accomplishes the selection of average price for each category from the "products" table and includes only those products where the average price falls between 10 and 15.

Sample table: products



For example:

Result
category_id  AVG(Price)
-----------  ----------
1            12.375


```sql
select category_id,AVG(Price) from products group by category_id having AVG(Price) between 10 and 15;
```

**Output:**
<img width="770" height="430" alt="image" src="https://github.com/user-attachments/assets/ec160979-7942-47ba-8c60-f92b8f7ebe68" />



**Question 10**
---
How many prescriptions were written by each doctor?

Sample tablePrescriptions Table



For example:

Result
DoctorID    TotalPrescriptions
----------  ------------------
1           1
2           1
3           1
4           1
5           1
6           1
7           1
8           1
9           1
10          1

```sql
select DoctorID,count(PrescriptionID) as TotalPrescriptions
from Prescriptions
group by DoctorID;
```

**Output:**
<img width="722" height="902" alt="image" src="https://github.com/user-attachments/assets/33151c25-68e6-4cd8-83d8-3ce0d866e789" />



## RESULT
Thus, the SQL queries to implement aggregate functions, GROUP BY, and HAVING clause have been executed successfully.
