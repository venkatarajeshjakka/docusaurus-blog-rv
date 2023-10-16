---
title: Options Pattern
description: Options Pattern in .Net
sidebar_label: "Options Pattern"
sidebar_position: 1
---

The options pattern uses classes to provide strongly-typed access to groups of related settings. When configuration settings are isolated by scenario into separate classes, the app adheres to two important software engineering principles:

- **The Interface Segregation Principle (ISP) or Encapsulation**: Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.
- **Separation of Concerns**: Settings for different parts of the app aren't dependent or coupled with one another.

## Bind hierarchical configuration

The preferred way to read related configuration values is using the options pattern. The options pattern is possible through the IOptions`<TOptions>` interface, where the generic type parameter TOptions is constrained to a class. The IOptions`<TOptions>` can later be provided through dependency injection.

For example, to read the `DatabaseOptions` configuration values from an appsettings.json file:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "DatabaseOptions": {
    "MaxRetryCount": 3,
    "CommandTimeout": 30,
    "EnableDetailedError": false,
    "EnableSensitiveDataLogging": true
  }
}
```

Create the following `DatabaseOptions` class

```csharp
public class DatabaseOptions
{

    public string ConnectionString { get; set; } = string.Empty;

    public int MaxRetryCount { get; set; }

    public int CommandTimeout { get; set; }

    public bool EnableDetailedError { get; set; }

    public bool EnableSensitiveDataLogging { get; set; }

}
```

When using the options pattern, an options class:

- Must be non-abstract with a public parameterless constructor
- Contain public read-write properties to bind (fields are not bound)

The following code is part of the `DatabaseOptionsSetup.cs` C# file and:

Calls `ConfigurationBinder.Bind` to bind the `DatabaseOptions` class to the **DatabaseOptions** section.

```csharp
public class DatabaseOptionsSetup : IConfigureOptions<DatabaseOptions>
{
    private const string ConfigurationOptionSection = "DatabaseOptions";
    private readonly IConfiguration _configurations;

    public DatabaseOptionsSetup(IConfiguration configurations) => _configurations = configurations;
    public void Configure(DatabaseOptions options)
    {
        var connectionString = _configurations.GetConnectionString("Database");

        options.ConnectionString = connectionString;

        _configurations.GetSection(ConfigurationOptionSection).Bind(options);
    }
}
```

Configure Options in `program.cs`

```csharp
builder.Services.ConfigureOptions<DatabaseOptionsSetup>();
```

Access options in `Program.cs`

```csharp
builder.Services.AddDbContext<DatabaseContext>((serviceProvider, options) =>
{
    var databaseOptions = serviceProvider
    .GetRequiredService<IOptions<DatabaseOptions>>()!.Value;


    options.UseSqlServer(databaseOptions.ConnectionString,
    sqlServerActions =>
    {
        sqlServerActions.EnableRetryOnFailure(databaseOptions.MaxRetryCount);

        sqlServerActions.CommandTimeout(databaseOptions.CommandTimeout);
    });

    options.EnableDetailedErrors(databaseOptions.EnableDetailedError);
    options.EnableSensitiveDataLogging(databaseOptions.EnableSensitiveDataLogging); //Only in local or development environment

});
```
