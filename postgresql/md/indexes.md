# Indexes

* PostgreSQL indexes are effective tools to enhance database performance.
* An index is a separated data structure e.g., B-Tree that speeds up the data retrieval on a table at the cost of additional writes and storage to maintain it.
* To check if a query uses an index or not, you use the EXPLAIN statement.

## PostgreSQL CREATE INDEX

> **SYNTAX**

```SQL
CREATE INDEX index_name
ON table_name
[USING method]
(
  column_name [ASC | DESC] [NULLS {FIRST | LAST }],
  ...
);
```

> **EXAMPLE**

```SQL
CREATE INDEX idx_address_phone
ON address(phone);
```

## PostgreSQL DROP INDEX

> **SYNTAX**

```SQL
DROP INDEX  [ CONCURRENTLY]
[ IF EXISTS ]  index_name
[ CASCADE | RESTRICT ];
```

> **EXAMPLE**

```SQL
DROP INDEX idx_address_phone;
```

## PostgreSQL List Indexes

If you use psql to access the PostgreSQL database, you can use the \d command to view the index information for a table.

```SQL
SELECT
  tablename,
  indexname,
  indexdef
FROM pg_indexes
WHERE schemaname = 'public'
ORDER BY tablename, indexname;
```

```output
   tablename   |                      indexname                      |                                                                   indexdef

---------------+-----------------------------------------------------+--------------------------------------------------------------------------------
---------------------------------------------------------------
 actor         | actor_pkey                                          | CREATE UNIQUE INDEX actor_pkey ON public.actor USING btree (actor_id)
 actor         | idx_actor_last_name                                 | CREATE INDEX idx_actor_last_name ON public.actor USING btree (last_name)
 actor2        | index1                                              | CREATE UNIQUE INDEX index1 ON public.actor2 USING btree (actor_id)
 address       | address_pkey                                        | CREATE UNIQUE INDEX address_pkey ON public.address USING btree (address_id)
 address       | idx_fk_city_id                                      | CREATE INDEX idx_fk_city_id ON public.address USING btree (city_id)
 category      | category_pkey                                       | CREATE UNIQUE INDEX category_pkey ON public.category USING btree (category_id)
 city          | city_pkey                                           | CREATE UNIQUE INDEX city_pkey ON public.city USING btree (city_id)
 city          | idx_fk_country_id                                   | CREATE INDEX idx_fk_country_id ON public.city USING btree (country_id)
 country       | country_pkey                                        | CREATE UNIQUE INDEX country_pkey ON public.country USING btree (country_id)
 customer      | customer_pkey                                       | CREATE UNIQUE INDEX customer_pkey ON public.customer USING btree (customer_id)
 customer      | idx_fk_address_id                                   | CREATE INDEX idx_fk_address_id ON public.customer USING btree (address_id)
 customer      | idx_fk_store_id                                     | CREATE INDEX idx_fk_store_id ON public.customer USING btree (store_id)
 customer      | idx_last_name                                       | CREATE INDEX idx_last_name ON public.customer USING btree (last_name)
 employee      | employee_pkey                                       | CREATE UNIQUE INDEX employee_pkey ON public.employee USING btree (employee_id)
 film          | film_fulltext_idx                                   | CREATE INDEX film_fulltext_idx ON public.film USING gist (fulltext)
 film          | film_pkey                                           | CREATE UNIQUE INDEX film_pkey ON public.film USING btree (film_id)
 film          | idx_fk_language_id                                  | CREATE INDEX idx_fk_language_id ON public.film USING btree (language_id)
 film          | idx_title                                           | CREATE INDEX idx_title ON public.film USING btree (title)
 film_actor    | film_actor_pkey                                     | CREATE UNIQUE INDEX film_actor_pkey ON public.film_actor USING btree (actor_id, film_id)
 film_actor    | idx_fk_film_id                                      | CREATE INDEX idx_fk_film_id ON public.film_actor USING btree (film_id)
 film_category | film_category_pkey                                  | CREATE UNIQUE INDEX film_category_pkey ON public.film_category USING btree (film_id, category_id)
 inventory     | idx_store_id_film_id                                | CREATE INDEX idx_store_id_film_id ON public.inventory USING btree (store_id, film_id)
 inventory     | inventory_pkey                                      | CREATE UNIQUE INDEX inventory_pkey ON public.inventory USING btree (inventory_id)
 language      | language_pkey                                       | CREATE UNIQUE INDEX language_pkey ON public.language USING btree (language_id)
 payment       | idx_fk_customer_id                                  | CREATE INDEX idx_fk_customer_id ON public.payment USING btree (customer_id)
 payment       | idx_fk_rental_id                                    | CREATE INDEX idx_fk_rental_id ON public.payment USING btree (rental_id)
 payment       | idx_fk_staff_id                                     | CREATE INDEX idx_fk_staff_id ON public.payment USING btree (staff_id)
 payment       | payment_pkey                                        | CREATE UNIQUE INDEX payment_pkey ON public.payment USING btree (payment_id)
 persons       | person1                                             | CREATE INDEX person1 ON public.persons USING btree (first_name)
```

