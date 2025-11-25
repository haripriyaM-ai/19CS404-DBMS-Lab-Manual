# Experiment 5: Subqueries and Views

## AIM
To study and implement subqueries and views.

## THEORY

### Subqueries
A subquery is a query inside another SQL query and is embedded in:
- WHERE clause
- HAVING clause
- FROM clause

**Types:**
- **Single-row subquery**:
  Sub queries can also return more than one value. Such results should be made use along with the operators in and any.
- **Multiple-row subquery**:
  Here more than one subquery is used. These multiple sub queries are combined by means of ‘and’ & ‘or’ keywords.
- **Correlated subquery**:
  A subquery is evaluated once for the entire parent statement whereas a correlated Sub query is evaluated once per row processed by the parent statement.

**Example:**
```sql
SELECT * FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```
### Views
A view is a virtual table based on the result of an SQL SELECT query.
**Create View:**
```sql
CREATE VIEW view_name AS
SELECT column1, column2 FROM table_name WHERE condition;
```
**Drop View:**
```sql
DROP VIEW view_name;
```

**Question 1**
--
Write a SQL query to Retrieve the medications with dosages equal to the highest dosage

Table Name: Medications (attributes: medication_id, medication_name, dosage)
| name            | type         |
|-----------------|--------------|
| medication_id   | INT          |
| medication_name | VARCHAR(50)  |
| dosage          | VARCHAR(20)  |


```sql
SELECT
medication_id as medic,
medication_name,
dosage 
FROM Medications
WHERE dosage = (
SELECT 
MAX(dosage)
FROM Medications
);
```

**Output:**

<img width="840" height="438" alt="image" src="https://github.com/user-attachments/assets/9c210eec-76d4-4079-8fdd-d26ecc69ba2b" />


**Question 2**
---
Write a SQL query to retrieve all columns from the CUSTOMERS table for customers whose salary is greater than $1500.

Sample table: CUSTOMERS
| ID | NAME     | AGE | ADDRESS    | SALARY |
|----|----------|-----|------------|--------|
| 1  | Ramesh   | 32  | Ahmedabad  | 2000   |
| 2  | Khilan   | 25  | Delhi      | 1500   |
| 3  | Kaushik  | 23  | Kota       | 2000   |
| 4  | Chaitali | 25  | Mumbai     | 6500   |
| 5  | Hardik   | 27  | Bhopal     | 8500   |
| 6  | Komal    | 22  | Hyderabad  | 4500   |
| 7  | Muffy    | 24  | Indore     | 10000  |


```sql
SELECT *
FROM CUSTOMERS
WHERE salary > 1500;
```

**Output:**
<img width="1213" height="663" alt="image" src="https://github.com/user-attachments/assets/c0549fbe-ad20-412e-9d53-7f65eae79124" />



**Question 3**
---
Write a SQL query that retrieves the all the columns from the Table Grades, where the grade is equal to the minimum grade achieved in each subject.

Sample table: GRADES (attributes: student_id, student_name, subject, grade)
| student_id | student_name | subject | grade |
|------------|--------------|---------|--------|
| 1          | Alice        | Math    | 90     |
| 2          | Bob          | Math    | 85     |
| 3          | Charlie      | Math    | 95     |
| 4          | David        | Science | 88     |
| 5          | Emma         | Science | 92     |
| 6          | Frank        | Science | 85     |
| 7          | John         | Social  | 85     |

```sql
SELECT *
FROM GRADES
WHERE grade = (
SELECT 
MIN(g2.grade)
FROM GRADES as g2
WHERE g2.subject = GRADES.subject
);
```

**Output:**

<img width="1213" height="473" alt="image" src="https://github.com/user-attachments/assets/d005b39d-3067-4867-9794-dce14cb5b7a1" />


**Question 4**
---
Write a SQL query to retrieve all columns from the CUSTOMERS table for customers whose AGE is LESS than $30

Sample table: CUSTOMERS
| ID | NAME     | AGE | ADDRESS    | SALARY |
|----|----------|-----|------------|--------|
| 1  | Ramesh   | 32  | Ahmedabad  | 2000   |
| 2  | Khilan   | 25  | Delhi      | 1500   |
| 3  | Kaushik  | 23  | Kota       | 2000   |
| 4  | Chaitali | 25  | Mumbai     | 6500   |
| 5  | Hardik   | 27  | Bhopal     | 8500   |
| 6  | Komal    | 22  | Hyderabad  | 4500   |
| 7  | Muffy    | 24  | Indore     | 10000  |


```sql
SELECT *
FROM CUSTOMERS
WHERE AGE < 30;
```

**Output:**

<img width="1206" height="649" alt="image" src="https://github.com/user-attachments/assets/84531139-5561-4551-9a11-a023236f1e7d" />


**Question 5**
---
Write a SQL query to retrieve all columns from the CUSTOMERS table for customers whose Address as Delhi and age below 30

