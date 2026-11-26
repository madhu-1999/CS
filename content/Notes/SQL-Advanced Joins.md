---
publish: true
created: 2026-11-26 14:35
modified: 2026-11-26 02:54
published: 2026-11-26T22:54:19.280Z
tags:
  - sql
  - postgres
prerequisite:
  - "[[Notes/SQL-Joins|SQL-Joins]]"
aliases:
---

# Anti Joins

## Left anti Join

[^1]
A left join that **only** returns <mark style="background: #ADCCFFA6;">rows from the left table that do not have matching rows in the right table</mark>.
![[Assets/SQL-Joins-1795715053572.webp]]

```sql
SELECT c.CustomerName
FROM Customers c
LEFT JOIN Orders o
  ON c.CustomerID = o.CustomerID
WHERE o.OrderID IS NULL;
```

## Right anti Join

[^1]
A right join that **only** returns <mark style="background: #ADCCFFA6;">rows from the right table that do not have matching rows in the left table</mark>.
![[Assets/SQL-Joins-1795715583971.webp]]

```sql
SELECT o.OrderID
FROM Customers c
RIGHT JOIN Orders o
  ON c.CustomerID = o.CustomerID
WHERE c.CustomerID IS NULL;
```

## Full anti Join

[^1]
A full join that returns <mark style="background: #ADCCFFA6;">rows from both the left and right table that do not have matching rows in the right and left table respectively</mark>.
![[Assets/SQL-Joins-1795715756845.webp]]

```sql
SELECT c.CustomerName, o.OrderID
FROM Customers c
FULL OUTER JOIN Orders o
  ON c.CustomerID = o.CustomerID;
```

# Semi Joins

#todo

# Join Optimization

Move to separate note #todo

[^1]: [SQL Joins Explained: INNER, LEFT, RIGHT, FULL (+Venn)](https://red9.com/blog/sql-joins-venn-diagram/)
