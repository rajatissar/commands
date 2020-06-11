# TRIGGERS

* A PostgreSQL trigger is a function invoked automatically whenever an event e.g., insert, update, or delete occurred.
* A trigger is a special user-defined function associated with a table. To create a new trigger, you must define a trigger function first, and then bind this trigger function to a table. The difference between a trigger and a user-defined function is that a trigger is automatically invoked when an event occurs.
* PostgreSQL provides two main types of triggers: **row** and **statement-level** triggers. The differences between the two kinds are how many times the trigger is invoked and at what time. For example, if you issue an UPDATE statement that affects 20 rows, the row-level trigger will be invoked 20 times, while the statement level trigger will be invoked 1 time.
* A trigger function does not take any argument and has a return value with the type of trigger.

## CREATE Trigger Function

A trigger function receives data about its calling environment through a special structure called **TriggerData**, which contains a set of local variables. For example, **OLD** and **NEW** represent the states of the row in the table before or after the triggering event.
PostgreSQL provides other local variables starting with **TG_** as the prefix such as TG_WHEN, and TG_TABLE_NAME.

* **SYNTAX**

```SQL
CREATE FUNCTION trigger_function()
  RETURNS trigger AS
```

```SQL
CREATE TRIGGER trigger_name
{BEFORE | AFTER | INSTEAD OF} {event [OR ...]}
  ON table_name
  [FOR [EACH] {ROW | STATEMENT}]
    EXECUTE PROCEDURE trigger_function
```

* **EXAMPLE**

```SQL
CREATE OR REPLACE FUNCTION log_last_name_changes()
  RETURNS trigger AS
$BODY$
BEGIN
  IF NEW.last_name <> OLD.last_name THEN
    INSERT INTO employee_audits(employee_id,last_name,changed_on)
    VALUES(OLD.id,OLD.last_name,now());
  END IF;

  RETURN NEW;
END;
$BODY$
```

```SQL
CREATE TRIGGER last_name_changes
  BEFORE UPDATE
  ON employees
  FOR EACH ROW
  EXECUTE PROCEDURE log_last_name_changes();
```

## MANAGE TRIGGER

### RENAME TRIGGER

* **SYNTAX**

```SQL
ALTER TRIGGER trigger_name ON table_name
RENAME TO new_name;
```

* **EXAMPLE**

```SQL
ALTER TRIGGER last_name_changes
ON employees
RENAME TO log_last_name_changes;
```

### DISABLE TRIGGER

* **SYNTAX**

```SQL
ALTER TABLE table_name
DISABLE TRIGGER trigger_name | ALL
```

* **EXAMPLE**

```SQL
ALTER TABLE employees
DISABLE TRIGGER log_last_name_changes;
```

```SQL
ALTER TABLE employees
DISABLE TRIGGER ALL;
```

### DROP TRIGGER

* **SYNTAX**

```SQL
DROP TRIGGER [IF EXISTS] trigger_name
ON table_name [RESTRICT | CASCADE]
```

* **EXAMPLE**

```SQL
DROP TRIGGER log_last_name_changes
ON employees;
```
