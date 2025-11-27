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
Write a SQL statement to Update the grade of all customers in Chennai city as  5. 

Customer table (customer_id,cust_name,city,grade,salesman_id)

```sql
UPDATE Customer SET grade = "5" WHERE city = "Chennai";
```

**Output:**

<img width="1219" height="417" alt="image" src="https://github.com/user-attachments/assets/89945820-431a-4b25-aea7-317c17af1de7" />


**Question 2**
---
Write a SQL query to Delete a Specific Surgery whose ID is 3

Sample table: Surgeries

attributes: surgery_id, patient_id, surgeon_id, surgery_date

```sql
DELETE FROM Surgeries WHERE surgery_id = "3";
```

**Output:**

<img width="1121" height="323" alt="image" src="https://github.com/user-attachments/assets/823f716b-2adc-430d-951c-7abeddb9c5ee" />


**Question 3**
---
Write a SQL query to Select all patients who were admitted for one day.

Table: Patients
| Name            | Type          |
|-----------------|---------------|
| patient_id      | INT           |
| first_name      | VARCHAR(50)   |
| last_name       | VARCHAR(50)   |
| date_of_birth   | DATE          |
| admission_date  | DATE          |
| discharge_date  | DATE          |
| doctor_id       | INT           |


```sql
SELECT patient_id,first_name,admission_date,discharge_date
FROM Patients
WHERE admission_date = discharge_date;
```

**Output:**

<img width="1008" height="287" alt="image" src="https://github.com/user-attachments/assets/ce90dfd8-1285-43c9-9c50-46af7925bed2" />


**Question 4**
---
 Write a query to fetch details of all employees excluding the employees with first names, “Sanjay” and “Sonia” from the EmployeeInfo table.
| EmpID | EmpFname | EmpLname | Department | Project | Address         | DOB        | Gender |
|-------|----------|-----------|-------------|----------|------------------|------------|---------|
| 1     | Sanjay   | Mehra     | HR          | P1       | Hyderabad (HYD)  | 01/12/1976 | M       |
| 2     | Ananya   | Mishra    | Admin       | P2       | Delhi (DEL)      | 02/05/1968 | F       |

```sql
SELECT * FROM EmployeeInfo 
WHERE EmpFname NOT IN ("Sanjay","Sonia");
```

**Output:**

<img width="1222" height="251" alt="image" src="https://github.com/user-attachments/assets/ca0fd093-97fb-4af4-ae1c-a32f6fced2d5" />


**Question 5**
---
Write a SQL query to classify value2 in the Calculations table as 'Small', 'Medium', or 'Large' based on whether it is less than 10, between 10 and 50, or greater than 50, respectively.
| cid | name     | type    | notnull | dflt_value | pk |
|-----|----------|---------|---------|------------|----|
| 0   | id       | INTEGER | 0       |            | 1  |
| 1   | value1   | REAL    | 0       |            | 0  |
| 2   | value2   | REAL    | 0       |            | 0  |
| 3   | base     | INTEGER | 0       |            | 0  |
| 4   | exponent | INTEGER | 0       |            | 0  |
| 5   | number   | REAL    | 0       |            | 0  |
| 6   | decimal  | REAL    | 0       |            | 0  |


```sql
SELECT id,value2,
CASE 
   WHEN value2 < 10 THEN 'Small'
   WHEN value2 BETWEEN 10 AND 50 THEN 'Medium'
   ELSE 'Large'
END AS size_category
FROM Calculations;
```

**Output:**

<img width="730" height="417" alt="image" src="https://github.com/user-attachments/assets/7804ab1d-300a-4141-bf4b-0d7c269217f3" />


**Question 6**
---
Write a SQL query to identify the top 3 most expensive discounted products. Return product_id, original_price, discount_percentage, and discounted_price.

Sample table: Products
| product_id | original_price | discount_percentage |
|------------|----------------|---------------------|
| 101        | 50.00          | 0.10                |
| 102        | 150.00         | 0.15                |
| 103        | 200.00         | 0.20                |
| 104        | 300.00         | 0.25                |


