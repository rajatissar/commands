# Queries

## 1. Database

### CREATE

> **SYNTAX**

```SQL
CREATE DATABASE db_name
 OWNER = role_name
 TEMPLATE = template
 ENCODING = encoding
 LC_COLLATE = collate
 LC_CTYPE = ctype
 TABLESPACE = tablespace_name
 CONNECTION LIMIT = max_concurrent_connection
```

* db_name: is the name of the new database that you want to create. The database name must be unique in the PostgreSQL database server. If you try to create a new database that has the same name as an existing database, PostgreSQL will issue an error.
* role_name: is the role name of the user who will own the new database. PostgreSQL uses user’s role name who executes the CREATE DATABASE statement as the default role name.
* template: is the name of the database template from which the new database creates. PostgreSQL allows you to create a database based on a template database. The template1 is the default template database.
* encoding: specifies the character set encoding for the new database. By default, it is the encoding of the template database.
* collate: specifies a collation for the new database. The collation specifies the sort order of strings that affect the result of the ORDER BY clause in the SELECT statement. The template database’s collation is the default collation for the new database if you don’t specify it explicitly in the LC_COLLATE parameter.
* ctype: specifies the character classification for the new database. The ctype affects the categorization e.g., digit, lower and upper. The default is the character classification of the template database.
* tablespace_name: specifies the tablespace name for the new database. The default is the template database’s tablespace.
* max_concurrent_connection: specifies the maximum concurrent connections to the new database. The default is -1 i.e., unlimited. This feature is very useful in the shared hosting environments where you can configure the maximum concurrent connections for a particular database.

> **EXAMPLE**

```SQL
CREATE DATABASE db1;
```

### :pencil: RENAME

> **SYNTAX**

```SQL
ALTER DATABASE target_database RENAME TO new_database;
```

To rename a database, you have to connect to another database e.g., postgres.

### Change Owner

> **SYNTAX**

```SQL
ALTER DATABASE target_database OWNER TO new_owner;
```

Only the super_user or owner of the database can change the database’s owner. The database owner must also have the CREATEDB privilege to rename the database.

### Change tablespace

> **SYNTAX**

```SQL
ALTER DATABASE target_database SET TABLESPACE new_tablespace;
```

```SQL
ALTER DATABASE target_database SET configuration_parameter = value;
```

Notice that only a supper_user or the database owner can change the default session variables for a database.

> **EXAMPLE**

```SQL
CREATE ROLE hr
VALID UNTIL 'infinity';
```

```SQL
CREATE TABLESPACE hr_default
  OWNER hr
  LOCATION E'C:\\pgdata\\hr';
```

```SQL
ALTER DATABASE db1 SET escape_string_warning TO off;
```

### Check all active connections to database

> **SYNTAX**

```SQL
SELECT
  *
FROM
  pg_stat_activity
WHERE
  datname = 'dvdrental';
```

```table
-[ RECORD 1 ]----+------------------------------------------------------------
datid            | 16384
datname          | dvdrental
pid              | 31252
usesysid         | 10
usename          | postgres
application_name | psql
client_addr      |
client_hostname  |
client_port      | -1
backend_start    | 2020-06-08 14:42:38.90932+05:30
xact_start       | 2020-06-08 16:21:18.311672+05:30
query_start      | 2020-06-08 16:21:18.311672+05:30
state_change     | 2020-06-08 16:21:18.311681+05:30
wait_event_type  |
wait_event       |
state            | active
backend_xid      |
backend_xmin     | 769
query            | SELECT * FROM pg_stat_activity WHERE datname = 'dvdrental';
backend_type     | client backend
```

### Terminate all connections to database

```SQL
SELECT
  pg_terminate_backend (pid)
FROM
  pg_stat_activity
WHERE
  datname = 'dvdrental';
```

### DROP Database

> **SYNTAX**

```SQL
DROP DATABASE target_database;
```

### Copy Database

> **SYNTAX**

```SQL
CREATE DATABASE target_db
WITH TEMPLATE source_db;
```

