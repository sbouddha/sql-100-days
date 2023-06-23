## **Oracle PL/SQL Notes**
---

**PL/SQl Full form:**  Procedural Language extension to Structured Query Language

**PL/SQL Structure**: Uses Blocks    

```sql
DECLARE --Optional
--Define Variables, cursors, user defined exceptions

BEGIN --Mandatory
--Write SQL statements
--PL/SQL statements

EXCEPTION --Optional
--Actions to perform when error occurs

END; --Mandatory
```
Types of Blocks (2 Types):  
* Anonymous Blocks : DBEE (Generic, without names)
* Subprograms : Procedures, Functions (With names)

In Oracle, there are two primary ways to encapsulate and execute a block of code: anonymous blocks and subprograms.   Here's a comparison between the two:  
1. Anonymous Blocks:  
• Definition: An anonymous block is a nameless block of code that can be directly executed in the Oracle database without being stored as a separate database object.  
• Scope: Anonymous blocks are typically used for short, one-time code snippets or ad-hoc tasks that do not require reusability or sharing across multiple program units.  
• Execution: Anonymous blocks are executed using the "EXECUTE IMMEDIATE" statement or by simply running the block of code as a script.  
• Parameters: Anonymous blocks cannot have parameters, so you cannot pass values into or receive values from the block.  

1. Subprograms:  
• Definition: A subprogram is a named block of code that is stored in the Oracle database as a separate database object. It can be called and executed multiple times from different program units.  
• Scope: Subprograms provide modularity, reusability, and maintainability. They are typically used for larger and more complex pieces of code that need to be called and reused by multiple program units.  
• Execution: Subprograms are invoked by calling their name within another program unit (e.g., stored procedures, functions, triggers). They are compiled and stored in the database, allowing for repeated execution.  
• Parameters: Subprograms can have input parameters, output parameters, or both. Parameters enable passing values into the subprogram and returning values back to the calling program.  


|             | Anonymous Blocks                 | Subprograms                      |
| ----------- | -------------------------------- | -------------------------------- |
| Definition  | Nameless block of code           | Named block of code              |
| Name        | No explicit name                 | Has a distinct name              |
| Compilation | Compiles Every time              | Compile Once                     |
| Return      | Cannot return Values             | Can return Values                |
| Execution   | Immediate execution or as script | Invoked by calling the name      |
| Parameters  | No parameters                    | Can have input/output parameters |
| Storage     | Not stored in DB                 | Stored as database object in DB  |
| Scope       | One-time tasks, ad-hoc snippets  | Reusable, shared across units    |
| Reusability | Not designed for reusability     | Designed for reusability         |
| Invocation  | Direct execution or script       | Called from other program units  |
| Purpose     | Short, simple tasks              | Larger, complex tasks            |


---

To Print :  
```sql
BEGIN
    dbms_output.put_line('Hello Anonymous Block');
END;
```
Fire these before :  
```sql
set serveroutput on;
```

```sql
--to fetch variable value
DECLARE
    v number;

BEGIN
    v:=5;
    dbms_output.put_line('Hello Anonymous Block ' || v);

END;
```


```sql
-- Description: This PL/SQL block calculates the factorial of a given number.
-- Author: Bouddha Codes
-- Date: June 1, 2023

-- Declaration Section
DECLARE
  -- Constants are named as c_ and have to put keyword CONSTANT
  c_input_number CONSTANT NUMBER := 5;

  -- Variables are named as v_
  v_factorial NUMBER := 1;

BEGIN
  -- Calculation of Factorial
  FOR i IN 1..c_input_number LOOP
    v_factorial := v_factorial * i;
  END LOOP;

  -- Output
  DBMS_OUTPUT.PUT_LINE('Factorial of ' || c_input_number || ' is: ' || v_factorial);
END;
/
```

***Accepting input Values on the fly*** : Using & character   
```sql
DECLARE
    v_input NUMBER := &v;
BEGIN
    IF v_input > 10 THEN
        dbms_output.put_line('You entered: GREATER ' || v_input);
    ELSIF v_input = 10 THEN
        dbms_output.put_line('EQUAL ' || v_input);
    ELSE
        dbms_output.put_line('You entered: LESSER ' || v_input);
    END IF;
END;
```
---
### **Handling NULL in PL/SQL Code**:     
---

Any operation with to NULL will result in NULL. Like below:  
1. Arithmetic Operations:  
• NULL + 10 = NULL  
• NULL - 5 = NULL  
• NULL * 3 = NULL  
• NULL / 2 = NULL  
2. String Concatenation:  
• NULL || 'Hello' = NULL  
• 'Hi' || NULL = NULL  
3. Comparison Operators:  
• NULL = NULL --> NULL (not true or false, but NULL)  
• NULL <> 5 --> NULL  
• NULL > 3 --> NULL  
4. Logical Operators:  
• NULL AND TRUE = NULL  
• NULL OR FALSE = NULL  
• NOT NULL = NULL  

So, in PL/SQL, you can handle **NULL** values using conditional statements and built-in functions. Here are a few approaches to handle NULL values in PL/SQL code:

Example:
```sql
DECLARE
    v1  NUMBER;
    v2  NUMBER := 5;
BEGIN
    IF v1 <> v2 THEN
        dbms_output.put_line('i love oracle');
    ELSE
        dbms_output.put_line('i like oracle');
    END IF;
END;
```
Note: The output of the code will be: i like oracle 
The reason is that the variable v1 is not assigned a value, so it has a NULL value by default. In the IF statement condition, v1 <> v2 is evaluated, which is equivalent to NULL <> 5. 
When comparing NULL with any value, including 5, the result is always NULL, not true or false.  
Since the condition v1 <> v2 evaluates to NULL, the ELSE block of the IF statement is executed, and the output i like oracle is displayed.  
It's important to note that when comparing values to NULL using equality operators (=, <>, etc.), the result is always NULL, not a boolean true or false value. To handle NULL values in comparisons, you can use the IS NULL or IS NOT NULL operators.  


**IF-THEN-ELSE Statement:**
You can use an IF-THEN-ELSE statement to check if a variable or expression is NULL and perform specific actions accordingly.
```sql
IF variable IS NULL THEN
    -- Handle NULL case
ELSE
    -- Handle non-NULL case
END IF;
```
**NVL Function:**
The NVL function is used to substitute a NULL value with a specified default value. It returns the default value if the expression is NULL; otherwise, it returns the expression itself.
```sql
variable := NVL(expression, default_value);
```
**COALESCE Function:**
The COALESCE function is similar to the NVL function, but it allows you to check multiple expressions for NULL values and return the first non-NULL expression.
```sql
variable := COALESCE(expression1, expression2, ..., default_value);
```
**IS NULL Condition:**
You can use the IS NULL condition to check if a variable or expression is NULL.
```sql
IF variable IS NULL THEN
    -- Handle NULL case
END IF;
```
**CASE Statement:**
The CASE statement can be used to evaluate conditions and perform different actions based on the NULL status of a variable or expression.
```sql
CASE
    WHEN variable IS NULL THEN
        -- Handle NULL case
    ELSE
        -- Handle non-NULL case
END CASE;
```
These are some common approaches to handle NULL values in PL/SQL code. Depending on the specific requirements and context, you can choose the appropriate approach to handle NULL values effectively.

---
### **PL/SQL Variables:**  
---
* Scalar Variables:Scalar variables hold a single value of a specific data type, such as numbers, strings, dates, or Boolean values.   Here are some examples:  
  * Numeric Variables
  * String Variables
  * Date Variables
  * Boolean Variables      
    
* Composite Variables: Composite variables can hold multiple values of the same or different data types. Common examples of composite variables in Oracle are records and collections.  
    * Record Variables
        ```sql
        DECLARE
        emp_rec EMPLOYEES%ROWTYPE;
        BEGIN
        -- Code using emp_rec
        END;
        ```

    * Collection Variables 
      ```sql
        DECLARE
        type num_array_type IS TABLE OF NUMBER;
        num_arr num_array_type := num_array_type(1, 2, 3, 4, 5);
        BEGIN
        -- Code using num_arr
        END;
      ``` 
   
* Cursor Variables: Cursor variables are used to hold references to result sets returned by queries. They are commonly used for dynamic SQL operations.  
  
    ```sql
    DECLARE
    TYPE emp_cur_type IS REF CURSOR;
    emp_cur emp_cur_type;
    BEGIN
    OPEN emp_cur FOR SELECT * FROM EMPLOYEES;
    -- Code using emp_cur
    END;
    ```

* LOB Data Type Variables: In PL/SQL, LOB (Large Object) variables are used to store and manipulate large amounts of data, such as text, images, audio, or video files. LOBs can hold up to 4 gigabytes of data and provide functionality for efficient handling and manipulation of large data objects. There are three types of LOB variables in PL/SQL:  
1. **CLOB** (Character LOB): CLOB variables are used to store large amounts of character data, such as *text documents or XML files*.  
2. **BLOB** (Binary LOB): BLOB variables are used to store binary data, *such as images, audio files, or video files*.  
3. **NCLOB** (National Character LOB): NCLOB variables are used to store large amounts of national character set data, supporting multibyte character sets.  
LOB variables have specific methods and operators that allow you to perform various operations on the LOB data, such as inserting, updating, appending, deleting, and reading data from the LOB.  