```sql
SELECT 
product_id,
original_price,
discount_percentage,
(original_price-(original_price*discount_percentage)) AS discounted_price
FROM Products
ORDER BY discounted_price DESC
LIMIT 3;
```

**Output:**

<img width="1183" height="230" alt="image" src="https://github.com/user-attachments/assets/bff25a7f-d660-46bd-9d81-fc1e1422ae20" />


**Question 7**
---
Write a SQL query to display hire dates in the format "DD-MM-YYYY" from the emp table
| cid | name     | type             |
|-----|----------|------------------|
| 0   | empno    | INT              |
| 1   | ename    | VARCHAR(100)     |
| 2   | job      | VARCHAR(50)      |
| 3   | mgr      | INT              |
| 4   | hiredate | DATE             |
| 5   | sal      | DECIMAL(10,2)    |
| 6   | comm     | DECIMAL(10,2)    |
| 7   | deptno   | INT              |


```sql
SELECT ename,
strftime("%d-%m-%Y",hiredate) AS HireDateFormatted
FROM emp;
```

**Output:**

<img width="603" height="316" alt="image" src="https://github.com/user-attachments/assets/9eef7cbe-a070-4d9c-97e2-e64da213902a" />


**Question 8**
---
Write a SQL query to calculate the number of days between the hiredate and a specified date ('2024-12-31') for each employee using the JULIANDAY function from the emp table.
| cid | name     | type             |
|-----|----------|------------------|
| 0   | empno    | INT              |
| 1   | ename    | VARCHAR(100)     |
| 2   | job      | VARCHAR(50)      |
| 3   | mgr      | INT              |
| 4   | hiredate | DATE             |
| 5   | sal      | DECIMAL(10,2)    |
| 6   | comm     | DECIMAL(10,2)    |
| 7   | deptno   | INT              |


```sql
SELECT ename,
hiredate,
JULIANDAY('2024-12-31') - JULIANDAY(hiredate) AS days_worked
FROM emp;
```

**Output:**

<img width="692" height="230" alt="image" src="https://github.com/user-attachments/assets/7fdfdfcc-e69c-45c5-b315-d9e57b0e8963" />


**Question 9**
---
 Write a query to retrieve the first four characters of  EmpLname from the EmployeeInfo table.

EmployeeInfo Table
| EmpID | EmpFname | EmpLname | Department | Project | Address         | DOB        | Gender |
|-------|----------|-----------|-------------|----------|------------------|------------|---------|
| 1     | Sanjay   | Mehra     | HR          | P1       | Hyderabad(HYD)   | 01/12/1976 | M       |
| 2     | Ananya   | Mishra    | Admin       | P2       | Delhi(DEL)       | 02/05/1968 | F       |


```sql
SELECT SUBSTR(EmpLname, 1, 4) 
FROM EmployeeInfo;
```

**Output:**

<img width="596" height="335" alt="image" src="https://github.com/user-attachments/assets/93962c0a-a4e4-480f-8914-62f906a77550" />


**Question 10**
---
Write a SQL statement to Increase the selling price by 15% in the products table where quantity in stock is less than 50 and supplier ID is 10.
| name         | type              |
|--------------|-------------------|
| product_id   | INT PRIMARY KEY   |
| product_name | VARCHAR(10)       |
| category     | VARCHAR(50)       |
| cost_price   | DECIMAL(10)       |
| sell_price   | DECIMAL(10)       |
| reorder_lv   | INT               |
| quantity     | INT               |
| supplier_id  | INT               |


```sql
UPDATE Products SET sell_price = sell_price*1.15 WHERE quantity < 50 AND supplier_id = "10";
```

**Output:**

<img width="1163" height="533" alt="image" src="https://github.com/user-attachments/assets/3601ec05-bd76-48b4-9b37-48e44a12ca8c" />

**SEB Grades**
<img width="1500" height="800" alt="image" src="https://github.com/user-attachments/assets/d7508d47-a253-4759-a05d-50dce322d488" />


## RESULT
Thus, the SQL queries to implement DML commands have been executed successfully.
