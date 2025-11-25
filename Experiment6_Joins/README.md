# Experiment 6: Joins

## AIM
To study and implement different types of joins.

## THEORY

SQL Joins are used to combine records from two or more tables based on a related column.

### 1. INNER JOIN
Returns records with matching values in both tables.

**Syntax:**
```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

### 2. LEFT JOIN
Returns all records from the left table, and matched records from the right.

**Syntax:**

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```
### 3. RIGHT JOIN
Returns all records from the right table, and matched records from the left.

**Syntax:**

```sql
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;
```
### 4. FULL OUTER JOIN
Returns all records when there is a match in either left or right table.

**Syntax:**

```sql
SELECT columns
FROM table1
FULL OUTER JOIN table2
ON table1.column = table2.column;
```

**Question 1**
--
Write the SQL query that achieves the selection of the date of birth from the "patients" table (aliased as "p") and all columns from the "appointments" table (aliased as "a"), with an inner join on the "patient_id" column and a condition filtering for patients with the first name 'Alice'.

PATIENTS TABLE:

ATTRIBUTES - patient_id, first_name, last_name, date_of_birth, admission_date, discharge_date, doctor_id
| patient_id | first_name | last_name | date_of_birth | admission_date | discharge_date | doctor_id |
|------------|------------|-----------|----------------|----------------|----------------|-----------|
| 1          | Alice      | Williams  | 1980-05-12     | 2024-01-10     | NULL           | 1         |
| 2          | Bob        | Miller    | 1995-08-23     | 2024-02-15     | 2024-03-01     | 2         |
| 3          | Charlie    | Davis     | 1972-11-30     | 2024-03-10     | NULL           | 3         |

APPOINTMENTS TABLE:

ATTRIBUTES - appointment_id, patient_id, doctor_id, appointment_date
| appointment_id | patient_id | doctor_id | appointment_date       |
|----------------|------------|-----------|-------------------------|
| 1              | 1          | 1         | 2024-01-05 10:00:00     |
| 2              | 2          | 2         | 2024-02-20 14:30:00     |
| 3              | 3          | 3         | 2024-03-15 09:15:00     |

```sql
SELECT
p.date_of_birth,
a.appointment_id,
a.patient_id,
a.doctor_id,
a.appointment_date
FROM patients p
INNER JOIN appointments a
ON p.patient_id = a.patient_id
WHERE p.first_name = 'Alice';
```

**Output:**

<img width="1204" height="434" alt="image" src="https://github.com/user-attachments/assets/6a59a533-b96c-4394-aa49-84a21d8cc096" />


**Question 2**
---
From the following tables write a SQL query to locate those salespeople who do not live in the same city where their customers live and have received a commission of more than 12% from the company. Return Customer Name, customer city, Salesman, salesman city, commission.  

Sample table: customer
| customer_id | cust_name       | city       | grade | salesman_id |
|-------------|-----------------|------------|--------|--------------|
| 3002        | Nick Rimando    | New York   | 100    | 5001         |
| 3007        | Brad Davis      | New York   | 200    | 5001         |
| 3005        | Graham Zusi     | California | 200    | 5002         |
| 3008        | Julian Green    | London     | 300    | 5002         |
| 3004        | Fabian Johnson  | Paris      | 300    | 5006         |
| 3009        | Geoff Cameron   | Berlin     | 100    | 5003         |
| 3003        | Jozy Altidor    | Moscow     | 200    | 5007         |
| 3001        | Brad Guzan      | London     |        | 5005         |

Sample table: salesman
| salesman_id | name        | city     | commission |
|-------------|-------------|----------|-------------|
| 5001        | James Hoog  | New York | 0.15        |
| 5002        | Nail Knite  | Paris    | 0.13        |
| 5005        | Pit Alex    | London   | 0.11        |
| 5006        | Mc Lyon     | Paris    | 0.14        |
| 5007        | Paul Adam   | Rome     | 0.13        |
| 5003        | Lauson Hen  | San Jose | 0.12        |

```sql
SELECT
c.cust_name as "Customer Name",
c.city,
s.name as Salesman,
s.city,
s.commission
FROM customer c
INNER JOIN salesman s
ON c.salesman_id = s.salesman_id
WHERE c.city <> s.city
AND s.commission > 0.12;
```

**Output:**

<img width="1209" height="696" alt="image" src="https://github.com/user-attachments/assets/b562fd1f-46bb-4f02-9a91-4b33341b874b" />


