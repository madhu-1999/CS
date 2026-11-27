---
publish: true
created: 2026-11-26 08:40
modified: 2026-11-26 03:27
published: 2026-11-27T00:36:11.371Z
tags:
  - sql
  - postgres
prerequisite:
  - "[SQL-SELECT](Notes/SQL-SELECT.md)"
aliases:
---

# `ORDER BY`

- [i] ASC is default, so it can be omitted.

```sql
SELECT
  select_list
FROM
  table_name
ORDER BY
  sort_expression1 [ASC | DESC][NULLS FIRST | NULLS LAST],
  sort_expression2 [ASC | DESC][NULLS FIRST | NULLS LAST],
  ...;
```

```sql
-- Fetch all customer names in alphabetical order of first_name and descending order of last_name.
SELECT
  first_name,
  last_name
FROM
  customer
ORDER BY
  first_name,
  last_name DESC
LIMIT 5;
```

## With `NULL`

When you sort rows that contain `NULL`, you can specify the order of `NULL` by using the `NULLS FIRST` or `NULLS LAST` option of the ORDER BY clause:

```sql
ORDER BY sort_expresssion [ASC | DESC] [NULLS FIRST | NULLS LAST]
```

- [*] The `NULLS FIRST` option places NULL before other non-null values

```sql
--List rentals and prioritize those that have not been returned
SELECT rental_id, return_date
FROM rental
ORDER BY return_date ASC NULLS FIRST
LIMIT 5;
```

- [*] The `NULLS LAST` option places NULL after other non-null values.

```sql
--List all films and their return dates, showing those already returned first.
SELECT rental_id, return_date
FROM rental
ORDER BY return_date ASC NULLS LAST
LIMIT 5;
```