```sql
DECLARE
  my_clob CLOB;
BEGIN
  -- Assigning a value to the CLOB variable
  my_clob := 'Learning about LOBs'; 
  
  -- Inserting data into a table using the CLOB variable
  INSERT INTO my_table (clob_column) VALUES (my_clob);
  
  -- Reading data from the CLOB variable
  DBMS_OUTPUT.PUT_LINE(my_clob);
  
  -- Manipulating the CLOB data
  my_clob := my_clob || ' Additional text.';
  
  -- Updating a table column with the modified CLOB variable
  UPDATE my_table SET clob_column = my_clob WHERE id = 1;
  
  -- Deleting data from the CLOB variable
  my_clob := EMPTY_CLOB();
  
  -- ... additional operations on the CLOB variable
END;
```


**NON-PL/SQL Variables:**

* **Bind Variables**: Bind variables are placeholders used in SQL statements or PL/SQL code to represent values that will be provided at runtime. Instead of embedding actual values directly into the SQL statement, bind variables are used to improve performance, security, and maintainability.

NOTE:
- varchar **should not have** size while defining
- varchar2 **should have** size when defining 


```sql
DECLARE

    VARIABLE v_employee_id NUMBER; -- Bind variable declaration **imp use keyword VARIABLE** to declare
    -- Other variable declarations as needed
    v_employee_name VARCHAR2(100);
    v_employee_salary NUMBER;

BEGIN
    -- Assign values to other variables as needed

    -- Use bind variable in SQL statement
    SELECT employee_name, salary INTO v_employee_name, v_employee_salary
    FROM employees
    WHERE employee_id = :v_employee_id; -- Colon prefix indicates bind variable

    -- Do further processing with the retrieved values
    DBMS_OUTPUT.PUT_LINE('Employee Name: ' || v_employee_name);
    DBMS_OUTPUT.PUT_LINE('Employee Salary: ' || v_employee_salary);

    -- Use bind variable in another SQL statement
    UPDATE employees
    SET salary = v_employee_salary + 1000
    WHERE employee_id = :v_employee_id; -- Colon prefix indicates bind variable

    -- More code...

END;
```

```sql
SELECT * FROM employees WHERE department_id = :dept_id;
```

**Declaring Variable with %TYPE** : In Oracle PL/SQL, the ***%TYPE*** attribute is used to declare variables by deriving their data type from an existing column or variable. This simplifies the declaration process and ensures that the new variable has the same data type as the referenced object. The %TYPE attribute is particularly useful when you want to declare a variable that should match the data type of an existing database column.

Here's the syntax for declaring a variable using the %TYPE attribute:

```sql
DECLARE
  variable_name table_name.column_name%TYPE;
BEGIN
  -- Code using the variable
END;
```
---
### **Important commands/queries**
---
In Oracle SQLPlus, there are several important commands that can be used to configure the environment and customize the behavior of the SQLPlus session. Here are some commonly used commands:  

1. **SET SERVEROUTPUT ON** - This command enables the display of the output generated by the DBMS_OUTPUT.PUT_LINE statements in PL/SQL code. It allows you to see the output messages during the execution of PL/SQL blocks.  
2. **SET AUTOPRINT ON** - This command enables the automatic printing of select query results after each execution. When enabled, SQL*Plus automatically displays the fetched rows from a SELECT statement without explicitly using the PRINT or SELECT commands.   
3. **SET VERIFY ON** - This command controls whether SQL*Plus displays the text of a SQL statement or PL/SQL block after substitution variables have been replaced. When VERIFY is enabled, the SQL statements or PL/SQL blocks are displayed with substituted values.  
4. **SET TIMING ON** - This command enables the display of timing information for each SQL statement executed. It shows the elapsed time for the execution of the SQL statement.  
5. **SET PAGESIZE n** - This command sets the number of lines displayed on each page of output. The value n represents the number of lines per page.  
6. **SET LINESIZE n** - This command sets the maximum width of a line displayed on the screen or spooled to a file. The value n represents the number of characters per line.  
7. **SET FEEDBACK {ON | OFF}** - This command controls the display of the "n rows selected" message after executing a SQL statement. When FEEDBACK is enabled, the message is displayed. When FEEDBACK is disabled, the message is suppressed.  
8. **SPOOL filename** - This command enables the spooling of the output to a file. It directs subsequent output to be written to the specified file rather than displayed on the screen.  
These are just a few examples of commonly used commands in Oracle SQLPlus. There are many more commands available to customize and configure the SQLPlus environment according to your specific requirements. You can explore the Oracle documentation for a comprehensive list of available commands and their usage.

---
### **Inside PL/SQL Block**
---
In a PL/SQL block, you can include various types of statements and constructs to perform different operations.   Here are some of the elements that can be included in a PL/SQL block:  
1. Variable Declarations: You can declare variables of different data types to store values during the execution of the block.  
2. Identifiers: Identifiers are used to name variables, constants, procedures, functions, cursors, and other program elements within the PL/SQL block. They follow naming conventions and rules such as starting with a letter and consisting of letters, numbers, and underscores. Examples of identifiers are variable names like employee_name, procedure names like calculate_salary, or table names like employees.  
3. Delimiters: Delimiters are symbols or characters used to separate or enclose different parts of PL/SQL statements. Commonly used delimiters include semicolon (;), which indicates the end of a statement, and double quotation marks (""), used to enclose identifiers that are case-sensitive or contain special characters.  
4. Literals: Literals are fixed values that are directly written in the PL/SQL code. They can be numeric literals, character literals, string literals, Boolean literals, or date literals. Examples of literals include 123 (numeric literal), 'John' (character literal), "Hello, World!" (string literal), TRUE (Boolean literal), and DATE '2023-06-01' (date literal).  
5. Comments: Comments are used to add explanatory or descriptive text within the PL/SQL code. They are ignored by the compiler and are only meant for human readers. PL/SQL supports both single-line comments and multi-line comments. Single-line comments start with --, while multi-line comments are enclosed between /* and */. Comments are useful for documenting code, providing explanations, or temporarily disabling parts of the code.  
6. Constants: You can define constants that hold fixed values throughout the execution of the block.  
7. Control Flow Statements: PL/SQL provides control flow statements such as IF-THEN-ELSE, CASE, LOOP, WHILE, and FOR to control the flow of execution based on specific conditions or iterations.  
8. Exception Handling: You can use exception handling blocks to catch and handle errors or exceptions that may occur during the execution of the block.  
9. SQL Statements: PL/SQL allows you to execute SQL statements such as SELECT, INSERT, UPDATE, DELETE, and more within the block to interact with the database and manipulate data.  
10. Cursors: Cursors are used to retrieve and manipulate result sets returned by SQL queries. You can define and use explicit cursors to fetch rows from the result set and process them.  
11. Subprograms: You can define and call subprograms such as procedures and functions within a PL/SQL block to encapsulate reusable code and modularize your logic.  
12. Exception Handlers: Exception handlers are used to catch and handle specific exceptions that may occur during the execution of the block.  
13. Error Logging and Reporting: You can use the DBMS_OUTPUT package to log and display debug information or messages during the execution of the block.  
14. User-Defined Types: PL/SQL allows you to create user-defined types such as records, collections, and object types, which can be used within the block to structure and manipulate data.  

---
### **CONTROLLING FLOW OF EXECUTION ( )**  
---
In Oracle PL/SQL, there are several commands used to control the flow of execution within a program. These commands allow you to alter the normal sequential execution of statements and handle various conditions and scenarios.     
Here are some of the key commands for controlling the flow of execution:  
**IF-THEN-ELSE**: This command is used to conditionally execute a block of statements based on a specified condition. It allows you to choose between two different sets of statements depending on whether the condition is true or false.  
**CASE**: The CASE statement provides a way to perform conditional branching based on the value of an expression. It allows you to evaluate multiple conditions and execute different blocks of code based on the matched condition.  
**LOOP**: The LOOP command enables you to create loops for repetitive execution of a block of statements. You can use various types of loops such as basic LOOP, WHILE loop, and FOR loop, depending on the specific requirements.  
**EXIT**: The EXIT command is used to prematurely exit a loop or a block of code based on a specified condition. It allows you to break out of a loop or terminate the current block of code and continue execution from the next statement.  
**GOTO**: The GOTO command transfers the control of execution to a labeled statement within the same block or subprogram. It is generally not recommended to use the GOTO command due to its potential to make code harder to understand and maintain.  
**EXCEPTION**: Exception handling commands like RAISE, EXCEPTION, and WHEN are used to handle and manage exceptions (errors) that occur during the execution of the program. They provide a way to gracefully handle errors and take appropriate actions.  
These commands, along with other control structures like WHILE, FOR, and EXIT WHEN, provide the necessary tools to control the flow of execution and create more flexible and robust PL/SQL programs. It's important to use them judiciously and follow best practices to ensure code readability and maintainability.

*Now in Details:*

**IF** Statement:  Here are the syntaxes for the four types of IF statements in Oracle PL/SQL:

Simple IF Statement:
```sql
IF condition THEN
   -- statements to be executed if the condition is true
END IF;
```

IF-THEN-ELSE Statement:
```sql
IF condition THEN
   -- statements to be executed if the condition is true
ELSE
   -- statements to be executed if the condition is false
END IF;
```