> **EXAMPLE**

```SQL
CREATE DATABASE dvdrental_test
WITH TEMPLATE dvdrental;
```

```SQL
pg_dump -U postgres -O dvdrental dvdrental.sql
```

```SQL
psql -U postgres -d dvdrental_test -f dvdrental.sql
```

## 2. Table

### Create Table

> **SYNTAX**

```SQL
CREATE TABLE table_name (
  column_name TYPE column_constraint,
  table_constraint table_constraint
) INHERITS existing_table_name;
```

> **EXAMPLE**

```SQL
CREATE TABLE account(
  user_id serial PRIMARY KEY,
  username VARCHAR (50) UNIQUE NOT NULL,
  password VARCHAR (50) NOT NULL,
  email VARCHAR (355) UNIQUE NOT NULL,
  created_on TIMESTAMP NOT NULL,
  last_login TIMESTAMP
);
```

> **SYNTAX**

```SQL
CREATE TABLE new_table_name
AS query;
```

```SQL
CREATE TEMP TABLE new_table_name
AS query;
```

```SQL
CREATE UNLOGGED TABLE new_table_name
AS query;
```

> **EXAMPLE**

```SQL
CREATE TABLE action_film AS
SELECT
  film_id,
  title,
  release_year,
  length,
  rating
FROM
    film
INNER JOIN film_category USING (film_id)
WHERE category_id = 1 ; -- action
```

### SERIAL

> **SYNTAX**

```SQL
CREATE TABLE table_name(
  id SERIAL
);
```

### SEQUENCE

> **SYNTAX**

```SQL
CREATE SEQUENCE [ IF NOT EXISTS ] sequence_name
  [ AS { SMALLINT | INT | BIGINT } ]
  [ INCREMENT [ BY ] increment ]
  [ MINVALUE minvalue | NO MINVALUE ]
  [ MAXVALUE maxvalue | NO MAXVALUE ]
  [ START [ WITH ] start ]
  [ CACHE cache ]
  [ [ NO ] CYCLE ]
  [ OWNED BY { table_name.column_name | NONE } ]
```

> **EXAMPLE**

```SQL
CREATE SEQUENCE sequence1
INCREMENT 5
START 100;
```

```SQL
CREATE SEQUENCE order_item_id
START 10
INCREMENT 10
MINVALUE 10
OWNED BY order_details.item_id;
```

List all sequences in the current database

```SQL
SELECT
  c.relname sequence_name
FROM
  pg_class
WHERE
  relkind = 'S';
```

> **SYNTAX**

```SQL
DROP SEQUENCE [ IF EXISTS ] sequence_name [, ...]
[ CASCADE | RESTRICT ];
```

> **EXAMPLE**

```SQL
DROP SEQUENCE IF EXISTS sequence1, sequence2;
```

## 3. Data Type

PostgreSQL supports the following data types:

* Boolean.
* Character types such as char, varchar, and text.
* Numeric types such as integer and floating-point number.
* Temporal types such as date, time, timestamp, and interval.
* UUID for storing Universally Unique Identifiers.
* Array for storing array strings, numbers, etc.
* JSON stores JSON data.
* hstore stores key-value pair.
* Special types such as network address and geometric data.

## 4. SELECT

| Query                      | Explanation            |
|----------------------------|------------------------|
| DISTINCT                   | Distinct rows          |
| ORDER BY                   | Sort rows              |
| WHERE                      | Filter rows            |
| LIMIT / FETCH              | subset of rows         |
| GROUP BY                   | Group rows into groups |
| HAVING                     | Filter groups          |
| JOIN                       | Join with other tables |
| UNION / INTERSECT / EXCEPT | Perform set operations |

> **SYNTAX**

```SQL
SELECT
  select_list
FROM
  table_name;
```

### concat operator

> **EXAMPLE**

```SQL
SELECT first_name || '-' || last_name AS full_name FROM customer;
```

### PostgreSQL as Simple Calculator

> **EXAMPLE**

```SQL
SELECT 5 * 3 AS result;
```

