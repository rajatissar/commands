# Terminology

## 1. Databases

A database is a container of other objects such as tables, views, functions, and indexes. You can create as many databases as you want inside a PostgreSQL server.

## 2. Tables

Tables store data. A table belongs to a database and each database has multiple tables.

## 3. Schemas

A schema is a logical container of tables and other objects inside a database. Each PostgreSQL database may have multiple schemas. It is important to note that schema is a part of the ANSI-SQL standard.

## 4. Tablespaces

Tablespaces are where PostgreSQL stores the data. Tablespaces allow you to move your data to different physical locations across drivers easily by using simple commands.

By default, PostgreSQL provides you with two tablespaces:

- The pg_default is for storing user data.
- The pg_global  is for storing system data.

## 5. Views

Views are named query stored in the database. Besides the read-only views, PostgreSQL support updatable views.

## 6. Functions

A function is a reusable block of SQL code that returns a scalar value of a set of rows.

## 7. Operators

Operators are symbolic functions. PostgreSQL allows you to define custom operators.

## 8. Casts

Casts enable you to convert one data type into another data type. Casts backed by functions to perform the conversion. You can also create your casts to override the default casting provided by PostgreSQL.

## 9. Sequence

Sequences are used to manage auto-increment columns that defined in a table as a serial column.

## 10. Extension

PostgreSQL introduced extension concept since version 9.1 to wrap other objects including types, casts, indexes, functions, etc., into a single unit.  The purpose of extensions is to make it easier to maintain.
