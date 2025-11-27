# Experiment 10: PL/SQL – Triggers

## AIM
To write and execute PL/SQL trigger programs for automating actions in response to specific table events like INSERT, UPDATE, or DELETE.

---

## THEORY

A **trigger** is a stored PL/SQL block that is automatically executed or fired when a specified event occurs on a table or view. Triggers can be used for enforcing business rules, auditing changes, or automatic updates.

### Types of Triggers:
- **Before Trigger**: Executes before the operation (INSERT, UPDATE, DELETE).
- **After Trigger**: Executes after the operation.
- **Row-level Trigger**: Executes for each affected row.
- **Statement-level Trigger**: Executes once for the triggering statement.

**Basic Syntax:**
```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE ON table_name
[FOR EACH ROW]
BEGIN
   -- trigger logic
END;
```

## 1. Write a trigger to log every insertion into a table.
**Steps:**
- Create two tables: `employees` (for storing data) and `employee_log` (for logging the inserts).
- Write an **AFTER INSERT** trigger on the `employees` table to log the new data into the `employee_log` table.

```sql
CREATE TABLE employees (
    emp_id   NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    salary   NUMBER
);
CREATE TABLE employee_log (
    log_id      NUMBER GENERATED ALWAYS AS IDENTITY,
    emp_id      NUMBER,
    emp_name    VARCHAR2(50),
    salary      NUMBER,
    log_time    TIMESTAMP
);
CREATE OR REPLACE TRIGGER trg_log_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log(emp_id, emp_name, salary, log_time)
    VALUES (:NEW.emp_id, :NEW.emp_name, :NEW.salary, SYSTIMESTAMP);
END;
/
INSERT INTO employees (emp_id, emp_name, salary)
VALUES (101, 'TestUser1', 5000);

INSERT INTO employees (emp_id, emp_name, salary)
VALUES (102, 'TestUser2', 6000);

COMMIT;

SELECT * FROM employee_log;
```

**Expected Output:**
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.

**Output:**

<img width="988" height="243" alt="image" src="https://github.com/user-attachments/assets/f0008b42-6938-48bd-8e26-073585367cdf" />

---

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.

```sql
CREATE TABLE sensitive_data (
    id NUMBER PRIMARY KEY,
    info VARCHAR2(100)
);
CREATE OR REPLACE TRIGGER trg_block_delete
BEFORE DELETE ON sensitive_data
BEGIN
    RAISE_APPLICATION_ERROR(-20001, 'ERROR: Deletion not allowed on this table.');
END;
/
DELETE FROM sensitive_data WHERE id = 1;

```
**Expected Output:**
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`

**Output:**

<img width="744" height="171" alt="image" src="https://github.com/user-attachments/assets/140954aa-d4ed-4082-a16c-98b573272357" />

---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.
```sql

CREATE TABLE products (
    product_id     NUMBER PRIMARY KEY,
    product_name   VARCHAR2(50),
    price          NUMBER,
    last_modified  TIMESTAMP
);

INSERT INTO products (product_id, product_name, price, last_modified)
VALUES (10, 'Laptop', 50000, NULL);

COMMIT;

CREATE OR REPLACE TRIGGER trg_update_last_modified
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    :NEW.last_modified := SYSTIMESTAMP;
END;
/

UPDATE products
SET price = 999
WHERE product_id = 10;

COMMIT;

SELECT * FROM products WHERE product_id = 10;
```
**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.

**Output:**

<img width="910" height="236" alt="image" src="https://github.com/user-attachments/assets/2fb28b89-69de-4c4d-8cec-9863454dc0d9" />

---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.
  
```sql
CREATE TABLE audit_log (
    update_count NUMBER
);

INSERT INTO audit_log VALUES (0);
CREATE OR REPLACE TRIGGER trg_count_updates
AFTER UPDATE ON customer_orders
BEGIN
    UPDATE audit_log
    SET update_count = update_count + 1;
END;
/

UPDATE customer_orders SET amount = 200 WHERE order_id = 1;
```

**Expected Output:**
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.

**Output:**

<img width="565" height="179" alt="image" src="https://github.com/user-attachments/assets/c75ace33-7e0b-45de-9919-b524ee25655c" />

---

## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.

```sql
CREATE OR REPLACE TRIGGER trg_check_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF :NEW.salary < 3000 THEN
        RAISE_APPLICATION_ERROR(-20002, 'ERROR: Salary below minimum threshold.');
    END IF;
END;
/

INSERT INTO employees (emp_id, emp_name, salary)
VALUES (2, 'Anita', 2000);
```

**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

**Output:**

<img width="740" height="197" alt="image" src="https://github.com/user-attachments/assets/d0c2d3e1-8a3b-4e1f-8a48-47257deabca5" />

---

**SEB Grades**
<img width="1500" height="800" alt="image" src="https://github.com/user-attachments/assets/d7508d47-a253-4759-a05d-50dce322d488" />


## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.



