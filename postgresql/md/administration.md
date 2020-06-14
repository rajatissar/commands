# Administration

## PostgreSQL Roles Management

* A role can be a user or a group, depending on how you setup the role.
* A role that has login right is called user.
* A role may be a member of other roles, which are known as groups.

> **SYNTAX**

```SQL
CREATE ROLE role_name;
```

To get all available roles in the cluster

```SQL
SELECT
  rolname
FROM
  pg_roles;
```

If you use the psql tool, you can use the **\du** command to list all existing roles.

```SQL
\du
```

### Role attributes

> **EXAMPLE**

```SQL
CREATE ROLE rajat1 WITH PASSWORD '14' VALID UNTIL '2020-06-14';
```

```SQL
CREATE ROLE rajat2 SUPERUSER;
```

Notice that you must be a superuser in order to create another superuser.

If you want a role to have database creation privilege, you use the following statement:

```SQL
CREATE ROLE rajat3 CREATEDB;
```

Use the following statement to create a role that has creation privilege:

```SQL
CREATE ROLE rajat4 CREATEROLE;
```

### Role membership

By convention, a group role does not have LOGIN privilege.

> **SYNTAX**

```SQL
CREATE ROLE group_role;
```

> **EXAMPLE**

```SQL
CREATE ROLE sales;
```

Now, you can add a user role to a group role by using the GRANT statement:

> **SYNTAX**

```SQL
GRANT group_role to user_role;
```

For example, to add the doe user role to the sales group role, you use the following statement:

> **EXAMPLE**

```SQL
GRANT sales TO doe;
```

To remove a user role from a group role, you use REVOKE statement:

> **SYNTAX**

```SQL
REVOKE group_role FROM user_role;
```

> **EXAMPLE**

```SQL
REVOKE sales FROM doe;
```

### Group and user role inheritance

A user role can use privileges of the group role in the following ways:

* First, a user role can use the SET ROLE statement to temporarily become the group role, which means the user role use privileges of the group role rather than the original privileges. In addition, any database objects created in the session are owned by the group role, instead of the user role.
* Second, a user role that has the INHERIT attribute will automatically have the privileges of the group roles of which it is a member, including all privileges inherited by the group roles.

> **EXAMPLE**

```SQL
CREATE ROLE doe LOGIN INHERIT;
CREATE ROLE sales NOINHERIT;
CREATE ROLE marketing NOINHERIT;
GRANT sales to doe;
GRANT marketing to sales;
```

To restore the original privilege, you can use the following statement:

```SQL
RESET ROLE;
```

### Removing roles

> **SYNTAX**

```SQL
DROP ROLE role_name;
```

Before removing a role, you must reassign or remove all objects it owns and revoke its privileges.

## PostgreSQL Tablespaces

A tablespace is a location on disk where PostgreSQL stores data files containing database objects e.g., indexes., and tables. PostgreSQL uses a tablespace to map a logical name to a physical location on disk.

PostgreSQL comes with two default tablespaces:

1. **pg_default** tablespace stores all user data.
2. **pg_global** tablespace stores all global data.

### CREATE TABLESPACE

> **SYNTAX**

```SQL
CREATE TABLESPACE tablespace_name
OWNER user_name
LOCATION directory_path;
```

The name of the tablespace should not begin with pg_, because these names are reserved for the system tablespaces.

> **EXAMPLE**

```SQL
CREATE TABLESPACE dvdrental LOCATION 'c:\data\dvdrental';
```

```SSH
# chown postgres /usr/data/tablespace_dir
change to the owner of the data directory to postgres
```

### ALTER TABLESPACE

Currently, PostgreSQL does not support change the location of the tablespace.

> **SYNTAX**

```SQL
ALTER TABLESPACE action;
```

To change the name of the tablespace, use the following statement:

> **SYNTAX**

```SQL
ALTER TABLESPACE tablespace_name RENAME TO new_name;
```

> **EXAMPLE**

```SQL
ALTER TABLESPACE dvdrental RENAME TO dvdrental_raid;
```

To change the owner of the tablespace, use the following statement:

> **SYNTAX**

```SQL
ALTER TABLESPACE tablespace_name OWNER TO new_owner;
```

