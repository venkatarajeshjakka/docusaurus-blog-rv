---
title: Entity in Domain Driven Design
description: Entity in Domain Driven Design
sidebar_label: "Entity"
sidebar_position: 4
---

In **Domain-Driven Design (DDD)**, an entity is a key building block used to model and represent domain objects with distinct identities and lifecycles. Entities are fundamental to the design of a domain model, and they typically represent real-world objects, concepts, or entities with a unique identity. Entities are often mutable, meaning they can change their state over time while preserving their identity.

The entities with the same `Id` are considered equal.

The only characteristic of an Entity is that it has a long-lived and (semi-)permanent identity. You can encapsulate and express that by implementing `IEquatable<T>` by overriding the Equals method and provides equality operators (== and !=). This allows you to easily compare entities based on their `Id` property.

## Entity implementation in C#

```csharp
public abstract class Entity<TId> : IEquatable<Entity<TId>>
    where TId : notnull
{
    public TId Id { get; protected set; }

    protected Entity(TId id)
    {
        Id = id;
    }

    public override bool Equals(object? obj)
    {
        return obj is Entity<TId> entity && Equals(entity.Id);

    }

    public bool Equals(Entity<TId>? other)
    {
        return Equals((object?)other);
    }

    public static bool operator ==(Entity<TId> left, Entity<TId> right)
    {
        return Equals(left, right);
    }

    public static bool operator !=(Entity<TId> left, Entity<TId> right)
    {
        return !Equals(left, right);
    }

    public override int GetHashCode()
    {
        return Id.GetHashCode();
    }
}
```

implemeting `Entity`

```csharp
public sealed class MenuItem : Entity<MenuItemId>
{
    public string Name { get; }
    public string Description { get; }

    private MenuItem(MenuItemId menuItemId, string name, string description)
        : base(menuItemId)
    {
        Name = name;
        Description = description;
    }

    public static MenuItem Create(
        string name,
        string description)
    {
        return new(
            MenuItemId.CreateUnique(),
            name,
            description);
    }

}
```
