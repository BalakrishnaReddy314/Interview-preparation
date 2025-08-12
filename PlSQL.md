# Oracle PL/SQL Mastery: From Fundamentals to Complex Database Development

## Table of Contents
1. Database Fundamentals
2. PL/SQL Core Concepts
3. Stored Program Units
4. Advanced PL/SQL Features
5. Database Design Patterns
6. Performance Optimization
7. Security Implementation
8. Real-World Systems Development

---

## Chapter 1: Database Fundamentals

### 1.1 Oracle Architecture Overview

**Explanation:**
Oracle Database uses a multi-tier architecture consisting of:
- **Instance**: Memory structures (SGA) and background processes
- **Database**: Physical files (datafiles, control files, redo logs)
- **Schema**: Logical container for database objects owned by a user

**Key Components:**
```sql
-- View database components
SELECT * FROM v$database;  -- Database information
SELECT * FROM v$instance;  -- Instance information
SELECT * FROM all_users;   -- All users in the database
```

### 1.2 Essential Database Objects

**Tables:**
```sql
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(100) NOT NULL,
    hire_date DATE DEFAULT SYSDATE,
    salary NUMBER(10,2) CHECK (salary > 0),
    dept_id NUMBER REFERENCES departments(dept_id)
);
```

**Indexes:**
```sql
-- B-tree index (default)
CREATE INDEX idx_emp_name ON employees(emp_name);

-- Bitmap index for low-cardinality columns
CREATE BITMAP INDEX idx_emp_gender ON employees(gender);

-- Function-based index
CREATE INDEX idx_emp_upper_name ON employees(UPPER(emp_name));
```

---

## Chapter 2: PL/SQL Core Concepts

### 2.1 PL/SQL Block Structure

**Anatomy of a PL/SQL Block:**
```plsql
DECLARE
  -- Declaration section (variables, cursors, etc.)
  v_counter NUMBER := 0;
  v_name VARCHAR2(100);
  CURSOR emp_cur IS SELECT * FROM employees;
BEGIN
  -- Execution section
  OPEN emp_cur;
  LOOP
    FETCH emp_cur INTO v_name;
    EXIT WHEN emp_cur%NOTFOUND;
    v_counter := v_counter + 1;
  END LOOP;
  CLOSE emp_cur;
  
  DBMS_OUTPUT.PUT_LINE('Processed ' || v_counter || ' employees');
EXCEPTION
  -- Exception handling
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
```

## **Chapter 3: Stored Program Units**  

### **3.1 Procedures**  
**Explanation:**  
Procedures are reusable PL/SQL blocks that perform specific tasks. They can accept parameters, execute SQL, and implement business logic.  

**Key Features:**  
- Can have `IN`, `OUT`, and `IN OUT` parameters  
- Can include transaction control (`COMMIT`, `ROLLBACK`)  
- Improve modularity and security  

**Example: Employee Salary Update Procedure**  
```plsql
CREATE OR REPLACE PROCEDURE update_employee_salary(
    p_emp_id     IN  NUMBER,
    p_percentage IN  NUMBER,
    p_updated_by IN  VARCHAR2,
    p_status     OUT VARCHAR2
) AS
    v_old_salary NUMBER;
    v_new_salary NUMBER;
BEGIN
    -- Get current salary
    SELECT salary INTO v_old_salary
    FROM employees
    WHERE employee_id = p_emp_id
    FOR UPDATE; -- Lock row to prevent concurrent updates
    
    -- Calculate new salary
    v_new_salary := v_old_salary * (1 + (p_percentage / 100));
    
    -- Update salary
    UPDATE employees
    SET salary = v_new_salary,
        last_updated = SYSDATE,
        updated_by = p_updated_by
    WHERE employee_id = p_emp_id;
    
    -- Log the change
    INSERT INTO salary_audit (
        employee_id, old_salary, new_salary,
        change_date, changed_by
    ) VALUES (
        p_emp_id, v_old_salary, v_new_salary,
        SYSDATE, p_updated_by
    );
    
    -- Set output status
    p_status := 'SUCCESS: Salary updated from ' || v_old_salary || ' to ' || v_new_salary;
    
    -- Commit the transaction
    COMMIT;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        p_status := 'ERROR: Employee not found';
        ROLLBACK;
    WHEN OTHERS THEN
        p_status := 'ERROR: ' || SQLERRM;
        ROLLBACK;
END update_employee_salary;
```

**Usage:**  
```plsql
DECLARE
    v_status VARCHAR2(200);
BEGIN
    update_employee_salary(
        p_emp_id => 101,
        p_percentage => 10,
        p_updated_by => 'ADMIN',
        p_status => v_status
    );
    DBMS_OUTPUT.PUT_LINE(v_status);
END;
```

---

### **3.2 Functions**  
**Explanation:**  
Functions return a single value and can be used in SQL queries. They are ideal for calculations and data transformations.  

**Key Features:**  
- Must return a value  
- Can be used in `SELECT`, `WHERE`, and other SQL clauses  
- Should avoid DML operations (unless autonomous transactions)  

**Example: Calculate Tax Function**  
```plsql
CREATE OR REPLACE FUNCTION calculate_tax(
    p_salary NUMBER
) RETURN NUMBER 
DETERMINISTIC -- Optimizes performance if inputs always produce same output
AS
    v_tax_rate NUMBER;
    v_tax_amount NUMBER;
BEGIN
    -- Progressive tax calculation
    IF p_salary <= 10000 THEN
        v_tax_rate := 0.10;
    ELSIF p_salary <= 50000 THEN
        v_tax_rate := 0.20;
    ELSE
        v_tax_rate := 0.30;
    END IF;
    
    v_tax_amount := p_salary * v_tax_rate;
    RETURN ROUND(v_tax_amount, 2);
END calculate_tax;
```

