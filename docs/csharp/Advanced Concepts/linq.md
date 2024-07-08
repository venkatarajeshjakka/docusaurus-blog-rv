---
title: Linq in C#
description: C# Advanced concepts | Linq
sidebar_label: "Linq in C#"
sidebar_position: 6
---

With LINQ, programmers can write queries directly in their favorite language and get strong typed benefits.

One query syntax for all data sources.

- LINQ is a cohesive query tool independent from the underlying source.
- Composable

## Sources

- Arrays
- COllections and dictionaries
- Relational databases
- NoSQL databases
- ORM entities and data classes
- XML and JSON

## LINQ Providers

- LINQ to objects
- LINQ to XML

There are three phases of a LINQ query.

- Set up a Data Source
  - Use the appropriate LINQ provider.
  - The default provider is LINQ to Objects.
- Define the Query
  - Write the query code.
  - Query is converted to an expression tree.
- Execute the Query
  - Query runs and returns the result data.

The provider is responsible for turining your query intent inot domain-specific commands.