### ORDER BY

> **SYNTAX**

```SQL
SELECT
  column_1,
  column_2
FROM
  table_name
ORDER BY
  column_1 [ASC | DESC],
  column_2 [ASC | DESC];
```

### LENGTH

> **EXAMPLE**

```SQL
SELECT
  first_name,
  LENGTH(first_name) len
FROM
  customer
ORDER BY
  LENGTH(first_name) DESC;
```

### DISTINCT

> **SYNTAX**

```SQL
SELECT
  DISTINCT column_1, column_2
FROM
  table_name;
```

### DISTINCT ON

> **SYNTAX**

```SQL
SELECT
  DISTINCT ON (column_1) column_alias,
  column_2
FROM
  table_name
ORDER BY
  column_1,
  column_2;
```

### WHERE

> **SYNTAX**

```SQL
SELECT select_list
FROM table_name
WHERE condition;
```

> **EXAMPLE**

```SQL
SELECT
  last_name,
  first_name
FROM
  customer
WHERE
  first_name = 'Jamie';
```

```SQL
SELECT
  last_name,
  first_name
FROM
  customer
WHERE
  first_name = 'Jamie' AND last_name = 'Rice';
```

```SQL
SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  last_name = 'Rodriguez' OR first_name = 'Adam';
```

```SQL
SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  first_name IN ('Ann','Anne','Annie');
```

```SQL
SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  first_name LIKE 'Ann%'
```

```SQL
SELECT
  first_name,
  LENGTH(first_name) name_length
FROM
  customer
WHERE
  first_name LIKE 'A%' AND LENGTH(first_name) BETWEEN 3 AND 5
ORDER BY
  name_length;
```

```SQL
SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  first_name LIKE 'Bra%' AND last_name <> 'Motley';
```

### LIMIT & OFFSET

> **SYNTAX**

```SQL
SELECT
  *
FROM
  table_name
LIMIT n OFFSET m;
```

### FETCH

> **SYNTAX**

```SQL
OFFSET start { ROW | ROWS }
FETCH { FIRST | NEXT } [ row_count ] { ROW | ROWS } ONLY
```

> **EXAMPLE**

```SQL
SELECT
  film_id,
  title
FROM
  film
ORDER BY
  title
FETCH FIRST 5 ROW ONLY;
```

### IN

> **SYNTAX**

```SQL
value IN (value1,value2,...)
```

> **EXAMPLE**

```SQL
SELECT
  customer_id,
  rental_id,
  return_date
FROM
  rental
WHERE
  customer_id IN (1, 2);
```

```SQL
SELECT
  customer_id,
  rental_id,
  return_date
FROM
  rental
WHERE
  customer_id NOT IN (1, 2);
```

```SQL
SELECT
  customer_id
FROM
  rental
WHERE
  CAST (return_date AS DATE) = '2005-05-27';
```

```SQL
SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  customer_id IN (
    SELECT
      customer_id
    FROM
      rental
    WHERE
      CAST (return_date AS DATE) = '2005-05-27');
```

### BETWEEN

> **SYNTAX**

```SQL
value BETWEEN low AND high;
```

```SQL
value NOT BETWEEN low AND high;
```

> **EXAMPLE**

```SQL
SELECT
  customer_id,
  payment_id,
  amount
FROM
  payment
WHERE
  amount BETWEEN 8 AND 9;
```

```SQL
SELECT
  customer_id,
  payment_id,
  amount,
  payment_date
FROM
  payment
WHERE
  payment_date BETWEEN '2007-02-07' AND '2007-02-15';
```

### LIKE

> **EXAMPLE**

```SQL
SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  first_name LIKE 'Jen%';
```

```SQL
SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  first_name NOT LIKE 'Jen%';
```

```SQL
SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  first_name ILIKE 'BAR%';
```

```SQL
~~ is equivalent to LIKE
~~* is equivalent to ILIKE
!~~ is equivalent to NOT LIKE
!~~* is equivalent to NOT ILIKE
```

### IS NULL

> **EXAMPLE**

