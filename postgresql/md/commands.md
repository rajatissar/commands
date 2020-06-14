# Commands

## 1. Check current PostgreSQL version that you have in the system

```SSH
$ SELECT version();
---------------------------------------------------------------------------------------------------------------------------------------------
PostgreSQL 12.3 (Ubuntu 12.3-1.pgdg16.04+1) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609, 64-bit
(1 row)
```

## 2. Check PostgreSQL running status

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

```SSH
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

## 3. Show psql query results more clearly

```SSH
dvdrental=# \x on
Expanded display is on.
```

## 4. Database

### Check list of Databases

```SSH
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

```SQL
SELECT
    datname
FROM
    pg_database;
```

### Connect to Database

```SSH
postgres=# \c dvdrental
You are now connected to database "dvdrental" as user "postgres".
```

### Disconnect from Database

By connecting to another database, you are automatically disconnected from the database to which you connected.

```SSH
dvdrental-# \c postgres
You are now connected to database "postgres" as user "postgres".
```

### Import Database

```SSH
postgres@rajat-Inspiron-3542:~$ pg_restore -U postgres -d dvdrental C:\dvdrental\dvdrental.tar
Import database
```

### Check Database size

```SQL
SELECT pg_size_pretty ( pg_database_size ('dvdrental') );
```

### Check all databases size

```SQL
SELECT
    pg_database.datname,
    pg_size_pretty(pg_database_size(pg_database.datname)) AS size
FROM pg_database;
```

## 5. TABLE

### Check list of tables

```SSH
dvdrental=# \dt
\             List of relations
 Schema |     Name      | Type  |  Owner
--------+---------------+-------+----------
 public | actor         | table | postgres
 public | address       | table | postgres
 public | category      | table | postgres
 public | city          | table | postgres
 public | country       | table | postgres
 public | customer      | table | postgres
 public | employee      | table | postgres
 public | film          | table | postgres
 public | film_actor    | table | postgres
 public | film_category | table | postgres
 public | inventory     | table | postgres
 public | language      | table | postgres
 public | payment       | table | postgres
 public | persons       | table | postgres
 public | rental        | table | postgres
 public | staff         | table | postgres
 public | store         | table | postgres
(17 rows)
```

```SQL
SELECT
    *
FROM
    pg_catalog.pg_tables
WHERE
    schemaname != 'pg_catalog'
AND schemaname != 'information_schema';
```

### Check structure of Table

```SSH
dvdrental=# \d actor;
                                            Table "public.actor"
   Column    |            Type             | Collation | Nullable |                 Default
-------------+-----------------------------+-----------+----------+-----------------------------------------
 actor_id    | integer                     |           | not null | nextval('actor_actor_id_seq'::regclass)
 first_name  | character varying(45)       |           | not null |
 last_name   | character varying(45)       |           | not null |
 last_update | timestamp without time zone |           | not null | now()
Indexes:
    "actor_pkey" PRIMARY KEY, btree (actor_id)
    "idx_actor_last_name" btree (last_name)
Referenced by:
    TABLE "film_actor" CONSTRAINT "film_actor_actor_id_fkey" FOREIGN KEY (actor_id) REFERENCES actor(actor_id) ON UPDATE CASCADE ON DELETE RESTRICT
Triggers:
    last_updated BEFORE UPDATE ON actor FOR EACH ROW EXECUTE FUNCTION last_updated()
```

```SQL
SELECT
    COLUMN_NAME
FROM
    information_schema.COLUMNS
WHERE
    TABLE_NAME = 'city';
```

### Table size

**pg_relation_size** is used to calculate the size of table.

* The pg_relation_size() function returns the size of a specific table in bytes.
* The pg_relation_size() function returns the size of the table only, not included indexes or additional objects.
* To get the total size of a table, you use the pg_total_relation_size() function.

```SQL
SELECT pg_relation_size('table_name');
```

```SQL
SELECT pg_size_pretty (pg_relation_size('actor'));
```

### Indexes size

```SQL
SELECT pg_size_pretty (pg_indexes_size('actor'));
```

### Tablespace size

```SQL
SELECT pg_size_pretty ( pg_tablespace_size ('pg_default'));
```

### Value size

To find how much space that needs to store a specific value

```SQL
SELECT pg_column_size(5::smallint);
```

## 5. View

### Check list of Views

```SSH
dvdrental=# \dv
                   List of relations
 Schema |            Name            | Type |  Owner
--------+----------------------------+------+----------
 public | actor_info                 | view | postgres
 public | customer_list              | view | postgres
 public | film_list                  | view | postgres
 public | nicer_but_slower_film_list | view | postgres
 public | sales_by_film_category     | view | postgres
 public | sales_by_store             | view | postgres
 public | staff_list                 | view | postgres
(7 rows)
```

## 6. Schema

### Check list of Schema

```SSH
dvdrental=# \dv
                   List of relations
 Schema |            Name            | Type |  Owner
--------+----------------------------+------+----------
 public | actor1                     | view | postgres
 public | actor_info                 | view | postgres
 public | customer_list              | view | postgres
 public | film_list                  | view | postgres
 public | nicer_but_slower_film_list | view | postgres
 public | sales_by_film_category     | view | postgres
 public | sales_by_store             | view | postgres
 public | staff_list                 | view | postgres
(8 rows)
```

## 7. Function

### Check list of functions

```SSH
dvdrental=# \df
                                                          List of functions
 Schema |            Name            | Result data type |                         Argument data types                         | Type
--------+----------------------------+------------------+---------------------------------------------------------------------+------
 public | _group_concat              | text             | text, text                                                          | func
 public | film_in_stock              | SETOF integer    | p_film_id integer, p_store_id integer, OUT p_film_count integer     | func
 public | film_not_in_stock          | SETOF integer    | p_film_id integer, p_store_id integer, OUT p_film_count integer     | func
 public | fun1                       | integer          | param1 integer                                                      | func
 public | get_customer_balance       | numeric          | p_customer_id integer, p_effective_date timestamp without time zone | func
 public | group_concat               | text             | text                                                                | agg
 public | inventory_held_by_customer | integer          | p_inventory_id integer                                              | func
 public | inventory_in_stock         | boolean          | p_inventory_id integer                                              | func
 public | last_day                   | date             | timestamp without time zone                                         | func
 public | last_updated               | trigger          |                                                                     | func
 public | rewards_report             | SETOF customer   | min_monthly_purchases integer, min_dollar_amount_purchased numeric  | func
(11 rows)
```

## 8. Users

### Check list users and their roles

```SSH
dvdrental=# \du
                                   List of roles
 Role name |                         Attributes                         | Member of
-----------+------------------------------------------------------------+-----------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 rajat     | Cannot login                                              +| {}
           | Password valid until 2020-06-14 00:00:00+05:30             |
 rajat1    | Superuser, Cannot login                                    | {}
 rajat3    | Create role, Cannot login                                  | {}
```

## 9. Command history

```SSH
dvdrental=# \s
SELECT fun1(1);
\c dvdrental
\dv
\df
\du
\s
```

```SSH
dvdrental=# \s /home/rajat/Documents/my-work/github/commands
```

## 10. Execute psql commands from a file

```SSH
dvdrental=# \i /home/rajat/Documents/my-work/github/commands
```

## 11. Turn on/off query execution time

```SSH
dvdrental=# \timing ON
```

```SSH
dvdrental=# \timing OFF
```

## 12.Edit command in your own editor

```SSH
dvdrental=# \e
```
