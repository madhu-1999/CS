---
publish: true
created: 2026-11-26 08:38
modified: 2026-11-26 08:57
tags:
  - sql
  - postgres
---

# `SELECT`

```sql
SELECT column1, column2, ...
FROM table_name;
```

```sql
SELECT actor_id 
FROM actor
LIMIT 5;
```

## Select All Columns

```sql
SELECT *
 FROM table_name;
```

```sql
SELECT * 
FROM actor
LIMIT 5;
```

## Select with Expression

**AS** keyword is used to provide column alias

```sql
SELECT expr AS column_alias
FROM table_name
```

```sql
SELECT
    actor_id,
    -- || ' ' || is concat operation
    first_name || ' ' || last_name AS name 
FROM
   actor
LIMIT 5;
```

If column alias contains **spaces** then surround it with " ".

```sql
SELECT
    first_name || ' ' || last_name AS "full name"
FROM
    actor
LIMIT 5;
```

## Select from Multiple Tables

```sql
SELECT t1.column1, t2.column2, ...
FROM table1 t1, table2 t2
```

This performs a cross join.

## Select Distinct

The `DISTINCT` subclause removes duplicate rows from result set.

```sql
SELECT
  DISTINCT column1
FROM
  table_name;
```

```sql
SELECT DISTINCT
  rental_rate
FROM
  film
ORDER BY
  rental_rate;
```

If multiple columns are specified, all column values are considered when deciding duplicates.

```sql
SELECT
   DISTINCT column1, column2
FROM
   table_name;
```

```sql
SELECT 
    DISTINCT film_id, store_id
FROM
    inventory
ORDER BY film_id
LIMIT 5;
```

> [!WARNING] We **cannot** specify that the `DISTINCT` subclause should only be applied on a subset of columns in the `SELECT`clause.

```sql
SELECT
-- Distinct on column1 & column2 but not on column3. This is NOT valid.
   DISTINCT (column1, column2), column3
FROM
   table_name;
```

## Select Distinct On

The `DISTINCT ON` subclause removes duplicate rows from result set, based on the subset of columns in `SELECT` clause

```sql
SELECT
  DISTINCT ON (column1, column2,...) column1,
  column2,
  ...
FROM
  table_name
ORDER BY
  column1,
  column2,
  ...;
```

The _first_ unique entry of specified columns in `DISTINCT ON` clause is picked as _representative_.
Without the`ORDER BY` clause, the _first_ unique entry is picked randomly.

### `DISTINCT` Vs `DISTINCT ON`

<u>Table:</u> rental

| **rental\_id** | **customer\_id** | **rental\_date** |
| ------------- | --------------- | --------------- |
| 101           | 1               | 2026-05-10      |
| 102           | 1               | 2026-05-03      |
| 103           | 1               | 2026-05-10      |
| 104           | 2               | 2026-06-01      |
| 105           | 2               | 2026-05-20      |
| 106           | 3               | 2026-04-15      |
| 107           | 4               | 2026-07-02      |
| 108           | 4               | 2026-07-02      |
| 109           | 5               | 2026-03-11      |
| 110           | 6               | 2026-08-19      |

`DISTINCT` treats the pair `(customer_id, rental_date)` as a single unit. It removes exact duplicate pairs (like `rental_id` 107 and 108) while keeping multiple dates per customer:

```sql
SELECT DISTINCT customer_id, rental_date 
FROM rental 
ORDER BY customer_id
LIMIT 5;
```

| customer\_id | rental\_date |
| ----------- | ----------- |
| 1           | 2026-05-10  |
| 1           | 2026-05-03  |
| 2           | 2026-06-01  |
| 2           | 2026-05-20  |
| 3           | 2026-04-15  |
| 4           | 2026-07-02  |
| 5           | 2026-03-11  |
| 6           | 2026-08-19  |

`DISTINCT ON (customer_id)` groups by `customer_id` and<mark style="background: #FFB8EBA6;"> keeps only the <b>first row</b> encountered </mark>for each customer according to the `ORDER BY` clause. Sorting by `rental_date ASC` ensures that first row is the earliest rental date:

```sql
SELECT DISTINCT ON (customer_id) 
    customer_id, 
    rental_date
FROM rental
ORDER BY customer_id, rental_date ASC
LIMIT 5;
```

| customer\_id | rental\_date |
| ----------- | ----------- |
| 1           | 2026-05-03  |
| 2           | 2026-05-20  |
| 3           | 2026-04-15  |
| 4           | 2026-07-02  |
| 5           | 2026-03-11  |
| 6           | 2026-08-19  |