IF-THEN-ELSIF-ELSE Statement:
```sql
IF condition1 THEN
   -- statements to be executed if condition1 is true
ELSIF condition2 THEN
   -- statements to be executed if condition2 is true
ELSE
   -- statements to be executed if both condition1 and condition2 are false
END IF;
```

Nested IF Statement:
```sql
IF condition1 THEN
   -- statements to be executed if condition1 is true
   IF condition2 THEN
      -- statements to be executed if condition2 is true
   ELSE
      -- statements to be executed if condition2 is false
   END IF;
ELSE
   -- statements to be executed if condition1 is false
END IF;
```
In these syntaxes, condition represents an expression that evaluates to a Boolean value (TRUE or FALSE). If the condition evaluates to TRUE, the corresponding statements within the IF block are executed. Otherwise, if the condition evaluates to FALSE, the statements within the ELSE block (if present) are executed.  
You can use logical operators such as AND, OR, and NOT to form complex conditions within the IF statements.  
Remember to terminate each statement block within the IF construct with an appropriate keyword like END IF, END IF-THEN-ELSE, or END IF-THEN-ELSIF-ELSE, depending on the structure of the IF statement.  
By using these IF statement variations, you can control the flow of execution in your PL/SQL code based on specific conditions and perform different actions accordingly.  

**CASE statement and CASE expression** 
The CASE statement and CASE expression in Oracle SQL are used for different purposes:
1. **CASE Statement**:  
• The CASE statement is used for conditional control flow in SQL and PL/SQL. It allows you to perform different actions based on different conditions.  
• It can have multiple WHEN-THEN branches, and the first condition that evaluates to true is executed.  
• The CASE statement is typically used for control flow, branching, and executing different sets of statements based on different conditions.  
• It cannot be used directly as an expression in the SELECT statement or other parts of a query.  
Example of a CASE statement:
```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE result3
END CASE;
```
2. **CASE Expression**:  
• The CASE expression is used to return a single value based on different conditions. It is used within an expression and can be part of the SELECT statement or other parts of a query.  
• It can have multiple WHEN-THEN branches, and the first condition that evaluates to true is used to determine the result.  
• The CASE expression is typically used to derive a calculated value or transform data within a single expression.
Example of a CASE expression:  

```sql
SELECT
    column1,
    column2,
    CASE
        WHEN condition1 THEN result1
        WHEN condition2 THEN result2
        ELSE result3
    END AS derived_column
FROM
    table;
``` 
In summary, the CASE statement is used for control flow and executing different sets of statements, while the CASE expression is used to derive a calculated value within an expression.

**LOOPS** :In PL/SQL, there are different types of loops available for iterating over a set of statements. Here are the three commonly used loop constructs in PL/SQL along with examples:  
```sql
--PRINT HELLO 3 times
--USING GENERIC LOOP
DECLARE
    v_counter NUMBER := 0;
BEGIN
    LOOP
        dbms_output.put_line('Hello');
        v_counter := v_counter + 1;
        EXIT WHEN v_counter >= 3;
    END LOOP;
END;
```

**FOR Loop**: The FOR loop is used when you know the exact number of iterations in advance. It iterates over a range of values and executes a set of statements for each iteration.
```sql
--PRINT HELLO 3 times
--USING FOR LOOP
DECLARE
    v_counter NUMBER;
BEGIN
    FOR v_counter IN 1..3 LOOP
        dbms_output.put_line('Hello');
    END LOOP;
END;

```


**WHILE Loop:** The WHILE loop is used when the number of iterations is not known in advance. It repeatedly executes a set of statements as long as a condition is true.
```sql
--PRINT HELLO 3 times
--USING WHILE LOOP
DECLARE
    v_counter NUMBER := 0;
BEGIN
    WHILE v_counter < 4 LOOP
        dbms_output.put_line('Hello');
        v_counter := v_counter + 1;
    END LOOP;
END;

```

**LOOP...EXIT**Statement: The LOOP...EXIT statement is used when you want to manually control the loop termination based on a specific condition. It allows you to exit the loop at any point within the loop body.
```sql
DECLARE
    -- Declare variables
    v_counter NUMBER := 1;
BEGIN
    -- Loop indefinitely
    LOOP
        -- Print the current value of v_counter
        DBMS_OUTPUT.PUT_LINE('Iteration: ' || v_counter);
        -- Increment v_counter
        v_counter := v_counter + 1;
        -- Exit the loop if v_counter exceeds 5
        IF v_counter > 5 THEN
            EXIT;
        END IF;
    END LOOP;
END;
```

**NESTED LOOP**: Loop inside a loop is called Nested loop. Below is one example:
```sql
--MAKING 5 STARS
DECLARE
    v_star_cnt NUMBER := 0;
    v_star_print VARCHAR2(100);
BEGIN
    WHILE v_star_cnt < 6 LOOP
        v_star_print := ''; -- Reset the variable to an empty string
        FOR i IN 1..v_star_cnt LOOP
            v_star_print := v_star_print || '*';
        END LOOP;
        dbms_output.put_line(v_star_print);
        v_star_cnt := v_star_cnt + 1;
    END LOOP;
END;
```
```sql
--MAKING 5 STARS (With Nested Loop Labels)
DECLARE
    v_star_cnt NUMBER := 0;
    v_star_print VARCHAR2(100);
BEGIN
    <<outer_loop>>
    WHILE v_star_cnt < 6 LOOP
        v_star_print := ''; -- Reset the variable to an empty string
        <<inner_loop>>
        FOR i IN 1..v_star_cnt LOOP
            v_star_print := v_star_print || '*';
        END LOOP inner_loop;
        dbms_output.put_line(v_star_print);
        v_star_cnt := v_star_cnt + 1;
    END LOOP outer_loop;
END;
```
Note : You can exit OUTER loop from INNER loop using Nested loop labels.Below is an example:  
```sql
--EXIT OUTER loop from INNER loop
DECLARE
    v_star_cnt NUMBER := 0;
    v_star_print VARCHAR2(100);
BEGIN
    <<outer_loop>>
    WHILE v_star_cnt < 6 LOOP
        v_star_print := ''; -- Reset the variable to an empty string
        <<inner_loop>>
        FOR i IN 1..v_star_cnt LOOP
            v_star_print := v_star_print || '*';
        EXIT outer_loop when v_star_cnt=3;
        END LOOP inner_loop;
        dbms_output.put_line(v_star_print);
        v_star_cnt := v_star_cnt + 1;
    END LOOP outer_loop;
END;
```

---
### **USING WITH**
---
The "WITH" clause, also known as a Common Table Expression (CTE), is useful in several scenarios.   Here are some situations where you might consider using the "WITH" clause:  
1. Querying Repeated Subqueries: If your query includes subqueries that are used multiple times or in different parts of the main query, you can use a CTE to define the subquery once and reference it multiple times, improving the query's readability and performance.  
2. Breaking Down Complex Queries: When you have a complex query involving multiple joins, aggregations, or transformations, using a CTE can help break down the query into smaller, more manageable parts. Each CTE can represent a logical step or subquery, making the overall query structure clearer.   
3. Recursive Queries: The "WITH" clause is commonly used for recursive queries, where a query refers to its own output. CTEs provide a structured way to define and control recursive operations in the query.  
4. Data Transformation: If you need to transform or manipulate the data before using it in the main query, you can use a CTE to perform the necessary data transformations in a separate subquery and then reference the transformed data in the main query.  
5. Code Readability and Maintainability: Even for simple queries, using a CTE can improve code readability and maintainability by separating logical sections of the query and providing meaningful names for subquery results.  
While the "WITH" clause can be beneficial in these scenarios, it's important to consider the query complexity, performance impact, and readability. For simpler queries, using a CTE might not provide significant benefits and could make the code unnecessarily complex.  
Ultimately, the decision to use the "WITH" clause depends on the specific requirements of your query and the trade-offs you are willing to make in terms of readability, maintainability, and performance.

```sql
--write a code to find the dates/weeks/months/year from your DOB
WITH days_finder AS (
    SELECT
        trunc(sysdate - TO_DATE('01-01-1989', 'DD-MM-YYYY')) AS days_diff
    FROM
        dual
)
SELECT
    days_diff                      AS "Days Passed",
    trunc(days_diff / 7)           AS "Weeks Passed",
    trunc(days_diff / 30)          AS "Months Passed",
    round(days_diff / 365, 1)      AS "Years Passed"
FROM
    days_finder;
```

---
#### **NESTED BLOCKS**
---

A nested block in PL/SQL refers to the concept of having one block of code (inner block) contained within another block (outer block). The inner block is nested inside the outer block and has its own scope and visibility of variables. Nested blocks are useful for organizing code, encapsulating logic, and controlling variable scope.  
Here's an example to demonstrate a nested block in PL/SQL:

```sql
DECLARE
   -- Outer block variables
   outer_variable NUMBER := 10;

BEGIN
   -- Outer block statements
   DBMS_OUTPUT.PUT_LINE('Outer Block');
   DBMS_OUTPUT.PUT_LINE('Outer Variable: ' || outer_variable);

   DECLARE
      -- Inner block variables
      inner_variable NUMBER := 20;

   BEGIN
      -- Inner block statements
      DBMS_OUTPUT.PUT_LINE('Inner Block');
      DBMS_OUTPUT.PUT_LINE('Inner Variable: ' || inner_variable);

      -- Access outer block variable
      DBMS_OUTPUT.PUT_LINE('Accessing Outer Variable: ' || outer_variable);
   END;

   -- Access only outer block variable here
   DBMS_OUTPUT.PUT_LINE('Accessing Outer Variable Outside Inner Block: ' || outer_variable);
END;
```

