---
publish: true
created: 2026-11-26 14:56
modified: 2026-11-26 04:46
published: 2026-11-27T00:46:42.716Z
tags:
  - sql
  - postgres
prerequisite:
  - "[SQL-SELECT](Notes/SQL-SELECT.md)"
  - "[SQL-ORDER BY](Notes/SQL-ORDER%20BY.md)"
  - "[SQL-WHERE](Notes/SQL-WHERE.md)"
aliases:
---

# Syntax

```sql
SELECT
   column_1,
   column_2,
   ...,
   aggregate_function(column_3)
FROM
   table_name
GROUP BY
   column_1,
   column_2,
   ...;
```

- [!] All **unaggregated** columns **MUST** be included in the `GROUP BY` clause.
  The following query does not work since `city_of_delivery` is not included in the `GROUP BY` clause.

```sql
SELECT
  country_of_delivery,
  city_of_delivery,
  SUM(number_of_packages) AS total_number_of_packages
FROM 
    package_deliveries
GROUP BY 
    country_of_delivery;
```

# Aggregations

[^1]

## `COUNT`

- [*] `COUNT(*)`: Count the number of rows in the table.

```sql
SELECT COUNT(*)
FROM rental;
```

- [*] `COUNT(expr)`: Count the number of **non-null** values in a given column/expression.

```sql
SELECT COUNT(return_date)
FROM rental;
```

- [*] `COUNT(DISTINCT column)`: Count the number of distinct values in a column.

```sql
SELECT COUNT(district) - COUNT(DISTINCT district) AS Null_district_count
FROM address
```

## `SUM`

- [*] `SUM(expr)`: Sum of all **non-null** values in a column/expression.

```sql
--Find total replacement cost for films longer than 2 hours in the inventory.
SELECT 
    SUM(replacement_cost * 1.1) AS inflated_replacement_cost
FROM 
    film
WHERE 
    length > 120;
```

- [*] `SUM(DISTINCT col)`: Sum of all **non-null, unique** values in a given column.

```sql
-- Find the sum of unique payment amounts processed by each staff member
SELECT 
    staff_id, 
    SUM(DISTINCT amount) AS sum_unique_payments
FROM 
    payment
GROUP BY 
    staff_id;
```

## `AVG`

- [!] `AVG(expr)` ignores `NULL` values in its calculation, returning a value that deviates from the _true_ average.
  - [I] The solution is to convert `NULL` values to a sentinel value which `AVG` does not ignore during its calculation.

```sql
SELECT 
    staff_id,
    -- Standard AVG: Only looks at rows where return_date is NOT NULL
    AVG(return_date - rental_date) AS avg_returned_duration,
    
    -- Adjusted AVG: Treats NULLs as 0 to see how active rentals lower the average
    AVG(COALESCE(return_date - rental_date, '0 days')) AS avg_including_active
FROM 
    rental
GROUP BY 
    staff_id;
```

## `MIN`

- [*] `MIN(expr)` : Returns the minimum value.

```sql
SELECT MIN(spend) FROM product_spend;
```

## `MAX`

- [*] `MAX(expr)` : Returns the maximum value.

```sql
SELECT MAX(spend) FROM product_spend;
```

# `HAVING` Clause

[^2]
This clause allows us to filter on the basis of aggregated values.

```sql
SELECT
  column1,
  column2,
  ...,
  aggregate_function(column_name)
FROM table_name
GROUP BY column1, column2, …
HAVING filtering_condition;
```

```sql
SELECT 
    customer_id, 
    SUM(amount) AS total_spent
FROM 
    payment
GROUP BY 
    customer_id
HAVING 
    SUM(amount) > 200
ORDER BY 
    total_spent DESC;
```

## [`WHERE`](Notes/SQL-WHERE.md) Vs `HAVING`

|                       | WHERE                      | HAVING                                |
| --------------------- | -------------------------- | ------------------------------------- |
| When It Filters       | Values **BEFORE** Grouping | Values **AFTER** Grouping             |
| Operates On Data From | Individual Rows            | Aggregated Values from Groups of Rows |

[^1]: [Aggregate Functions With Examples | SQL Tutorial](https://datalemur.com/sql-tutorial/sql-aggregate-functions)

[^2]: [SQL HAVING With Examples! | SQL Tutorial for Data Science & Analytics](https://datalemur.com/sql-tutorial/sql-having)
