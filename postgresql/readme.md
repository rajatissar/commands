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

- alias :- A PostgreSQL alias assigns a table or a column a temporary name in a query. The aliases only exist during the execution of the query.

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
- Because PostgreSQL evaluates the ORDER BY clause after the SELECT clause, you can use the column alias in the ORDER BY clause.
- For the other clauses evaluated before the SELECT clause such as WHERE, GROUP BY, and HAVING, you cannot reference the column alias in these clauses.
- When you join a table to itself a.k.a self-join, you must use table aliases.