In this example, we have an outer block and an inner block. The outer block contains its own set of variables and statements, while the inner block is enclosed within the outer block and has its own set of variables and statements.  

The code begins with the outer block, where we define an outer variable and perform some statements specific to the outer block. Then, we enter the inner block, which is declared using the DECLARE keyword. Inside the inner block, we define an inner variable and execute statements specific to the inner block.  

Within the inner block, we can access both inner and outer variables. However, outside the inner block, we can only access the outer variable.  

By using nested blocks, you can control the scope and visibility of variables, encapsulate logic, and organize your code into logical sections.  


---
#### **Operations inside the PL/SQL Block Code**
---
You can do DDL,DML,DCL inside the PL/SQL Code. But you have to remember below things:  
- DDL and DCL will only be done by prefixing with "EXECUTE IMMEDIATE"
- DML can be run directly

Below is a sample:  
``` sql
DECLARE
   v_employee_id NUMBER := 100;
BEGIN
   -- DDL operation (CREATE TABLE)
   EXECUTE IMMEDIATE 'CREATE TABLE employees (
      employee_id   NUMBER,
      first_name    VARCHAR2(50),
      last_name     VARCHAR2(50)
   )';

   -- DML operation (INSERT)
   INSERT INTO employees (employee_id, first_name, last_name)
   VALUES (v_employee_id, 'John', 'Doe');

   -- DML operation (UPDATE)
   UPDATE employees
   SET last_name = 'Smith'
   WHERE employee_id = v_employee_id;

   -- DCL operation (GRANT)
   EXECUTE IMMEDIATE 'GRANT SELECT ON employees TO hr';

   -- DCL operation (REVOKE)
   EXECUTE IMMEDIATE 'REVOKE SELECT ON employees FROM hr';
END;
```

---
### **ORACLE BUILT IN ATTRIBUTES**
---

**IMPLICIT CURSOR**  
In Oracle PL/SQL, an implicit cursor is a type of cursor that is automatically created and managed by the system for certain types of SQL statements. Implicit cursors are used implicitly, meaning they are automatically associated with a specific SQL statement without requiring explicit declaration or control by the programmer.

Implicit cursors are primarily used for handling single-row queries or single-row DML statements. They are convenient for simple operations where you don't need to handle multiple rows or explicitly manage the cursor's lifecycle.

When you execute a SQL statement that returns a single row or a single row affected by a DML statement, Oracle automatically creates an implicit cursor for that statement. The implicit cursor allows you to fetch the result or retrieve the number of rows affected without explicitly declaring and opening a cursor.

In Oracle PL/SQL, **SQL%ROWCOUNT** is a built-in attribute that returns the number of rows affected by the most recent SQL statement. It is used to retrieve the count of rows returned by a SELECT statement or the count of rows affected by INSERT, UPDATE, or DELETE statements.  
Here's an example to illustrate the usage of SQL%ROWCOUNT:

```sql
DECLARE
   v_count NUMBER;
BEGIN
   -- Perform an UPDATE statement
   UPDATE employees SET salary = salary * 1.1 WHERE department_id = 10;
   
   -- Get the count of affected rows
   v_count := SQL%ROWCOUNT;
   
   -- Display the count
   DBMS_OUTPUT.PUT_LINE('Number of rows updated: ' || v_count);
END;
```

In this example, after executing the UPDATE statement, the SQL%ROWCOUNT attribute is used to retrieve the count of rows affected by the UPDATE statement. The count is then stored in the variable v_count and displayed using DBMS_OUTPUT.PUT_LINE.  

Similarly, there are other built-in attributes related to SQL statements that provide useful information. Some of them are:  

**SQL%FOUND**: Returns a Boolean value indicating whether the most recent SQL statement affected any rows.  
**SQL%NOTFOUND**: Returns a Boolean value indicating whether the most recent SQL statement did not affect any rows.  
**SQL%ISOPEN**: Returns a Boolean value indicating whether a cursor is open.  
SQL%FOUND, SQL%NOTFOUND, and SQL%ISOPEN are typically used in conjunction with explicit cursors to handle the cursor's state and the result set.  

These attributes allow you to access information about the execution and outcome of SQL statements within your PL/SQL code, providing valuable feedback and control for further processing.

---
### **Using Explicit CURSORS** (Very Important Topic)
---
An explicit cursor is a cursor that you declare, open, fetch data from, and close explicitly within a PL/SQL block. It gives you more control over the cursor operations compared to implicit cursors.

Here's a step-by-step explanation of how an explicit cursor works:
* Declaration
* Opening the Cursor
* Fetching Data
* Closing the Cursor

```sql 
---EXPLICIT CURSOR METHOD:1---
declare
    --this is how you declare a cursor
    cursor c_stud_grade_a is
    select id, name from student
    where grade ='A'
    order by name;
    
    --define 2 variables to save the values of ID and Name
    v_id student.id%TYPE;
    v_name student.name%TYPE;
    
begin
    --cursor goes here
    open c_stud_grade_a;
        loop
            fetch c_stud_grade_a into v_id, v_name;
            exit when c_stud_grade_a%notfound;
            DBMS_OUTPUT.PUT_LINE(v_id||' '||v_name); --the exit should be before the output
        end loop;
    close c_stud_grade_a;
    
end;

---EXPLICIT CURSOR METHOD 2---

DECLARE
    --this is how you declare a cursor
    CURSOR c_stud_grade_a IS
    SELECT ID, NAME FROM student
    WHERE grade ='A'
    ORDER BY NAME;
    
    --define ROWTYPE variable, so we do not have to define 2 variables
    v_stud_rec student%ROWTYPE;
    
BEGIN
    --cursor goes here
    OPEN c_stud_grade_a;
        LOOP
            FETCH c_stud_grade_a INTO v_stud_rec.id, v_stud_rec.name;
            EXIT WHEN c_stud_grade_a%notfound;
            dbms_output.put_line(v_stud_rec.id||' '||v_stud_rec.name); --the exit should be before the output
        END LOOP;
    CLOSE c_stud_grade_a;
    
END;

---EXPLICIT CURSOR METHOD 3---

DECLARE
    --this is how you declare a cursor
    CURSOR c_stud_grade_a IS
    SELECT ID, NAME FROM student
    WHERE grade ='A'
    ORDER BY NAME;
    
    --define CURSOR RECORD variable, so we do not have to define 2 variables
    v_stud_rec c_stud_grade_a%ROWTYPE;
    
BEGIN
    --cursor goes here
    OPEN c_stud_grade_a;
        LOOP
            FETCH c_stud_grade_a INTO v_stud_rec;
            EXIT WHEN c_stud_grade_a%notfound;
            dbms_output.put_line(v_stud_rec.id||' '||v_stud_rec.name); --the exit should be before the output
        END LOOP;
    CLOSE c_stud_grade_a;
    
END;
```
NOTE : In Method-3, note that we need to insert into 2 distinct variables, as we used ROWTYPE as CURSOR RECORD itself, this is best one is propose. 

**To update records in a table using Cursor:**
```sql
---Updating data using EXPLICIT Cursor---

DECLARE
    --this is how you declare a cursor
    CURSOR c_stud_grade_a IS
    SELECT ID, NAME FROM student
    WHERE grade ='A'
    ORDER BY NAME;
    
    --define CURSOR RECORD variable, so we do not have to define 2 variables
    v_stud_rec c_stud_grade_a%ROWTYPE;
    
BEGIN
    --cursor goes here
    OPEN c_stud_grade_a;
        LOOP
            FETCH c_stud_grade_a INTO v_stud_rec;
            EXIT WHEN c_stud_grade_a%notfound;
            --Do whatever you want to do here, print/update etc..
            update student
            set grade='A+'
            where id=v_stud_rec.id;
        END LOOP;
    CLOSE c_stud_grade_a;
    
END;
```
**FOR loop cursor** : If you want to use **FOR** loop cursor, then you *need not* to Open or Close the Cursor, code below:
```sql
DECLARE
    CURSOR c_top_graders IS
    SELECT ID, NAME FROM student 
    WHERE grade = 'A'
    ORDER BY NAME;

BEGIN
    FOR stud IN c_top_graders
        LOOP
            dbms_output.put_line(stud.id ||' '|| stud.name);
        END LOOP;
END;
```

**Cursor with Parameters**: You can pass parameters to Cursors:

```sql
DECLARE
    --Here v_grade is passed as an input parameter to the cursor
    CURSOR c_top_graders(v_grade varchar2) IS
    SELECT ID, NAME FROM student 
    where grade=v_grade
    ORDER BY NAME;
    
BEGIN
    dbms_output.put_line('----A Graders----');
    FOR stud IN c_top_graders('A')
        LOOP
            dbms_output.put_line(stud.id ||' '|| stud.name);
        END LOOP;

    dbms_output.put_line('----B Graders----');
    FOR stud IN c_top_graders('B')
        LOOP
            dbms_output.put_line(stud.id ||' '|| stud.name);
        END LOOP;
    
    dbms_output.put_line('----C Graders----');
    FOR stud IN c_top_graders('C')
        LOOP
            dbms_output.put_line(stud.id ||' '|| stud.name);
        END LOOP;    
END;
```

