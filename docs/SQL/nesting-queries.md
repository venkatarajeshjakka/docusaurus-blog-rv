---
title: Nesting Queries
description: Nesting Queries
sidebar_label: "Nesting Queries"
sidebar_position: 5
---

A subquery is simply one query written inside of another.

A query is wrapped inside of another query is called **Nested Query**.

### SELECT clause subquery

```sql
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	total
FROM
	Invoice
WHERE
	total < (SELECT round(avg(total),2) AS [Average Total] FROM  Invoice)
ORDER BY
	total DESC
```

### Aggregated subqueries

```sql
SELECT
	BillingCity,
	ROUND(avg(total),2) as [City Averag],
	(SELECT ROUND(avg(total)) FROM Invoice) AS [Average]
FROM
	Invoice
GROUP BY
	BillingCity
ORDER BY
	BillingCity
```

### IN Clause subquery

```sql
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity
FROM
	Invoice
WHERE
	InvoiceDate IN
(SELECT
	InvoiceDate
FROM
	Invoice
WHERE
	InvoiceId IN (251,252,254))

```

### DISTINCT clause subquery

```sql

SELECT
	TrackId,
	Composer,
	Name
FROM
	Track
WHERE
	TrackId NOT IN
(SELECT
	DISTINCT
	TrackId
FROM
	InvoiceLine
ORDER BY
	TrackId)

```
