---
publish: true
created: 2026-11-16 22:57
modified: 2026-11-21 05:23
published: 2026-11-27T01:33:46.202Z
tags:
  - "#sql/interview"
  - "#data/interview"
prerequisite:
  - "[SQL-Window Functions](Notes/SQL-Window%20Functions.md)"
aliases:
---

# Deduplication

[^1]
**Cause**:

- [I] Pipeline retries
- [I] Event fired twice

**Effect**:

- [i] Multiple same rows with different timestamps.

<u>Table:</u> orders

| **id** | **order\_id** | **amount** | **updated\_at** |
| ------ | ------------ | ---------- | -------------- |
| 1      | 1001         | 99         | 10:00:00       |
| 2      | 1002         | 250        | 10:05:00       |
| 3      | 1001         | 99         | 10:00:04       |
| 4      | 1002         | 250        | 10:05:00       |
| 5      | 1001         | 109        | 10:20:00       |

> [!FAQ] Return only the latest row

```sql
WITH ranked AS (
	SELECT *,
	ROW_NUMBER() OVER (
		PARTITION BY order_id
		ORDER BY updated_at DESC
	) AS rn
	FROM orders
)

SELECT *
FROM ranked
WHERE rn = 1;
```

|**id**|**order\_id**|**amount**|**updated\_at**|
|---|---|---|---|
|5|1001|109|10:20:00
|2|1002|250|10:05:00

> [!WARNING] Use `row_number()` NOT `rank()` !
> Notice that multiple entries in the `orders` table with the **same** id, have the same timestamp.
>
> If using `rank()`,  row 2 and 4 will have the same `rn=1` value, while `row_number()` gives both rows unique `rn` values.

# Running Totals

[^1]
**Use case**: QTD/YTD sales, Measure cumulative (weekly/monthly) progress.

<u>Table</u>: daily\_sales

|sale\_date     |amount     |
| --- | --- |
| Jan 01    |1,000     |
| Jan 02 |1,500
|Jan 03|800
|Jan 04|2000
|Jan 05|1,200

[^1]: [SQL was HARD until I Learned these 12 Patterns - YouTube](https://www.youtube.com/watch?v=cGtPup-EwrM)