```SQL
SELECT
  id,
  first_name,
  last_name,
  email,
  phone
FROM
  contacts
WHERE
  phone IS NULL;
```

```SQL
SELECT
  id,
  first_name,
  last_name,
  email,
  phone
FROM
  contacts
WHERE
  phone IS NOT NULL;
```

### ALIAS

A PostgreSQL ALIAS assigns a table or a column a temporary name in a query. The aliases only exist during the execution of the query.

> **SYNTAX**

```SQL
SELECT column_name AS alias_name
FROM table;
```

```SQL
SELECT
  column_list
FROM
  table_name AS alias_name;
```

> **EXAMPLE**

```SQL
SELECT t.column_name
FROM a_very_long_table_name AS t;
```

```SQL
SELECT t1.column_name,
       t2.column_name
FROM table_name1 t1
INNER JOIN table_name2 t2 ON join_predicate;
```

### SELECT INTO

> **SYNTAX**

```SQL
SELECT
  column_list
INTO [ TEMPORARY | TEMP | UNLOGGED ] [ TABLE ] new_table_name
FROM
  table_name
WHERE
  condition;
```

> **EXAMPLE**

```SQL
SELECT
  film_id,
  title,
  rental_rate
INTO TABLE film_r
FROM
  film
WHERE
  rating = 'R';
```

## 5. JOIN

PostgreSQL join is used to combine columns from one (self-join) or more tables based on the values of the common columns between the tables. The common columns are typically the primary key columns of the first table and foreign key columns of the second table.

### INNER JOIN

> **EXAMPLE**

```SQL
SELECT
  a.id id_a,
  a.fruit fruit_a,
  b.id id_b,
  b.fruit fruit_b
FROM
  basket_a a
INNER JOIN basket_b b ON a.fruit = b.fruit;
```

```SQL
SELECT
  customer.customer_id,
  customer.first_name customer_first_name,
  customer.last_name customer_last_name,
  customer.email,
  staff.first_name staff_first_name,
  staff.last_name staff_last_name,
  amount,
  payment_date
FROM
  customer
INNER JOIN payment ON payment.customer_id = customer.customer_id
INNER JOIN staff ON payment.staff_id = staff.staff_id;
```

### LEFT JOIN

> **EXAMPLE**

```SQL
SELECT
  a.id id_a,
  a.fruit fruit_a,
  b.id id_b,
  b.fruit fruit_b
FROM
  basket_a a
LEFT JOIN basket_b b ON a.fruit = b.fruit;
```

### RIGHT JOIN

> **EXAMPLE**

```SQL
SELECT
  a.id id_a,
  a.fruit fruit_a,
  b.id id_b,
  b.fruit fruit_b
FROM
  basket_a a
RIGHT JOIN basket_b b ON a.fruit = b.fruit;
```

### FULL OUTER JOIN

> **EXAMPLE**

```SQL
SELECT
  a.id id_a,
  a.fruit fruit_a,
  b.id id_b,
  b.fruit fruit_b
FROM
  basket_a a
FULL OUTER JOIN basket_b b ON a.fruit = b.fruit;
```

### SELF JOIN

A self-join is a query in which a table is joined to itself. Self-joins are useful for comparing values in a column of rows within the same table.
To form a self-join, you specify the same table twice with different aliases, set up the comparison, and eliminate cases where a value would be equal to itself.

> **SYNTAX**

```SQL
SELECT column_list
FROM A a1
INNER JOIN A b1 ON join_predicate;
```

> **EXAMPLE**

```SQL
SELECT
  e.first_name || ' ' || e.last_name employee,
  m .first_name || ' ' || m .last_name manager
FROM
  employee e
INNER JOIN employee m ON e.manager_id = m .employee_id;
```

### CROSS JOIN