**FOR UPDATE clause and CURRENT OF clause**: The **FOR UPDATE** clause is used in SQL queries within PL/SQL to lock rows in a table for the purpose of performing updates. When a cursor is opened with the FOR UPDATE clause, it allows the PL/SQL block to lock the selected rows, preventing other transactions from modifying them until the current transaction is committed or rolled back.

The **CURRENT OF** clause is used in the context of a cursor to reference the current row being processed within a loop. It allows you to perform actions or updates specifically on the current row of the cursor.

Below is the code:
```sql
DECLARE
    CURSOR c_grade_change IS
    SELECT ID, NAME 
    FROM student 
    WHERE grade = 'A'
    FOR UPDATE;

BEGIN
    FOR stud IN c_grade_change
        LOOP 
            UPDATE student 
            SET grade = 'A+'
            WHERE CURRENT OF c_grade_change;
        END LOOP;
        --COMMIT;
END;
```

---
### **Exception Handling** (Very Important Topic)
---
Predefined exceptions are a set of standard exceptions that are predefined in Oracle PL/SQL. These exceptions provide a way to handle common error conditions that can occur during the execution of a PL/SQL block. Some of the commonly used predefined exceptions include:

* NO_DATA_FOUND: Raised when a SELECT INTO statement returns no rows.
* TOO_MANY_ROWS: Raised when a SELECT INTO statement returns more than one row.
* DUP_VAL_ON_INDEX: Raised when a unique constraint or index violation occurs during an INSERT or UPDATE statement.
* INVALID_NUMBER: Raised when a conversion of a character string to a number fails.
* ZERO_DIVIDE: Raised when a division or modulus operation is attempted with a divisor of zero.
* PROGRAM_ERROR: Raised when an unhandled exception occurs within a PL/SQL block.
* TIMEOUT_ON_RESOURCE: Raised when a timeout occurs while waiting for a resource, such as a lock.
* VALUE_ERROR: Raised when an assignment or conversion of a value fails due to an invalid data type or value.

These predefined exceptions can be caught and handled using exception handling blocks in PL/SQL. By using predefined exceptions, you can write code to handle specific error conditions and take appropriate actions based on the type of exception raised.

```sql
--EXCEPTION Handling
DECLARE
    v_age NUMBER;
BEGIN
    SELECT age INTO v_age
    FROM student
    WHERE name = 'Siddharth';

    dbms_output.put_line('Age is ' || v_age);
EXCEPTION
    WHEN no_data_found THEN
        dbms_output.put_line('No data was found');
    WHEN OTHERS THEN
        dbms_output.put_line('Other Errors');
END;
```
<br>

---
### **Procedures** (Very Important Topic)
---

Modularizing Development with PL/SQL Blocks. PLSQL is a block-structured language. The PLSQL
code block helps modularize code by using:  
- Anonymous blocks
- Procedures and functions
- Packages
- Database triggers

The benefits of using modular program constructs are:  
- Easy maintenance
- Improved data security and integrity
- Improved performance
- Improved code clarity

**What Are PL/SQL Subprograms?**  
A PL/SQL subprogram is a named PL/SQL block that can be called with a set of parameters.  
* You can declare and define a subprogram within either a PL/SQL block or another subprogram.
* A subprogram consists of a specification and a body.
* A subprogram can be a procedure or a function.
* Typically, you use a procedure to perform an action and a function to compute and return a value.

**What Are Procedures?**
* Are a type of subprogram that perform an action
* Can be stored in the database as a schema object
* Promote reusability and maintainability

```sql
--Creating a Procedure
CREATE OR REPLACE PROCEDURE UPDATE_STUD_AGE (p_stud_id IN NUMBER)
AS
BEGIN
    UPDATE student
    SET    age = 20
    WHERE  id = p_stud_id;

    COMMIT;
EXCEPTION
  WHEN OTHERS THEN
             dbms_output.Put_line(SQLCODE);

             dbms_output.Put_line(SQLERRM);
END; 

--Execute/Running a Procedure
EXECUTE UPDATE_STUD_AGE(1);
```

**Calling the Procedure**  
To call procedure there are 2 options:  
1. Calling standalone 
   ```sql
   EXECUTE UPDATE_STUD_AGE(1);
   ```
2. Calling in PLSQL BEGIN-END block:
   ```sql
   BEGIN
   UPDATE_STUD_AGE(1);
   END;
   ```
3. Calling with Parameters:  
   1. Positional, the position matters here, same as how it is defined in the procedure
    ```sql
   EXECUTE UPDATE_STUD_AGE(1);
    ```
    2. Named, implemented with the symbol =>
    ```sql
   EXECUTE UPDATE_STUD_AGE(p_stud_id=>1);
    ```
    3. You can also use the combination of Positional and Named parameters while calling the procedure.  

Note : Try to used Named, as it is more professional.
<br>

---
### **Function**
---
  
In Oracle, a function is a named PL/SQL block that performs a specific task and returns a value. It can accept input parameters, perform computations, and return a single value as the result. Functions can be used in SQL statements or called from other PL/SQL blocks.

The key differences between a function and a procedure are:

**Return Value**: A function must return a single value, which can be of any valid data type in Oracle, such as a number, string, date, or even a complex data structure. On the other hand, a procedure does not return a value. It may have parameters for input or output, but it does not have a return value.

**Usage**: Functions are primarily used to perform computations and return a result. They can be called from SQL statements, used in expressions, or assigned to variables. Procedures, on the other hand, are used to perform a series of actions or operations. They can have input and output parameters but do not return a value directly.

**Integration with SQL**: Functions can be used directly in SQL statements, such as SELECT statements, WHERE clauses, or expressions. They are often used to encapsulate complex calculations or transformations within SQL queries. Procedures, on the other hand, cannot be used directly in SQL statements. They are called explicitly from PL/SQL blocks or other stored program units.

Here's an example of a simple function in Oracle:

```sql
--Function Creation
CREATE OR REPLACE FUNCTION get_date
RETURN DATE --this is important
AS 
    v_date DATE;
BEGIN
    SELECT sysdate INTO v_date
    FROM dual;
    
    RETURN v_date; --this is important
END;

--Fetch the value from the function
DECLARE
    v_get_result DATE;
BEGIN
    v_get_result := get_date(); --this is assigning the value from function to this variable
    dbms_output.put_line('Current DateTime is '||v_get_result);
END;

--You can also fetch the value of a function in a SELECT statement:
select get_date() from dual;
--21.06.2023 12.56.28
```

On the other hand, a procedure would perform a series of actions or operations without returning a value.

| Aspect              | Function                                    | Procedure                                     |
| ------------------- | ------------------------------------------- | --------------------------------------------- |
| Return Value        | Must return a single value                  | Does not return a value                       |
| Return Clause       | Must contain RETURN clause in the Header    | Do not contain RETURN clause in the Header    |
| Usage               | Perform computations and return a result    | Perform a series of actions or operations     |
| SQL Integration     | Can be used directly in SQL statements      | Cannot be used directly in SQL statements     |
| Input Parameters    | Can have input parameters                   | Can have input parameters                     |
| Output Parameters   | Can have output parameters                  | Can have output parameters                    |
| Parameter Direction | Can have IN, OUT, or IN OUT parameter types | Can have IN, OUT, or IN OUT parameter types   |
| Call Convention     | Called from SQL statements or PL/SQL blocks | Called explicitly from PL/SQL blocks/programs |

Points to Note : 
- A procedure with one single OUT parameter should rather be written as Function.
- A function cannot have a DML statement in it, else the function execution will fail.
- You can use functions in SELECT, WHERE and ORDER BY expressions. eg:
```sql
select rectangle_area(4,5) from dual;
```
<br>

---
### **Packages** (Very Important)
---
A package in Oracle PL/SQL is a schema object that groups related procedures, functions, variables, and other program objects together. It provides a way to organize and encapsulate code in a modular and reusable manner.  
A package consists of two parts:
- **Specification** 
- **Body**.  
 
The specification defines the interface of the package, while the body contains the implementation.  
Benefits of Using Packages:

- **Modularity and Organization (Easier Maintenance)**: Packages allow you to organize related code into logical units, making it easier to manage and maintain your codebase.
- **Encapsulation (Hiding information)**: Packages provide encapsulation, hiding the implementation details and exposing only the necessary interfaces to other program units.
- **Easier Application Design**: Coding and compiling the Specification and Body separately.
- **Code Reusability**: Packages enable code reuse as they can be referenced and used by multiple program units.
- **Global Data Sharing**: Variables and cursors defined in a package can be shared across different procedures and functions within the package.
- **Privileges and Security**: Packages allow you to control access to code by granting privileges at the package level.

**Creating and Using Packages:**
To create a package, you need to define its specification and body. Here's an example that demonstrates the creation and usage of a simple package:

```sql
-- Package Specification
CREATE OR REPLACE PACKAGE your_package_name IS
  PROCEDURE greet;
  FUNCTION add_numbers(a NUMBER, b NUMBER) RETURN NUMBER;
END your_package_name;

-- Package Body
CREATE OR REPLACE PACKAGE BODY your_package_name IS
  PROCEDURE greet IS
  BEGIN
    dbms_output.put_line('Hello, World!');
  END greet;

  FUNCTION add_numbers(a NUMBER, b NUMBER) RETURN NUMBER IS
  BEGIN
    RETURN a + b;
  END add_numbers;
END your_package_name;
```
In the above example, the package your_package_name is created with a specification and body. The specification declares two program objects: a procedure greet and a function add_numbers.

