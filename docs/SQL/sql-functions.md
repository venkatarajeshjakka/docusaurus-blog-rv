---
title: SQL Functions
description: SQL Functions
sidebar_label: "SQL Functions"
sidebar_position: 3
---

SQL functions are essential tools for working with databases. They allow you to perform operations on data, extract information, and transform results.

| Function Type           | Example                        | Description                                                     |
| ----------------------- | ------------------------------ | --------------------------------------------------------------- |
| **String Functions**    | `CONCAT()`                     | Concatenates two or more strings.                               |
|                         | `LENGTH()`                     | Returns the length of a string.                                 |
|                         | `UPPER()` / `LOWER()`          | Converts a string to uppercase or lowercase.                    |
| **Numeric Functions**   | `SUM()`                        | Calculates the sum of values in a numeric column.               |
|                         | `AVG()`                        | Calculates the average of values in a numeric column.           |
|                         | `ROUND()`                      | Rounds a numeric value to a specified number of decimal places. |
| **Date Functions**      | `NOW()`                        | Returns the current date and time.                              |
|                         | `YEAR()` / `MONTH()` / `DAY()` | Extracts the year, month, or day from a date.                   |
|                         | `DATEDIFF()`                   | Calculates the difference between two dates.                    |
| **Aggregate Functions** | `COUNT()`                      | Counts the number of rows in a result set.                      |
|                         | `MIN()` / `MAX()`              | Finds the minimum or maximum value in a column.                 |

### Concatenating String

```sql
SELECT
	FirstName,
	LastName,
	Address,
	FirstName || ' ' ||LastName AS 'Name'
FROM
	Customer
WHERE
	Country = "USA"
```

### Truncate

```sql
SELECT
	FirstName,
	LastName,
	Address,
	FirstName || ' ' ||LastName AS 'Name',
	LENGTH(PostalCode),
	PostalCode,
	SUBSTR(PostalCode,1,5) AS [Zip Code]
FROM
	Customer
WHERE
	Country = "USA"
```

### UPPER and LOWER string functions

```sql

SELECT
	FirstName,
	LastName,
	Address,
	FirstName || ' ' ||LastName AS 'Name',
	LENGTH(PostalCode),
	PostalCode,
	SUBSTR(PostalCode,1,5) AS [Zip Code],
	UPPER(FirstName) AS [Upper FirstName],
	LOWER(LastName) AS [lower FirstName]
FROM
	Customer
WHERE
	Country = "USA"
```

### DATE Functions

Allow the manipulation of data that is stored in various date and time formats.

```sql
SELECT
	FirstName,
	LastName,
	BirthDate,
	strftime('%Y-%m-%d',BirthDate) AS [Formatted BirthDate],
	strftime('%Y-%m-%d','now')-strftime('%Y-%m-%d',BirthDate) AS Age
FROM
	Employee
```

### AGGREGATE() Function

Turns a range of numbers into a single point of data.

```sql
SELECT
	SUM(total) as [Total Sales],
	ROUND(AVG(total),2) as [Average Sales],
	MAX(total) as [Maximum Sale],
	MIN(total) as [Minimum Sale],
	COUNT(*) as [Sales Count]
FROM
	Invoice
```

### Nesting Function

A function contained witin another function

```sql
SELECT
	ROUND(AVG(total),2) as [Average Sales],
FROM
	Invoice
```