To show all the indexes of a table, you use the following statement:

> **SYNTAX**

```SQL
SELECT
  indexname,
  indexdef
FROM pg_indexes
WHERE tablename = 'table_name';
```

## PostgreSQL Index Types

* PostgreSQL has several index types: **B-tree**, **Hash**, **GiST**, **SP-GiST**, **GIN**, and **BRIN**. Each index type uses a different storage structure and algorithm to cope with different kinds of queries.
* When you use the CREATE INDEX statement without specifying the index type, PostgreSQL uses B-tree index type by default because it is best fit the most common queries.
* Note that only B-tree indexes can be declared as unique indexes.

## PostgreSQL UNIQUE Index

> **SYNTAX**

```SQL
CREATE UNIQUE INDEX index_name
ON table_name(column_name, [...]);
```

> **EXAMPLE**

```SQL
CREATE UNIQUE INDEX idx_employees_mobile_phone
ON employees(mobile_phone);
```

```SQL
CREATE UNIQUE INDEX idx_employees_work_phone
ON employees(work_phone, extension);
```

## PostgreSQL Index On Expression

The indexes on expressions are also known as functional-based indexes.

> **SYNTAX**

```SQL
CREATE INDEX index_name
ON table_name (expression);
```

> **EXAMPLE**

```SQL
CREATE INDEX idx_ic_last_name
ON customer(LOWER(last_name));
```

## PostgreSQL Partial Index

PostgreSQL partial index even allows you to specify the rows of a table that should be indexed. This partial index helps speed up the query while reducing the size of the index.

> **SYNTAX**

```SQL
CREATE INDEX index_name
ON table_name(column_list)
WHERE condition;
```

> **EXAMPLE**

```SQL
CREATE INDEX idx_customer_inactive
ON customer(active)
WHERE active = 0;
```

## PostgreSQL REINDEX

> **SYNTAX**

```SQL
REINDEX [ ( VERBOSE ) ] { INDEX | TABLE | SCHEMA | DATABASE | SYSTEM } name;
```

```SQL
REINDEX INDEX index_name;
```

```SQL
REINDEX TABLE table_name;
```

```SQL
REINDEX SCHEMA schema_name;
```

```SQL
REINDEX DATABASE database_name;
```

```SQL
REINDEX SYSTEM database_name;
```

## PostgreSQL Multicolumn Indexes

* You can create an index on more than one column of a table. This index is called a multicolumn index, a composite index, a combined index, or a concatenated index.
* A multicolumn index can have maximum 32 columns of a table. The limit can be changed by modifying the pg_config_manual.h when building PostgreSQL.
* In addition, only B-tree, GIST, GIN, and BRIN index types support multicolumn indexes.

> **SYNTAX**

```SQL
CREATE INDEX index_name
ON table_name(a,b,c,...);
```

> **EXAMPLE**

```SQL
CREATE INDEX idx_people_names
ON people (last_name, first_name);
```