To use the package, you can call its procedures and functions as follows:
```sql
-- Calling a Procedure
BEGIN
  your_package_name.greet;
END;

-- Calling a Function
DECLARE
  result NUMBER;
BEGIN
  result := your_package_name.add_numbers(10, 5);
  dbms_output.put_line('Result: ' || result);
END;
```

Notes:
- Package specification do not need BEGIN
- When calling a procedure or function from a package, you need to use the package name followed by the procedure or function name. Package objects can also have parameters, exceptions, and other features to enhance their functionality.  
One more Example below:
```sql
--This is how you create a package

--Package specification 
CREATE OR REPLACE PACKAGE sid_package_1 AS
    FUNCTION greetings RETURN VARCHAR2;

    PROCEDURE area_of_square (
        side NUMBER
    );
END sid_package_1;

--Package body
CREATE OR REPLACE PACKAGE BODY sid_package_1 AS

    FUNCTION greetings RETURN VARCHAR2 AS
    BEGIN
        RETURN 'Hello this is my first Package';
    END greetings;

    PROCEDURE area_of_square (
        side NUMBER
    ) AS
    BEGIN
        dbms_output.put_line('The area is ' || side * side);
    END area_of_square;
END sid_package_1;

--Calling a Procedure from the package
BEGIN
   sid_package_1.area_of_square(6);
END;

--Calling a Function from the package
DECLARE
    v_out_string VARCHAR2(100);
BEGIN
    v_out_string := sid_package_1.greetings();
    dbms_output.put_line('The message is ' || v_out_string);
END;
```

---
### **Working with Composite DataTypes**
---
**RECORDS :** In Oracle PL/SQL, a RECORD is a composite data type that allows you to define a structure to hold multiple related variables. It is similar to a structure or a record in other programming languages.  
A RECORD in PL/SQL can be defined as a user-defined data type that consists of multiple fields or attributes, each with its own data type. You can use the RECORD data type to group related data elements together.  
Here's an example of how you can define and use a RECORD in PL/SQL:
```sql
DECLARE
  TYPE EmployeeRec IS RECORD (
    emp_id NUMBER,
    emp_name VARCHAR2(100),
    emp_salary NUMBER
  );

  emp_data EmployeeRec;

BEGIN
  emp_data.emp_id := 1;
  emp_data.emp_name := 'John Doe';
  emp_data.emp_salary := 5000;

  -- Accessing record fields
  dbms_output.put_line('Employee ID: ' || emp_data.emp_id);
  dbms_output.put_line('Employee Name: ' || emp_data.emp_name);
  dbms_output.put_line('Employee Salary: ' || emp_data.emp_salary);
END;
```
In this example, we define a RECORD type called EmployeeRec with three fields: emp_id, emp_name, and emp_salary. We then declare a variable emp_data of type EmployeeRec and assign values to its fields. Finally, we can access and manipulate the individual fields of the record as needed.  
The practical usage of RECORDs in PL/SQL is to group related data together and pass them as parameters or return values from procedures and functions. It allows you to encapsulate multiple variables into a single unit, making your code more organized, modular, and easier to maintain. RECORDs can be particularly useful when dealing with complex data structures or when working with cursor variables, collections, or bulk operations.

**%ROWTYPE:** In Oracle PL/SQL, the %ROWTYPE attribute is used to define a record type that represents a row in a table or a cursor. It allows you to declare variables that have the same data structure as the columns of a specific table or the result set of a cursor.  

When you use %ROWTYPE, the PL/SQL compiler automatically creates a record type based on the structure of the specified table or cursor. This means that the record variable will have the same data type and structure as the columns of the table or the columns selected in the cursor's query.  

The %ROWTYPE attribute simplifies the process of declaring variables to hold the data retrieved from a table or a cursor. It eliminates the need to manually declare individual variables for each column, as the record variable automatically encompasses all columns in the table or cursor.

Here's an example of how %ROWTYPE is used:  
```sql
DECLARE
  -- Declare a record variable based on the structure of the 'employees' table
  emp_rec employees%ROWTYPE;
BEGIN
  -- Assign values to the record variable
  emp_rec.employee_id := 101;
  emp_rec.first_name := 'John';
  emp_rec.last_name := 'Doe';

  -- Insert the record into the 'employees' table
  INSERT INTO employees VALUES emp_rec;
  
  -- Query the record variable
  SELECT * INTO emp_rec FROM employees WHERE employee_id = 101;

  -- Display the record variable values
  DBMS_OUTPUT.PUT_LINE(emp_rec.first_name || ' ' || emp_rec.last_name);
END;
```
In this example, the emp_rec record variable is declared using the %ROWTYPE attribute, which automatically creates a record type based on the structure of the employees table. This allows us to conveniently assign values to the record and manipulate it as if it were a row in the table.

**NESTED PL/SQL RECORDS** : In Oracle PL/SQL, nested records are records that are defined within the scope of another record or data structure. They provide a way to create hierarchical or nested structures to represent complex data relationships.  

Nested records are useful when you need to represent structured data that has multiple levels of hierarchy. For example, you might have a record type that represents an employee, and within that employee record, you have another record type that represents the employee's address. This nested record structure allows you to encapsulate related data together and access it in a hierarchical manner.  

Here's an example of how nested PL/SQL records can be used:
```sql
DECLARE
  -- Define a nested record type for the employee address
  TYPE address_type IS RECORD (
    street VARCHAR2(100),
    city VARCHAR2(100),
    state VARCHAR2(100),
    zip_code VARCHAR2(10)
  );

  -- Define a record type for the employee
  TYPE employee_type IS RECORD (
    employee_id NUMBER,
    first_name VARCHAR2(100),
    last_name VARCHAR2(100),
    emp_address address_type  -- Nested record type
  );

  -- Declare a variable of the employee record type
  emp_rec employee_type;
BEGIN
  -- Assign values to the employee record and its nested address record
  emp_rec.employee_id := 101;
  emp_rec.first_name := 'John';
  emp_rec.last_name := 'Doe';
  emp_rec.emp_address.street := '123 Main St';
  emp_rec.emp_address.city := 'New York';
  emp_rec.emp_address.state := 'NY';
  emp_rec.emp_address.zip_code := '10001';

  -- Display the employee information
  DBMS_OUTPUT.PUT_LINE('Employee ID: ' || emp_rec.employee_id);
  DBMS_OUTPUT.PUT_LINE('Name: ' || emp_rec.first_name || ' ' || emp_rec.last_name);
  DBMS_OUTPUT.PUT_LINE('Address: ' || emp_rec.emp_address.street || ', ' ||
                       emp_rec.emp_address.city || ', ' || emp_rec.emp_address.state ||
                       ' ' || emp_rec.emp_address.zip_code);
END;
```
In this example, we define a nested record type address_type for the employee address, and then define the main employee_type record type that includes the nested address_type record. We declare a variable emp_rec of the employee_type record type and assign values to its fields, including the nested emp_address record.

By using nested records, you can create more structured and organized data models that reflect the relationships between different entities or components of your data.

**INDEX BY tables (Associative arrays)**:In Oracle PL/SQL, an INDEX BY table, also known as an associative array or a PL/SQL table, is a collection type that allows you to store data as key-value pairs. It is similar to an array or a list, but the elements are accessed using an index value (the key) instead of a numeric position.

The syntax for defining an INDEX BY table is as follows:
```sql
TYPE index_by_table_type IS TABLE OF data_type INDEX BY PLS_INTEGER;
```
Here, index_by_table_type is the user-defined type for the INDEX BY table, data_type is the data type of the elements you want to store, and PLS_INTEGER is the index type.

Once you have defined the INDEX BY table type, you can declare variables of that type and manipulate the data using the provided methods and syntax.  
Here's an example of how to use an INDEX BY table:
```sql
DECLARE
  TYPE employee_salary_type IS TABLE OF NUMBER INDEX BY VARCHAR2(100);
  salaries employee_salary_type;
BEGIN
  -- Assign values to the INDEX BY table
  salaries('John') := 5000;
  salaries('Jane') := 6000;
  salaries('Mike') := 4500;

  -- Access and display values
  DBMS_OUTPUT.PUT_LINE('John Salary: ' || salaries('John'));
  DBMS_OUTPUT.PUT_LINE('Jane Salary: ' || salaries('Jane'));
  DBMS_OUTPUT.PUT_LINE('Mike Salary: ' || salaries('Mike'));
END;
```
In this example, we define an INDEX BY table type employee_salary_type with keys of type VARCHAR2(100) and values of type NUMBER. We declare a variable salaries of this type and assign values to it using the key-value syntax. We can then access and display the values using the key.

INDEX BY tables are useful when you want to store data in a non-sequential manner and access it using a meaningful key. They provide flexibility and efficiency in handling collections of data with arbitrary keys and allow for quick lookup and retrieval of values based on the key.

