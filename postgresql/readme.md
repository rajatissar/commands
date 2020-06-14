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

1. [Download PostgreSQL for Ubuntu](https://www.postgresql.org/download/linux/ubuntu/)
2. [PostgreSQL tutorial](https://www.postgresqltutorial.com/)
3. [17 Practical psql Commands That You Don’t Want To Miss](https://www.postgresqltutorial.com/psql-commands/)

## 4. [Terminology](/postgresql/md/terminology.md)

## 5. [Commands](/postgresql/md/commands.md)

## 6. [Queries](/postgresql/md/queries.md)

## 7. [Stored Procedures](/postgresql/md/stored-procedures.md)

## 8. [Triggers](/postgresql/md/triggers.md)

## 9. [Views](/postgresql/md/views.md)

## 10. [Indexes](/postgresql/md/indexes.md)

## 11. [Administration](/postgresql/md/administration.md)

## 12. [Notes :pushpin:](/postgresql/md/notes.md)