A CROSS JOIN clause allows you to produce the Cartesian Product of rows in two or more tables. Different from the other JOIN operators such as LEFT JOIN  or INNER JOIN, the CROSS JOIN does not have any matching condition in the join clause.
Suppose we have to perform the CROSS JOIN of two tables T1 and T2. For every row from T1 and T2 i.e., a cartesian product, the result set will contain a row that consists of all columns in the T1 table followed by all columns in the T2 table. If T1 has N rows, T2 has M rows, the result set will have N x M rows.

> **SYNTAX**

```SQL
SELECT *
FROM T1
CROSS JOIN T2;
```

```SQL
SELECT *
FROM T1, T2;
```

```SQL
SELECT *
FROM T1
INNER JOIN T2 ON TRUE;
```

### NATURAL JOIN

A natural join is a join that creates an implicit join based on the same column names in the joined tables.
If you use the asterisk (*) in the select list, the result will contain the following columns:

* All the common columns, which are the columns in the both tables that have the same name.
* Every column in the first and second tables that is not a common column.

> **SYNTAX**

```SQL
SELECT *
FROM T1
NATURAL [INNER, LEFT, RIGHT] JOIN T2;
```

> **EXAMPLE**

```SQL
SELECT *
FROM products
NATURAL JOIN categories;
```

![JOIN](../assets/PostgreSQL-Joins.png)

## 6. Grouping Data

> **SYNTAX**

```SQL
SELECT
  column_1,
  column_2,
  aggregate_function(column_3)
FROM
  table_name
GROUP BY
  column_1,
  column_2;
```

> **EXAMPLE**

```SQL
SELECT
  customer_id
FROM
  payment
GROUP BY
  customer_id;
```

```SQL
SELECT
  customer_id,
  SUM (amount)
FROM
  payment
GROUP BY
  customer_id
ORDER BY
  SUM (amount) DESC;
```

```SQL
SELECT
  DATE(payment_date) paid_date,
  SUM(amount) sum
FROM
  payment
GROUP BY
  DATE(payment_date);
```

> **SYNTAX**

```SQL
SELECT
  column_1,
  aggregate_function (column_2)
FROM
  tbl_name
GROUP BY
  column_1
HAVING
  condition;
```

## 7. Set Operation

### UNION

The UNION operator combines result sets of two or more SELECT statements into a single result set.

The following are rules applied to the queries:

* Both queries must return the same number of columns.
* The corresponding columns in the queries must have compatible data types.

> **SYNTAX**

```SQL
SELECT *
FROM
  sales2007q1
UNION
SELECT *
FROM
  sales2007q2;
```

### INTERSECT

INTERSECT operator combines the result sets of two or more SELECT statements into a single result set. The INTERSECT operator returns any rows that are available in both result set or returned by both queries.

To use the INTERSECT operator, the columns that appear in the SELECT statements must follow the rules below:

* The number of columns and their order in the SELECT clauses must the be the same.
* The data types of the columns must be compatible.

> **SYNTAX**

```SQL
SELECT
  column_list
FROM
  A
INTERSECT
SELECT
  column_list
FROM
  B;
```

> **EXAMPLE**

```SQL
SELECT
  employee_id
FROM
  keys
INTERSECT
SELECT
  employee_id
FROM
  hipos;
```

![INTERSECT-Operator](../assets/PostgreSQL-INTERSECT-Operator-300x206.png)

### EXCEPT

The EXCEPT operator returns distinct rows from the first (left) query that are not in the output of the second (right) query.

To combine the queries using the EXCEPT operator, you must obey the following rules:

* The number of columns and their orders must be the same in the two queries.
* The data types of the respective columns must be compatible.

> **SYNTAX**

```SQL
SELECT column_list
FROM A
WHERE condition_a
EXCEPT
SELECT column_list
FROM B
WHERE condition_b;
```

> **EXAMPLE**

```SQL
SELECT
  film_id,
  title
FROM
  film
EXCEPT
  SELECT DISTINCT
    inventory.film_id,
    title
  FROM
    inventory
INNER JOIN film ON film.film_id = inventory.film_id
ORDER BY title;
```

![EXCEPT-Operator](../assets/PostgreSQL-EXCEPT-300x202.png)

## 8. Grouping Sets