**Question 3**
---
Write the SQL query that achieves the selection of the first name from the "patients" table (aliased as "patient_name"), with an inner join on the "doctor_id" column and conditions filtering for patients whose doctor has the first name 'Emily', last name 'Johnson', and a non-null discharge date.

PATIENTS TABLE:

ATTRIBUTES - patient_id, first_name, last_name, date_of_birth, admission_date, discharge_date, doctor_id
| patient_id | first_name | last_name | date_of_birth | admission_date | discharge_date | doctor_id |
|------------|------------|-----------|----------------|----------------|----------------|-----------|
| 1          | Alice      | Williams  | 1980-05-12     | 2024-01-10     | NULL           | 1         |
| 2          | Bob        | Miller    | 1995-08-23     | 2024-02-15     | 2024-03-01     | 2         |
| 3          | Charlie    | Davis     | 1972-11-30     | 2024-03-10     | NULL           | 3         |

DOCTORS TABLE:

ATTRIBUTES - doctor_id, first_name, last_name, specialization
| doctor_id | first_name | last_name | specialization |
|-----------|------------|-----------|----------------|
| 1         | John       | Smith     | Cardiology     |
| 2         | Emily      | Johnson   | Orthopedics    |
| 3         | Michael    | Brown     | Pediatrics     |


```sql
SELECT 
p.first_name as patient_name
FROM patients p
INNER JOIN doctors d
ON p.doctor_id = d.doctor_id
WHERE d.first_name = 'Emily' AND d.last_name = 'Johnson'
AND p.discharge_date NOT NULL;
```

**Output:**

<img width="461" height="431" alt="image" src="https://github.com/user-attachments/assets/f0a54f22-e79a-47a8-a244-48b81f367afa" />


**Question 4**
---
 From the following tables write a SQL query to find the salesperson(s) and the customer(s) he represents. Return Customer Name, city, Salesman, commission.

Sample table: customer
| customer_id | cust_name       | city       | grade | salesman_id |
|-------------|-----------------|------------|--------|-------------|
| 3002        | Nick Rimando    | New York   | 100    | 5001        |
| 3007        | Brad Davis      | New York   | 200    | 5001        |
| 3005        | Graham Zusi     | California | 200    | 5002        |
| 3008        | Julian Green    | London     | 300    | 5002        |
| 3004        | Fabian Johnson  | Paris      | 300    | 5006        |
| 3009        | Geoff Cameron   | Berlin     | 100    | 5003        |
| 3003        | Jozy Altidor    | Moscow     | 200    | 5007        |
| 3001        | Brad Guzan      | London     |        | 5005        |

Sample table: salesman
| salesman_id | name        | city     | commission |
|-------------|-------------|----------|------------|
| 5001        | James Hoog  | New York | 0.15       |
| 5002        | Nail Knite  | Paris    | 0.13       |
| 5005        | Pit Alex    | London   | 0.11       |
| 5006        | Mc Lyon     | Paris    | 0.14       |
| 5007        | Paul Adam   | Rome     | 0.13       |
| 5003        | Lauson Hen  | San Jose | 0.12       |


```sql
SELECT
c.cust_name AS "Customer Name",
c.city,
s.name as Salesman,
s.commission
FROM customer c
INNER JOIN salesman s
ON c.salesman_id = s.salesman_id;
```

**Output:**

<img width="1209" height="634" alt="image" src="https://github.com/user-attachments/assets/0215e8fc-c772-4440-900d-263105df1d0a" />


**Question 5**
---
From the following tables write a SQL query to find those customers with a grade less than 300. Return cust_name, customer city, grade, Salesman, salesmancity. The result should be ordered by ascending customer_id. 

Sample table: customer
| customer_id | cust_name       | city       | grade | salesman_id |
|-------------|-----------------|------------|--------|-------------|
| 3002        | Nick Rimando    | New York   | 100    | 5001        |
| 3007        | Brad Davis      | New York   | 200    | 5001        |
| 3005        | Graham Zusi     | California | 200    | 5002        |
| 3008        | Julian Green    | London     | 300    | 5002        |
| 3004        | Fabian Johnson  | Paris      | 300    | 5006        |
| 3009        | Geoff Cameron   | Berlin     | 100    | 5003        |
| 3003        | Jozy Altidor    | Moscow     | 200    | 5007        |
| 3001        | Brad Guzan      | London     |        | 5005        |

