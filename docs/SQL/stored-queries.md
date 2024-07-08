---
title: Stored Queries
description: Stored Queries
sidebar_label: "Stored Queries"
sidebar_position: 6
---

An SQL query that is saved and can be executed repeatedly or referenced by other queries

### Creating a view

```sql
CREATE VIEW V_AvgTotal AS
SELECT
	round(avg(total),2) AS [Average Total]
FROM
	Invoice
```

### Editing a view

```sql
DROP VIEW "main"."V_AvgTotal";
CREATE VIEW V_AvgTotal AS
SELECT
	avg(total) AS [Average Total]
FROM
	Invoice
```

### Joining views

```sql
CREATE VIEW V_Tracks_InvoiceLine AS
SELECT
	il.InvoiceId,
	il.UnitPrice,
	il.Quantity,
	t.Name,
	t.Composer,
	t.Milliseconds
FROM
	InvoiceLine il
INNER JOIN
	Track t
ON
	il.TrackId = t.TrackId
```

### Deleting views

```sql
DROP VIEW  V_AvgTotal
```