Sample table: CUSTOMERS
| ID | NAME     | AGE | ADDRESS    | SALARY |
|----|----------|-----|------------|--------|
| 1  | Ramesh   | 32  | Ahmedabad  | 2000   |
| 2  | Khilan   | 25  | Delhi      | 1500   |
| 3  | Kaushik  | 23  | Kota       | 2000   |
| 4  | Chaitali | 25  | Mumbai     | 6500   |
| 5  | Hardik   | 27  | Bhopal     | 8500   |
| 6  | Komal    | 22  | Hyderabad  | 4500   |
| 7  | Muffy    | 24  | Indore     | 10000  |


```sql
SELECT *
FROM CUSTOMERS
WHERE ADDRESS IN ('Delhi') AND AGE < 30
ORDER BY ID;
```

**Output:**

<img width="1202" height="397" alt="image" src="https://github.com/user-attachments/assets/02a033d5-cae2-4575-872d-9a5c8e67a47c" />


**Question 6**
---
Write a SQL query to Identify customers whose city is different from the city of the customer with the highest ID

SAMPLE TABLE: customer
| name | type    |
|------|---------|
| id   | INTEGER |
| name | TEXT    |
| city | TEXT    |
| email| TEXT    |
| phone| INTEGER |


```sql
SELECT *
FROM customer
WHERE city <> (
SELECT city 
FROM customer
WHERE id = (SELECT MAX(id) FROM customer)
);
```

**Output:**

<img width="1212" height="527" alt="image" src="https://github.com/user-attachments/assets/da0db877-ee39-4a1b-8e22-03453e03b0d6" />


**Question 7**
---
From the following tables write a SQL query to find all orders generated by New York-based salespeople. Return ord_no, purch_amt, ord_date, customer_id, salesman_id.

salesman table
| name        | type         |
|-------------|--------------|
| salesman_id | numeric(5)   |
| name        | varchar(30)  |
| city        | varchar(15)  |
| commission  | decimal(5,2) |


orders table
| name        | type |
|-------------|------|
| order_no    | int  |
| purch_amt   | real |
| order_date  | text |
| customer_id | int  |
| salesman_id | int  |


```sql
SELECT *
FROM orders
WHERE salesman_id IN (
SELECT salesman_id 
FROM salesman 
WHERE city IN ('New York')
);
```

**Output:**

<img width="1207" height="509" alt="image" src="https://github.com/user-attachments/assets/ced9eb64-ba97-4904-8610-8807d78381c2" />


**Question 8**
---
Write a SQL query to Retrieve the medications with dosages equal to the lowest dosage

Table Name: Medications (attributes: medication_id, medication_name, dosage)
| name            | type         |
|-----------------|--------------|
| medication_id   | INT          |
| medication_name | VARCHAR(50)  |
| dosage          | VARCHAR(20)  |

```sql
SELECT 
medication_id,
medication_name,
dosage
FROM Medications
WHERE dosage = (
SELECT 
MIN(dosage)
FROM Medications
);
```

**Output:**

<img width="928" height="425" alt="image" src="https://github.com/user-attachments/assets/8806dfb2-e5ef-4537-b4fa-c5f21fe43dda" />




**Question 9**
---
Write a SQL query that retrieves the names of students and their corresponding grades, where the grade is equal to the maximum grade achieved in each subject.

Sample table: GRADES (attributes: student_id, student_name, subject, grade)
| name            | type         |
|-----------------|--------------|
| medication_id   | INT          |
| medication_name | VARCHAR(50)  |
| dosage          | VARCHAR(20)  |


```sql
SELECT 
student_name,
grade 
FROM GRADES
WHERE grade = (
SELECT MAX(g2.grade)
FROM GRADES AS g2
WHERE g2.subject = GRADES.subject
);
```

**Output:**

<img width="793" height="456" alt="image" src="https://github.com/user-attachments/assets/42c32727-24b9-4440-8298-c1d218b8f45a" />


**Question 10**
---
From the following tables, write a SQL query to find all the orders generated in New York city. Return ord_no, purch_amt, ord_date, customer_id and salesman_id.

SALESMAN TABLE
| name         | type        |
|--------------|-------------|
| salesman_id  | numeric(5)  |
| name         | varchar(30) |
| city         | varchar(15) |
| commission   | decimal(5,2)|

ORDERS TABLE
| name        | type |
|-------------|------|
| ord_no      | int  |
| purch_amt   | real |
| ord_date    | text |
| customer_id | int  |
| salesman_id | int  |

```sql
SELECT *
FROM ORDERS
WHERE salesman_id IN (
SELECT salesman_id
FROM SALESMAN
WHERE city IN ('New York')
);
```

**Output:**

<img width="1215" height="533" alt="image" src="https://github.com/user-attachments/assets/6bf9b874-b51b-4d7a-9146-e940b00ae9f3" />



## RESULT
Thus, the SQL queries to implement subqueries and views have been executed successfully.
