---
title: Optimize database update
description: Optimize database update using EF Core, SQL & Dapper
sidebar_label: "Optimize database update"
sidebar_position: 5
---

Create `Employee` & `Company` class

```csharp

public class Company
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;

    public DateTime? LastSalaryUpdatedUtc { get; set; }

    public List<Employee> Employees { get; set; }

}

public class Employee
{
    public int Id { get; set; }

    public string Name { get; set; } = string.Empty;

    public decimal Salary { get; set; }

    public int CompanyId { get; set; } //Foreign Key

}
```

## Setting up Entity Framework

Create `DatabaseContext` and override `OnModelCreating` method

```csharp

public class DatabaseContext : DbContext
{

    public DatabaseContext(DbContextOptions options) : base(options)
    {

    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Company>(builder =>
        {
            builder.ToTable("Companies");
            builder
            .HasMany(company => company.Employees)
            .WithOne()
            .HasForeignKey(employee => employee.CompanyId);
        });

        modelBuilder.Entity<Employee>(builder =>
        {
            builder.ToTable("Employees");
        });

    }

}
```

The preceding code defines the database context, which is the main class that coordinates Entity Framework functionality for a data model. This class derives from the `Microsoft.EntityFrameworkCore.DbContext` class.

## Adding database migration

Add the database context to the dependency injection (DI) container in `program.cs`

```csharp

builder.Services.AddDbContext<DatabaseContext>(options =>
{
    options.UseSqlServer(builder.Configuration.GetConnectionString("Database"));

});
```

### EF update approach

```csharp
app.MapPut("increase-salaries", async (int companyId, DatabaseContext context) =>
{
    //Fetch Company from database

    var company = await context
    .Set<Company>()
    .Include(c => c.Employees)
    .FirstOrDefaultAsync(c => c.Id == companyId);

    if (company is null)
    {
        return Results.NotFound($"The company with Id '{companyId}' was not found");
    }

    foreach (var employee in company.Employees)
    {
        employee.Salary *= 1.1m;
    }

    company.LastSalaryUpdatedUtc = DateTime.UtcNow;

    await context.SaveChangesAsync();

    return Results.NoContent();
});
```

### Update using SQL

```csharp
app.MapPut("increase-salaries-sql", async (int companyId, DatabaseContext context) =>
{
    //Fetch Company from database

    var company = await context
    .Set<Company>()
    .FirstOrDefaultAsync(c => c.Id == companyId);

    if (company is null)
    {
        return Results.NotFound($"The company with Id '{companyId}' was not found");
    }

    await context.Database.BeginTransactionAsync();

    await context.Database.ExecuteSqlInterpolatedAsync(
        $"UPDATE Employees SET Salary = Salary * 1.1 WHERE CompanyId = {company.Id}");

    company.LastSalaryUpdatedUtc = DateTime.UtcNow;

    await context.SaveChangesAsync();

    await context.Database.CommitTransactionAsync();

    return Results.NoContent();
});
```

### Dapper

Install `Dapper` package

```csharp
app.MapPut("increase-salaries-sql-dapper", async (int companyId, DatabaseContext context) =>
{
    //Fetch Company from database

    var company = await context
    .Set<Company>()
    .FirstOrDefaultAsync(c => c.Id == companyId);

    if (company is null)
    {
        return Results.NotFound($"The company with Id '{companyId}' was not found");
    }

    var transaction = await context.Database.BeginTransactionAsync();

    await context.Database.GetDbConnection().ExecuteAsync(
        "UPDATE Employees SET Salary = Salary * 1.1 WHERE CompanyId = @CompanyId",
         new
         {
             CompanyId = company.Id
         },
         transaction.GetDbTransaction()
    );

    company.LastSalaryUpdatedUtc = DateTime.UtcNow;

    await context.SaveChangesAsync();

    await context.Database.CommitTransactionAsync();

    return Results.NoContent();
});
```
