# Commands

## 1. Check current PostgreSQL version that you have in the system

```ssh
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

## 3. Show psql query results more clearly

```ssh
dvdrental=# \x on
Expanded display is on.
```

## 4. Database

- List of Databases

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

- Connect Database

```ssh
postgres=# \c dvdrental
You are now connected to database "dvdrental" as user "postgres".
```

- Disconnect Database

By connecting to another database, you are automatically disconnected from the database to which you connected.

```ssh
dvdrental-# \c postgres
You are now connected to database "postgres" as user "postgres".
```

- Import Database

```ssh
postgres@rajat-Inspiron-3542:~$ pg_restore -U postgres -d dvdrental C:\dvdrental\dvdrental.tar
Import database
```

- Database Size

```SQL
SELECT
    pg_size_pretty (
        pg_database_size ('dvdrental')
    );
```

- All Database size

```SQL
SELECT
    pg_database.datname,
    pg_size_pretty(pg_database_size(pg_database.datname)) AS size
    FROM pg_database;
```

## 5. TABLE

- Table size

pg_relation_size is used to calculate the size of table.

- The pg_relation_size() function returns the size of a specific table in bytes.
- The pg_relation_size() function returns the size of the table only, not included indexes or additional objects.
- To get the total size of a table, you use the pg_total_relation_size() function.

```SQL
SELECT pg_relation_size('table_name');
```

```SQL
SELECT pg_size_pretty (pg_relation_size('actor'));
```

- Indexes size

```SQL
SELECT
    pg_size_pretty (pg_indexes_size('actor'));
```

- Tablespace size

```SQL
SELECT
    pg_size_pretty (
        pg_tablespace_size ('pg_default')
    );
```

- Value size
To find how much space that needs to store a specific value

```SQL
SELECT pg_column_size(5::smallint);
```
