# SQL Cheatsheet

## Table of Contents
1. [Introduction](#introduction)
2. [Data Types](#data-types)
3. [Basic Queries](#basic-queries)
4. [Joins](#joins)
5. [Aggregate Functions](#aggregate-functions)
6. [Subqueries](#subqueries)
7. [Indexes](#indexes)
8. [Constraints](#constraints)
9. [Transactions](#transactions)
10. [Views](#views)

## Introduction
SQL (Structured Query Language) is a standard language for accessing and manipulating databases.

## Data Types
- **INT**: Integer
- **VARCHAR**: Variable-length string
- **CHAR**: Fixed-length string
- **DATE**: Date value
- **FLOAT**: Floating point number
- **BOOLEAN**: True/False

## Basic Queries
- **SELECT**: Retrieve data from a database
    ```sql
    SELECT column1, column2 FROM table_name;
    ```
    ```sql
    SELECT * FROM table_name;
    ```
- **INSERT**: Insert data into a table
    ```sql
    INSERT INTO table_name (column1, column2) VALUES (value1, value2);
    ```
    ```sql
    INSERT INTO table_name VALUES (value1, value2, value3, ...);
    ```
- **UPDATE**: Update existing data within a table
    ```sql
    UPDATE table_name SET column1 = value1 WHERE condition;
    ```
- **DELETE**: Delete data from a table
    ```sql
    DELETE FROM table_name WHERE condition;
    ```

## Joins
- **INNER JOIN**: Returns records that have matching values in both tables
    ```sql
    SELECT columns FROM table1 INNER JOIN table2 ON table1.column = table2.column;
    ```
- **LEFT JOIN**: Returns all records from the left table, and the matched records from the right table
    ```sql
    SELECT columns FROM table1 LEFT JOIN table2 ON table1.column = table2.column;
    ```
- **RIGHT JOIN**: Returns all records from the right table, and the matched records from the left table
    ```sql
    SELECT columns FROM table1 RIGHT JOIN table2 ON table1.column = table2.column;
    ```
- **FULL JOIN**: Returns all records when there is a match in either left or right table
    ```sql
    SELECT columns FROM table1 FULL JOIN table2 ON table1.column = table2.column;
    ```

## Aggregate Functions
- **COUNT()**: Returns the number of rows
    ```sql
    SELECT COUNT(column) FROM table_name;
    ```
- **SUM()**: Returns the sum of a numeric column
    ```sql
    SELECT SUM(column) FROM table_name;
    ```
- **AVG()**: Returns the average value of a numeric column
    ```sql
    SELECT AVG(column) FROM table_name;
    ```
- **MIN()**: Returns the smallest value
    ```sql
    SELECT MIN(column) FROM table_name;
    ```
- **MAX()**: Returns the largest value
    ```sql
    SELECT MAX(column) FROM table_name;
    ```

## Subqueries
- **Subquery in SELECT**
    ```sql
    SELECT column, (SELECT column FROM table WHERE condition) AS alias FROM table;
    ```
- **Subquery in WHERE**
    ```sql
    SELECT column FROM table WHERE column = (SELECT column FROM table WHERE condition);
    ```

## Indexes
- **Create Index**
    ```sql
    CREATE INDEX index_name ON table_name (column1, column2);
    ```
- **Drop Index**
    ```sql
    DROP INDEX index_name;
    ```

## Constraints
- **NOT NULL**: Ensures that a column cannot have a NULL value
- **UNIQUE**: Ensures that all values in a column are unique
- **PRIMARY KEY**: A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table
- **FOREIGN KEY**: Uniquely identifies a row/record in another table
- **CHECK**: Ensures that all values in a column satisfy a specific condition
- **DEFAULT**: Sets a default value for a column if no value is specified

## Transactions
- **BEGIN TRANSACTION**: Starts a new transaction
    ```sql
    BEGIN TRANSACTION;
    ```
- **COMMIT**: Saves the transaction
    ```sql
    COMMIT;
    ```
- **ROLLBACK**: Undoes the transaction
    ```sql
    ROLLBACK;
    ```

## Views
- **Create View**
    ```sql
    CREATE VIEW view_name AS SELECT column1, column2 FROM table_name WHERE condition;
    ```
- **Drop View**
    ```sql
    DROP VIEW view_name;
    ```

## User Management

### Creating Users
- **Create User**
    ```sql
    CREATE USER 'username'@'host' IDENTIFIED BY 'password';
    ```

### Granting Privileges
- **Grant Privileges**
    ```sql
    GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'host';
    ```
- **Grant Specific Privileges**
    ```sql
    GRANT SELECT, INSERT, UPDATE ON database_name.table_name TO 'username'@'host';
    ```

### Revoking Privileges
- **Revoke Privileges**
    ```sql
    REVOKE ALL PRIVILEGES ON database_name.* FROM 'username'@'host';
    ```
- **Revoke Specific Privileges**
    ```sql
    REVOKE SELECT, INSERT ON database_name.table_name FROM 'username'@'host';
    ```

### Deleting Users
- **Drop User**
    ```sql
    DROP USER 'username'@'host';
    ```

### Viewing User Privileges
- **Show Grants**
    ```sql
    SHOW GRANTS FOR 'username'@'host';
    ```

[MySQL docs](https://dev.mysql.com/doc/)