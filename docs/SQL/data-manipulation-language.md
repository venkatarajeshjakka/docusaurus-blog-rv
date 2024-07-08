---
title: Data Manipulation Language
description: Data Manipulation Language
sidebar_label: "Data Manipulation Language"
sidebar_position: 7
---

SQL statements used to alter the data stored in the tables of a database

### Inserting data

```sql
INSERT INTO
	Artist (Name)
VALUES ('Rajesh')
```

### Updating data

- Modifies exisiting data
- Used with the `WHERE` clause

```sql
UPDATE
Artist
SET Name = 'Venkata Rajesh'
WHERE
	ArtistId = 276
```

### Deleting data

Removes existing records from a table, Used with the `WHERE` clause.

```sql
DELETE FROM
Artist
WHERE
	ArtistId = 276
```
