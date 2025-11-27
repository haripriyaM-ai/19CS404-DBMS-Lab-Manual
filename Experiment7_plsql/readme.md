# Experiment 7: PL/SQL – Variables, Control Structures and Loops

## AIM
To write and execute simple PL/SQL programs using variables, loops, and conditional statements.


## THEORY

PL/SQL, which stands for Procedural Language extensions to the Structured Query Language (SQL). It is a combination of SQL along with the procedural features of programming languages.

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

# PL/SQL Programs – Steps and Expected Output

## 1. Write a PL/SQL program to find the Greatest of Two Numbers

### Steps:
- Declare two numeric variables and initialize them.
- Use an `IF` statement to compare the values.
- Display the greater number using `DBMS_OUTPUT.PUT_LINE`.

```sql
DECLARE
    num1 NUMBER := 50;
    num2 NUMBER := 80;
BEGIN
    IF num1 > num2 THEN
        DBMS_OUTPUT.PUT_LINE('Greater number is: ' || num1);
    ELSE
        DBMS_OUTPUT.PUT_LINE('Greater number is: ' || num2);
    END IF;
END;
/
```


**Expected Output:**  

Greater number is: 80

**Output:**

<img width="397" height="287" alt="image" src="https://github.com/user-attachments/assets/094858dd-0bc3-48af-aea7-465f1687e433" />


---

## 2. Write a PL/SQL program to Calculate Sum of First N Natural Numbers

### Steps:
- Declare a variable `n` and assign a value (e.g., 10).
- Initialize a `sum` variable to 0.
- Use a `WHILE` loop to iterate from 1 to `n`, adding each number to the sum.
- Display the result using `DBMS_OUTPUT.PUT_LINE`.

```sql
SET SERVEROUTPUT ON;

DECLARE
    n   NUMBER := 10;
    i   NUMBER := 1;
    sum NUMBER := 0;
BEGIN
    WHILE i <= n LOOP
        sum := sum + i;
        i := i + 1;
    END LOOP;

    DBMS_OUTPUT.PUT_LINE('Sum of first ' || n || ' natural numbers is: ' || sum);
END;
/
```

**Expected Output:**  

Sum of first 10 natural numbers is: 55


**Output:**

<img width="423" height="279" alt="image" src="https://github.com/user-attachments/assets/b984f266-40d3-404b-bae3-c8804bfcf0a6" />

---

## 3. Write a PL/SQL program to generate Fibonacci series

### Steps:
- Declare the variable `n` to indicate how many terms to generate.
- Initialize the first two Fibonacci numbers (0 and 1).
- Use a loop to generate the next terms using the formula `c = a + b`.
- Print each term in the series.

```sql
SET SERVEROUTPUT ON;

DECLARE
    n NUMBER := 7;        -- number of terms
    a NUMBER := 0;        -- first term
    b NUMBER := 1;        -- second term
    c NUMBER;
    i NUMBER := 1;
BEGIN
    DBMS_OUTPUT.PUT_LINE('Fibonacci sequence for n = ' || n || ':');

    WHILE i <= n LOOP
        IF i = 1 THEN
            DBMS_OUTPUT.PUT(a || ', ');
        ELSIF i = 2 THEN
            DBMS_OUTPUT.PUT(b || ', ');
        ELSE
            c := a + b;
            DBMS_OUTPUT.PUT(c || ', ');
            a := b;
            b := c;
        END IF;

        i := i + 1;
    END LOOP;
END;
/
```

**Expected Output:**  

n = 7  
Fibonacci sequence: 0, 1, 1, 2, 3, 5, 8

**Output:**


<img width="455" height="270" alt="image" src="https://github.com/user-attachments/assets/e73c3b77-65d5-4f54-b211-eb2aed01993e" />

---

## 4. Write a PL/SQL Program to display the number in Reverse Order

### Steps:
- Declare a variable `n` and assign a value (e.g., 1535).
- Use a loop to extract each digit using modulo and reverse the number.
- Display the reversed number.

```sql
SET SERVEROUTPUT ON;

DECLARE
    n        NUMBER := 1535;
    temp     NUMBER := n;
    reverseN NUMBER := 0;
    digit    NUMBER;
BEGIN
    WHILE temp > 0 LOOP
        digit := MOD(temp, 10);          -- extract last digit
        reverseN := (reverseN * 10) + digit;
        temp := FLOOR(temp / 10);        -- remove last digit
    END LOOP;

    DBMS_OUTPUT.PUT_LINE('n = ' || n);
    DBMS_OUTPUT.PUT_LINE('Reversed number is ' || reverseN);
END;
/
```

**Expected Output:**  

n = 1535  
Reversed number is 5351

**Output:**

<img width="498" height="303" alt="image" src="https://github.com/user-attachments/assets/98a87f0f-378a-440c-92be-071f13096145" />



---

## 5. Write a PL/SQL program to find the largest of three numbers

### Steps:
- Declare three numeric variables `a`, `b`, and `c`.
- Use nested `IF-ELSIF-ELSE` conditions to find the largest among the three.
- Display the largest number.

```sql
SET SERVEROUTPUT ON;

DECLARE
    a NUMBER := 10;
    b NUMBER := 9;
    c NUMBER := 15;
    largest NUMBER;
BEGIN
    IF (a >= b) AND (a >= c) THEN
        largest := a;
    ELSIF (b >= a) AND (b >= c) THEN
        largest := b;
    ELSE
        largest := c;
    END IF;

    DBMS_OUTPUT.PUT_LINE('a = ' || a || ', b = ' || b || ', c = ' || c);
    DBMS_OUTPUT.PUT_LINE('Largest of three numbers is ' || largest);
END;
/
```

**Expected Output:** 

a = 10, b = 9, c = 15  
Largest of three number is 15


**Output:**

<img width="417" height="300" alt="image" src="https://github.com/user-attachments/assets/66ddfda2-8d71-4b7f-8e88-7dfea1f8ba38" />

**SEB Grades**
<img width="1500" height="800" alt="image" src="https://github.com/user-attachments/assets/d7508d47-a253-4759-a05d-50dce322d488" />


## RESULT
Thus, the PL/SQL programs using variables, conditionals, and loops were executed successfully.


