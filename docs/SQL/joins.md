---
title: Accessing Data from Multiple Tables
description: Accessing Data from Multiple Tables
sidebar_label: "Join"
sidebar_position: 2
---

A join is a command that connects the fields from two or more tables of a relational database.

```sql
SELECT
   *
FROM
	Invoice
INNER JOIN
	Customer
ON
	Invoice.CustomerId = Customer.CustomerId

```

```sql
SELECT
   c.LastName,
   c.FirstName,
   i.InvoiceId,
   i.CustomerId,
   i.InvoiceDate,
   i.total
FROM
	Invoice AS i
INNER JOIN
	Customer as c
ON
	i.CustomerId = c.CustomerId
ORDER BY
	c.CustomerId
```

## Join Types

### INNER JOIN

Only returns the matching records, Any unmatched data from either table is ignored.

### LEFT OUTER JOIN

A `LEFT OUTER JOIN` combines all the records from the left table from any matching records.

```sql
SELECT
   c.LastName,
   c.FirstName,
   i.InvoiceId,
   i.CustomerId,
   i.InvoiceDate,
   i.total
FROM
	Invoice AS i
LEFT OUTER JOIN
	Customer as c
ON
	i.CustomerId = c.CustomerId
ORDER BY
	c.CustomerId
```

### RIGHT OUTER JOIN

A `RIGHT OUTER JOIN` combines all the records from the right table from any matching records.

```sql
SELECT
   c.LastName,
   c.FirstName,
   i.InvoiceId,
   i.CustomerId,
   i.InvoiceDate,
   i.total
FROM
	Invoice AS i
RIGHT OUTER JOIN
	Customer as c
ON
	i.CustomerId = c.CustomerId
ORDER BY
	c.CustomerId
```

## Joining many tables

```sql
SELECT
	e.FirstName AS 'Employee FirstName',
	e.LastName AS 'Employee LastName',
	e.EmployeeId,
	c.FirstName,
	c.LastName,
	c.SupportRepId,
	i.CustomerId,
	i.total
FROM
	Invoice AS i
INNER JOIN
	Customer AS c
ON
	i.CustomerId = c.CustomerId
INNER JOIN
	Employee AS e
ON
	c.SupportRepId = e.EmployeeId
ORDER BY
	i.total DESC
LIMIT 10
```
