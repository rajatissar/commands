# Stored Procedures

In PostgreSQL, procedural languages such as PL/pgSQL, C, Perl, Python, and Tcl are referred to as stored procedures.

PostgreSQL divides the procedural languages into two main groups:

- **Safe languages** can be used by any users. SQL and PL/pgSQL are the safe languages.
- **Sand-boxed languages** are only used by superusers because sand-boxed languages provide the capability to bypass security and allow access to external sources. C is an example of a sandboxed language.

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

- Each block has two sections: **declaration** and **body**. The declaration section is optional while the body section is required. The block is ended with a semicolon **;** after the END keyword.
- A block may have an optional label located at the beginning and at the end. You use the block label in case you want to specify it in the EXIT statement of the block body or if you want to qualify the names of variables declared in the block.
- The declaration section is where you declare all variables used within the body section. Each statement in the declaration section is terminated with a semicolon **;**.
- The body section is where you place the code. Each statement in the body section is also terminated with a semicolon **;**.

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

## Replacement of $$

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

## Block

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
- After that, place the code inside the BEGIN and END block. The function always ends with a semicolon **;** followed by the END keyword.
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

### OUT

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

### INOUT

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

### VARIADIC

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

### Default Value

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

### DROP Function

```SQL
DROP FUNCTION get_rental_duration(INTEGER, DATE);
```

You must specify the parameters together with the function name when you drop the function.

### Returns Table

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

## Control Structures

### IF statement

```SQL
IF condition THEN
  -- statements;
END IF;
```

```SQL
DO $$
DECLARE
  a integer := 10;
  b integer := 20;
BEGIN
  IF a > b THEN
    RAISE NOTICE 'a is greater than b';
  END IF;

  IF a < b THEN
    RAISE NOTICE 'a is less than b';
  END IF;

  IF a = b THEN
    RAISE NOTICE 'a is equal to b';
  END IF;
END $$;
```

```SQL
IF condition THEN
  -- statements;
ELSE
  -- alternative-statements;
END IF;
```

```SQL
DO $$
DECLARE
  a integer := 10;
  b integer := 20;
BEGIN
  IF a > b THEN
    RAISE NOTICE 'a is greater than b';
  ELSE
    RAISE NOTICE 'a is not greater than b';
  END IF;
END $$;
```

```SQL
IF condition-1 THEN
  --- if-statements-1;
ELSIF condition-2 THEN
  -- elsif-statements-2;
-- ...
ELSIF condition-n THEN
  -- elsif-statements-n;
ELSE
  -- else-statements;
END IF:
```

```SQL
DO $$
DECLARE
  a integer := 10;
  b integer := 10;
BEGIN
  IF a > b THEN
    RAISE NOTICE 'a is greater than b';
  ELSIF a < b THEN
    RAISE NOTICE 'a is less than b';
  ELSE
    RAISE NOTICE 'a is equal to b';
  END IF;
END $$;
```

### CASE statement

- **Simple CASE statement**

There are two forms of the CASE statement: **simple** and **searched** CASE statements.
The CASE expression evaluates to a value, while the CASE statement executes statements based on condition.

```SQL
CASE search-expression
  WHEN expression_1 [, expression_2, ...] THEN
      -- when-statements
  [ ... ]
  [ELSE
    -- else-statements
  ]
END CASE;
```

```SQL
CREATE OR REPLACE FUNCTION get_price_segment(p_film_id integer)
  RETURNS VARCHAR(50) AS $$
DECLARE
  rate NUMERIC;
  price_segment VARCHAR(50);
BEGIN
  -- get the rate based on film_id
  SELECT INTO rate rental_rate
  FROM film
  WHERE film_id = p_film_id;

  CASE rate
    WHEN 0.99 THEN
      price_segment = 'Mass';
    WHEN 2.99 THEN
      price_segment = 'Mainstream';
    WHEN 4.99 THEN
      price_segment = 'High End';
    ELSE
      price_segment = 'Unspecified';
  END CASE;

  RETURN price_segment;
END; $$
LANGUAGE plpgsql;
```

- **Searched CASE statement**

```SQL
CASE
  WHEN boolean-expression-1 THEN
    -- statements
  [ WHEN boolean-expression-2 THEN
    -- statements
  ]
  [ ELSE
      -- statements
  ]
END CASE;
```

```SQL
CREATE OR REPLACE FUNCTION get_customer_service (p_customer_id INTEGER)
  RETURNS VARCHAR (25) AS $$
DECLARE
  total_payment NUMERIC;
  service_level VARCHAR (25);
BEGIN
  -- get the rate based on film_id
  SELECT
  INTO total_payment SUM (amount)
  FROM payment
  WHERE customer_id = p_customer_id;
  
  CASE
    WHEN total_payment > 200 THEN
      service_level = 'Platinum';
    WHEN total_payment > 100 THEN
      service_level = 'Gold';
    ELSE
      service_level = 'Silver';
   END CASE;

   RETURN service_level;
END; $$
LANGUAGE plpgsql;
```

### LOOP Statement

PostgreSQL provides you with three loop statements: **LOOP**, **WHILE loop**, and **FOR loop**.

- **LOOP**

```SQL
<<label>>
LOOP
  -- Statements;
  EXIT [<<label>>] WHEN condition;
END LOOP;
```

