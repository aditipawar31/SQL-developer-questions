# **Database Developer Interview Questions & Answers**

---

### **Beginner Level**

---

#### **1. Basic SQL**

- **What is SQL, and why is it important?**  
  **SQL** (Structured Query Language) is used to manage and manipulate data in relational databases. It's important because it helps retrieve, update, and organize data efficiently.
  
- **Explain the difference between `DDL` and `DML`.**  
  - **DDL (Data Definition Language):** Defines database structures like tables. Common commands: `CREATE`, `ALTER`, `DROP`.  
  - **DML (Data Manipulation Language):** Manages data inside tables. Common commands: `SELECT`, `INSERT`, `UPDATE`, `DELETE`.
  
- **How do you create a table in SQL? Provide an example.**  
  ```sql
  CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10, 2)
  );
  ```

- **How do you insert data into a table? Provide an example.**  
  ```sql
  INSERT INTO employees (id, name, department, salary)
  VALUES (1, 'John Doe', 'Engineering', 75000);
  ```

- **What is the difference between `DELETE` and `TRUNCATE`?**  
  - **DELETE:** Removes specific rows, but the table structure and records that don’t meet the condition remain intact. It can be rolled back.  
  - **TRUNCATE:** Deletes all rows from a table quickly but cannot be rolled back.

- **How can you retrieve all records from a table?**  
  ```sql
  SELECT * FROM employees;
  ```

- **What is the use of the `WHERE` clause in SQL?**  
  The `WHERE` clause filters records based on a condition. Example:  
  ```sql
  SELECT * FROM employees WHERE department = 'Engineering';
  ```

- **How would you update a record in SQL?**  
  ```sql
  UPDATE employees
  SET salary = 80000
  WHERE id = 1;
  ```

- **Explain the difference between `GROUP BY` and `ORDER BY`.**  
  - **`GROUP BY`:** Groups rows with similar values (useful for aggregate functions).  
  - **`ORDER BY`:** Sorts the result set in ascending or descending order.  
  Example:
  ```sql
  SELECT department, AVG(salary)
  FROM employees
  GROUP BY department
  ORDER BY AVG(salary) DESC;
  ```

- **What are SQL `JOINs`? Explain different types of joins.**  
  **JOINs** combine data from two or more tables based on a related column.  
  - **INNER JOIN:** Returns records with matching values in both tables.  
  - **LEFT JOIN:** Returns all records from the left table and matching records from the right table.  
  - **RIGHT JOIN:** Returns all records from the right table and matching records from the left table.  
  - **FULL OUTER JOIN:** Returns all records when there is a match in either table.  
  Example:
  ```sql
  SELECT employees.name, departments.department
  FROM employees
  INNER JOIN departments ON employees.department_id = departments.id;
  ```

- **How do you filter records using `LIKE`, `BETWEEN`, `IN`?**  
  - **`LIKE`:** Filters records based on patterns.  
    Example:  
    ```sql
    SELECT * FROM employees WHERE name LIKE 'J%';
    ```
  - **`BETWEEN`:** Filters data between two values.  
    Example:  
    ```sql
    SELECT * FROM employees WHERE salary BETWEEN 50000 AND 100000;
    ```
  - **`IN`:** Filters records based on a list of values.  
    Example:  
    ```sql
    SELECT * FROM employees WHERE department IN ('Engineering', 'HR');
    ```

- **What is a `NULL` value in SQL and how do you handle it?**  
  `NULL` represents missing or unknown data. To handle `NULL` values, use `IS NULL` or `IS NOT NULL`.  
  Example:  
  ```sql
  SELECT * FROM employees WHERE salary IS NULL;
  ```

- **How would you find duplicate records in a table?**  
  ```sql
  SELECT name, COUNT(*)
  FROM employees
  GROUP BY name
  HAVING COUNT(*) > 1;
  ```

- **How would you retrieve only unique values from a column?**  
  ```sql
  SELECT DISTINCT department FROM employees;
  ```

- **What are SQL aggregate functions?**  
  Aggregate functions perform calculations on multiple rows of data. Common functions:  
  - `COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()`.  
  Example:  
  ```sql
  SELECT AVG(salary) FROM employees;
  ```

---

#### **2. Database Basics**

- **What is a primary key?**  
  A **primary key** uniquely identifies each record in a table. It cannot be `NULL`.  
  Example:
  ```sql
  CREATE TABLE employees (
    id INT PRIMARY KEY
  );
  ```

- **What is a foreign key?**  
  A **foreign key** is used to link two tables. It references the **primary key** of another table.  
  Example:
  ```sql
  CREATE TABLE orders (
    order_id INT,
    employee_id INT,
    FOREIGN KEY (employee_id) REFERENCES employees(id)
  );
  ```

- **What are constraints in SQL?**  
  **Constraints** enforce rules on data. Common constraints include:  
  - **PRIMARY KEY**, **FOREIGN KEY**, **UNIQUE**, **NOT NULL**, **CHECK**.

- **What is a view in SQL?**  
  A **view** is a virtual table based on a SQL query that can be used like a regular table.  
  Example:
  ```sql
  CREATE VIEW employee_salaries AS
  SELECT name, salary FROM employees;
  ```

