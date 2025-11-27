# Experiment 8: PL/SQL Cursor Programs

## AIM
To write and execute PL/SQL programs using cursors and exception handling to manage runtime errors effectively and display appropriate messages.

## THEORY

In PL/SQL, cursors are used to handle query result sets row-by-row. 

There are two types of cursors:

- Implicit Cursors: Automatically created by PL/SQL for single-row queries.
- Explicit Cursors: Declared and controlled by the programmer for multi-row queries.

Types of Explicit Cursors:

1. Simple Cursor: Basic cursor to iterate over multiple rows.

2. Parameterized Cursor: Accepts parameters to filter the result dynamically.

3. Cursor FOR Loop: Simplifies cursor operations (open, fetch, close).

4. %ROWTYPE Cursor: Fetches entire row into a record using %ROWTYPE.

5. Cursor with FOR UPDATE: Used for row-level locking and updating the rows while looping.

**Syntax:**
```sql
DECLARE 
   <declarations section> 
BEGIN 
   <executable command(s)>
EXCEPTION 
   <exception handling> 
END;
```

### Basic Components of PL/SQL Block:

- DECLARE: Section to declare variables and constants.
- BEGIN: The execution section that contains PL/SQL statements.
- EXCEPTION: Handles errors or exceptions that occur in the program.
- END: Marks the end of the PL/SQL block.

**Exception Handling**

PL/SQL provides a robust mechanism to handle runtime errors using exception handling blocks. When an error occurs during execution, control is passed to the EXCEPTION section, where specific or general errors can be handled gracefully.

### Components of Exception Handling:
- Predefined Exceptions: Automatically raised by PL/SQL for common errors (e.g., NO_DATA_FOUND, TOO_MANY_ROWS, ZERO_DIVIDE).
- User-defined Exceptions: Declared explicitly in the declaration section using the EXCEPTION keyword.
- WHEN OTHERS: A generic handler for all exceptions not handled explicitly.

```sql
BEGIN
   -- Statements
EXCEPTION
   WHEN exception_name THEN
      -- Handling code
   WHEN OTHERS THEN
      -- Handling for unknown errors
END;
```

### **Question 1: Simple Cursor with Exception Handling**

**Write a PL/SQL program using a simple cursor to fetch employee names and designations from the `employees` table. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: When no rows are fetched.
2. **OTHERS**: Any other unexpected errors during execution.

**Steps:**

- Create an `employees` table with fields `emp_id`, `emp_name`, and `designation`.
- Insert some sample data into the table.
- Use a simple cursor to fetch and display employee names and designations.
- Implement exception handling to catch the relevant exceptions and display appropriate messages.

```sql
CREATE TABLE employees (
    emp_id       NUMBER PRIMARY KEY,
    emp_name     VARCHAR2(50),
    designation  VARCHAR2(50)
);
INSERT INTO employees VALUES (1, 'Ramesh', 'Manager');
INSERT INTO employees VALUES (2, 'Sanjay', 'Analyst');
INSERT INTO employees VALUES (3, 'Anita', 'Clerk');

COMMIT;
SET SERVEROUTPUT ON;

DECLARE
    CURSOR emp_cursor IS 
        SELECT emp_name, designation FROM employees;

    v_name employees.emp_name%TYPE;
    v_desg employees.designation%TYPE;

    no_data EXCEPTION;
BEGIN
    OPEN emp_cursor;

    FETCH emp_cursor INTO v_name, v_desg;

    IF emp_cursor%NOTFOUND THEN
        RAISE no_data;
    END IF;

    -- Fetching multiple rows using loop
    WHILE emp_cursor%FOUND LOOP
        DBMS_OUTPUT.PUT_LINE('Name: ' || v_name || ' | Designation: ' || v_desg);
        FETCH emp_cursor INTO v_name, v_desg;
    END LOOP;

    CLOSE emp_cursor;

EXCEPTION
    WHEN no_data THEN
        DBMS_OUTPUT.PUT_LINE('No employee data found.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```

**Output:** 

The program should display the employee details or an error message.

<img width="508" height="312" alt="image" src="https://github.com/user-attachments/assets/71cb2d55-db39-4fd7-9c5f-831eb64838e3" />


---

### **Question 2: Parameterized Cursor with Exception Handling**

**Write a PL/SQL program using a parameterized cursor to retrieve and display employees with a salary in a given range. Implement exception handling for the following errors:**

1. **NO_DATA_FOUND**: When no employees meet the salary criteria.
2. **OTHERS**: For any unexpected errors during the execution.