**INDEX by TABLE Methods**:In Oracle, an INDEX BY table (also known as an associative array or PL/SQL table) is a collection type that uses an index value to access elements. INDEX BY tables support several built-in methods that provide various operations and functionalities.   
Some commonly used INDEX BY table methods are:
* COUNT: Returns the number of elements in the INDEX BY table.
* EXISTS: Checks if a given index value exists in the INDEX BY table.
* FIRST: Returns the lowest index value in the INDEX BY table.
* LAST: Returns the highest index value in the INDEX BY table.
* PRIOR: Returns the index value that precedes a given index value.
* NEXT: Returns the index value that follows a given index value.
* DELETE: Removes an element from the INDEX BY table using the specified index value.
* EXTEND: Extends the size of the INDEX BY table to accommodate additional elements.
* TRIM: Reduces the size of the INDEX BY table to the specified number of elements.
* DELETE: Removes all elements from the INDEX BY table.  
  
These methods allow you to perform various operations on INDEX BY tables, such as checking for the existence of an index value, accessing the first or last index value, navigating through the index values, deleting specific elements, and resizing the INDEX BY table.

You can use these methods to manipulate and manage the elements in an INDEX BY table efficiently. The specific syntax and usage of these methods may vary depending on your PL/SQL code and requirements.
Below are some examples in the code:
```sql
DECLARE
   TYPE EmployeeList IS TABLE OF VARCHAR2(100) INDEX BY PLS_INTEGER;
   empList EmployeeList;
   empCount INTEGER;
   empIndex INTEGER;
   empName VARCHAR2(100);
BEGIN
   -- Adding elements to the INDEX BY table
   empList(1) := 'John';
   empList(2) := 'Alice';
   empList(3) := 'Bob';
   empList(4) := 'Jane';

   -- Count the number of elements in the INDEX BY table
   empCount := empList.COUNT;
   DBMS_OUTPUT.PUT_LINE('Number of Employees: ' || empCount);

   -- Check if index value 2 exists in the INDEX BY table
   IF empList.EXISTS(2) THEN
      DBMS_OUTPUT.PUT_LINE('Employee at index 2 exists');
   ELSE
      DBMS_OUTPUT.PUT_LINE('Employee at index 2 does not exist');
   END IF;

   -- Get the first index value in the INDEX BY table
   empIndex := empList.FIRST;
   DBMS_OUTPUT.PUT_LINE('First Index: ' || empIndex);

   -- Get the last index value in the INDEX BY table
   empIndex := empList.LAST;
   DBMS_OUTPUT.PUT_LINE('Last Index: ' || empIndex);

   -- Get the index value that precedes index 3
   empIndex := empList.PRIOR(3);
   DBMS_OUTPUT.PUT_LINE('Prior Index of 3: ' || empIndex);

   -- Get the index value that follows index 2
   empIndex := empList.NEXT(2);
   DBMS_OUTPUT.PUT_LINE('Next Index of 2: ' || empIndex);

   -- Delete an element from the INDEX BY table
   empList.DELETE(4);

   -- Trim the INDEX BY table to a specific size
   empList.TRIM(2);

   -- Delete all elements from the INDEX BY table
   empList.DELETE;

   -- Check the number of elements after deletion
   empCount := empList.COUNT;
   DBMS_OUTPUT.PUT_LINE('Number of Employees after deletion: ' || empCount);
END;
```

**Index by Table of Records**: An "Index by Table of Records" is a data structure in Oracle PL/SQL that allows you to store and access records using an index.  
Think of it like a table where each row is a record with different fields (columns). The index allows you to quickly locate a specific record in the table.

For example, imagine you have a collection of employee records, and each record contains information like employee ID, first name, and last name. With an "Index by Table of Records," you can store these employee records in an associative array, where you can access them using an index value.  
You can assign values to the fields of each record and use the index to identify and retrieve the desired record. This allows you to organize and work with structured data in a convenient and efficient way.

By using an "Index by Table of Records," you can store multiple records, access them using an index, and perform operations on the data, such as searching, modifying, or displaying the records as needed.
```sql
DECLARE
   TYPE EmployeeRecord IS RECORD (
      employee_id   NUMBER,
      first_name    VARCHAR2(100),
      last_name     VARCHAR2(100)
   );

   TYPE EmployeeTable IS TABLE OF EmployeeRecord INDEX BY BINARY_INTEGER;

   employees   EmployeeTable;

   -- Declare a variable of %ROWTYPE based on the table structure
   emp_rec     employees%ROWTYPE;
BEGIN
   -- Fetch employee data into the %ROWTYPE variable
   SELECT * INTO emp_rec FROM employees WHERE employee_id = 1001;

   -- Assign the %ROWTYPE variable to the index-by table
   employees(1) := emp_rec;

   -- Accessing and printing employee records
   DBMS_OUTPUT.PUT_LINE('Employee 1: ' || employees(1).first_name || ' ' || employees(1).last_name);

   -- Modify the last name of the employee in the index-by table
   employees(1).last_name := 'Smith';

   DBMS_OUTPUT.PUT_LINE('Modified Last Name of Employee 1: ' || employees(1).last_name);
END;
```

**NESTED Table**:A nested table in Oracle PL/SQL is a collection type that allows you to store multiple instances of a data type within a single database column. It is similar to an array or a list in other programming languages.  
Here's an example to illustrate the concept of a nested table:

Let's say we have a table called "Order" that stores order information. Each order can have multiple items associated with it. Instead of creating a separate table for items, we can use a nested table to store multiple items within a single row of the "Order" table.
```sql
CREATE TYPE ItemRecord AS OBJECT (
   item_id    NUMBER,
   item_name  VARCHAR2(100),
   quantity   NUMBER
);
/

CREATE TYPE ItemTable AS TABLE OF ItemRecord;
/

CREATE TABLE Orders (
   order_id     NUMBER,
   order_date   DATE,
   items        ItemTable
);
/

DECLARE
   v_items ItemTable := ItemTable(); -- Initialize the nested table
   
   -- Populate the nested table
   v_item1 ItemRecord := ItemRecord(1, 'Product A', 5);
   v_item2 ItemRecord := ItemRecord(2, 'Product B', 10);
BEGIN
   v_items.EXTEND(2); -- Extend the nested table by 2 elements
   v_items(1) := v_item1;
   v_items(2) := v_item2;
   
   -- Insert data into the Orders table
   INSERT INTO Orders (order_id, order_date, items)
   VALUES (1, SYSDATE, v_items);
   
   -- Retrieve and display data from the Orders table
   FOR rec IN (SELECT * FROM Orders)
   LOOP
      DBMS_OUTPUT.PUT_LINE('Order ID: ' || rec.order_id);
      DBMS_OUTPUT.PUT_LINE('Order Date: ' || rec.order_date);
      
      -- Access and display items from the nested table
      FOR i IN 1..rec.items.COUNT
      LOOP
         DBMS_OUTPUT.PUT_LINE('Item ID: ' || rec.items(i).item_id);
         DBMS_OUTPUT.PUT_LINE('Item Name: ' || rec.items(i).item_name);
         DBMS_OUTPUT.PUT_LINE('Quantity: ' || rec.items(i).quantity);
      END LOOP;
   END LOOP;
END;
/
```
In this example, we define a nested table type called ItemTable that stores instances of ItemRecord. The ItemRecord is an object type with attributes for item_id, item_name, and quantity.  

We create a table called "Orders" with columns for order_id, order_date, and items of type ItemTable. The items column will hold the nested table data.  

In the PL/SQL block, we initialize the v_items nested table and populate it with two items. We extend the nested table to accommodate the items and assign values to each element.

We then insert the order data, including the nested table, into the "Orders" table.

Finally, we retrieve and display the data from the "Orders" table, including iterating over the nested table elements and printing the item details.

This example demonstrates how a nested table can be used to store and manage multiple instances of a data type within a single row of a table. It provides a flexible and efficient way to handle collections of data in Oracle PL/SQL.

**VARRAY**:VARRAY, short for Variable-Size Array, is another collection type in Oracle PL/SQL. It allows you to store a fixed number of elements of the same data type within a single database column. Unlike nested tables, VARRAYs have a defined size that cannot be changed once created.

Here's an example to illustrate the concept of a VARRAY:
```sql
CREATE TYPE PhoneNumbers AS VARRAY(3) OF VARCHAR2(15);
/

CREATE TABLE Employees (
   employee_id   NUMBER,
   employee_name VARCHAR2(100),
   phone_numbers PhoneNumbers
);
/

DECLARE
   v_phone_numbers PhoneNumbers := PhoneNumbers('123-456-7890', '987-654-3210');
BEGIN
   -- Insert data into the Employees table
   INSERT INTO Employees (employee_id, employee_name, phone_numbers)
   VALUES (1, 'John Doe', v_phone_numbers);
   
   -- Retrieve and display data from the Employees table
   FOR rec IN (SELECT * FROM Employees)
   LOOP
      DBMS_OUTPUT.PUT_LINE('Employee ID: ' || rec.employee_id);
      DBMS_OUTPUT.PUT_LINE('Employee Name: ' || rec.employee_name);
      
      -- Access and display phone numbers from the VARRAY
      FOR i IN 1..rec.phone_numbers.COUNT
      LOOP
         DBMS_OUTPUT.PUT_LINE('Phone Number ' || i || ': ' || rec.phone_numbers(i));
      END LOOP;
   END LOOP;
END;
/
```
In this example, we define a VARRAY type called PhoneNumbers with a maximum size of 3 elements, each of type VARCHAR2(15). This means that each employee can have up to three phone numbers.

