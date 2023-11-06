---
title: Getting Started with EF Core
description: Getting Started with EF Core
sidebar_label: "EF Core Overview"
sidebar_position: 1
---

Entity Framework (EF) Core is a lightweight, extensible, open source and cross-platform version of the popular Entity Framework data access technology.

EF Core can serve as an object-relational mapper (O/RM), which:

- Enables .NET developers to work with a database using .NET objects.
- Eliminates the need for most of the data-access code that typically needs to be written.

## Get Entity Framework Core

EF Core is shipped as NuGet packages. To add EF Core to an application, install the NuGet package for the database provider you want to use.

```csharp
Microsoft.EntityFrameworkCore.SqlServer
Microsoft.EntityFrameworkCore.Tools
```

## The model

With EF Core, data access is performed using a model. A model is made up of entity classes and a context object that represents a session with the database. The context object allows querying and saving data.

EF supports the following model development approaches:

- Generate a model from an existing database.
- Hand code a model to match the database.
- Once a model is created, use EF Migrations to create a database from the model. Migrations allow evolving the database as the model changes.
