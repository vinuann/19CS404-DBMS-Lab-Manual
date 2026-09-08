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

**Program**
```
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50)
);

INSERT INTO employees VALUES (101, 'Arun', 'Manager');
INSERT INTO employees VALUES (102, 'Priya', 'Developer');
INSERT INTO employees VALUES (103, 'Kumar', 'Tester');

COMMIT;

DECLARE
    CURSOR emp_cursor IS
        SELECT emp_name, designation
        FROM employees;

    v_emp_name employees.emp_name%TYPE;
    v_designation employees.designation%TYPE;
    v_count NUMBER := 0;

BEGIN
    OPEN emp_cursor;

    LOOP
        FETCH emp_cursor INTO v_emp_name, v_designation;
        EXIT WHEN emp_cursor%NOTFOUND;

        v_count := v_count + 1;

        DBMS_OUTPUT.PUT_LINE(
            'Employee Name: ' || v_emp_name ||
            ', Designation: ' || v_designation
        );
    END LOOP;

    CLOSE emp_cursor;

    IF v_count = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employee records found.');

    WHEN OTHERS THEN
        IF emp_cursor%ISOPEN THEN
            CLOSE emp_cursor;
        END IF;

        DBMS_OUTPUT.PUT_LINE('An unexpected error occurred.');
END;
```

**Output:**  
<img width="780" height="816" alt="image" src="https://github.com/user-attachments/assets/0f6920be-a05f-4f1b-954b-bc13ebacdf49" />
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

**Program**
```
DECLARE
    min_sal NUMBER := 25000;
    max_sal NUMBER := 35000;
    emp_count NUMBER := 0;

    CURSOR emp_cursor(p_min NUMBER, p_max NUMBER) IS
        SELECT emp_id, emp_name, designation, salary
        FROM employees
        WHERE salary BETWEEN p_min AND p_max;

BEGIN
    FOR emp IN emp_cursor(min_sal, max_sal) LOOP
        emp_count := emp_count + 1;

        DBMS_OUTPUT.PUT_LINE(
            'Employee ID: ' || emp.emp_id ||
            ', Name: ' || emp.emp_name ||
            ', Designation: ' || emp.designation ||
            ', Salary: ' || emp.salary
        );
    END LOOP;

    IF emp_count = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the given salary range.');

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
```
/

**Output:**  
<img width="716" height="817" alt="image" src="https://github.com/user-attachments/assets/3a71271a-b8d4-4643-81eb-a16eb196bba4" />

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

**Program**
```
DECLARE
    emp_count NUMBER := 0;

    CURSOR emp_cursor IS
        SELECT emp_name, dept_no
        FROM employees;

BEGIN
    FOR emp IN emp_cursor LOOP
        emp_count := emp_count + 1;

        DBMS_OUTPUT.PUT_LINE(
            'Employee Name: ' || emp.emp_name ||
            ', Department No: ' || emp.dept_no
        );
    END LOOP;

    -- Raise exception if no employees are found
    IF emp_count = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the database.');

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
```

**Output:**  
<img width="701" height="828" alt="image" src="https://github.com/user-attachments/assets/eb95fbc1-0346-48bc-8c43-d0b33dd96959" />

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

**Program**
```
DECLARE
    CURSOR emp_cursor IS
        SELECT emp_id, emp_name, designation, salary
        FROM employees;

    -- Record variable using %ROWTYPE
    emp_record emp_cursor%ROWTYPE;

    emp_count NUMBER := 0;

BEGIN
    OPEN emp_cursor;

    LOOP
        FETCH emp_cursor INTO emp_record;

        EXIT WHEN emp_cursor%NOTFOUND;

        emp_count := emp_count + 1;

        DBMS_OUTPUT.PUT_LINE(
            'Employee ID: ' || emp_record.emp_id ||
            ', Name: ' || emp_record.emp_name ||
            ', Designation: ' || emp_record.designation ||
            ', Salary: ' || emp_record.salary
        );
    END LOOP;

    CLOSE emp_cursor;

    -- Check if no records were found
    IF emp_count = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the database.');

    WHEN OTHERS THEN
        IF emp_cursor%ISOPEN THEN
            CLOSE emp_cursor;
        END IF;

        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```

**Output:**  
<img width="692" height="822" alt="image" src="https://github.com/user-attachments/assets/5d3d69ee-698d-4d0a-8153-a9533c0d1719" />

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

**Program**
```
DECLARE
    CURSOR emp_cursor IS
        SELECT emp_id, emp_name, salary
        FROM employees
        WHERE dept_no = 10
        FOR UPDATE;

    v_increment NUMBER := 5000;
    v_count NUMBER := 0;

BEGIN
    FOR emp_rec IN emp_cursor LOOP

        UPDATE employees
        SET salary = emp_rec.salary + v_increment
        WHERE CURRENT OF emp_cursor;

        DBMS_OUTPUT.PUT_LINE('Updated Salary for ' || emp_rec.emp_name);

        v_count := v_count + 1;
    END LOOP;

    IF v_count = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;

    COMMIT;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in department 10.');

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/
```

**Output:**  
<img width="1007" height="692" alt="image" src="https://github.com/user-attachments/assets/09e66c58-12bb-42f7-a261-ced81fdc6983" />

---

## RESULT
Thus, the program successfully executed and displayed employee details using a cursor. 

