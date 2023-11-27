---
title: EF Core Interceptors
description: Interceptors using EF Core
sidebar_label: "EF Core Interceptors"
sidebar_position: 5
---

EF Core interceptors allow you to intercept, change, or suppress EF Core operations. Every interceptor implements the `IInterceptor` interface. A few common derived interfaces include `IDbCommandInterceptor`, `IDbConnectionInterceptor`, and `IDbTransactionInterceptor`.

The most popular one is the ISaveChangesInterceptor. It allows you to add behavior before or after saving changes.

Interceptors are registered for each `DbContext` instance when configuring the context.

## Publish Domain Events With EF Interceptors

Domain events are a DDD tactical pattern to create loosely coupled systems.

Domain events allow you to express side effects explicitly and provide a better separation of concerns in the domain.

You can create an `IDomainEvent` interface, which derives from MediatR.INotification. This allows you to use the `IPublisher` to publish domain events and handle them asynchronously.

```csharp
using MediatR;

public interface IDomainEvent : INotification
{
}
```

`PublishDomainEventsInterceptor` that also inherits from `SaveChangesInterceptor`. However, this time, we're using the `SavingChangesAsync` to publish the domain events while saving changes in the database.

```csharp
public class PublishDomainEventsInterceptor : SaveChangesInterceptor
{
    private readonly IPublisher _publisher;

    public PublishDomainEventsInterceptor(IPublisher publisher)
    {
        _publisher = publisher;
    }

    public override InterceptionResult<int> SavingChanges(DbContextEventData eventData, InterceptionResult<int> result)
    {
        PublishDomainEvents(eventData.Context).GetAwaiter().GetResult();
        return base.SavingChanges(eventData, result);

    }

    public async override ValueTask<InterceptionResult<int>> SavingChangesAsync(DbContextEventData eventData, InterceptionResult<int> result, CancellationToken cancellationToken)
    {
        await PublishDomainEvents(eventData.Context);
        return await base.SavingChangesAsync(eventData, result, cancellationToken);

    }

    private async Task PublishDomainEvents(DbContext? dbcontext)
    {
        if (dbcontext is null)
        {
            return;
        }
        // Get Hold of all the various entities
        var entitiesWithDomainEvents = dbcontext.ChangeTracker.Entries<IHasDomainEvents>()
        .Where(entry => entry.Entity.DomainEvents.Any())
        .Select(entry => entry.Entity)
        .ToList();
        //Get Hold of all the various domain events
        var domainEvents = entitiesWithDomainEvents.SelectMany(entry => entry.DomainEvents).ToList();

        //Clear domain events
        entitiesWithDomainEvents.ForEach(entry => entry.ClearDomainEvents());
        //Publish domain events

        foreach (var domainEvent in domainEvents)
        {
            await _publisher.Publish(domainEvent);
        }

    }
}
```

## Configuring Dependency Injection

EF interceptors should be lightweight and stateless. You can add them to the DbContext by calling AddInterceptors and passing in the interceptor instances.

I like to configure the interceptors with Dependency Injection for two reasons:

- It allows me also to use DI in the interceptors (be mindful that they are singletons)
- To simplify adding the interceptors to the DbContext using AddDbContext

```csharp
services.AddScoped<PublishDomainEventsInterceptor>();

services.AddDbContext<BuberDinnerDbContent>(
    (sp, options) => options
        .UseSqlServer(connectionString)
        .AddInterceptors(
            sp.GetRequiredService<PublishDomainEventsInterceptor>()));
```