Sample table: salesman
| salesman_id | name        | city     | commission |
|-------------|-------------|----------|------------|
| 5001        | James Hoog  | New York | 0.15       |
| 5002        | Nail Knite  | Paris    | 0.13       |
| 5005        | Pit Alex    | London   | 0.11       |
| 5006        | Mc Lyon     | Paris    | 0.14       |
| 5007        | Paul Adam   | Rome     | 0.13       |
| 5003        | Lauson Hen  | San Jose | 0.12       |


```sql
SELECT 
c.cust_name,
c.city,
c.grade,
s.name as Salesman,
s.city
FROM customer c
INNER JOIN salesman s
ON c.salesman_id = s.salesman_id
WHERE c.grade < 300
ORDER BY c.customer_id ASC;
```

**Output:**

<img width="1241" height="778" alt="image" src="https://github.com/user-attachments/assets/8a7c8a64-7c04-4933-9563-158c6052174c" />




**Question 6**
---
Write the SQL query that achieves the selection of admission dates from the "patients" table and surgery dates from the "surgeries" table, with an inner join on the "patient_id" column.

PATIENTS TABLE:
| name           | type         |
|----------------|--------------|
| patient_id     | INT          |
| first_name     | VARCHAR(50)  |
| last_name      | VARCHAR(50)  |
| date_of_birth  | DATE         |
| admission_date | DATE         |
| discharge_date | DATE         |
| doctor_id      | INT          |

SURGERIES TABLE:
| name         | type |
|--------------|------|
| surgery_id   | INT  |
| patient_id   | INT  |
| surgeon_id   | INT  |
| surgery_date | DATE |


```sql
SELECT
p.admission_date,
s.surgery_date
FROM patients p
INNER JOIN surgeries s
ON p.patient_id = s.patient_id;
```

**Output:**

<img width="722" height="528" alt="image" src="https://github.com/user-attachments/assets/d4075bec-40ed-477d-b9a1-00d4ef47c9d7" />


**Question 7**
---
Write an SQL query to select all columns from the 'customer' table (aliased as 'c') by performing a LEFT JOIN with the 'orders' table on the 'customer_id' column, including only those orders where the order date falls between '2012-08-01' and '2012-08-30'.

'customer' Table: (customer_id, cust_name, city, grade, salesman_id)

'orders' Table: (ord_no, purch_amt, ord_date, customer_id, salesman_id)

```sql
SELECT
c.*
FROM customer c
LEFT JOIN orders o
ON c.customer_id = o.customer_id
WHERE o.ord_date BETWEEN '2012-08-01' AND '2012-08-30';
```

**Output:**

<img width="1242" height="485" alt="image" src="https://github.com/user-attachments/assets/9bdc1ccf-693c-412b-b4c2-1db8913812f2" />


**Question 8**
---
From the following tables write a SQL query to find those orders where the order amount exists between 500 and 2000. Return ord_no, purch_amt, cust_name, city.

Sample table: customer
| customer_id | cust_name       | city       | grade | salesman_id |
|-------------|-----------------|------------|--------|-------------|
| 3002        | Nick Rimando    | New York   | 100    | 5001        |
| 3007        | Brad Davis      | New York   | 200    | 5001        |
| 3005        | Graham Zusi     | California | 200    | 5002        |
| 3008        | Julian Green    | London     | 300    | 5002        |
| 3004        | Fabian Johnson  | Paris      | 300    | 5006        |
| 3009        | Geoff Cameron   | Berlin     | 100    | 5003        |
| 3003        | Jozy Altidor    | Moscow     | 200    | 5007        |
| 3001        | Brad Guzan      | London     |        | 5005        |

Sample table: orders
| ord_no | purch_amt | ord_date    | customer_id | salesman_id |
|--------|-----------|-------------|--------------|-------------|
| 70001  | 150.50    | 2012-10-05  | 3005         | 5002        |
| 70009  | 270.65    | 2012-09-10  | 3001         | 5005        |
| 70002  | 65.26     | 2012-10-05  | 3002         | 5001        |
| 70004  | 110.50    | 2012-08-17  | 3009         | 5003        |
| 70007  | 948.50    | 2012-09-10  | 3005         | 5002        |
| 70005  | 2400.60   | 2012-07-27  | 3007         | 5001        |
| 70008  | 5760      | 2012-09-10  | 3002         | 5001        |
| 70010  | 1983.43   | 2012-10-10  | 3004         | 5006        |
| 70003  | 2480.40   | 2012-10-10  | 3009         | 5003        |
| 70012  | 250.45    | 2012-06-27  | 3008         | 5002        |
| 70011  | 75.29     | 2012-08-17  | 3003         | 5007        |
| 70013  | 3045.60   | 2012-04-25  | 3002         | 5001        |


