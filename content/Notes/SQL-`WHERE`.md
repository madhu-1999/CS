---
publish: true
created: 2026-11-26 08:54
modified: 2026-11-26 08:56
tags:
  - sql
  - postgres
---

# `WHERE`

```sql
SELECT
  select_list
FROM
  table_name
WHERE
  condition
ORDER BY
  sort_expression;
```

The following is a list of all operators that can be used in the `WHERE` clause:

| Operator     | Description                                                                                                                |
| :----------- | :------------------------------------------------------------------------------------------------------------------------- |
| `=`          | Equal                                                                                                                      |
| `>`          | Greater than                                                                                                               |
| `<`          | Less than                                                                                                                  |
| `>=`         | Greater than or equal                                                                                                      |
| `<=`         | Less than or equal                                                                                                         |
| `<>` or `!=` | Not equal                                                                                                                  |
| `AND`        | Logical operator AND                                                                                                       |
| `OR`         | Logical operator OR                                                                                                        |
| `IN`         | Return true if a value matches any value in a list                                                                         |
| `BETWEEN`    | Return true if a value is between a range of values (inclusive). If using `NOT BETWEEN`, the range of values is exclusive. |
| `LIKE`       | Return true if a value matches a pattern                                                                                   |
| `IS NULL`    | Return true if a value is NULL                                                                                             |
| `NOT`        | Negate the result of other operators                                                                                       |

## `AND`/ `OR` Operators

A **boolean** has 3 values: _true_, _false_ and `NULL`.
`AND` / `OR` truth tables are trivial, so the table below details the truth table for `NULL` values.

| Expression          | Result |
| ------------------- | ------ |
| true `AND` `NULL`   | `NULL` |
| `NULL` `AND` `NULL` | `NULL` |
| false `AND` `NULL`  | false  |
| true `OR` `NULL`    | true   |
| `NULL` `OR` `NULL`  | `NULL` |
| false `OR` `NULL`   | `NULL` |

## `IN` Operator

The `IN` operator is used to find value(s) in a list.

```sql
value IN (value1,value2,...)
```

Functionally, the `IN` operator is equivalent to combining multiple boolean expressions with the `OR` operators:

```sql
value = value1 OR value = value2 OR ...
```

```sql
--With list of strings
SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  first_name IN ('Ann', 'Anne', 'Annie');
```

```sql
--With list of numbers
SELECT
  film_id,
  title
FROM
  film
WHERE
  film_id in (1, 2, 3);
```

```sql
--With list of dates
SELECT
  payment_id,
  amount,
  payment_date
FROM
  payment
WHERE
--payment is of type timestamp. It is cast to type date for the check.
  payment_date::date IN ('2007-02-15', '2007-02-16')
LIMIT 5;
```

## Negating with `NOT IN` Operator

To negate the `IN` operator, you use the `NOT IN` operator

```sql
value NOT IN (value1, value2, ...)
```

The `NOT IN` operator is equivalent to a combination of multiple boolean expressions with the `AND` operators:

```sql
value <> value1 AND value <> value2 AND ...
```

```sql
SELECT
  film_id,
  title
FROM
  film
WHERE
  film_id NOT IN (1, 2, 3)
ORDER BY
  film_id
LIMIT 5;
```

## `LIKE` Operator <a id="15"></a>

`LIKE` operator is used to match a pattern. (**case-sensitive**)

PostgreSQL offers two wildcards:

- Percent sign (%) matches any sequence of zero or more characters.
- Underscore sign (\_) matches any single character.

```sql
value LIKE pattern

-- To negate
value NOT LIKE pattern
```

- [i] For **case-insensitive** matching use `ILIKE` and `NOT ILIKE` operators

```sql
--LIKE operator is used to match a pattern
SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  first_name LIKE 'Ann%';
```

## `BETWEEN` Operator

The `BETWEEN` operator allows you to check if a value falls within a range of values.

```sql
value BETWEEN low AND high;
--which is the same as 
value >= low AND value <= high
```

```sql
SELECT
  payment_id,
  amount
FROM
  payment
WHERE
  payment_id BETWEEN 17503 AND 17505
ORDER BY
  payment_id;
```

> [!WARNING] Timestamp Trap with `BETWEEN`
> Although, `BETWEEN` is inclusive, using it with date-only timestamp strings, causes records from the final day to be dropped!
> For example, `BETWEEN '2026-01-01' AND '2026-01-05'` evaluates to `'2026-01-05 00:00:00'`, which excludes records occurring later on January 5th.
>
> > **Scenario:** You want to pull all orders placed from January 1 through January 5, 2026.
>
> ❌ Risky (misses orders after midnight on Jan 5):
>
> ```sql
> SELECT * FROM orders
> ```

WHERE created\_at BETWEEN '2026-01-01' AND '2026-01-05';
\-- Evaluates as: created\_at >= '2026-01-01 00:00:00' AND created\_at <= '2026-01-05 00:00:00'

> ````
>
> ✅ Safer (half-open interval captures the entire final day):
> ```sql
> SELECT * FROM orders
> ````

WHERE created\_at >= '2026-01-01'
AND created\_at < '2026-01-06';

> ```
> ```

### Negating With `NOT BETWEEN`

If you want to check if a value is outside a specific range, you can use the `NOT BETWEEN` operator.

```sql
value NOT BETWEEN low AND high
-- which is the same is
value < low OR value > high
```

## `IS NULL` Operator

The `IS NULL` operator returns true if the value is `NULL` or false otherwise

```sql
value IS NULL

-- To negate
value IS NOT NULL
```

```sql
SELECT
  address,
  address2
FROM
  address
WHERE
  address2 IS NULL;
```