```SQL
CREATE OR REPLACE FUNCTION fibonacci (n INTEGER)
  RETURNS INTEGER AS $$
DECLARE
  counter1 INTEGER := 0;
  i INTEGER := 0;
  j INTEGER := 1;
BEGIN
  
  IF (n < 1) THEN
    RETURN 0;
  END IF;

  LOOP
    EXIT WHEN counter1 = n;
    counter1 := counter1 + 1;
    SELECT j, i + j INTO i, j;
  END LOOP;

  RETURN i;
END;
$$ LANGUAGE plpgsql;
```

- **WHILE loop**

```SQL
[ <<label>> ]
WHILE condition LOOP
  -- statements;
END LOOP;
```

```SQL
CREATE OR REPLACE FUNCTION fibonacci (n INTEGER)
  RETURNS INTEGER AS $$
DECLARE
  counter1 INTEGER := 0;
  i INTEGER := 0;
  j INTEGER := 1;
BEGIN

  IF (n < 1) THEN
    RETURN 0;
  END IF;

  WHILE counter1 <= n LOOP
    counter1 := counter1 + 1;
    SELECT j, i + j INTO i, j;
  END LOOP;

  RETURN i;
END;
```

- **FOR loop**

```SQL
[ <<label>> ]
FOR loop_counter IN [ REVERSE ] from.. to [ BY expression ] LOOP
    statements
END LOOP [ label ];
```

```SQL
DO $$
BEGIN
  FOR counter1 IN 1..5 LOOP
  RAISE NOTICE 'counter1: %', counter1;
  END LOOP;
END; $$
```

```SQL
DO $$
BEGIN
  FOR counter1 IN REVERSE 5..1 LOOP
    RAISE NOTICE 'counter1: %', counter1;
  END LOOP;
END; $$
```

```SQL
[ <<label>> ]
FOR target IN query LOOP
  -- statements
END LOOP [ label ];
```

```SQL
CREATE OR REPLACE FUNCTION for_loop_through_query(n INTEGER DEFAULT 10)
RETURNS VOID AS $$
DECLARE
  rec RECORD;
BEGIN
  FOR rec IN
    SELECT title
    FROM film
    ORDER BY title
    LIMIT n
  LOOP
  RAISE NOTICE '%', rec.title;
  END LOOP;
END;
$$ LANGUAGE plpgsql;
```

## Cursor

A PL/pgSQL cursor allows us to encapsulate a query and process each individual row at a time. We use cursors when we want to divide a large result set into parts and process each part individually. Cursor is of two types **unbound** and **bound** cursors.

1. First, declare a cursor.
2. Next, open the cursor.
3. Then, fetch rows from the result set into a target.
4. After that, check if there is more row left to fetch. If yes, go to step 3, otherwise, go to step 5.
5. Finally, close the cursor.

```SQL
DECLARE
  cur_films  CURSOR FOR SELECT * FROM film;
  cur_films2 CURSOR (year integer) FOR SELECT * FROM film WHERE release_year = year;
```

```SQL
CREATE OR REPLACE FUNCTION get_film_titles(p_year INTEGER)
  RETURNS text AS $$
DECLARE
  titles TEXT DEFAULT '';
  rec_film RECORD;
  cur_films CURSOR(p_year INTEGER)
    FOR SELECT title, release_year
        FROM film
        WHERE release_year = p_year;
BEGIN
  -- Open the cursor
  OPEN cur_films(p_year);

  LOOP
    -- fetch row into the film
    FETCH cur_films INTO rec_film;
    -- exit when no more row to fetch
    EXIT WHEN NOT FOUND;

    -- build the output
    IF rec_film.title LIKE '%ful%' THEN
      titles := titles || ',' || rec_film.title || ':' || rec_film.release_year;
    END IF;
  END LOOP;
  
   -- Close the cursor
   CLOSE cur_films;

   RETURN titles;
END; $$
LANGUAGE plpgsql;
```

## CREATE PROCEDURE

A drawback of user-defined functions is that they cannot execute transactions. PostgreSQL 11 introduced stored procedures that support transactions.

```SQL
CREATE [OR REPLACE] PROCEDURE procedure_name(parameter_list)
LANGUAGE language_name
AS $$
  stored_procedure_body;
$$;
```

In this syntax:

1. First, specify the name of the stored procedure after the CREATE PROCEDURE clause.
2. Next, define a parameter list which is similar to the parameter list of user-defined functions.
3. Then, specify the programming language for the stored procedure such as PLpgSQL and SQL.
4. After that, place the code in the body of the stored procedure after that AS keyword.
5. Finally, use double dollar ($$) to end the stored procedure.

Unlike a user-defined function, a stored procedure does not have a return value. If you want to end a procedure earlier, you can use the RETURN statement with no expression.

```SQL
CREATE OR REPLACE PROCEDURE transfer(INT, INT, DEC)
LANGUAGE plpgsql
AS $$
BEGIN
    -- subtracting the amount from the sender's account
    UPDATE accounts
    SET balance = balance - $3
    WHERE id = $1;

    -- adding the amount to the receiver's account
    UPDATE accounts
    SET balance = balance + $3
    WHERE id = $2;

    COMMIT;
END;
$$;
```

```SQL
CALL transfer(1,2,1000);
```