> **EXAMPLE**

```SQL
ALTER TABLESPACE dvdrental_raid OWNER to hr;
```

You can change the tablespace ’s parameters including seq_page_cost and random_page_cost, which specify the cost of reading pages from tables in the tablespace.

> **SYNTAX**

```SQL
ALTER TABLESPACE tablespace_name SET parameter = value;
```

### DROP TABLESPACE

* Only tablespace owner or the superuser can delete the tablespace.
* Before deleting the tablespace, make sure that it is empty, which means there are no database objects inside it.

> **SYNTAX**

```SQL
DROP TABLESPACE IF EXISTS tablespace_name;
```

> **EXAMPLE**

```SQL
DROP DATABASE db_demo;
```

```SQL
DROP TABLESPACE demo;
```

## Backup Databases

Before backing up the databases, you should consider the following points:

* Full / partial databases
* Both data and structures, or only structures
* Point In Time recovery
* Restore performance

> **EXAMPLE**

```SQL
> pg_dump -U username -W -F t database_name > c:\backup_file.tar
```

### How to backup one database

> **EXAMPLE**

```SSH
C:\>cd C:\Program Files\PostgreSQL\9.2\bin
```

```SQL
>pg_dump -U postgres -W -F t dvdrental > c:\pgbackup\dvdrental.tar
```

### How to backup all databases

> **EXAMPLE**

```SQL
>pg_dumpall -U postgres > c:\pgbackup\all.sql
```

### How to backup database object definitions

> **EXAMPLE**

```SSH
>pg_dumpall --schema-only > c:\pgdump\definitiononly.sql
```

```SSH
>pg_dumpall --roles-only > c:\pgdump\allroles.sql
```

```SSH
>pg_dumpall --tablespaces-only > c:\pgdump\allroles.sql
```

## Restore Database

Before restoring a database, you need to terminate all connections to that database and prepare the backup file. In PostgreSQL, you can restore a database in two ways:

1. Using psql to restore plain SQL script file generated by **pg_dump** and **pg_dumpall** tools.
2. Using **pg_restore** to restore tar file and directory format created by the **pg_dump** tool.

> **SYNTAX**

```SQL
>psql -U username -f backup_file.sql
```

```SQL
>psql -U username -d database_name -f objects.sql
```

### How to restore databases using pg_restore

> **EXAMPLE**

```SQL
CREATE DATABASE new_dvdrental;
```

```SSH
>pg_restore --dbname=new_dvdrental --verbose c:\pgbackup\dvdrental.tar
```

As PostgreSQL 9.2, you can use the  --section option to restore table structure only.

```SSH
>pg_restore --dbname=dvdrental_tpl --section=pre-data  c:\pgbackup\dvdrental.tar
```

## PostgreSQL Tips

### Change The Password of a PostgreSQL User

* Note that if you omit the VALID UNTIL clause, the password will be valid for all time.

> **SYNTAX**

```SQL
ALTER ROLE username
WITH PASSWORD 'password';
```

> **EXAMPLE**

```SQL
ALTER ROLE super
WITH PASSWORD 'secret123';
```

> **SYNTAX**

```SQL
ALTER ROLE username
WITH PASSWORD 'new_password'
VALID UNTIL timestamp;
```

> **EXAMPLE**

```SQL
ALTER ROLE super
VALID UNTIL 'December 31, 2020';
```

### Reset Forgotten Password For postgres User

* Backup the pg_dba.conf file by copying it to a different location or just rename it to pg_dba_bk.conf
* Edit the pg_dba.conf file by adding the following line as the first line after the comment lines. The comment line starts with the # sign.

```TEXT
local  all   all   trust
```

OR

```TEXT
host    all              postgres        127.0.0.1/32            trust
```

* Restart PostgreSQL server e.g., in Linux, you use the following command:

```SSH
sudo /etc/init.d/postgresql restart
```

* Connect to PostgreSQL database server and change the password of the postgres user.

```SQL
ALTER USER postgres with password 'very_secure_password';
```

* Restore the pg_db.conf file and restart the server, and connect to the PostgreSQL database server with new password.

```SSH
sudo /etc/init.d/postgresql restart
```