- **What are indexes in SQL?**  
  **Indexes** improve query performance by speeding up data retrieval.  
  Example:
  ```sql
  CREATE INDEX idx_name ON employees (name);
  ```

- **Explain normalization.**  
  **Normalization** organizes database tables to reduce redundancy and dependency. It improves efficiency.

- **What are the different normal forms?**  
  - **1NF (First Normal Form):** Eliminates duplicate columns.  
  - **2NF (Second Normal Form):** Removes partial dependencies.  
  - **3NF (Third Normal Form):** Removes transitive dependencies.

- **What is denormalization?**  
  **Denormalization** adds redundant data to improve read performance, often at the cost of write performance.

- **Explain the ACID properties.**  
  - **Atomicity:** All operations in a transaction complete or none do.  
  - **Consistency:** Data must be consistent before and after a transaction.  
  - **Isolation:** Transactions do not interfere with each other.  
  - **Durability:** Changes are saved permanently once a transaction is committed.

- **What is a schema in SQL?**  
  A **schema** is a collection of database objects (tables, views, indexes) within a database.

---

### **Intermediate Level**

---

#### **1. SQL Queries and Optimization**

- **What is a subquery?**  
  A **subquery** is a query nested inside another query.  
  Example:
  ```sql
  SELECT * FROM employees WHERE salary > (SELECT AVG(salary) FROM employees);
  ```

- **How can you optimize SQL queries?**  
  - Use **indexes**.  
  - Avoid `SELECT *` (use specific columns).  
  - Use **JOINs** instead of subqueries where possible.  
  - Filter data with **WHERE** clauses.

- **Explain the difference between `HAVING` and `WHERE`.**  
  - **WHERE** filters records before grouping.  
  - **HAVING** filters records after grouping.  
  Example:
  ```sql
  SELECT department, AVG(salary)
  FROM employees
  GROUP BY department
  HAVING AVG(salary) > 50000;
  ```

- **What are window functions?**  
  **Window functions** perform calculations across a set of table rows.  
  Example using `ROW_NUMBER()`:
  ```sql
  SELECT name, salary, ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC)
  FROM employees;
  ```

- **How do you perform a `UNION`?**  
  A **UNION** combines the result of two queries. It removes duplicate records unless you use `UNION ALL`.  
  Example:
  ```sql
  SELECT name FROM employees
  UNION
  SELECT name FROM contractors;
  ```

- **What is a transaction?**  
  A **transaction** is a sequence of operations that are treated as a single unit. All changes within a transaction are committed together or rolled back.  
  Example:
  ```sql
  BEGIN TRANSACTION;
    UPDATE employees SET salary = salary + 1000 WHERE id = 1;
    DELETE FROM employees WHERE id = 5;
  COMMIT;
  ```

- **What is indexing and how does it improve performance?**  
 

 **Indexing** speeds up the retrieval of records by creating a data structure that allows quick lookups.

---

#### **2. Database Design**

- **What is an ER diagram?**  
  An **ER (Entity-Relationship) diagram** visually represents relationships between tables in a database.

- **How would you design a database for an e-commerce application?**  
  Design separate tables for **customers**, **products**, **orders**, etc. Ensure tables are connected through foreign keys. Example:  
  - Customers have **customer_id**.  
  - Orders have **order_id** and reference **customer_id** from the customers table.

- **What is a star schema?**  
  A **star schema** is used in data warehousing. A central fact table is connected to dimension tables that describe the facts.

- **How would you handle many-to-many relationships?**  
  You would use a **junction table**.  
  Example:  
  ```sql
  CREATE TABLE student_courses (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id)
  );
  ```

---

### **Advanced Level**

---

#### **1. Advanced SQL and Optimization**

- **What is a CTE?**  
  A **CTE (Common Table Expression)** is a temporary result set that you can reference within a `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement.  
  Example:
  ```sql
  WITH employee_cte AS (
    SELECT * FROM employees WHERE salary > 50000
  )
  SELECT * FROM employee_cte;
  ```

- **What is a recursive query?**  
  A **recursive query** is a query that references itself. It's useful for hierarchical data.  
  Example:
  ```sql
  WITH RECURSIVE employee_hierarchy AS (
    SELECT id, manager_id FROM employees WHERE id = 1
    UNION
    SELECT e.id, e.manager_id FROM employees e
    JOIN employee_hierarchy eh ON e.manager_id = eh.id
  )
  SELECT * FROM employee_hierarchy;
  ```

- **Explain the difference between `RANK()`, `DENSE_RANK()`, and `ROW_NUMBER()`.**  
  - **`ROW_NUMBER()`:** Assigns a unique row number starting from 1.  
  - **`RANK()`:** Assigns a rank to each row, but skips numbers if there are ties.  
  - **`DENSE_RANK()`:** Similar to `RANK()`, but doesn’t skip numbers for ties.  
  Example:
  ```sql
  SELECT name, salary, RANK() OVER (ORDER BY salary DESC) AS rank
  FROM employees;
  ```

---
