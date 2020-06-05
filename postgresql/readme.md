# PostgreSQL

PostgreSQL is a general purpose and object-relational database management system, the most advanced open source database system.

## 1. Popular Links

[PostgreSQL tutorial](https://www.postgresqltutorial.com/)

## 2. Terminology

### (a). Databases

A database is a container of other objects such as tables, views, functions, and indexes. You can create as many databases as you want inside a PostgreSQL server.

### (b). Tables

Tables store data. A table belongs to a database and each database has multiple tables.

### (c). Schemas

A schema is a logical container of tables and other objects inside a database. Each PostgreSQL database may have multiple schemas. It is important to note that schema is a part of the ANSI-SQL standard.

### (d). Tablespaces

Tablespaces are where PostgreSQL stores the data. Tablespaces allow you to move your data to different physical locations across drivers easily by using simple commands.

By default, PostgreSQL provides you with two tablespaces:

- The pg_default is for storing user data.
- The pg_global  is for storing system data.

### (e). Views

Views are named query stored in the database. Besides the read-only views, PostgreSQL support updatable views.

### (f). Functions

A function is a reusable block of SQL code that returns a scalar value of a set of rows.

### (g). Operators

Operators are symbolic functions. PostgreSQL allows you to define custom operators.

### (h). Casts

Casts enable you to convert one data type into another data type. Casts backed by functions to perform the conversion. You can also create your casts to override the default casting provided by PostgreSQL.

### (i). Sequence

Sequences are used to manage auto-increment columns that defined in a table as a serial column.

### (j). Extension

PostgreSQL introduced extension concept since version 9.1 to wrap other objects including types, casts, indexes, functions, etc., into a single unit.  The purpose of extensions is to make it easier to maintain.

## 3. Commands

### (a). Current PostgreSQL version that you have in the system

```ssh
$ SELECT version();
---------------------------------------------------------------------------------------------------------------------------------------------
 PostgreSQL 12.3 (Ubuntu 12.3-1.pgdg16.04+1) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609, 64-bit
(1 row)
```

### (b). postgresql status

```sh
$ sudo /etc/init.d/postgresql status
● postgresql.service - PostgreSQL RDBMS
   Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor preset: enabled)
   Active: active (exited) since Wed 2020-06-03 14:28:33 IST; 2min 47s ago
  Process: 1550 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
 Main PID: 1550 (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/postgresql.service

Jun 03 14:28:33 rajat-Inspiron-3542 systemd[1]: Starting PostgreSQL RDBMS...
Jun 03 14:28:33 rajat-Inspiron-3542 systemd[1]: Started PostgreSQL RDBMS.
```

```ssh
$ service postgresql status
● postgresql.service - PostgreSQL RDBMS
   Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor preset: enabled)
   Active: active (exited) since Wed 2020-06-03 14:28:33 IST; 13min ago
  Process: 1550 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
 Main PID: 1550 (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/postgresql.service

Jun 03 14:28:33 rajat-Inspiron-3542 systemd[1]: Starting PostgreSQL RDBMS...
Jun 03 14:28:33 rajat-Inspiron-3542 systemd[1]: Started PostgreSQL RDBMS.
```

### (c). Import Database

```sh
postgres@rajat-Inspiron-3542:~$ pg_restore -U postgres -d dvdrental C:\dvdrental\dvdrental.tar
Import database
```

### (d). Database

```ssh
postgres=# \l
                             List of databases
   Name    |  Owner   | Encoding | Collate | Ctype |   Access privileges
-----------+----------+----------+---------+-------+-----------------------
 dvdrental | postgres | UTF8     | en_IN   | en_IN |
 postgres  | postgres | UTF8     | en_IN   | en_IN |
 template0 | postgres | UTF8     | en_IN   | en_IN | =c/postgres          +
           |          |          |         |       | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_IN   | en_IN | =c/postgres          +
           |          |          |         |       | postgres=CTc/postgres
(4 rows)
```

```ssh
postgres=# \c dvdrental
You are now connected to database "dvdrental" as user "postgres".
```

## 4. SQL Queries

### (a). SELECT

|Query|Explanation|
|---|---|
|DISTINCT|Distinct rows|
|ORDER BY|Sort rows|
|WHERE|Filter rows|
|LIMIT / FETCH|subset of rows|
|GROUP BY|Group rows into groups|
|HAVING|Filter groups|
|JOIN|Join with other tables|
|UNION / INTERSECT / EXCEPT|Perform set operations|

```SQL
SELECT
  select_list
FROM
  table_name;
```

- concat operator

```SQL
SELECT first_name || '-' || last_name as full_name from customer;
```

- PostgreSQL as Simple Calculator

```SQL
SELECT 5 * 3 AS result;
```

- ORDER BY

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

- LENGTH

```SQL
SELECT
  first_name,
  LENGTH(first_name) len
FROM
  customer
ORDER BY
  LENGTH(first_name) DESC;
```

- DISTINCT

```SQL
SELECT
   DISTINCT column_1, column_2
FROM
   table_name;
```

- DISTINCT ON

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

- WHERE

```SQL
SELECT select_list
FROM table_name
WHERE condition;
```

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

- LIMIT & OFFSET

```SQL
SELECT
  *
FROM
  table
LIMIT n OFFSET m;
```

- FETCH

```SQL
OFFSET start { ROW | ROWS }
FETCH { FIRST | NEXT } [ row_count ] { ROW | ROWS } ONLY
```

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

- IN

```SQL
value IN (value1,value2,...)
```

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

- BETWEEN

```SQL
value BETWEEN low AND high;
```

```SQL
value NOT BETWEEN low AND high;
```

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

- LIKE

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

- IS NULL

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

- ALIAS :- A PostgreSQL alias assigns a table or a column a temporary name in a query. The aliases only exist during the execution of the query.

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

### (b). JOIN

PostgreSQL join is used to combine columns from one (self-join) or more tables based on the values of the common columns between the tables. The common columns are typically the primary key columns of the first table and foreign key columns of the second table.

- INNER JOIN

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

- LEFT JOIN

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

- RIGHT JOIN

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

- FULL OUTER JOIN

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

- SELF JOIN

A self-join is a query in which a table is joined to itself. Self-joins are useful for comparing values in a column of rows within the same table.
To form a self-join, you specify the same table twice with different aliases, set up the comparison, and eliminate cases where a value would be equal to itself.

```SQL
SELECT column_list
FROM A a1
INNER JOIN A b1 ON join_predicate;
```

```SQL
SELECT
    e.first_name || ' ' || e.last_name employee,
    m .first_name || ' ' || m .last_name manager
FROM
    employee e
INNER JOIN employee m ON e.manager_id = m .employee_id;
```

- CROSS JOIN

A CROSS JOIN clause allows you to produce the Cartesian Product of rows in two or more tables. Different from the other JOIN operators such as LEFT JOIN  or INNER JOIN, the CROSS JOIN does not have any matching condition in the join clause.
Suppose we have to perform the CROSS JOIN of two tables T1 and T2. For every row from T1 and T2 i.e., a cartesian product, the result set will contain a row that consists of all columns in the T1 table followed by all columns in the T2 table. If T1 has N rows, T2 has M rows, the result set will have N x M rows.

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

- NATURAL JOIN

A natural join is a join that creates an implicit join based on the same column names in the joined tables.
If you use the asterisk (*) in the select list, the result will contain the following columns:

- All the common columns, which are the columns in the both tables that have the same name.
- Every column in the first and second tables that is not a common column.

```SQL
SELECT *
FROM T1
NATURAL [INNER, LEFT, RIGHT] JOIN T2;
```

```SQL
SELECT *
FROM products
NATURAL JOIN categories;
```

![JOIN](./assets/PostgreSQL-Joins.png)

### (c). Grouping Data

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

### (d). Set Operation

- UNION

The UNION operator combines result sets of two or more SELECT statements into a single result set.

The following are rules applied to the queries:

- Both queries must return the same number of columns.
- The corresponding columns in the queries must have compatible data types.

```SQL
SELECT *
FROM
  sales2007q1
UNION
SELECT *
FROM
  sales2007q2;
```

- INTERSECT

INTERSECT operator combines the result sets of two or more SELECT statements into a single result set. The INTERSECT operator returns any rows that are available in both result set or returned by both queries.

To use the INTERSECT operator, the columns that appear in the SELECT statements must follow the rules below:

- The number of columns and their order in the SELECT clauses must the be the same.
- The data types of the columns must be compatible.

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

![INTERSECT-Operator](./assets/PostgreSQL-INTERSECT-Operator-300x206.png)

- EXCEPT

The EXCEPT operator returns distinct rows from the first (left) query that are not in the output of the second (right) query.

To combine the queries using the EXCEPT operator, you must obey the following rules:

- The number of columns and their orders must be the same in the two queries.
- The data types of the respective columns must be compatible.

```SQL
SELECT column_list
FROM A
WHERE condition_a
EXCEPT
SELECT column_list
FROM B
WHERE condition_b;
```

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

![EXCEPT-Operator](./assets/PostgreSQL-EXCEPT-300x202.png)

### (e). Grouping Sets

A grouping set is a set of columns to which you want to group.

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

- CUBE

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

- ROLLUP

Different from the CUBE subclause, ROLLUP does not generate all possible grouping sets based on the specified columns. It just makes a subset of those.
A common use of  ROLLUP is to calculate the aggregations of data by year, month, and date, considering the hierarchy year > month > date.

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

### (f). Sub Query

The query inside the brackets is called a sub query or an inner query. The query that contains the sub query is known as an outer query.

PostgreSQL executes the query that contains a sub query in the following sequence:

- First, executes the sub query.
- Second, gets the result and passes it to the outer query.
- Third, executes the outer query.

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

- ANY

ANY operator compares a value to a set of values returned by a sub query. The following illustrates the syntax of  the ANY operator:

```SQL
expression operator ANY(sub query)
```

In this syntax:

- The sub query must return exactly one column.
- The ANY operator must be preceded by one of the following comparison operator =, <=, >, <, > and <>
- The ANY operator returns true if any value of the sub query meets the condition, otherwise, it returns false.

```SQL
SELECT
    title,
    category_id
FROM
    film
INNER JOIN film_category
        USING(film_id)
WHERE
    category_id = ANY(
        SELECT
            category_id
        FROM
            category
        WHERE
            NAME = 'Action'
            OR NAME = 'Drama'
    );
```

- ALL

ALL operator allows you to query data by comparing a value with a list of values returned by a sub query.

```SQL
comparison_operator ALL (sub query)
```

- The ALL operator must be preceded by a comparison operator such as equal (=), not equal (!=), greater than (>), greater than or equal to (>=), less than (<), and less than or equal to (<=).
- The ALL operator must be followed by a sub query which also must be surrounded by the parentheses.

With the assumption that the sub query returns some rows, the ALL operator works as follows:

- column_name > ALL (sub query) the expression evaluates to true if a value is greater than the biggest value returned by the sub query.
- column_name >= ALL (sub query) the expression evaluates to true if a value is greater than or equal to the biggest value returned by the sub query.
- column_name < ALL (sub query) the expression evaluates to true if a value is less than the smallest value returned by the sub query.
- column_name <= ALL (sub query) the expression evaluates to true if a value is less than or equal to the smallest value returned by the sub query.
- column_name = ALL (sub query) the expression evaluates to true if a value is equal to any value returned by the sub query.
- column_name != ALL (sub query) the expression evaluates to true if a value is not equal to any value returned by the sub query.

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
ORDER BY
    length;
```

- EXISTS

EXISTS operator is used to test for existence of rows in a sub query.

A sub query can be an input of the EXISTS operator. If the sub query returns any row, the EXISTS operator returns true. If the sub query returns no row, the result of EXISTS operator is false.

```SQL
EXISTS (sub query)
```

```SQL
SELECT first_name,
       last_name
FROM customer c
WHERE EXISTS
    (SELECT 1
     FROM payment p
     WHERE p.customer_id = c.customer_id
       AND amount > 11 )
ORDER BY first_name,
         last_name;
```

```SQL
SELECT first_name,
       last_name
FROM customer c
WHERE NOT EXISTS
    (SELECT 1
     FROM payment p
     WHERE p.customer_id = c.customer_id
       AND amount > 11 )
ORDER BY first_name,
         last_name;
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

### (f). CTE (Common Table Expressions)

```SQL
WITH cte_name (column_list) AS (
    CTE_query_definition
)
statement;
```

In this syntax:

- First, specify the name of the CTE following by an optional column list.
- Second, inside the body of the WITH clause, specify a query that returns a result set. If you do not explicitly specify the column list after the CTE name, the select list of the CTE_query_definition will become the column list of the CTE.
- Third, use the CTE like a table or view in the statement which can be a SELECT, INSERT, UPDATE, or DELETE.

```SQL
WITH cte_film AS (
    SELECT
        film_id,
        title,
        (CASE
            WHEN length < 30 THEN 'Short'
            WHEN length < 90 THEN 'Medium'
            ELSE 'Long'
        END) length
    FROM
        film
)
SELECT
    film_id,
    title,
    length
FROM
    cte_film
WHERE
    length = 'Long'
ORDER BY
    title;
```

- Recursive Query

A recursive query is a query that refers to a recursive CTE. The recursive queries are useful in many situations such as for querying hierarchical data like organizational structure, bill of materials, etc.

```SQL
WITH RECURSIVE cte_name AS(
    CTE_query_definition -- non-recursive term
    UNION [ALL]
    CTE_query definion  -- recursive term
) SELECT * FROM cte_name;
```

A recursive CTE has three elements:

- Non-recursive term: the non-recursive term is a CTE query definition that forms the base result set of the CTE structure.
- Recursive term: the recursive term is one or more CTE query definitions joined with the non-recursive term using the UNION or UNION ALL operator. The recursive term references the CTE name itself.
- Termination check: the recursion stops when no rows are returned from the previous iteration.

PostgreSQL executes a recursive CTE in the following sequence:

- Execute the non-recursive term to create the base result set (R0).
- Execute recursive term with Ri as an input to return the result set Ri+1 as the output.
- Repeat step 2 until an empty set is returned. (termination check)
- Return the final result set that is a UNION or UNION ALL of the result set R0, R1, … Rn

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

## 5. Notes

- SQL language is case insensitive. It means that SELECT or select has the same effect.
- The semicolon is not a part of the SQL statement. It is used to signal PostgreSQL the end of an SQL statement. The semicolon is also used to separate two SQL statements.
- ORDER BY uses ASC by default.
- SQL standard only allows you to sort rows based on the columns that appear in the SELECT clause. However, PostgreSQL allows you to sort rows based on the columns that even does not appear in the selection list
- The order of rows returned from the SELECT statement is unpredictable therefore the “first” row of each group of the duplicate is also unpredictable. It is good practice to always use the ORDER BY clause with the DISTINCT ON(expression) to make the result set obvious.
- LIMIT- The statement returns n rows generated by the query. If n is zero, the query returns an empty set. In case n is NULL, the query returns the same result set as omitting the LIMIT clause.
- Because the order of the rows in the database table is unspecified, when you use the LIMIT clause, you should always use the ORDER BY clause to control the row order. If you don’t do so, you will get a result set whose rows are in an unspecified order.
- LIMIT clause is not a SQL-standard. To conform with the SQL standard, PostgreSQL provides the FETCH clause to retrieve a portion of rows returned by a query. Note that the FETCH clause was introduced in SQL:2008.
- PostgreSQL executes the query with the IN operator much faster than the same query that uses a list of OR operators.
- BETWEEN - If you want to check a value against of date ranges, you should use the literal date in ISO 8601 format i.e., YYYY-MM-DD.
- LIKE - If the pattern does not contain any wildcard character, the LIKE operator acts like the equal ( =) operator.
- NULL - The comparison of NULL with a value will always result in NULL, which means an unknown result.
- Because PostgreSQL evaluates the ORDER BY clause after the SELECT clause, you can use the column alias in the ORDER BY clause. Other clauses evaluated before the SELECT clause such as WHERE, GROUP BY, and HAVING, you cannot reference the column alias in these clauses.
- When you join a table to itself a.k.a self-join, you must use table aliases.
- JOIN - A natural join can be an inner join, left join, or right join. If you do not specify a join explicitly e.g., INNER JOIN, LEFT JOIN, RIGHT JOIN, PostgreSQL will use the INNER JOIN by default.
- GROUP BY - The GROUP BY clause must appear right after the FROM or WHERE clause.
- GROUP BY - The GROUP BY clause is useful when it is used in conjunction with an aggregate function
- GROUP BY - To filter groups, you use the HAVING clause instead of WHERE clause.
- HAVING - The HAVING clause sets the condition for group rows created by the GROUP BY clause after the GROUP BY clause applies while the WHERE clause sets the condition for individual rows before GROUP BY clause applies. This is the main difference between the HAVING and WHERE clauses.
- HAVING - In PostgreSQL, you can use the HAVING clause without the GROUP BY clause. In this case, the HAVING clause will turn the query into a single group.
- PostgreSQL provides the GROUPING SETS, CUBE which is the subclause of the GROUP BY clause.
- ANY / SOME - SOME is a synonym for ANY, meaning that you can substitute SOME for ANY in any SQL statement.
- ANY - The = ANY is equivalent to IN operator.
- ANY - The <> ANY operator is different from NOT IN. The following expression:

```SQL
x <> ANY (a,b,c)
is equivalent to
x <> a OR <> b OR x <> c
```
