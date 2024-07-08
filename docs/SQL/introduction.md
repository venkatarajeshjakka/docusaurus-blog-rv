---
title: Introduction to SQL
description: Introduction to SQL
sidebar_label: "Introduction"
sidebar_position: 1
---

SQL (Structured Query Language) is a powerful tool for managing and manipulating data in relational databases. SQL statements are the building blocks of database queries and operations. In this guide, we'll explore the essential SQL statements and their usage.

## Composing Queries

### Query Composition

```sql
SELECT
	FirstName,
	LastName,
	Email
FROM  Customer
```

### Custom Column Name

```sql
SELECT
	FirstName AS [Customer First Name],
	LastName AS 'Customer Last Name',
	Email AS EMAIL
FROM  Customer
```

### Sorting Query Results

```sql
SELECT
	FirstName AS [Customer First Name],
	LastName AS 'Customer Last Name',
	Email AS EMAIL
FROM  Customer
ORDER BY
	LastName DESC
```

Sorting multiple columns

```sql
SELECT
	FirstName AS [Customer First Name],
	LastName AS 'Customer Last Name',
	Email AS EMAIL
FROM  Customer
ORDER BY
    FirstName ASC,
	LastName DESC
```

### Limiting query results

```sql
SELECT
	FirstName AS [Customer First Name],
	LastName AS 'Customer Last Name',
	Email AS EMAIL
FROM  Customer
ORDER BY
    FirstName ASC,
	LastName DESC
LIMIT 10
```

## Discovering Insights in Data

### SQL Operators

| Type           | Operator  | Example                                                           | Description                                                             |
| -------------- | --------- | ----------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Arithmetic** | `+`       | `SELECT column1 + column2 AS sum_result`                          | Adds two values.                                                        |
|                | `-`       | `SELECT column1 - column2 AS difference_result`                   | Subtracts the right-hand operand from the left-hand operand.            |
|                | `*`       | `SELECT column1 * column2 AS product_result`                      | Multiplies two values.                                                  |
|                | `/`       | `SELECT column1 / column2 AS quotient_result`                     | Divides the left-hand operand by the right-hand operand.                |
| **Comparison** | `=`       | `SELECT column FROM table WHERE column = value`                   | Checks if two values are equal.                                         |
|                | `<>`      | `SELECT column FROM table WHERE column <> value`                  | Checks if two values are not equal.                                     |
|                | `!=`      | `SELECT column FROM table WHERE column != value`                  | Checks if two values are not equal (alternative syntax).                |
|                | `>`       | `SELECT column FROM table WHERE column > value`                   | Checks if the left-hand operand is greater than the right-hand operand. |
|                | `<`       | `SELECT column FROM table WHERE column < value`                   | Checks if the left-hand operand is less than the right-hand operand.    |
| **Logical**    | `AND`     | `SELECT column FROM table WHERE condition1 AND condition2`        | Returns true if both conditions are true.                               |
|                | `OR`      | `SELECT column FROM table WHERE condition1 OR condition2`         | Returns true if at least one condition is true.                         |
|                | `NOT`     | `SELECT column FROM table WHERE NOT condition`                    | Returns true if the condition is false and vice versa.                  |
|                | `LIKE`    | `SELECT column FROM table WHERE column LIKE 'pattern'`            | Matches a column value against a pattern using wildcard characters.     |
|                | `BETWEEN` | `SELECT column FROM table WHERE column BETWEEN value1 AND value2` | Checks if a value is within a specified range.                          |

### Filter numeric data

```sql
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	total
FROM Invoice
WHERE
	total = 1.98
ORDER BY
	InvoiceDate
```

### BETWEEN and IN operators

```sql
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	total
FROM Invoice
WHERE
	total BETWEEN 1.98 AND 5.00
ORDER BY
	InvoiceDate
```

```sql
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	total
FROM Invoice
WHERE
	total IN (1.98,3.96)
ORDER BY
	InvoiceDate
```

### Filter text data

```sql
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	total
FROM Invoice
WHERE
	BillingCity = 'Brussels'
ORDER BY
	InvoiceDate
```

```sql
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	total
FROM Invoice
WHERE
	BillingCity IN  ('Brussels','Orlando','Paris')
ORDER BY
	InvoiceDate,
	BillingCity
```

### Search records without an exact match

```sql
-- % wild card operator
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	total
FROM Invoice
WHERE
	BillingCity LIKE 'B%'
ORDER BY
	InvoiceDate,
	BillingCity
```

### Filter date data

```sql
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	total
FROM Invoice
WHERE
	DATE(InvoiceDate) = '2010-05-22'
ORDER BY
	InvoiceDate,
	BillingCity
```

Filter more than one condition

```sql
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	total
FROM Invoice
WHERE
	DATE(InvoiceDate) > '2010-05-22' AND total < 3
ORDER BY
	InvoiceDate,
	BillingCity
```

### Logical operator OR

```sql
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	total
FROM Invoice
WHERE
	BillingCity LIKE 'P%' OR  BillingCity LIKE 'D%'
ORDER BY
	InvoiceDate,
	BillingCity
```

### IF THEN logic with CASE

```sql
SELECT
    InvoiceDate,
    BillingAddress,
    BillingCity,
    total,
	CASE
	WHEN total < 2 THEN "Baseline Purchase"
	WHEN total BETWEEN 2 AND 6.99 THEN "Low Purchase"
	WHEN total BETWEEN 7 AND 15 THEN "Target Purchase"
	ELSE "Top Performer"
	END as PurchaseType
FROM Invoice
WHERE
    total > 1.98 AND (BillingCity LIKE 'P%' OR  BillingCity LIKE 'D%')
ORDER BY
    InvoiceDate,
    BillingCity
```
