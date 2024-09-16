
# **Database Developer Interview Questions & Answers**

---

### **Beginner Level**

---

#### **1. Basic SQL**

- **What is SQL, and why is it important?**
  SQL (Structured Query Language) is used to manage and manipulate data in relational databases. It's important because it helps us query, update, and organize data easily.

- **Explain the difference between `DDL` and `DML`.**
  - **DDL (Data Definition Language):** Used to define database structure like tables (e.g., `CREATE`, `ALTER`, `DROP`).
  - **DML (Data Manipulation Language):** Used to manage data within tables (e.g., `SELECT`, `INSERT`, `UPDATE`, `DELETE`).

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
  - `DELETE` removes specific rows based on a condition.
  - `TRUNCATE` removes all rows but keeps the table structure. It's faster than `DELETE`.

- **How can you retrieve all records from a table?**
  ```sql
  SELECT * FROM employees;
  ```

- **What is the use of the `WHERE` clause in SQL?**
  The `WHERE` clause is used to filter records. For example:
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
  - **`GROUP BY`:** Groups rows with the same values.
  - **`ORDER BY`:** Sorts the result set.

  Example:
  ```sql
  SELECT department, AVG(salary)
  FROM employees
  GROUP BY department;
  ```

- **What are SQL `JOINs`? Explain different types of joins.**
  `JOINs` are used to combine data from two or more tables based on a related column.

  Types:
  - **INNER JOIN:** Returns rows with matching values in both tables.
  - **LEFT JOIN:** Returns all rows from the left table and matched rows from the right table.
  - **RIGHT JOIN:** Returns all rows from the right table and matched rows from the left table.
  - **FULL OUTER JOIN:** Returns all rows when there is a match in either table.

- **How do you filter records using `LIKE`, `BETWEEN`, `IN`?**
  - **`LIKE`:** Finds patterns in strings. Example:
    ```sql
    SELECT * FROM employees WHERE name LIKE 'J%';
    ```
  - **`BETWEEN`:** Filters within a range. Example:
    ```sql
    SELECT * FROM employees WHERE salary BETWEEN 50000 AND 100000;
    ```
  - **`IN`:** Filters specific values. Example:
    ```sql
    SELECT * FROM employees WHERE department IN ('Engineering', 'HR');
    ```

- **What is a `NULL` value in SQL and how do you handle it?**
  `NULL` means "no value". Use `IS NULL` or `IS NOT NULL` to handle it:
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
  These perform calculations on multiple rows:
  - `COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()`.
  Example:
  ```sql
  SELECT AVG(salary) FROM employees;
  ```

---

#### **2. Database Basics**

- **What is a primary key?**
  A primary key uniquely identifies each row in a table. It cannot have `NULL` values.
  Example:
  ```sql
  CREATE TABLE employees (
    id INT PRIMARY KEY
  );
  ```

- **What is a foreign key?**
  A foreign key links two tables together, referencing the primary key of another table.
  Example:
  ```sql
  CREATE TABLE orders (
    order_id INT,
    employee_id INT,
    FOREIGN KEY (employee_id) REFERENCES employees(id)
  );
  ```

- **What are constraints in SQL?**
  Constraints enforce rules at the column level, like `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `NOT NULL`, `CHECK`.

- **What is a view in SQL?**
  A view is a saved SQL query that acts like a table. Example:
  ```sql
  CREATE VIEW employee_salaries AS
  SELECT name, salary FROM employees;
  ```

- **What are indexes in SQL?**
  Indexes speed up data retrieval. Example:
  ```sql
  CREATE INDEX idx_name ON employees (name);
  ```

- **Explain normalization.**
  Normalization is organizing data to reduce redundancy. It improves efficiency.

- **What are the different normal forms?**
  - **1NF:** Eliminate duplicate columns.
  - **2NF:** Remove partial dependencies.
  - **3NF:** Remove transitive dependencies.

- **What is denormalization?**
  Denormalization adds redundancy to improve read performance, at the cost of write efficiency.

- **Explain the ACID properties.**
  - **Atomicity:** All operations in a transaction complete or none do.
  - **Consistency:** Data remains consistent before and after the transaction.
  - **Isolation:** Transactions don't affect each other.
  - **Durability:** Changes persist after a transaction is complete.

- **What is a schema in SQL?**
  A schema is a collection of database objects like tables, views, and indexes.

---

### **Intermediate Level**

---

#### **1. SQL Queries and Optimization**

- **What is a subquery?**
  A subquery is a query inside another query. Example:
  ```sql
  SELECT * FROM employees WHERE salary > (SELECT AVG(salary) FROM employees);
  ```

- **How can you optimize SQL queries?**
  - Use indexes.
  - Avoid `SELECT *`.
  - Use `JOIN` instead of subqueries.
  - Use proper filtering with `WHERE`.

- **Explain the difference between `HAVING` and `WHERE`.**
  - `WHERE`: Filters before grouping.
  - `HAVING`: Filters after grouping.

  Example:
  ```sql
  SELECT department, AVG(salary)
  FROM employees
  GROUP BY department
  HAVING AVG(salary) > 50000;
  ```

- **What are window functions?**
  These perform calculations across a set of table rows. Example of `ROW_NUMBER()`:
  ```sql
  SELECT name, salary, ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC)
  FROM employees;
  ```

- **How do you perform a `UNION`?**
  Combines results from two queries. Example:
  ```sql
  SELECT name FROM employees
  UNION
  SELECT name FROM contractors;
  ```

- **What is a transaction?**
  A transaction is a sequence of operations performed as a single unit. Example:
  ```sql
  BEGIN TRANSACTION;
    UPDATE employees SET salary = salary + 1000 WHERE id = 1;
    DELETE FROM employees WHERE id = 5;
  COMMIT;
  ```

- **What is indexing and how does it improve performance?**
  Indexing speeds up search queries by creating a sorted data structure on columns.

---

#### **2. Database Design**

- **What is an ER diagram?**
  An ER (Entity-Relationship) diagram shows the relationship between tables in a database.

- **How would you design a database for an e-commerce application?**
  You’d create tables for customers, products, orders, etc. Ensure each table has a unique identifier and relationships (e.g., `customer_id` in the `orders` table references `customers`).

- **What is a star schema?**
  In a star schema, a central fact table is connected to dimension tables, typically used in data warehousing.

- **How would you handle many-to-many relationships?**
  Use a junction table. Example:
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
  A CTE (Common Table Expression) is a temporary result set that you can reference in a query. Example:
  ```sql
  WITH employee_cte AS (
    SELECT * FROM employees WHERE salary > 50000
  )
  SELECT * FROM employee_cte;
  ```

- **What is a recursive query?**
  A

 recursive query references itself. Example:
  ```sql
  WITH RECURSIVE employee_hierarchy AS (
    SELECT id, manager_id FROM employees WHERE id = 1
    UNION
    SELECT e.id, e.manager_id FROM employees e
    JOIN employee_hierarchy eh ON e.manager_id = eh.id
  )
  SELECT * FROM employee_hierarchy;
  ```

---

This formatting should make it easier for you to study the concepts and examples as notes for interview preparation. You can now copy this into your Word document and use it whenever needed.
