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

## Setting Up Options Pattern Using IConfiguration

### Creating The Options Class

Create the `JwtSettings` class to hold that configuration

```csharp
public class JwtSettings
{
    public string Secret { get; init; } = null!;

    public int ExpiryMinutes { get; init; }

    public string Issuer { get; init; } = null!;

    public string Audience { get; init; } = null!;
}
```

Inside of appsettings.json file we have the following configuration values

```json
"JwtSetting": {
    "Secret": "super-secret-key",
    "ExpiryMinutes": 20,
    "Issuer": "BuberDinner",
    "Audience": "BuberDinner"
  }
```

### Setting Up Options Pattern Using IConfiguration

Use the `IConfiguration` instance that we can access while registering services.

We need to call the `IServiceCollection.Configure<TOptions>` method, and specify the `JwtSettings` as the generic argument:

```csharp

 services.Configure<JwtSettings>(configuration.GetSection("JwtSettings"));

```

## Setting Up Options Pattern Using IConfigureOptions

`IConfigureOptions` interface to define a class to configure our strongly typed options.

There are two steps that we need to follow in this case:

- Create the `IConfigureOptions` implementation
- Call `IServiceCollection.ConfigureOptions<TOptions>` with our `IConfigureOptions` implementation as the generic argument

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

We now have access to dependency injection in the `DatabaseOptionsSetup` class. This means that we can resolve other services that we can use to get the configuration values.

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

## Injecting Options With IOptions

Inject `IOptions<JwtSettings>` from the constructor.

```csharp
private readonly JwtSettings _jwtSettings;
public JwtTokenGenerator(IDateTimeProvider dateTimeProvider,
    IOptions<JwtSettings> jwtSettings)
{
   _dateTimeProvider = dateTimeProvider;
  _jwtSettings = jwtSettings.Value;
}
```

The actual `JwtSettings` instance is available on the `IOptions<JwtSettings>.Value` property.

The `IOptions` instance that we injected here is configured as a Singleton in dependency injection.
