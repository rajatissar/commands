# Stored Procedures

In PostgreSQL, procedural languages such as PL/pgSQL, C, Perl, Python, and Tcl are referred to as stored procedures.

PostgreSQL divides the procedural languages into two main groups:

- *Safe languages* can be used by any users. SQL and PL/pgSQL are the safe languages.
- *Sand-boxed languages* are only used by superusers because sand-boxed languages provide the capability to bypass security and allow access to external sources. C is an example of a sandboxed language.

By default, PostgreSQL supports three procedural languages: SQL, PL/pgSQL, and C. You can also load other procedural languages e.g., Perl, Python, and TCL into PostgreSQL using extensions.

```SQL
[ <<label>> ]
[
  DECLARE
    -- declarations
]
BEGIN
  -- statements;
END [ label ];
```

- Each block has two sections: *declaration* and *body*. The declaration section is optional while the body section is required. The block is ended with a semicolon *;* after the END keyword.
- A block may have an optional label located at the beginning and at the end. You use the block label in case you want to specify it in the EXIT statement of the block body or if you want to qualify the names of variables declared in the block.
- The declaration section is where you declare all variables used within the body section. Each statement in the declaration section is terminated with a semicolon *;*.
- The body section is where you place the code. Each statement in the body section is also terminated with a semicolon *;*.

```SQL
DO $$
<<first_block>>
DECLARE
  counter1 integer := 0;
BEGIN
   counter1 := counter1 + 1;
   RAISE NOTICE 'The current value of counter1 is %', counter1;
END first_block $$;
```

- Replacement of *$$*

```SQL
DO
'<<first_block>>
DECLARE
  counter1 integer := 0;
BEGIN
    counter1 := counter1 + 1;
    RAISE NOTICE ''The current value of counter1 is %'', counter1;
END first_block';
```

- *Block*

```SQL
DO $$
<<outer_block>>
-- Outer Block starts
DECLARE
  counter1 integer := 0;
BEGIN
  counter1 := counter1 + 1;
  RAISE NOTICE 'The current value of counter1 is %', counter1;
  -- Inner Block starts
  DECLARE
  counter1 integer := 0;
  BEGIN
    counter1 := counter1 + 10;
    RAISE NOTICE 'The current value of counter1 in the subblock is %', counter1;
    RAISE NOTICE 'The current value of counter1 in the outer block is %', outer_block.counter1;
   END;
  -- Inner Block ends
   RAISE NOTICE 'The current value of counter1 in the outer block is %', counter1;
END outer_block $$;
-- Outer Block ends
```

## Data Type

```SQL
variable_name table_name.column_name%TYPE;
```

```SQL
variable_name variable%TYPE;
```

## Assigning aliases to variables

```SQL
new_name ALIAS FOR old_name;
```

## Constant

```SQL
constant_name CONSTANT data_type := expression;
```

```SQL
DO $$
<<first_block>>
DECLARE
  VAT CONSTANT NUMERIC := 1;
BEGIN
  RAISE NOTICE 'The current value of VAT is %', VAT;
END first_block $$;
```

## PL/pgSQL reporting messages

```SQL
RAISE level format;
```

Following the RAISE statement is the level option that specifies the error severity. PostgreSQL provides the following levels:

| Level     |
|-----------|
| DEBUG     |
| LOG       |
| NOTICE    |
| INFO      |
| WARNING   |
| EXCEPTION |

```SQL
DO $$
BEGIN
  RAISE INFO 'information message %', now();
  RAISE LOG 'log message %', now();
  RAISE DEBUG 'debug message %', now();
  RAISE WARNING 'warning message %', now();
  RAISE NOTICE 'notice message %', now();
END $$;
```

```output
INFO:  information message 2015-09-10 21:17:39.398+07
WARNING:  warning message 2015-09-10 21:17:39.398+07
NOTICE:  notice message 2015-09-10 21:17:39.398+07
```

```SQL
USING option = expression
```

The option can be:

| Option  | Description                                                                                                                                                           |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| MESSAGE | set error message text                                                                                                                                                |
| HINT    | provide the hint message so that the root cause of the error is easier to be discovered                                                                               |
| DETAIL  | give detailed information about the error                                                                                                                             |
| ERRCODE | identify the error code, which can be either by condition name or directly five-character SQLSTATE code. Please refer to the table of error codes and condition names |

```SQL
DO $$
DECLARE
  email varchar(255) := 'info@gmail.com';
BEGIN
  -- check email for duplicate
  -- ...
  -- report duplicate email
  RAISE EXCEPTION 'Duplicate email: %', email;
  USING HINT = 'Check the email again';
END $$;
```

