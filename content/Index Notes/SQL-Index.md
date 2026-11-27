---
publish: true
created: 2026-11-25 20:39
modified: 2026-11-26 05:36
published: 2026-11-27T01:36:41.151Z
tags:
  - sql
  - postgres
prerequisite:
aliases:
---

# DQL Commands

1. [`SELECT`](Notes/SQL-SELECT.md)
2. [`ORDER BY`](Notes/SQL-ORDER%20BY.md)
3. [`WHERE`](Notes/SQL-WHERE.md)
4. [`LIMIT`/`OFFSET` Clause](Notes/SQL-LIMIT%20and%20OFFSET%20Clause.md)
5. [`GROUP BY` Clause](Notes/SQL-GROUP%20BY%20Clause.md)
6. [Joins](Notes/SQL-Joins.md)
7. [Set Operators](Notes/SQL-Set%20Operators.md)

## Intermediate Topics

1. [SQL-Conditional Expressions](SQL-Conditional%20Expressions)
2. [Advanced Joins](Notes/SQL-Advanced%20Joins.md)
3. [SQL-Math, String And Date Functions](SQL-Math,%20String%20And%20Date%20Functions)

## Advanced Topics

1. [SQL-Subqueries and CTEs](SQL-Subqueries%20and%20CTEs)
2. [Window Functions](Notes/SQL-Window%20Functions.md)
3. [SQL-Grouping Sets](SQL-Grouping%20Sets)

## Order of Execution

# Order of Execution

1. `FROM`, `JOIN`
2. `WHERE`
3. `GROUP BY`
4. [aggregate](SQL-GROUP%20BY%20Clause.md#Aggregations) functions
5. `HAVING`
6. **window functions**
7. `SELECT`
8. `DISTINCT`
9. `UNION`/`INTERSECT`/`EXCEPT`
10. `ORDER BY`
11. `OFFSET`
12. `LIMIT`/`FETCH`/`TOP`