**Steps:**

- Modify the `employees` table by adding a `salary` column.
- Insert sample salary values for the employees.
- Use a parameterized cursor to accept a salary range as input and fetch employees within that range.
- Implement exception handling to catch and display relevant error messages.

```sql
  ALTER TABLE employees
ADD salary NUMBER;

UPDATE employees SET salary = 45000 WHERE emp_id = 1;
UPDATE employees SET salary = 30000 WHERE emp_id = 2;
UPDATE employees SET salary = 25000 WHERE emp_id = 3;

COMMIT;

SET SERVEROUTPUT ON;

DECLARE
    -- Parameters (you can change range here)
    min_sal NUMBER := 20000;
    max_sal NUMBER := 40000;

    -- Cursor Declaration
    CURSOR emp_cursor(p_min NUMBER, p_max NUMBER) IS
        SELECT emp_name, designation, salary 
        FROM employees 
        WHERE salary BETWEEN p_min AND p_max;

    -- Variables to hold values fetched from cursor
    v_name employees.emp_name%TYPE;
    v_desg employees.designation%TYPE;
    v_salary employees.salary%TYPE;

    no_data EXCEPTION;  -- custom exception
BEGIN
    OPEN emp_cursor(min_sal, max_sal);

    FETCH emp_cursor INTO v_name, v_desg, v_salary;

    -- If no rows match criteria
    IF emp_cursor%NOTFOUND THEN
        RAISE no_data;
    END IF;

    DBMS_OUTPUT.PUT_LINE('Employees with salary between ' ||
                         min_sal || ' and ' || max_sal || ':');

    -- Fetch all records using loop
    WHILE emp_cursor%FOUND LOOP
        DBMS_OUTPUT.PUT_LINE('Name: ' || v_name ||
                             ' | Designation: ' || v_desg ||
                             ' | Salary: ' || v_salary);
        FETCH emp_cursor INTO v_name, v_desg, v_salary;
    END LOOP;

    CLOSE emp_cursor;

EXCEPTION
    WHEN no_data THEN
        DBMS_OUTPUT.PUT_LINE(
          'No employees found in the given salary range.'
        );
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```

**Output:**  
<br>
The program should display the employee details within the specified salary range or an error message if no data is found.

<img width="555" height="320" alt="image" src="https://github.com/user-attachments/assets/b7b6a993-c291-42af-8217-11eb9f6bfdc5" />


---

### **Question 3: Cursor FOR Loop with Exception Handling**

**Write a PL/SQL program using a cursor FOR loop to retrieve and display all employee names and their department numbers from the `employees` table. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: If no employees are found in the database.
2. **OTHERS**: For any other unexpected errors.

**Steps:**

- Modify the `employees` table by adding a `dept_no` column.
- Insert sample department numbers for employees.
- Use a cursor FOR loop to fetch and display employee names along with their department numbers.
- Implement exception handling to catch the relevant exceptions.

```sql
ALTER TABLE employees
ADD dept_no NUMBER;

UPDATE employees SET dept_no = 10 WHERE emp_id = 1;
UPDATE employees SET dept_no = 20 WHERE emp_id = 2;
UPDATE employees SET dept_no = 30 WHERE emp_id = 3;

COMMIT;

SET SERVEROUTPUT ON;

DECLARE
    no_data EXCEPTION;
    emp_count NUMBER;
BEGIN
    -- Check if table has any rows
    SELECT COUNT(*) INTO emp_count FROM employees;

    IF emp_count = 0 THEN
        RAISE no_data;
    END IF;

    DBMS_OUTPUT.PUT_LINE('Employee Name  |  Department No');
    DBMS_OUTPUT.PUT_LINE('--------------------------------');

    -- Cursor FOR loop (no need to OPEN or FETCH manually)
    FOR emp_rec IN (
        SELECT emp_name, dept_no
        FROM employees
    ) LOOP
        DBMS_OUTPUT.PUT_LINE(emp_rec.emp_name || '  |  ' || emp_rec.dept_no);
    END LOOP;

EXCEPTION
    WHEN no_data THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the database.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```

**Output:**  

The program should display employee names with their department numbers or the appropriate error message if no data is found.

<img width="531" height="346" alt="image" src="https://github.com/user-attachments/assets/b209fb53-7900-4a9e-ae82-82708e998b37" />

---

### **Question 4: Cursor with `%ROWTYPE` and Exception Handling**

**Write a PL/SQL program that uses a cursor with `%ROWTYPE` to fetch and display complete employee records (emp_id, emp_name, designation, salary). Implement exception handling for the following errors:**

