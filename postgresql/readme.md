# PostgreSQL

PostgreSQL is a general purpose and object-relational database management system, the most advanced open source database system.

## 1. Installation

```ssh
 $ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
 Import the signing key
 $ sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
 Import the repository
 $ sudo apt-get update
 update the package lists
 $ sudo apt-get install postgresql postgresql-contrib
 Install required packages
```

## 2. Usage

```ssh
rajat@rajat-Inspiron-3542:~$ sudo su - postgres
change to postgres user
postgres@rajat-Inspiron-3542:~$ psql
open psql
postgres=# \c dvdrental
Connect to dvdrental database
```

## 3. Popular Links

- [PostgreSQL Download Ubuntu](https://www.postgresql.org/download/linux/ubuntu/)
- [PostgreSQL tutorial](https://www.postgresqltutorial.com/)

## 4. [Terminology](https://github.com/rajatissar/commands/blob/master/postgresql/md/terminology.md)

## 5. [Commands](https://github.com/rajatissar/commands/blob/master/postgresql/md/commands.md)

## 6. [Queries](https://github.com/rajatissar/commands/blob/master/postgresql/md/queries.md)

## 7. [Stored Procedures](https://github.com/rajatissar/commands/blob/master/postgresql/md/stored-procedures.md)

## 8. [Notes :pushpin:](https://github.com/rajatissar/commands/blob/master/postgresql/md/notes.md)