A grouping set is a set of columns to which you want to group.

> **EXAMPLE**

```SQL
SELECT
  brand,
  segment,
  SUM (quantity)
FROM
  sales
GROUP BY
  brand,
  segment

UNION ALL

SELECT
  brand,
  NULL,
  SUM (quantity)
FROM
  sales
GROUP BY
  brand

UNION ALL

SELECT
  NULL,
  segment,
  SUM (quantity)
FROM
  sales
GROUP BY
  segment

UNION ALL

SELECT
  NULL,
  NULL,
  SUM (quantity)
FROM
  sales;
```

```SQL
SELECT
  brand,
  segment,
  SUM (quantity)
FROM
  sales
GROUP BY
  GROUPING SETS (
    (brand, segment),
    (brand),
    (segment),
    ()
    );
```

### CUBE

> **SYNTAX**

```SQL
SELECT
  c1,
  c2,
  c3,
  aggregate (c4)
FROM
  table_name
GROUP BY
  CUBE (c1, c2, c3);
```

> **EXAMPLE**

```SQL
SELECT
  brand,
  segment,
  SUM (quantity)
FROM
  sales
GROUP BY
  CUBE (brand, segment)
ORDER BY
  brand,
  segment;
```

### ROLLUP

Different from the CUBE subclause, ROLLUP does not generate all possible grouping sets based on the specified columns. It just makes a subset of those.
A common use of  ROLLUP is to calculate the aggregations of data by year, month, and date, considering the hierarchy year > month > date.

> **SYNTAX**

```SQL
SELECT
  c1,
  c2,
  c3,
  aggregate(c4)
FROM
  table_name
GROUP BY
  ROLLUP (c1, c2, c3);
```

> **EXAMPLE**

```SQL
SELECT
  brand,
  segment,
  SUM (quantity)
FROM
  sales
GROUP BY
  ROLLUP (brand, segment)
ORDER BY
  brand,
  segment;
```

## 9. Sub Query

The query inside the brackets is called a sub query or an inner query. The query that contains the sub query is known as an outer query.

PostgreSQL executes the query that contains a sub query in the following sequence:

1. First, executes the sub query.
2. Second, gets the result and passes it to the outer query.
3. Third, executes the outer query.

> **EXAMPLE**

```SQL
SELECT
  film_id,
  title,
  rental_rate
FROM
  film
WHERE
  rental_rate > (
  SELECT
    AVG (rental_rate)
  FROM
    film
  );
```

### ANY

ANY operator compares a value to a set of values returned by a sub query. The following illustrates the syntax of  the ANY operator:

> **SYNTAX**

```SQL
expression operator ANY(sub query)
```

In this syntax:

* The sub query must return exactly one column.
* The ANY operator must be preceded by one of the following comparison operator =, <=, >, <, > and <>
* The ANY operator returns true if any value of the sub query meets the condition, otherwise, it returns false.

> **EXAMPLE**

```SQL
SELECT
  title,
  category_id
FROM
  film
INNER JOIN film_category USING(film_id)
WHERE category_id = ANY(
        SELECT
          category_id
        FROM
          category
        WHERE NAME = 'Action' OR NAME = 'Drama'
    );
```

### ALL

ALL operator allows you to query data by comparing a value with a list of values returned by a sub query.

> **SYNTAX**

```SQL
comparison_operator ALL (sub query)
```

* The ALL operator must be preceded by a comparison operator such as equal (=), not equal (!=), greater than (>), greater than or equal to (>=), less than (<), and less than or equal to (<=).
* The ALL operator must be followed by a sub query which also must be surrounded by the parentheses.

With the assumption that the sub query returns some rows, the ALL operator works as follows:

* column_name > ALL (sub query) the expression evaluates to true if a value is greater than the biggest value returned by the sub query.
* column_name >= ALL (sub query) the expression evaluates to true if a value is greater than or equal to the biggest value returned by the sub query.
* column_name < ALL (sub query) the expression evaluates to true if a value is less than the smallest value returned by the sub query.
* column_name <= ALL (sub query) the expression evaluates to true if a value is less than or equal to the smallest value returned by the sub query.
* column_name = ALL (sub query) the expression evaluates to true if a value is equal to any value returned by the sub query.
* column_name != ALL (sub query) the expression evaluates to true if a value is not equal to any value returned by the sub query.

