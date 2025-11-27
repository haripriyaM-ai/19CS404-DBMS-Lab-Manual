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
Create a table named Bonuses with the following constraints:
 - BonusID as INTEGER should be the primary key.
 - EmployeeID as INTEGER should be a foreign key referencing Employees(EmployeeID).
 - BonusAmount as REAL should be greater than 0.
 - BonusDate as DATE.
 - Reason as TEXT should not be NULL.

```sql
CREATE TABLE Bonuses
(
BonusID  INT PRIMARY KEY,
EmployeeID INT,
BonusAmount REAL CHECK (BonusAmount > 0),
BonusDate DATE,
Reason TEXT NOT NULL,
FOREIGN KEY(EmployeeID) REFERENCES Employees(EmployeeID)
);
```

**Output:**

<img width="1218" height="246" alt="image" src="https://github.com/user-attachments/assets/262a3f38-eacf-48d5-ade5-4a2c3a442301" />


**Question 2**
---
Insert the below data into the Employee table, allowing the Department and Salary columns to take their default values.

| EmployeeID | Name        | Position |
|------------|-------------|----------|
| 4          | Emily White | Analyst  |


```sql
INSERT INTO Employee (EmployeeID, Name, Position)
VALUES(4,'Emily White','Analyst');
```

**Output:**
<img width="981" height="262" alt="image" src="https://github.com/user-attachments/assets/e6b1145a-0a84-4e1a-ad64-af0219368ae7" />


**Question 3**
Create a new table named item with the following specifications and constraints:
  - item_id as TEXT and as primary key.
  - item_desc as TEXT.
  - rate as INTEGER.
  - icom_id as TEXT with a length of 4.
  - icom_id is a foreign key referencing com_id in the company table.
  - The foreign key should cascade updates and deletes.
  - item_desc and rate should not accept NULL.-- Paste Question 3 here

```sql
CREATE TABLE item
(
item_id TEXT PRIMARY KEY,
item_desc TEXT NOT NULL,
rate INTEGER NOT NULL,
icom_id TEXT,
FOREIGN KEY (icom_id) REFERENCES company(com_id)
    ON UPDATE CASCADE
    ON DELETE CASCADE
);
```

**Output:**

<img width="1218" height="313" alt="image" src="https://github.com/user-attachments/assets/3bdaf99a-d3d7-45cf-a426-e37790541182" />


**Question 4**
---
Create a table named Attendance with the following constraints:
- AttendanceID as INTEGER should be the primary key.
- EmployeeID as INTEGER should be a foreign key referencing Employees(EmployeeID).
- AttendanceDate as DATE.
- Status as TEXT should be one of 'Present', 'Absent', 'Leave'.

```sql
CREATE TABLE Attendance
(
AttendanceID INTEGER PRIMARY KEY,
EmployeeID INTEGER,
AttendanceDate DATE,
Status TEXT CHECK (Status IN ('Present','Absent','Leave')),
FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)

);
```

**Output:**

<img width="1221" height="248" alt="image" src="https://github.com/user-attachments/assets/27304b61-63d1-49d3-af2a-751ada7ba90e" />


**Question 5**
---
Insert all customers from Old_customers into Customers

Table attributes are CustomerID, Name, Address, Email

```sql
INSERT INTO Customers (CustomerID,Name,Address,Email)
SELECT CustomerID,Name,Address,Email
FROM Old_Customers
```

**Output:**

<img width="1219" height="244" alt="image" src="https://github.com/user-attachments/assets/4d011fa6-1252-4ec9-8da8-af1c76e853cc" />


**Question 6**
---
Create a table named Reviews with the following columns:

- ReviewID as INTEGER
- ProductID as INTEGER
- Rating as REAL
- ReviewText as TEXT

```sql
CREATE TABLE Reviews
(
ReviewID INTEGER,
ProductID INTEGER,
Rating REAL,
ReviewText TEXT
);
```

**Output:**

<img width="1217" height="350" alt="image" src="https://github.com/user-attachments/assets/7ed46b7d-b4bf-4a95-b422-24f77cff0354" />


**Question 7**
---
Write an SQL query to change the name of the column id to employee_id in the table employee.

```sql
ALTER TABLE employee RENAME COLUMN id TO employee_id;
```

**Output:**

<img width="1219" height="229" alt="image" src="https://github.com/user-attachments/assets/1833855c-688e-461d-8949-6beb37a487a3" />


**Question 8**
---
Create a table named Invoices with the following constraints:
- InvoiceID as INTEGER should be the primary key.
- InvoiceDate as DATE.
- Amount as REAL should be greater than 0.
- DueDate as DATE should be greater than the InvoiceDate.
- OrderID as INTEGER should be a foreign key referencing Orders(OrderID).
```sql
CREATE TABLE Invoices
(
InvoiceID INTEGER PRIMARY KEY,
InvoiceDate DATE,
Amount REAL CHECK(Amount > 0),
DueDate DATE CHECK(DueDate > InvoiceDate),
OrderID INTEGER,
FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
)
```

**Output:**

<img width="1220" height="241" alt="image" src="https://github.com/user-attachments/assets/27227a38-fe24-419e-b0c0-cba7071a9e8d" />

**Question 9**
---
Write a SQL Query to add an attribute designation in the employee table with the data type VARCHAR(50).

```sql
ALTER TABLE employee
ADD COLUMN designation varchar(50);
```

**Output:**
<img width="1221" height="247" alt="image" src="https://github.com/user-attachments/assets/9dee5b4c-7d1a-4bdb-9abb-fc7cac9eee4d" />


**Question 10**
---
Insert the following employees into the Employee table:

| EmployeeID | Name       | Position  | Department | Salary |
|------------|------------|-----------|------------|--------|
| 2          | John Smith | Developer | IT         | 75000  |
| 3          | Anna Bell  | Designer  | Marketing  | 68000  |


```sql
INSERT INTO Employee(EmployeeID,Name,Position,Department,Salary)
VALUES
(2,'John Smith','Developer','IT',75000),
(3,'Anna Bell','Designer','Marketing',68000);
```

**Output:**

<img width="1219" height="311" alt="image" src="https://github.com/user-attachments/assets/89eabdc0-bc43-4e6d-b1b9-bc9071f9e30c" />

**SEB Grades**
<img width="1500" height="800" alt="image" src="https://github.com/user-attachments/assets/d7508d47-a253-4759-a05d-50dce322d488" />


## RESULT
Thus, the SQL queries to implement different types of constraints and DDL commands have been executed successfully.
