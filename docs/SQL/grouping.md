---
title: Grouping
description: Grouping
sidebar_label: "Grouping"
sidebar_position: 4
---

## Grouped condition

```sql
SELECT
	BillingCity,
	round(avg(total),2)
FROM
	Invoice
GROUP BY
	BillingCity
ORDER BY
	BillingCity
```

### Grouping with WHERE clause

Used for filtering non aggregate data

```sql
SELECT
	BillingCity,
	round(avg(total),2) AS [Average Bill]
FROM
	Invoice
WHERE
	BillingCity LIKE 'L%'
GROUP BY
	BillingCity
ORDER BY
	BillingCity
```

### Grouping with the HAVING clause

Used for filtering results that containing aggregates.

```sql
SELECT
	BillingCity,
	round(avg(total),2) AS [Average Bill]
FROM
	Invoice
GROUP BY
	BillingCity
HAVING
	avg(total) > 5
ORDER BY
	BillingCity
```

### Grouping with the WHERE and HAVING clause

```sql
SELECT
	BillingCity,
	round(avg(total),2) AS [Average Bill]
FROM
	Invoice
WHERE
	BillingCity LIKE 'B%'
GROUP BY
	BillingCity
HAVING
	avg(total) > 5
ORDER BY
	BillingCity
```

### Grouping by many fields

```sql
SELECT
	BillingCountry,
	BillingCity,
	round(avg(total),2) AS [Average Bill]
FROM
	Invoice
GROUP BY
	BillingCountry,
	BillingCity
HAVING
	avg(total) > 5
ORDER BY
	BillingCountry
```