> **EXAMPLE**

```SQL
SELECT
  film_id,
  title,
  length
FROM
  film
WHERE
  length > ALL (
            SELECT
              ROUND(AVG (length),2)
            FROM
              film
            GROUP BY
              rating
    )
ORDER BY length;
```

### EXISTS

EXISTS operator is used to test for existence of rows in a sub query.

A sub query can be an input of the EXISTS operator. If the sub query returns any row, the EXISTS operator returns true. If the sub query returns no row, the result of EXISTS operator is false.

> **SYNTAX**

```SQL
EXISTS (sub query)
```

> **EXAMPLE**

```SQL
SELECT
  first_name,
  last_name
FROM customer c
WHERE EXISTS
    (SELECT 1
     FROM payment p
     WHERE p.customer_id = c.customer_id AND amount > 11
    )
ORDER BY first_name, last_name;
```

```SQL
SELECT
  first_name,
  last_name
FROM customer c
WHERE NOT EXISTS
    (SELECT 1
     FROM payment p
     WHERE p.customer_id = c.customer_id AND amount > 11
    )
ORDER BY first_name, last_name;
```

If the sub query returns NULL, EXISTS returns true

```SQL
SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  EXISTS( SELECT NULL )
```

## 10. CTE (Common Table Expressions)

> **SYNTAX**

```SQL
WITH cte_name (column_list) AS (
  CTE_query_definition
)
statement;
```

In this syntax:

1. First, specify the name of the CTE following by an optional column list.
2. Second, inside the body of the WITH clause, specify a query that returns a result set. If you do not explicitly specify the column list after the CTE name, the select list of the CTE_query_definition will become the column list of the CTE.
3. Third, use the CTE like a table or view in the statement which can be a SELECT, INSERT, UPDATE, or DELETE.

> **EXAMPLE**

```SQL
WITH cte_film AS (
    SELECT
      film_id,
      title,
      (
        CASE
         WHEN length < 30 THEN 'Short'
         WHEN length < 90 THEN 'Medium'
         ELSE 'Long'
        END
      ) length
    FROM film
)
SELECT
  film_id,
  title,
  length
FROM cte_film
WHERE length = 'Long'
ORDER BY title;
```

### Recursive Query

A recursive query is a query that refers to a recursive CTE. The recursive queries are useful in many situations such as for querying hierarchical data like organizational structure, bill of materials, etc.

> **SYNTAX**

```SQL
WITH RECURSIVE cte_name AS(
  CTE_query_definition -- non-recursive term
  UNION [ALL]
  CTE_query definion  -- recursive term
) SELECT * FROM cte_name;
```

A recursive CTE has three elements:

1. Non-recursive term: the non-recursive term is a CTE query definition that forms the base result set of the CTE structure.
2. Recursive term: the recursive term is one or more CTE query definitions joined with the non-recursive term using the UNION or UNION ALL operator. The recursive term references the CTE name itself.
3. Termination check: the recursion stops when no rows are returned from the previous iteration.

PostgreSQL executes a recursive CTE in the following sequence:

* Execute the non-recursive term to create the base result set (R0).
* Execute recursive term with Ri as an input to return the result set Ri+1 as the output.
* Repeat step 2 until an empty set is returned. (termination check)
* Return the final result set that is a UNION or UNION ALL of the result set R0, R1, … Rn

> **EXAMPLE**

```SQL
WITH RECURSIVE subordinates AS (
  SELECT
    employee_id,
    manager_id,
    full_name
  FROM
    employees
  WHERE
    employee_id = 2
  UNION
    SELECT
      e.employee_id,
      e.manager_id,
      e.full_name
    FROM
      employees e
    INNER JOIN subordinates s ON s.employee_id = e.manager_id
) SELECT
  *
FROM
  subordinates;
```

