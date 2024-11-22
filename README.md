
```sql
-- Drop table if exist
DROP TABLE employees;

-- Drop type if it exists
DROP TYPE IF EXISTS person;

-- Create composite type
CREATE TYPE person AS (
    name VARCHAR,
    age INT,
    email VARCHAR
);

-- Create table with a composite column
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    details person
);

-- Inserting data using ROW constructor
INSERT INTO employees (details)
VALUES (ROW('Mwanthi', 25, 'dmwanthi2@gmail.com'));

-- Select all data from employees
SELECT * 
FROM employees;

-- Query individual fields of composite type
SELECT employees.details.name AS name, employees.details.age AS age
FROM employees;

SELECT employees.details.name FROM employees;


-- Query individual fields of composite type
SELECT (details).name, (details).age
FROM employees;

-- Update specific field witin a composite type
UPDATE employees
SET details = ROW((details).name, 29, (details).email)
WHERE (details).name = 'Mwanthi';

---###### PART 2: #########---
-- Array of composite Types
CREATE TABLE company (
    id SERIAL PRIMARY KEY,
    employees person[]
);

-- Insert values to the table
INSERT INTO company (employees)
VALUES 
(ARRAY[
    ROW('John Doe', 30, 'john.doe@example.com')::person,
    ROW('Jane Smith', 28, 'jane.smith@example.com')::person,
    ROW('Daniel Mwanthi', 35, 'daniel.mwanthi@example.com')::person,
    ROW('Peter Pan', 40, 'peter.pan@example.com')::person
]);

-- Select from the company table
SELECT * FROM company;

-- Create a new table to hold individual employees
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR,
    age INT,
    email VARCHAR
);

-- Unroll the employees array from the company table into the new employees table
INSERT INTO employees (name, age, email)
SELECT 
    (emp).name,
    (emp).age,
    (emp).email
FROM company,
LATERAL unnest(employees) AS emp;

-- Verify the Data in the New Table
SELECT * FROM employees;
```
