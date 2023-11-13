---
title: Configure Entity Framework Core
description: Configure Entity Framework Core
sidebar_label: "Configure EF Core"
sidebar_position: 4
---

### Enable Detailed Logging

Enables detailed errors when handling of data value exceptions that occur during processing of store query results. Such errors most often occur due to misconfiguration of entity properties. E.g. If a property is configured to be of type 'int', but the underlying data in the store is actually of type 'string', then an exception will be generated at runtime during processing of the data value. When this option is enabled and a data error is encountered, the generated exception will include details of the specific entity property that generated the error.

### EnableSensitiveDataLogging

Enables application data to be included in exception messages, logging, etc. This can include the values assigned to properties of your entity instances, parameter values for commands being sent to the database, and other such data. You should only enable this flag if you have the appropriate security measures in place based on the sensitivity of this data.

```csharp
builder.Services.AddDbContext<DatabaseContext>(options =>
{
    options.UseSqlServer(builder.Configuration.GetConnectionString("Database"));

    options.EnableDetailedErrors(true);
    options.EnableSensitiveDataLogging(true); //Only in local or development environment

});

```

## Connection Resiliency

Connection resiliency automatically retries failed database commands. The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands. EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.

```csharp
builder.Services.AddDbContext<DatabaseContext>(options =>
{
    options.UseSqlServer(builder.Configuration.GetConnectionString("Database"),
    sqlServerActions =>
    {
        sqlServerActions.EnableRetryOnFailure(3);

        sqlServerActions.CommandTimeout(30);
    });

    options.EnableDetailedErrors(true);
    options.EnableSensitiveDataLogging(true); //Only in local or development environment

});
```
