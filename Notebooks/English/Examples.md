## Ultimate SQL Commands

[SQL Tutorial - Full Database Course for Beginners](https://www.youtube.com/watch?v=HXV3zeQKqGY&ab_channel=freeCodeCamp.org)

## Common Data Types

```sql
1. INT           -- whole numbers
2. DECIMAL(M,N)  -- decimal numbers (exact value)
3. VARCHAR(255)  -- string of text of length 255
4. BLOB          -- Binary Large Object, stores large data
5. DATE          -- 'YYYY-MM-DD'
6. TIMESTAMP     -- 'YYYY-MM-DD HH:MM:SS' (used for recording)
```

## Constraints / Atributes

```sql
NOT NULL        -- value cannot be empty
UNIQUE          -- value cannot be repeated
AUTO_INCREMENT  -- data type is automatically incremented
PRIMARY KEY     -- a primary key will be both unique and not null by default
DEFAULT 'value' -- sets a default value to be whatever is between quotes
```

## Creating Databases

```sql
CREATE DATABASE database_name
```

## Accessing Databases

## Creating Tables

```sql
CREATE TABLE table_name (
  column_list data_type PRIMARY KEY
);

-- Another version defines the primary key later
CREATE TABLE table_name (
  column_list data_type,
  column_list data_type, NOT NULL,
  PRIMARY KEY(column_list)
);

-- Example
CREATE TABLE student (
  student_id INT,
  name VARCHAR(20),
  major VARCHAR(20),
  PRIMARY KEY(student_id)
);
```

## Output Tables

## Deleting Tables

## Altering Tables

```sql
-- Adding column to tables
ALTER TABLE table_name ADD new_column_list data_type;

ALTER TABLE student ADD gpa DECIMAL(3, 2);
-- 3 relates to the total of digits, 2 digits occuring after the decimal point 
```

```sql
-- Deleting column in tables
ALTER TABLE table_name DROP COLUMN column_list;
```

## Inserting Data

```sql
INSERT INTO table_name VALUES (value1, value2, value3);
-- values need to be in order defined in schema

INSERT INTO student(student_id, name) VALUES (2, 'Kate');
-- fields without data input will get NULL value UNLESS you specified a default value in your schema
```

## Getting Data from Database → Basic Queries

### Choose rows

```sql
-- retrieve all information from table
SELECT * FROM table_name;

-- specify which row(s) to return
SELECT column_name FROM table_name;
SELECT column_name, column_name FROM student;
```

### Ordered rows

```sql
-- retrieve an ordered information from table
SELECT table_name.column_name, table_name.column
FROM table_name
ORDER BY column_name;

-- descending order or ASC ascending order
SELECT student.name, student.major
FROM student
ORDER BY student_id DESC; 

-- order by 1st criteria, if it's same value order by 2nd criteria
SELECT student.name, student.major
FROM student
ORDER BY major, student_id;
```

### Limit rows

```sql
SELECT * 
FROM table_name
ORDER BY column_name DESC
LIMIT 2;
```

### Filter rows

```sql
-- Filter Operators
-- <, >, <=, >=, =, <>, AND, OR
-- <> means "not equal to"

SELECT * 
FROM student
WHERE student_id <= 3 AND NAME <> 'Jack';

-- select all where condition is in the group of values
SELECT * 
FROM student
WHERE name IN ('Claire', 'Kate', 'Mike');

SELECT * 
FROM student
WHERE name IN ('Claire', 'Kate', 'Mike') AND student_id < 2;
```

### Create alias

```sql
SELECT first_name AS forename, last_name AS surname
FROM employees;
```

### Find all different values

```sql
SELECT DISTINCT sex
FROM employee;
-- returns only unique values
```

## Deleting Data from Tables

```sql
-- deletes all rows
DELETE FROM <name of table>;

DELETE FROM student
WHERE student_id = 5;
```

## Updating Data from Tables using queries

```sql
UPDATE table_name
SET row_name = new_value_list
WHERE row_name = previous_value_list ;

-- updating the major values from rows with 'Biology' in field
UPDATE student
SET major = 'Bio'
WHERE major = 'Biology';

-- updating the major values on one student 
UPDATE student
SET major = 'Bio'
WHERE student_id = 4;

-- updating the major values if student is major on Bio or Chemistry
UPDATE student
SET major = 'Biochemistry'
WHERE major = 'Bio' OR major = 'Chemistry';

-- updating name and major information on a student
UPDATE student
SET name = 'Tom', major = 'undecided'
WHERE student_id = 1;
```

## Complex Data

## Creating Relational Tables

```sql
CREATE TABLE employee (
  emp_id INT PRIMARY KEY,
  first_name VARCHAR(40),
  last_name VARCHAR(40),
  birth_day DATE,
  sex VARCHAR(1),
  salary INT,
  super_id INT, --> will be a foreign key
  branch_id INT --> will be a foreign key
);

-- We define those as foreign keys, because the other tables don't exist yet.

CREATE TABLE branch (
  branch_id INT PRIMARY KEY,
  branch_name VARCHAR(40),
  mgr_id INT, --> will be a foreign key
  mgr_start_date DATE,
  FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
);
-- whenever we create a foreign key, we'll use ON DELETE SET NULL

ALTER TABLE employee
ADD FOREIGN KEY(branch_id)
REFERENCES branch(branch_id)
ON DELETE SET NULL;

ALTER TABLE employee
ADD FOREIGN KEY(super_id)
REFERENCES employee(emp_id)
ON DELETE SET NULL;

-- foreign keys couldn't be added sooner because the tables weren't created yet.

CREATE TABLE works_with (
  emp_id INT,
  client_id INT,
  total_sales INT,
  PRIMARY KEY(emp_id, client_id),
  FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
  FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
);
```

## Inserting into Relational Tables

```sql
INSERT INTO employee VALUE
```

## SQL Functions

### Count

```sql
--returns the number of entries in a field
SELECT COUNT(emp_id) FROM employee;

--Find the number of female employee born after 1970
SELECT COUNT(emp_id) FROM employee
WHERE sex = 'F' AND birth_date > '1971-01-01';
```

### Average

```sql
--Average salary
SELECT AVG(salary) FROM employee
WHERE sex = F;
```

### Sum

```sql
--Sum of all employees salary
SELECT SUM(salary) FROM employee;
```

### Aggregation

```sql
--How many males and females there are
SELECT COUNT(sex) FROM employee; -- 9
SELECT COUNT(sex), sex FROM employee; -- 9 | M
SELECT COUNT(sex), sex FROM employee GROUP BY sex; -- 3 | F - 6 | M

-- Find total sales of each salesman
SELECT SUM(total_sales), emp_id FROM works_with GROUP BY client_id;
```

## Wildcards

Wildcards are a way of defining different patterns that we want to match specific pieces of data to. It is a way to retrieve data that matches a specific pattern using the keyword `LIKE`.

```sql
%  -- any number of characters
_  -- any single character

--Find any client who are LLC
SELECT * FROM client WHERE client_name LIKE '%LLC'; 
-- definining a pattern with LIKE to match the search
-- any number of characters can come before the LLC

SELECT * FROM client WHERE client_name LIKE '% LLC%';
-- any number of characters can come before or after the LLC word

-- Find any employee born in October
SELECT * FROM employee WHERE birth_date LIKE '____-10%';
-- born in 10th month, any day or year 
```

## Union

Combining multiple selects statements into one. A couple of rules:

-   you to have the same number of columns (1 column in first statement, 1 column in second statement ) in each select statement
-   They need to be the same data type

```sql
-- Find a list of employee and branch names
SELECT first_name FROM employee;
SELECT banch_name FROM branch;

-- Combining into a single statement
SELECT first_name AS unions
FROM employee 
UNION 
SELECT banch_name 
FROM branch
UNION
SELECT client_name 
FROM client;
```

## Join

Combining rows from 2 or more tables based on a related column between them.

```sql
-- INNER JOIN: basic join

SELECT employee.emp_id, employee.first_name, branch.branch_name
FROM employee
JOIN branch
ON employee.emp_id = branch.mgr_id; -- this is the column they have in common

-- output will be the first and branch names of the employees that match
  -- both employee.emp_id AND branch.mgr_id
```

⚠️ Attention: Outer joins cannot be perfomed in MySQL.

```sql
-- OUTER JOIN: It's basically a left join and a right join combined

-- would grab all branches and all employee's names

```

```sql
-- LEFT JOIN: includes all rows from the left table

SELECT employee.emp_id, employee.first_name, branch.branch_name
FROM employee
LEFT JOIN branch
ON employee.emp_id = branch.mgr_id;

-- All rows from employee gets included, but only the rows 
  --in the branch table that matched the ON statement will get included
```

```sql
-- RIGHT JOIN: includes all rows from the right table

-- Need to add the extra branch for example
INSERT INTO branch VALUES(4, "Buffalo", NULL, NULL);

SELECT employee.emp_id, employee.first_name, branch.branch_name
FROM employee
RIGHT JOIN branch
ON employee.emp_id = branch.mgr_id;

-- all rows from the branch table get included
```

[SQL Joins Visualizer](https://sql-joins.leopard.in.ua/)

## Nested Queries

💡 Quick Tip: Start by querying the inner most query and work your way to the outer query

```sql
-- Find names of all employees who have sold over 50,000

SELECT employee.first_name, employee.last_name
FROM employee
WHERE employee.emp_id IN (  -- employee id will match the result of the query search
    SELECT works_with.emp_id
    FROM works_with
    WHERE works_with.total_sales > 50000
);

-- Find all clients who are handles by the branch that Michael Scott manages

SELECT client.client_id, client.client_name
FROM client
WHERE client.branch_id = (
    SELECT branch.branch_id
    FROM branch
    WHERE branch.mgr_id = (
        SELECT employee.emp_id
        FROM employee
        WHERE employee.first_name = 'Michael' AND employee.last_name ='Scott'
        LIMIT 1
    )
);
```

## On Delete

Deleting entries in the database when they have foreign keys associated to them.

```sql
ON DELETE SET NULL 
  -- when deleted, rows associated with that entry changes the value to NULL
ON DELETE CASCADE 
  -- when deleted, rows associated with that entry also gets deleted.

-- Examples:
CREATE TABLE branch (
  branch_id INT PRIMARY KEY,
  branch_name VARCHAR(40),
  mgr_id INT,
  FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
);

CREATE TABLE works_with (
  emp_id INT,
  client_id INT,
  total_sales INT,
  PRIMARY KEY(emp_id, client_id),
  FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
  FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
);
```

## Triggers

A trigger is a block of SQL code which will define a certain action that should happen when a certain operation gets performed on the database.

```sql
DELIMITER $$
CREATE
    TRIGGER trigger_name BEFORE INSERT
    ON table_name
    FOR EACH ROW BEGIN
        INSERT INTO trigger_table_name VALUES('added new employee');
    END$$
DELIMITER ;

-- before anything gets inserted in a table
  -- for each of the new items that are getting inserted
    -- insert first into the trigger test table some value

-- DELIMITER: this keyword will change the delimiter. 
  -- Usually that means the semicolon, but we need it for our trigger statement
    -- Line 1 changes the delimiter to "$$"
    -- Line 8 changes it back to ";"

-- Testing your trigger
INSERT INTO employee
VALUES(109, 'Oscar', 'Martinez', '1968-02-19', 'M', 69000, 106, 3);
SELECT * from trigger_name
```

```sql
TRIGGER trigger_name BEFORE INSERT
TRIGGER trigger_name BEFORE UPDATE
TRIGGER trigger_name BEFORE DELETE
TRIGGER trigger_name AFTER UPDATE
TRIGGER trigger_name AFTER DELETE
```

```sql
-- Returning information that just got added

DELIMITER $$
CREATE
    TRIGGER my_trigger BEFORE INSERT
    ON employee
    FOR EACH ROW BEGIN
        INSERT INTO trigger_test VALUES(NEW.first_name);
    END$$
DELIMITER ;
INSERT INTO employee
VALUES(110, 'Kevin', 'Malone', '1978-02-19', 'M', 69000, 106, 3);

-- NEW.first_name returns the first name of the employee that just got added
```

### Conditionals

```sql
DELIMITER $$
CREATE
    TRIGGER my_trigger BEFORE INSERT
    ON employee
    FOR EACH ROW BEGIN
         IF NEW.sex = 'M' THEN
               INSERT INTO trigger_test VALUES('added male employee');
         ELSEIF NEW.sex = 'F' THEN
               INSERT INTO trigger_test VALUES('added female');
         ELSE
               INSERT INTO trigger_test VALUES('added other employee');
         END IF;
    END$$
DELIMITER ;
INSERT INTO employee
VALUES(111, 'Pam', 'Beesly', '1988-02-19', 'F', 69000, 106, 3);
```

### Drop Trigger

## Entity Relationship (ER Diagrams)

1.  **Entity** → An object we want to model and store information about
    1.  These will become the tables in our database
    2.  A student object in a school database
2.  **Attributes** → Specific pieces of information about an entity
    1.  Info about student’s name, gpa, grade number in the student entity
3.  **Primary key** → An attribute which will uniquely identify an entry in the database entity
    1.  Student’s id
4.  **Composite attribute** → An attribute that can be broken up into sub-attributes
    1.  We can store a student’s name, but we can also store their first and last name
5.  **Multi-value attribute** → An attribute than can have more than one value
    1.  A student may be envolved in several clubs
6.  **Derived Attribute** → An attribute that can be derived from the other attributes
    1.  has\_honors could be information derived from the gpa score of a student
7.  **Multiple Entities** → You can define more than one entity in your Schema Design
8.  **Relationships →** Define a relationship between entities.
    1.  **Total Participation** → All members must participate in the relationship
    2.  **Partial Participation** → Members are not required to participate
9.  **Relationship Attribute** → An attribute about the relationship
    1.  A student can only have access to a grade if they are taking the class.
10.  **Relationship Cardinality →** the number of instances of an entity from a relation that can be associated with the relation.
    1.  Many to many: a student can take multiple classes. A class can have multiple students.
    2.  One to many: a student can take only one class. A class can have multiple students.
    3.  One to one: one student per one class.
11.  **Weak Entity Types** → An entity that cannot be uniquely identified by its attributes alone.
    1.  Weak entities depend on another entity
    2.  An exam can only exist if a class exists.
12.  **Identifying Relationship** → A relationship that serves to uniquely identify the weak entity.

## Sequences

```sql
data types: bigint, serial or bigserial

--creating a sequence (common naming convention is table-name_id_seq)
CREATE SEQUENCE serial_id_seq START 1;

--Select the next number from this sequence (do not skip this step!)
SELECT nextval('serial');

--updating a sequence after ETL import
COPY table_name FROM 'input_file';
SELECT setval('serial_id_seq', max(id)) FROM table_name;

-- deleting a sequence
DROP SEQUENCE IF EXISTS **name** RESTRICT
  --restrict(default) will refuse to drop the sequence if any objects depend on it.
  -- Alternatively, CASCADE drops objects that depend on the sequence.
  
-- Alternatively, you can also use:
SELECT pg_catalog.setval(pg_get_serial_sequence('table_name', 'id'), (SELECT MAX(id) FROM table_name)+1);
```

## Indexes

Indexes are primarily used to enhance database performance. When you create an index, the database will generate a method to find data based on one or more columns ([link](https://www.postgresql.org/docs/14/sql-createindex.html)).

NOTE: inappropriate use of indexes can result in slower performance.

```sql
-- Common naming convention for indexes is table_name + field_name + "idx" in the end
CREATE INDEX table_name_field_name_idx;
```