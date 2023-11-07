---
title: Creating and Configuring a Model
description: Creating and Configuring a Model using EF Core
sidebar_label: "Create a Model Overview"
sidebar_position: 2
---

**EF Core** uses a metadata model to describe how the application's entity types are mapped to the underlying database. This model is built using a set of conventions - heuristics that look for common patterns. The model can then be customized using mapping attributes (also known as data annotations) and/or calls to the ModelBuilder methods (also known as fluent API) in OnModelCreating, both of which will override the configuration performed by conventions.

## Grouping configuration

To reduce the size of the `OnModelCreating` method all configuration for an entity type can be extracted to a separate class implementing `IEntityTypeConfiguration<TEntity>`.

```csharp
public class BreakfastConfigurations : IEntityTypeConfiguration<Breakfast>
{
    public void Configure(EntityTypeBuilder<Breakfast> builder)
    {
        builder.HasKey(x => x.Id);

        builder.Property(x => x.Id)
        .ValueGeneratedNever();

        builder.Property(b => b.Name)
        .HasMaxLength(Breakfast.MaxNameLength);

        builder.Property(b => b.Description)
       .HasMaxLength(Breakfast.MaxDescriptionLength);

        builder.Property(b => b.Savory)
        .HasConversion(
            v => string.Join(',', v),
            v => v.Split(',', StringSplitOptions.RemoveEmptyEntries).ToList());

        builder.Property(b => b.Sweet)
        .HasConversion(
            v => string.Join(',', v),
            v => v.Split(',', StringSplitOptions.RemoveEmptyEntries).ToList());
    }
}
```

### Applying all configurations in an assembly

It is possible to apply all configuration specified in types implementing `IEntityTypeConfiguration` in a given assembly.

```csharp

public class BuberBreakfastDbContext : DbContext
{
    public BuberBreakfastDbContext(DbContextOptions<BuberBreakfastDbContext> options)
        : base(options)
    {

    }

    public DbSet<Breakfast> Breakfasts { get; set; } = null!;

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.ApplyConfigurationsFromAssembly(typeof(BuberBreakfastDbContext).Assembly);
    }

}
```