We create a table called "Employees" with columns for employee_id, employee_name, and phone_numbers of type PhoneNumbers.

In the PL/SQL block, we initialize the v_phone_numbers VARRAY with two phone numbers. We then insert the employee data, including the VARRAY, into the "Employees" table.

Finally, we retrieve and display the data from the "Employees" table, including iterating over the VARRAY elements and printing the phone numbers.

VARRAYs are useful when you need to store a fixed number of related elements in a single column. They provide a way to represent and manipulate structured data within a database table.

---
### **Points to Ponder** 
---

**Is sql a programming language ?**  
SQL (Structured Query Language) is a programming language specifically designed for managing and manipulating relational databases. It is primarily used for tasks such as querying and retrieving data, inserting, updating, and deleting data, creating and modifying database structures (tables, views, indexes, etc.), and defining access control and security measures.

While SQL is considered a programming language, it is important to note that it is primarily focused on interacting with databases rather than general-purpose programming tasks. SQL provides a standardized syntax and set of commands that can be used across different database management systems (such as Oracle, MySQL, SQL Server, etc.) to perform database-related operations.

SQL is a declarative language, meaning that you specify what you want to retrieve or modify from the database, and the database management system figures out how to execute the task efficiently. It is not a procedural or object-oriented language like Java or Python, which are used for general-purpose programming.

In summary, SQL is a programming language specifically designed for working with relational databases, allowing users to manage and manipulate data, as well as define and control database structures.


**SQL Architecture:** SQL (Structured Query Language) is a language used for managing and manipulating relational databases. The architecture of SQL involves multiple components that work together to facilitate the execution of SQL statements and the management of database operations. Here is an overview of the SQL architecture:  
1. SQL Engine: The SQL Engine is the core component of the SQL architecture. It interprets and executes SQL queries and statements. It includes various sub-components like the Parser, Query Optimizer, and Execution Engine.  
• Parser: The Parser checks the syntax and structure of SQL statements, ensuring they adhere to the rules and grammar of the SQL language. It breaks down the SQL statements into a parse tree, which represents the logical structure of the query.  
• Query Optimizer: The Query Optimizer analyzes the parse tree and determines the most efficient way to execute the query. It considers various factors like available indexes, statistics, and cost-based analysis to generate an optimal execution plan.  
• Execution Engine: The Execution Engine takes the optimized execution plan and performs the actual operations to retrieve or modify data. It interacts with the storage engine to access and manipulate the database objects.  

1. Storage Engine: The Storage Engine is responsible for managing the storage and retrieval of data within the database. It handles tasks like data persistence, indexing, transaction management, and concurrency control. The storage engine interacts with the file system or underlying storage technology to read and write data.  
• File System: In many database systems, the storage engine utilizes the file system to store the data files and associated metadata. The file system manages the physical storage and retrieval of database files on disk.  
• Buffer Manager: The Buffer Manager is responsible for caching data pages in memory to improve performance. It manages the transfer of data between disk and memory buffers, reducing the need for frequent disk I/O operations.  
• Index Manager: The Index Manager manages the creation, maintenance, and utilization of indexes. Indexes provide efficient access to data by creating a sorted structure based on certain columns, speeding up data retrieval operations.  
• Transaction Manager: The Transaction Manager ensures the ACID (Atomicity, Consistency, Isolation, Durability) properties of database transactions. It manages concurrent access to data, enforces data integrity rules, and provides mechanisms for committing or rolling back transactions.  
• Concurrency Control: The Concurrency Control component handles concurrent access to the database by multiple users or transactions. It prevents data inconsistencies or conflicts that may arise when multiple transactions are modifying the same data simultaneously.  

1. Data Dictionary: The Data Dictionary, also known as the System Catalog, is a metadata repository that stores information about the database structure, schema, tables, columns, indexes, constraints, and other database objects. It provides the necessary information for the SQL engine to understand and manipulate the database schema.  
   
The SQL architecture may vary slightly between different database management systems (e.g., Oracle, MySQL, SQL Server), but the overall components and their functionalities remain similar. The architecture ensures efficient query execution, data storage, transaction management, and data integrity in SQL-based database systems.


**Pluggable Database:** : Pluggable Database (PDB) is a self-contained database within an Oracle Container Database (CDB). It allows for the consolidation of multiple databases, provides isolation and independence for different applications or business units, and simplifies administrative tasks by managing common resources at the CDB level.

  
**Schema:** In Oracle, a schema is a logical container or namespace that is used to organize and manage database objects, such as tables, views, indexes, procedures, and functions. It acts as a way to group related database objects together.

A schema is associated with a specific user account, and the user can own and have privileges to access and manipulate the objects within that schema. Each user account in Oracle is typically associated with a default schema, and when a user creates database objects without specifying a schema name, they are created within the user's default schema.
  


**Entity Relationship Diagram (ER Diagram):**

An Entity Relationship Diagram (ERD) is a visual representation of the relationships between entities (objects or concepts) within a database. It is a popular tool used in database design to model the structure and organization of data.
In an ERD, entities are represented as rectangles or squares, and the relationships between entities are represented by lines connecting them. Each entity has attributes, which are characteristics or properties associated with the entity.
The main components of an ERD are:  
1. Entities: Entities represent the objects or concepts in the real world that we want to store and manage data about. For example, in a university database, entities could include Student, Course, and Faculty.  
2. Relationships: Relationships describe the associations between entities. They represent how entities interact or relate to each other. Relationships can be one-to-one, one-to-many, or many-to-many, indicating the cardinality or the number of instances of one entity that can be associated with another entity.  
3. Attributes: Attributes are the properties or characteristics of entities. They describe the data we want to capture about the entities. For example, attributes of a Student entity might include student ID, name, and date of birth.  
   
ERDs help in visualizing the structure of a database, identifying the relationships between entities, and ensuring data integrity and consistency. They serve as a communication tool between stakeholders, including database designers, developers, and end-users, helping them understand the database schema and how different entities are connected.  

ERDs can be created using various notations, such as Crow's Foot notation, Chen notation, or UML notation. These notations provide standardized symbols and conventions for representing entities, relationships, and attributes.  

In summary, an Entity Relationship Diagram (ERD) is a visual representation that illustrates the relationships between entities in a database. It helps in designing and understanding the structure of a database and is widely used in the field of database management and design.


---
### OTHERS
---


---
#### **Date functions in Oracle**

---

SYSDATE:
```sql
SELECT SYSDATE AS "Current Date and Time" FROM DUAL;
Output:
Current Date and Time
2023-06-01 12:34:56
```

TO_DATE:
```sql
SELECT TO_DATE('2023-06-01', 'YYYY-MM-DD') AS "Converted Date" FROM DUAL;
Output:
Converted Date
2023-06-01
```

TO_CHAR:
```sql
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS') AS "Formatted Date and Time" FROM DUAL;
Output:
Formatted Date and Time
2023-06-01 12:34:56
```

EXTRACT:
```sql
SELECT EXTRACT(YEAR FROM SYSDATE) AS "Current Year" FROM DUAL;
Output:
Current Year
2023
```

ADD_MONTHS:
```sql
SELECT ADD_MONTHS(SYSDATE, 3) AS "Date After Adding 3 Months" FROM DUAL;
Output:
Date After Adding 3 Months
2023-09-01
```

NEXT_DAY:
```sql
SELECT NEXT_DAY(SYSDATE, 'FRIDAY') AS "Next Friday" FROM DUAL;
Output:
Next Friday
2023-06-02
```

LAST_DAY:
```sql
SELECT LAST_DAY(SYSDATE) AS "Last Day of the Month" FROM DUAL;
Output:
Last Day of the Month
2023-06-30
```

MONTHS_BETWEEN:
```sql
SELECT MONTHS_BETWEEN(TO_DATE('2023-06-01', 'YYYY-MM-DD'), SYSDATE) AS "Months Between Dates" FROM DUAL;
Output:
Months Between Dates
0.967741935
```

TRUNC:
```sql
SELECT TRUNC(SYSDATE, 'MONTH') AS "Truncated to Month" FROM DUAL;
Output:
Truncated to Month
2023-06-01
```

ROUND:
```sql
SELECT ROUND(SYSDATE, 'HH24') AS "Rounded to Hour" FROM DUAL;
Output:
Rounded to Hour
2023-06-01 12:00:00
```

INTERVAL:
```sql
SELECT SYSDATE + INTERVAL '2' HOUR AS "Date After Adding 2 Hours" FROM DUAL;
Output:
Date After Adding 2 Hours
2023-06-01 14:34:56
```

CURRENT_DATE:
```sql
SELECT CURRENT_DATE AS "Current Date" FROM DUAL;
Output:
Current Date
2023-06-01
```

CURRENT_TIMESTAMP:
```sql
SELECT CURRENT_TIMESTAMP AS "Current Date and Time" FROM DUAL;
Output:
Current Date and Time
2023-06-01 12:34:56.789
```

DBTIMEZONE:
```sql
SELECT DBTIMEZONE AS "Database Time Zone" FROM DUAL;
Output:
Database Time Zone
+00:00
```

SESSIONTIMEZONE:
```sql
SELECT SESSIONTIMEZONE AS "Session Time Zone" FROM DUAL;
Output:
Session Time Zone
+00:00

```
---