**Usage in SQL:**  
```sql
SELECT employee_id, salary, calculate_tax(salary) AS tax
FROM employees;
```

---

### **3.3 Packages**  
**Explanation:**  
Packages group related procedures, functions, variables, and cursors into a single unit. They improve organization, security, and performance.  

**Components:**  
1. **Specification (Header)** – Declares public elements  
2. **Body** – Implements the logic  

**Example: Employee Management Package**  
```plsql
-- Package Specification (Public Interface)
CREATE OR REPLACE PACKAGE emp_pkg AS
    -- Constants
    g_max_salary CONSTANT NUMBER := 100000;
    
    -- Exceptions
    e_invalid_dept EXCEPTION;
    
    -- Procedures
    PROCEDURE hire_employee(
        p_name       VARCHAR2,
        p_job        VARCHAR2,
        p_salary     NUMBER,
        p_dept_id    NUMBER
    );
    
    PROCEDURE fire_employee(p_emp_id NUMBER);
    
    -- Functions
    FUNCTION get_employee_count(p_dept_id NUMBER) RETURN NUMBER;
    
    -- Cursor (returns employees in a department)
    CURSOR get_dept_emps(p_dept_id NUMBER) RETURN employees%ROWTYPE;
END emp_pkg;
```

**Package Body (Implementation):**  
```plsql
CREATE OR REPLACE PACKAGE BODY emp_pkg AS
    -- Procedure to hire an employee
    PROCEDURE hire_employee(
        p_name       VARCHAR2,
        p_job        VARCHAR2,
        p_salary     NUMBER,
        p_dept_id    NUMBER
    ) AS
        v_emp_id NUMBER;
    BEGIN
        -- Validate department
        IF NOT is_valid_dept(p_dept_id) THEN
            RAISE e_invalid_dept;
        END IF;
        
        -- Validate salary
        IF p_salary > g_max_salary THEN
            RAISE_APPLICATION_ERROR(-20001, 'Salary exceeds maximum allowed');
        END IF;
        
        -- Generate employee ID
        SELECT emp_seq.NEXTVAL INTO v_emp_id FROM dual;
        
        -- Insert employee
        INSERT INTO employees (
            employee_id, employee_name, job_title,
            salary, department_id, hire_date
        ) VALUES (
            v_emp_id, p_name, p_job,
            p_salary, p_dept_id, SYSDATE
        );
        
        COMMIT;
    EXCEPTION
        WHEN e_invalid_dept THEN
            RAISE_APPLICATION_ERROR(-20002, 'Invalid department ID');
        WHEN OTHERS THEN
            ROLLBACK;
            RAISE;
    END hire_employee;
    
    -- Function to check valid department
    FUNCTION is_valid_dept(p_dept_id NUMBER) RETURN BOOLEAN AS
        v_count NUMBER;
    BEGIN
        SELECT COUNT(*) INTO v_count
        FROM departments
        WHERE department_id = p_dept_id;
        
        RETURN (v_count > 0);
    END is_valid_dept;
    
    -- Additional implementations...
END emp_pkg;
```

**Usage:**  
```plsql
BEGIN
    emp_pkg.hire_employee(
        p_name    => 'John Doe',
        p_job     => 'DEVELOPER',
        p_salary  => 75000,
        p_dept_id => 10
    );
END;
```

---

## **Chapter 4: Advanced PL/SQL Features**  

### **4.1 Cursors (Explicit vs. Implicit)**  
**Explanation:**  
Cursors allow row-by-row processing of query results.  

| **Type**       | **Description** |
|--------------|---------------|
| **Implicit** | Managed by Oracle (e.g., `SELECT INTO`) |
| **Explicit** | Programmer-defined for complex queries |

**Example: Explicit Cursor with Parameter**  
```plsql
DECLARE
    CURSOR emp_cursor(p_dept_id NUMBER) IS
        SELECT employee_id, employee_name, salary
        FROM employees
        WHERE department_id = p_dept_id;
    
    v_emp_record emp_cursor%ROWTYPE;
BEGIN
    OPEN emp_cursor(10); -- Open for department 10
    LOOP
        FETCH emp_cursor INTO v_emp_record;
        EXIT WHEN emp_cursor%NOTFOUND;
        
        DBMS_OUTPUT.PUT_LINE(
            v_emp_record.employee_id || ' - ' ||
            v_emp_record.employee_name || ' ($' ||
            v_emp_record.salary || ')'
        );
    END LOOP;
    CLOSE emp_cursor;
END;
```

---

### **4.2 Bulk Processing (FORALL & BULK COLLECT)**  
**Explanation:**  
Improves performance by reducing context switching between SQL and PL/SQL.  

**Example: Bulk Update Salaries**  
```plsql
DECLARE
    TYPE emp_id_array IS TABLE OF employees.employee_id%TYPE;
    v_emp_ids emp_id_array := emp_id_array(101, 102, 103);
BEGIN
    -- Update multiple employees in a single operation
    FORALL i IN 1..v_emp_ids.COUNT
        UPDATE employees
        SET salary = salary * 1.05  -- 5% raise
        WHERE employee_id = v_emp_ids(i);
    
    DBMS_OUTPUT.PUT_LINE('Updated ' || SQL%ROWCOUNT || ' employees');
END;
```
