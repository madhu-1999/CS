---
publish: true
created: 2026-11-26 09:13
modified: 2026-11-26 03:27
published: 2026-11-26T23:27:43.168Z
tags:
  - sql
  - postgres
prerequisite:
  - "[SQL-`ORDER BY`](Notes/SQL-`ORDER%20BY`.md)"
aliases:
---

# `LIMIT`/`OFFSET` Clause

```sql
SELECT
  select_list
FROM
  table_name
ORDER BY
  sort_expression
LIMIT
  row_count
OFFSET
  row_to_skip;
```

```sql
-- retrieve 4 films starting from the fourth one
SELECT
  film_id,
  title,
  release_year
FROM
  film
ORDER BY
  film_id
LIMIT 4 OFFSET 3;
```

## `FETCH` Clause

`FETCH` is an alternative clause for `LIMIT`, which is typically more compatible with other database dialects.

```sql
OFFSET row_to_skip { ROW | ROWS }
FETCH { FIRST | NEXT } [ row_count ] { ROW | ROWS } ONLY
```

`FIRST`: Often used when you want the beginning of a result set (e.g., `FETCH FIRST 10 ROWS ONLY`).

`NEXT`: Frequently used when implementing pagination alongside an `OFFSET` to indicate you are grabbing the "next" batch of records.

```sql
SELECT
    film_id,
    title
FROM
    film
ORDER BY
    title
OFFSET 5 ROWS
FETCH NEXT 5 ROWS ONLY;
```
