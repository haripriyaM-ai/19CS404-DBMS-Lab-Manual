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
What is the total number of appointments scheduled by each doctor?
Sample table:Appointments Table
| AppointmentID | PatientID | DoctorID | AppointmentDateTime   | Purpose                | Status     |
|---------------|-----------|----------|------------------------|------------------------|------------|
| 1             | 1         | 1        | 2024-02-15 09:00:00    | Routine checkup        | Confirmed  |
| 2             | 2         | 2        | 2024-02-16 10:30:00    | Vaccination            | Confirmed  |
| 3             | 3         | 3        | 2024-02-17 11:00:00    | Follow-up on surgery   | Confirmed  |


```sql
SELECT
DoctorID,
COUNT(*) AS TotalAppointments
FROM Appointments
GROUP BY DoctorID;
```

**Output:**

<img width="746" height="664" alt="image" src="https://github.com/user-attachments/assets/012fdda0-ddf1-4a5a-b38f-c2659ef0b036" />


**Question 2**
---
How many prescriptions were written by each doctor?

Sample tablePrescriptions Table
| PrescriptionID | PatientID | DoctorID | Date       | Medication | Dosage | Frequency       |
|----------------|-----------|----------|------------|------------|--------|------------------|
| 1              | 1         | 1        | 2024-01-10 | Lisinopril | 10 mg  | Once daily       |
| 2              | 2         | 2        | 2023-12-20 | MMR        | 0.5 mL | Once             |
| 3              | 3         | 3        | 2024-01-05 | Ibuprofen  | 400 mg | Every 6 hours    |


```sql
SELECT
DoctorID,
COUNT(*) AS TotalPrescriptions
FROM Prescriptions
GROUP BY DoctorID;
```

**Output:**

<img width="755" height="809" alt="image" src="https://github.com/user-attachments/assets/72ad2882-1183-451c-a0df-ec1e569e4dfb" />


**Question 3**
---
What is the total number of appointments scheduled for each day?

Table: Appointments
| name                 | type     |
|----------------------|----------|
| AppointmentID        | INTEGER  |
| PatientID            | INTEGER  |
| DoctorID             | INTEGER  |
| AppointmentDateTime  | DATETIME |
| Purpose              | TEXT     |
| Status               | TEXT     |


```sql
SELECT 
strftime("%Y-%m-%d",AppointmentDateTime) AS AppointmentDate,
COUNT(*) AS TotalAppointments
FROM Appointments
GROUP BY AppointmentDate;
```

**Output:**

<img width="830" height="707" alt="image" src="https://github.com/user-attachments/assets/0ea14a7e-6a87-4b71-ad91-e0c2b0124ea7" />


**Question 4**
---
Write a SQL query to find the maximum purchase amount.

Sample table: orders
| ord_no | purch_amt | ord_date    | customer_id | salesman_id |
|--------|-----------|-------------|--------------|--------------|
| 70001  | 150.50    | 2012-10-05  | 3005         | 5002         |
| 70009  | 270.65    | 2012-09-10  | 3001         | 5005         |
| 70002  | 65.26     | 2012-10-05  | 3002         | 5001         |


```sql
SELECT
MAX(purch_amt) as "MAXIMUM"
FROM orders;
```

**Output:**

<img width="388" height="325" alt="image" src="https://github.com/user-attachments/assets/8b31e5d7-1e6d-4b78-a0e5-a781c2b672ca" />


**Question 5**
---
Write a SQL query to return the total number of rows in the 'customer' table where the city is not Noida.

Sample table: customer
| id | name        | city      | email          | phone      |
|----|-------------|-----------|----------------|------------|
| 1  | Ravi Kumar  | Delhi     | ravi@gmail.com | 985664321  |
| 2  | Neha Sharma | Mumbai    | neha@gmail.com | 987654321  |
| 3  | Rohit Singh | Bangalore | rohit@gmail.com| 887694721  |


```sql
SELECT 
COUNT(*) AS "COUNT"
FROM customer
WHERE city NOT IN("Noida");
```

**Output:**