```SQL
DO $$
BEGIN
  -- ...
  RAISE SQLSTATE '2210B';
END $$;
```

```SQL
DO $$
BEGIN
  -- ...
  RAISE invalid_regular_expression;
END $$;
```

```SQL
DO $$
DECLARE
   counter1 integer := -1;
BEGIN
   ASSERT counter1 = 0, 'Expect counter1 starts with 0';
END $$;
```

## Function

```SQL
CREATE FUNCTION function_name(p1 type, p2 type)
RETURNS type AS
BEGIN
 -- logic
END;
LANGUAGE language_name;
```

Let’s examine the CREATE FUNCTION statement in more detail.

- First, specify the name of the function after the CREATE FUNCTION keywords.
- Then, put a comma-separated list of parameters inside the parentheses following the function name.
- Next, specify the return type of the function after the RETURNS keyword.
- After that, place the code inside the BEGIN and END block. The function always ends with a semicolon *;* followed by the END keyword.
- Finally, indicate the procedural language of the function e.g., plpgsql in case PL/pgSQL is used.

```SQL
CREATE FUNCTION fun1 (param1 INTEGER)
RETURNS INTEGER AS $$
BEGIN
  RETURN param1 + 1;
END; $$
LANGUAGE PLPGSQL;
```

```SQL
SELECT fun1(20);
```

- *OUT*

```SQL
CREATE OR REPLACE FUNCTION hi_lo(a NUMERIC, b NUMERIC, c NUMERIC, OUT hi NUMERIC, OUT lo NUMERIC)
  AS $$
BEGIN
  hi := GREATEST(a,b,c);
  lo := LEAST(a,b,c);
END; $$
LANGUAGE plpgsql;
```

```SQL
SELECT hi_lo(10,20,30);
SELECT * FROM hi_lo(10,20,30);
```

- *INOUT*

```SQL
CREATE OR REPLACE FUNCTION square(INOUT a NUMERIC)
  AS $$
BEGIN
  a := a * a;
END; $$
LANGUAGE plpgsql;
```

```SQL
SELECT square(4);
```

- *VARIADIC*

```SQL
CREATE OR REPLACE FUNCTION sum_avg(VARIADIC list NUMERIC[], OUT total NUMERIC, OUT average NUMERIC)
  AS $$
BEGIN
   SELECT INTO total SUM(list[i])
   FROM generate_subscripts(list, 1) g(i);

   SELECT INTO average AVG(list[i])
   FROM generate_subscripts(list, 1) g(i);
END; $$
LANGUAGE plpgsql;
```

```SQL
SELECT * FROM sum_avg(10,20,30);
```

- *Default Value*

```SQL
CREATE OR REPLACE FUNCTION get_rental_duration(
    p_customer_id INTEGER,
    p_from_date DATE DEFAULT '2005-01-01' -- Default value
  )
RETURNS INTEGER AS $$
DECLARE
  rental_duration integer;
BEGIN
  -- get the rental duration based on customer_id and rental date
  SELECT INTO rental_duration
    SUM( EXTRACT( DAY FROM return_date + '12:00:00' - rental_date))
  FROM rental
  WHERE customer_id = p_customer_id AND rental_date >= p_from_date;

  RETURN rental_duration;
END; $$
LANGUAGE plpgsql;
```

- *DROP Function*

```SQL
DROP FUNCTION get_rental_duration(INTEGER, DATE);
```

You must specify the parameters together with the function name when you drop the function.

- Returns Table

```SQL
CREATE OR REPLACE FUNCTION get_film (p_pattern VARCHAR)
RETURNS TABLE (
  film_title VARCHAR,
  film_release_year INT
)
BEGIN
  RETURN QUERY SELECT
    title,
    cast( release_year as integer)
  FROM
    film
  WHERE
    title ILIKE p_pattern;
END; $$
LANGUAGE 'plpgsql';
```

```SQL
SELECT * FROM get_film('Al%');
```

```SQL
CREATE OR REPLACE FUNCTION get_film (p_pattern VARCHAR,p_year INT)
RETURNS TABLE (
    film_title VARCHAR,
    film_release_year INT
) AS $$
DECLARE
  var_r record;
BEGIN
  FOR var_r IN(SELECT
                  title,
                  release_year
                FROM film
                WHERE title ILIKE p_pattern AND release_year = p_year)  
  LOOP
        film_title := upper(var_r.title);
        film_release_year := var_r.release_year;
        RETURN NEXT;
  END LOOP;
END; $$
LANGUAGE 'plpgsql';
```
