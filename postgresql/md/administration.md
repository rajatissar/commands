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

```SQL
DROP ROLE role_name;
```

Before removing a role, you must reassign or remove all objects it owns and revoke its privileges.