```sql
SELECT
o.ord_no,
o.purch_amt,
c.cust_name,
c.city 
FROM orders o
INNER JOIN customer c
ON o.customer_id = c.customer_id
WHERE o.purch_amt BETWEEN 500 AND 2000;
```

**Output:**

<img width="1240" height="476" alt="image" src="https://github.com/user-attachments/assets/46a6dbba-7d45-4f3e-8c4d-ade4f7e0a864" />


**Question 9**
---
SQL statement to generate a report with customer name, city, order number, order date, order amount, salesperson name, and commission to determine if any of the existing customers have not placed orders or if they have placed orders through their salesman or by themselves.

Sample table: customer
| customer_id | cust_name       | city       | grade | salesman_id |
|-------------|-----------------|------------|--------|-------------|
| 3002        | Nick Rimando    | New York   | 100    | 5001        |
| 3007        | Brad Davis      | New York   | 200    | 5001        |
| 3005        | Graham Zusi     | California | 200    | 5002        |
| 3008        | Julian Green    | London     | 300    | 5002        |
| 3004        | Fabian Johnson  | Paris      | 300    | 5006        |
| 3009        | Geoff Cameron   | Berlin     | 100    | 5003        |
| 3003        | Jozy Altidor    | Moscow     | 200    | 5007        |
| 3001        | Brad Guzan      | London     |        | 5005        |

Sample table: orders
| ord_no | purch_amt | ord_date    | customer_id | salesman_id |
|--------|-----------|-------------|--------------|-------------|
| 70001  | 150.50    | 2012-10-05  | 3005         | 5002        |
| 70009  | 270.65    | 2012-09-10  | 3001         | 5005        |
| 70002  | 65.26     | 2012-10-05  | 3002         | 5001        |
| 70004  | 110.50    | 2012-08-17  | 3009         | 5003        |
| 70007  | 948.50    | 2012-09-10  | 3005         | 5002        |
| 70005  | 2400.60   | 2012-07-27  | 3007         | 5001        |
| 70008  | 5760      | 2012-09-10  | 3002         | 5001        |
| 70010  | 1983.43   | 2012-10-10  | 3004         | 5006        |
| 70003  | 2480.40   | 2012-10-10  | 3009         | 5003        |
| 70012  | 250.45    | 2012-06-27  | 3008         | 5002        |
| 70011  | 75.29     | 2012-08-17  | 3003         | 5007        |
| 70013  | 3045.60   | 2012-04-25  | 3002         | 5001        |

Sample table: salesman
| salesman_id | name        | city     | commission |
|-------------|-------------|----------|------------|
| 5001        | James Hoog  | New York | 0.15       |
| 5002        | Nail Knite  | Paris    | 0.13       |
| 5005        | Pit Alex    | London   | 0.11       |
| 5006        | Mc Lyon     | Paris    | 0.14       |
| 5007        | Paul Adam   | Rome     | 0.13       |
| 5003        | Lauson Hen  | San Jose | 0.12       |



```sql
SELECT 
c.cust_name,
c.city,
o.ord_no,
o.ord_date,
o.purch_amt  as 'Order Amount',
s.name,
s.commission
FROM customer c
LEFT JOIN orders o
ON c.customer_id = o.customer_id
LEFT JOIN salesman s
ON c.salesman_id = s.salesman_id;
```

**Output:**

<img width="1243" height="784" alt="image" src="https://github.com/user-attachments/assets/a2943cc3-55e1-4639-8c16-b33a83e000e2" />


**Question 10**
---
Write an SQL query to select the 'cust_name' column from the 'customer' table (aliased as 'c'), using a LEFT JOIN with the 'orders' table based on the 'customer_id' column.

'customer' Table: (customer_id, cust_name, city, grade, salesman_id)

'orders' Table: (ord_no, purch_amt, ord_date, customer_id, salesman_id)

```sql
SELECT
c.cust_name
FROM customer c
LEFT JOIN orders o
ON c.customer_id = o.customer_id;
```

**Output:**

<img width="420" height="424" alt="image" src="https://github.com/user-attachments/assets/3c30097c-8a54-4857-ae03-aff578479ef8" />



## RESULT
Thus, the SQL queries to implement different types of joins have been executed successfully.