## 11. Modifying Data

### INSERT

> **SYNTAX**

```SQL
INSERT INTO table_name(column1, column2, …)
VALUES (value1, value2, …);
```

> **EXAMPLE**

```SQL
INSERT INTO link (url, name)
VALUES ('https://www.postgresqltutorial.com','PostgreSQL Tutorial');
```

```SQL
INSERT INTO link (url, name, last_update)
VALUES ('https://www.tumblr.com/','Tumblr', DEFAULT);
```

### UPDATE

> **SYNTAX**

```SQL
UPDATE table1
SET column1 = value1,
    column2 = value2 ,...
WHERE
  condition;
```

> **EXAMPLE**

```SQL
UPDATE link
SET last_update = DEFAULT
WHERE
  last_update IS NULL;
```

```SQL
UPDATE link
SET column_1 = column_2;
```

```SQL
UPDATE link_tmp
SET rel = link.rel,
  description = link.description,
  last_update = link.last_update
FROM
  link
WHERE
  link_tmp.id = link.id;
```

```SQL
UPDATE link
SET description = 'Learn PostgreSQL fast and easy',
 rel = 'follow'
WHERE
  ID = 1
RETURNING id,
  description,
  rel;
```

### DELETE

> **SYNTAX**

```SQL
DELETE FROM table1
WHERE condition;
```

```SQL
DELETE FROM table1
USING another_table
WHERE table.id = another_table.id AND …
```

```SQL
DELETE FROM table1
WHERE table.id = (SELECT id FROM another_table);
```

> **EXAMPLE**

```SQL
DELETE FROM link_tmp
RETURNING *;
```

### UPSERT

In relational databases, the term upsert is referred to as a merge. The idea is that when you insert a new row into the table, PostgreSQL will update the row if it already exists, otherwise, PostgreSQL inserts the new row. That is why we call the action is upsert (update or insert).

> **SYNTAX**

```SQL
INSERT INTO table_name(column_list) VALUES(value_list)
ON CONFLICT target action;
```

> **EXAMPLE**

```SQL
INSERT INTO customers (NAME, email)
VALUES
  (
    'Microsoft',
    'hotline@microsoft.com'
  )
ON CONFLICT ON CONSTRAINT customers_name_key
DO NOTHING;
```

```SQL
INSERT INTO customers (name, email)
VALUES
  (
    'Microsoft',
    'hotline@microsoft.com'
  )
ON CONFLICT (name)
DO
    UPDATE
    SET email = EXCLUDED.email || ';' || customers.email;
```

## 12. Transaction

A database transaction is a single unit of work which may consist of one or more operations.

A transaction in PostgreSQL is atomic, consistent, isolated, and durable. These properties are often referred to as ACID:

1. Atomicity guarantees that the transaction completes in an all-or-nothing manner.
2. Consistency ensures the change to data written to the database must be valid and follow predefined rules.
3. Isolation determines how transaction integrity is visible to other transactions.
4. Durability makes sure that transactions which have been committed will be stored permanently.

> **SYNTAX**

```SQL
BEGIN TRANSACTION;
OR
BEGIN WORK;
OR
BEGIN;
```

```SQL
COMMIT TRANSACTION;
OR
COMMIT WORK;
OR
COMMIT;
```

```SQL
ROLLBACK TRANSACTION;
OR
ROLLBACK WORK;
OR
ROLLBACK;
```

## 13. Import & Export

> **EXAMPLE**

```SQL
COPY persons(first_name,last_name,dob,email)
FROM '/home/rajat/Documents/my-work/github/commands/postgresql/data/persons.csv' DELIMITER ',' CSV HEADER;
```

```SQL
COPY persons(first_name,last_name,email)
TO '/home/rajat/Documents/my-work/github/commands/postgresql/data/persons.csv' DELIMITER ',' CSV HEADER;
```

```SQL
\copy (SELECT * FROM persons) to 'C:\tmp\persons_client.csv' with csv;
```
