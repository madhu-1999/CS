---
publish: true
created: 2026-11-26 16:46
modified: 2026-11-26 05:25
published: 2026-11-27T01:25:46.184Z
tags:
  - sql
  - postgres
prerequisite:
  - "[SQL-SELECT](Notes/SQL-SELECT.md)"
  - "[SQL-ORDER BY](Notes/SQL-ORDER%20BY.md)"
aliases:
---

# Example Tables

<u>Table:</u> employees\_2023

| employee\_id | name    | department |
| ----------- | ------- | ---------- |
| 1           | Alice   | HR         |
| 2           | Bob     | IT         |
| 3           | Charlie | Finance    | <u>Table:</u> employees\_2024

| employee\_id | name    | department |
| ----------- | ------- | ---------- |
| 2           | Bob     | IT         |
| 3           | Charlie | Finance    |
| 4           | David   | Marketing  |

# `UNION/UNION ALL`

[^1]
It **vertically** combines the result set of two or more `SELECT` statements.

```sql
SELECT column_1, column_2
FROM table_1
 
UNION [ALL]
 
SELECT column_1, column_2
FROM table_2
[ORDER BY column1, column2, ...]
```

## Rules

1. Both `SELECT` statements should list the same number of columns, with the same type. (The type must be same in order i.e. type of `table_1.column_1` = type of `table_2.column_1`)
2. The columns are merged in order i.e. `table_1.column_1` is merged with `table_2.column_1` etc.
3. <mark style="background: #FF5582A6;"><code>UNION</code> removes duplicates from the result while <code>UNION ALL</code> retains them</mark>.

## Example

- [*] `UNION`

```sql
SELECT employee_id, name, department FROM employees_2023 
UNION 
SELECT employee_id, name, department FROM employees_2024;
```

| employee\_id | name    | department |
| ----------- | ------- | ---------- |
| 1           | Alice   | HR         |
| 2           | Bob     | IT         |
| 3           | Charlie | Finance    |
| 4           | David   | Marketing  |

- [*] `UNION ALL`

```sql
SELECT employee_id, name, department FROM employees_2023 
UNION ALL 
SELECT employee_id, name, department FROM employees_2024;
```

| employee\_id | name    | department |
| ----------- | ------- | ---------- |
| 1           | Alice   | HR         |
| 2           | Bob     | IT         |
| 3           | Charlie | Finance    |
| 2           | Bob     | IT         |
| 3           | Charlie | Finance    |
| 4           | David   | Marketing  |

> [!WARNING] `UNION` performs a `SELECT DISTINCT` under the hood. If the values are guaranteed to be unique, use `UNION ALL` instead.

# `INTERSECT`

[^2]
It **vertically** combines the result set of two or more `SELECT` statements and returns <mark style="background: #FFB8EBA6;"><b>distinct</b> rows available in both result sets</mark>.

```sql
SELECT select_list
FROM A
INTERSECT
SELECT select_list
FROM B
[ORDER BY order_list];
```

## Rules

1. Both `SELECT` statements should list the same number of columns, with the same type. (The type must be same in order i.e. type of `table_1.column_1` = type of `table_2.column_1`)
2. The columns are merged in order i.e. `table_1.column_1` is merged with `table_2.column_1` etc.

## Example

```sql
SELECT employee_id, name, department FROM employees_2023 
INTERSECT 
SELECT employee_id, name, department FROM employees_2024;
```

| employee\_id | name    | department |
| ----------- | ------- | ---------- |
| 2           | Bob     | IT         |
| 3           | Charlie | Finance    |

# `EXCEPT`

[^2]
It **vertically** combines the result set of two or more `SELECT` statements and returns <mark style="background: #FFB8EBA6;"><b>distinct</b> rows available only in the first result set</mark>.

## Rules

1. Both `SELECT` statements should list the same number of columns, with the same type. (The type must be same in order i.e. type of `table_1.column_1` = type of `table_2.column_1`)
2. The columns are merged in order i.e. `table_1.column_1` is merged with `table_2.column_1` etc.

## Example

```sql
SELECT employee_id, name, department FROM employees_2023 
EXCEPT 
SELECT employee_id, name, department FROM employees_2024;
```

| employee\_id | name  | department |
| ----------- | ----- | ---------- |
| 1           | Alice | HR         |

# Set Operators and Sorting

1. The `EXCEPT`, `UNION` and `INTERSECT` operators will <mark style="background: #FFB8EBA6;">always sort</mark> the returned results.
2. `UNION ALL` <mark style="background: #FFB8EBA6;">doesn't sort</mark> the returned results.

> [!WARNING] Results are sorted once after the set operation is performed, thus we **CANNOT** sort each `SELECT` separately!

[^1]: [UNION vs UNION ALL in SQL: Differences and Use Cases | DataCamp](https://www.datacamp.com/tutorial/sql-union-union-all)

[^2]: [SQL | INTERSECT and EXCEPT Clauses - GeeksforGeeks](https://www.geeksforgeeks.org/sql/sql-intersect-except-clause/)
