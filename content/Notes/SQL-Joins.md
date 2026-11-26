---
publish: true
created: 2026-11-26 09:30
modified: 2026-11-26 03:05
published: 2026-11-26T23:05:13.643Z
tags:
  - sql
  - postgres
prerequisite:
aliases:
---

# Basic Joins

## `INNER JOIN`

[^1]
Returns only the rows where there is a <mark style="background: #ADCCFFA6;">match in both tables</mark>.
![[Assets/SQL-Joins-1795714163277.webp]]

```sql
SELECT c.CustomerName, o.OrderID
FROM Customers c
INNER JOIN Orders o
  ON c.CustomerID = o.CustomerID;
```

## `LEFT JOIN`

[^1]
Keeps <mark style="background: #ADCCFFA6;">every row from the left table</mark> and adds <mark style="background: #ADCCFFA6;">matching data from the right table, filling in <code>NULL</code> when no match exists</mark>.
![[Assets/SQL-Joins-1795714666134.webp]]

```sql
SELECT c.CustomerName, o.OrderID
FROM Customers c
LEFT JOIN Orders o
  ON c.CustomerID = o.CustomerID;
```

## `RIGHT JOIN`

[^1]
Keeps <mark style="background: #ADCCFFA6;">every row from the right table</mark> and adds <mark style="background: #ADCCFFA6;">matching data from the left table, filling in <code>NULL</code> when no match exists</mark>.

![[Assets/SQL-Joins-1795715408883.webp]]

```sql
SELECT c.CustomerName, o.OrderID
FROM Customers c
RIGHT JOIN Orders o
  ON c.CustomerID = o.CustomerID;
```

## `FULL OUTER JOIN`

[^1] <mark style="background: #ADCCFFA6;">Combines <code>LEFT</code> and <code>RIGHT</code> joins</mark> to return all records from both tables, <mark style="background: #ADCCFFA6;">inserting <code>NULL</code> wherever a match is missing on either side</mark>.
![[Assets/SQL-Joins-1795715919591.webp]]

```sql
SELECT c.CustomerName, o.OrderID
FROM Customers c
FULL OUTER JOIN Orders o
  ON c.CustomerID = o.CustomerID;
```

## `CROSS JOIN`

[^1]
Pairs every row from the first table with every row from the second.
![[Assets/SQL-Joins-1795716457002.webp]]

```sql
SELECT c.CustomerName, o.OrderID
FROM Customers c
CROSS JOIN Orders o;
```

## Self Join

[^1]
Joins a table to itself using aliases. It is commonly used to:

- [*] Query hierarchical data
- [*] Compare rows within the same table.
  ![[Assets/SQL-Joins-1795717273980.webp]]

```sql
SELECT e.EmployeeName AS Employee,
       m.EmployeeName AS Manager
FROM Employees e
JOIN Employees m
  ON e.ManagerID = m.EmployeeID;
```

## `NATURAL JOIN`

[^3]
It can be a `INNER JOIN`, `LEFT JOIN` or `RIGHT JOIN`.

- [i] <u>Default</u>: `INNER JOIN`

```sql
SELECT *
FROM table_a NATURAL [ INNER | LEFT | RIGHT ] JOIN table_b;
```

It joins the left and right table <mark style="background: #ADCCFFA6;">based on the equality of <b>all</b> common columns i.e. columns with the same name.</mark>
![[Assets/SQL-Joins-1795718231752.webp]]

- [!] `NATURAL JOIN` matches on **every** column with the same name. Here `CustomerID` _and_ `City` are common, so order `103` (shipped to Seattle, customer lives in Boston) is silently dropped.

```sql
SELECT CustomerName, OrderID
FROM Customers c
NATURAL JOIN Orders o; -- Inner Join
```

> [!FAQ] What happens when there are no common columns?
> If there are no common columns, a `NATURAL JOIN` performs like a `CROSS JOIN`.

# Joins And `NULLs`

- [I] **Watch this**: [^2]

| Join Type            | Behavior with `NULLs`                                                                                     |
| :------------------- | :-------------------------------------------------------------------------------------------------------- |
| **Inner Join**       | Excludes `NULL` if present in either side of join                                                         |
| **Left Outer Join**  | Includes matching records plus all records from the left table (including `NULL` only on the left side)   |
| **Right Outer Join** | Includes matching records plus all records from the right table (including `NULL` only on the right side) |
| **Full Outer Join**  | Includes all records from both tables (including `NULL` on both sides)                                    |
| **Cross Join**       | Produces a Cartesian product; `NULLs` are treated as individual records                                   |
| **Natural Join**     | Behaves like Inner Join if a common column exists, or Cross Join if not                                   |

[^1]: [SQL Joins Explained: INNER, LEFT, RIGHT, FULL (+Venn)](https://red9.com/blog/sql-joins-venn-diagram/)

[^2]: [Making Sense of Null Values in SQL Joins: What You Need to Know! - YouTube](https://www.youtube.com/watch?v=e_8TdRhrQAw)

[^3]: [SQL NATURAL JOIN: Syntax, Examples, and Use Cases](https://www.puppygraph.com/blog/sql-natural-join)