1. **NO_DATA_FOUND**: When no employees are found in the database.
2. **OTHERS**: For any other errors that occur.

**Steps:**

- Modify the `employees` table by adding `emp_id`, `emp_name`, `designation`, and `salary` fields.
- Insert sample data into the `employees` table.
- Declare a cursor using `%ROWTYPE` to fetch complete rows from the `employees` table.
- Implement exception handling to catch the relevant exceptions and display appropriate messages.
```sql

ALTER TABLE employees ADD designation VARCHAR2(50);
ALTER TABLE employees ADD salary NUMBER;

INSERT INTO employees VALUES (1, 'Ramesh', 'Manager', 45000);
INSERT INTO employees VALUES (2, 'Sanjay', 'Analyst', 30000);
INSERT INTO employees VALUES (3, 'Anita',  'Clerk',   25000);

COMMIT;

SET SERVEROUTPUT ON;

DECLARE
    -- Declare cursor
    CURSOR emp_cursor IS
        SELECT emp_id, emp_name, designation, salary
        FROM employees;

    -- Using %ROWTYPE to store an entire row
    emp_record emp_cursor%ROWTYPE;

    no_data EXCEPTION;
    emp_count NUMBER;
BEGIN
    -- Check if table has any rows
    SELECT COUNT(*) INTO emp_count FROM employees;

    IF emp_count = 0 THEN
        RAISE no_data;
    END IF;

    DBMS_OUTPUT.PUT_LINE('Employee Records:');
    DBMS_OUTPUT.PUT_LINE('-------------------------------');

    OPEN emp_cursor;

    LOOP
        FETCH emp_cursor INTO emp_record;
        EXIT WHEN emp_cursor%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(
            'ID: ' || emp_record.emp_id ||
            ', Name: ' || emp_record.emp_name ||
            ', Designation: ' || emp_record.designation ||
            ', Salary: ' || emp_record.salary
        );
    END LOOP;

    CLOSE emp_cursor;

EXCEPTION
    WHEN no_data THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the database.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/

```
**Output:**  

The program should display employee records or the appropriate error message if no data is found.

<img width="545" height="333" alt="image" src="https://github.com/user-attachments/assets/4775cf34-0b7a-4d6f-aa94-ffaf921a1da6" />

---

### **Question 5: Cursor with FOR UPDATE Clause and Exception Handling**

**Write a PL/SQL program using a cursor with the `FOR UPDATE` clause to update the salary of employees in a specific department. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: If no rows are affected by the update.
2. **OTHERS**: For any unexpected errors during execution.

**Steps:**

- Modify the `employees` table to include a `dept_no` and `salary` field.
- Insert sample data into the `employees` table with different department numbers.
- Use a cursor with the `FOR UPDATE` clause to lock the rows of employees in a specific department and update their salary.
- Implement exception handling to handle `NO_DATA_FOUND` or other errors that may occur.
```sql
ALTER TABLE employees ADD dept_no NUMBER;
ALTER TABLE employees ADD salary NUMBER;

UPDATE employees SET dept_no = 10, salary = 45000 WHERE emp_id = 1;
UPDATE employees SET dept_no = 20, salary = 30000 WHERE emp_id = 2;
UPDATE employees SET dept_no = 10, salary = 50000 WHERE emp_id = 3;

COMMIT;

SET SERVEROUTPUT ON;

DECLARE
    v_dept NUMBER := 10;       -- department to update
    rows_updated NUMBER := 0;  -- to track number of updated rows

    CURSOR emp_cursor IS
        SELECT emp_id, salary
        FROM employees
        WHERE dept_no = v_dept
        FOR UPDATE;   -- lock the rows
BEGIN
    FOR emp_rec IN emp_cursor LOOP
        rows_updated := rows_updated + 1;

        -- Increase salary by 10% for example
        UPDATE employees
        SET salary = emp_rec.salary * 1.10
        WHERE CURRENT OF emp_cursor;
    END LOOP;

    IF rows_updated = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;

    COMMIT;
    DBMS_OUTPUT.PUT_LINE(rows_updated || ' employee salary record(s) updated for department ' || v_dept);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the specified department.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/

```
**Output:**  

The program should update employee salaries and display a message, or it should display an error message if no data is found.

<img width="595" height="282" alt="image" src="https://github.com/user-attachments/assets/63042367-41ae-4576-95bb-41f6257f838c" />





## RESULT
Thus, the program successfully executed and the output was verified, demonstrating the correct use of explicit cursors.