<img width="355" height="328" alt="image" src="https://github.com/user-attachments/assets/4c6e92c1-a6ae-4ecf-9d1e-b02280089dfc" />


**Question 6**
---
Write a SQL query to  find the average salary of all employees?

Table: employee
| name  | type    |
|-------|---------|
| id    | INTEGER |
| name  | TEXT    |
| age   | INTEGER |
| city  | TEXT    |
| income| INTEGER |

```sql
SELECT
AVG(income) as "Average_Salary"
FROM employee;
```

**Output:**

<img width="472" height="322" alt="image" src="https://github.com/user-attachments/assets/c1bf3ea1-adf7-4890-a983-a35b6ab60512" />


**Question 7**
---
Write a SQL query to find the youngest employee in the company?

Table: employee
| name   | type    |
|--------|---------|
| id     | INTEGER |
| name   | TEXT    |
| age    | INTEGER |
| city   | TEXT    |
| income | INTEGER |


```sql
SELECT
name as "Employee_Name",
MIN(age) as Age
FROM employee;
```

**Output:**

<img width="656" height="330" alt="image" src="https://github.com/user-attachments/assets/bb8e7a4b-1bda-4b5f-8f3f-834b7c652ba4" />


**Question 8**
---
Write the SQL query that achieves the grouping of data by age, calculates the minimum income for each age group, and includes only those age groups where the minimum income is less than 1,000,000.

Sample table: employee
| id  | name   | age | city      | income  |
|-----|--------|-----|-----------|---------|
| 101 | Peter  | 32  | NewYork   | 200000  |
| 102 | Mark   | 32  | California| 300000  |
| 103 | Donald | 40  | Arizona   | 1000000 |
| 104 | Obama  | 35  | Florida   | 5000000 |


```sql
SELECT
age,
MIN(Income) as Income
FROM employee
GROUP BY age
HAVING MIN(Income) < 1000000;
```

**Output:**

<img width="594" height="452" alt="image" src="https://github.com/user-attachments/assets/9fde0316-5247-428a-ac98-49cda5e65bdc" />


**Question 9**
---
Write the SQL query that achieves the grouping of data by occupation, calculates the total work hours for each occupation, and excludes occupations where the total work hour sum is not greater than 20.

Sample table: employee1
| id | name    | occupation | jdate | workhour |
|----|---------|------------|-------|-----------|
| 1  | Joseph  | Business   | 2006  | 10        |
| 2  | Stephen | Doctor     | 2006  | 15        |
| 3  | Mark    | Engineer   | 2006  | 12        |
| 4  | Peter   | Teacher    | 2006  | 9         |


```sql
SELECT
occupation,
SUM(workhour)
FROM employee1
GROUP BY occupation
HAVING SUM(workhour) > 20;
```

**Output:**

<img width="662" height="492" alt="image" src="https://github.com/user-attachments/assets/22ab31cb-0955-43f8-bfa6-b7db10c2bfeb" />


**Question 10**
---
Write the SQL query that performs grouping by age groups and displays the maximum salary for each group, excluding groups where the maximum salary is not greater than 8000. 

Note: Calculate the age group as multiples of 5.

Eg., 20,22,23 comes in age group 20. 

25,27,29 comes in age group 25.

Sample table: customer1
| id | name    | age | address     | salary |
|----|---------|-----|-------------|--------|
| 1  | Ramesh  | 32  | Ahmedabad   | 2000   |
| 2  | Khilan  | 25  | Delhi       | 1500   |
| 3  | Kaushik | 23  | Kota        | 2000   |


```sql
SELECT
(age/5)*5 as "age_group",
MAX(salary) 
FROM customer1
GROUP BY (age/5)*5
HAVING MAX(salary) > 8000;
```

**Output:**

<img width="618" height="391" alt="image" src="https://github.com/user-attachments/assets/612543bd-aef2-444e-99f2-a55d361be9ea" />

**SEB Grades**
<img width="1500" height="800" alt="image" src="https://github.com/user-attachments/assets/d7508d47-a253-4759-a05d-50dce322d488" />



## RESULT
Thus, the SQL queries to implement aggregate functions, GROUP BY, and HAVING clause have been executed successfully.
