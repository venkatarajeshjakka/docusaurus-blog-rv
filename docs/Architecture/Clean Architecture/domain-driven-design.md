---
title: Domain Driven Design in Clean Architecture
description: Project Setup using Clean Architecture CQRS using MediatR
sidebar_label: "Domain Driven Design"
sidebar_position: 2
---

## Entity

An object primarily defined by its identity is called an **Entity**

```csharp
public abstract class Entity : IEquatable<Entity>
{
    protected Entity(Guid id)
    {
        Id = id;
    }

    public Guid Id { get; private init; }

    public static bool operator ==(Entity? first, Entity? second)
    {
        return first is not null && second is not null && first.Equals(second);

    }

    public static bool operator !=(Entity? first, Entity? second)
    {
        return !(first == second);
    }
    public override bool Equals(object? obj)
    {
        if (obj is null)
        {
            return false;
        }
        if (obj.GetType() != GetType())
        {
            return false;
        }

        if (obj is not Entity entity)
        {
            return false;
        }

        return entity.Id == Id;
    }

    public bool Equals(Entity? other)
    {
        if (other is null)
        {
            return false;
        }
        if (other.GetType() != GetType())
        {
            return false;
        }

        return other.Id == Id;
    }

    public override int GetHashCode()
    {
        return Id.GetHashCode() * 41;
    }
}
```

### init

The `init` keyword defines an accessor method in a property. An init-only setter assigns a value to the property only during object construction. This enforces immutability, so that once the object is initialized, it can't be changed again.

```csharp
public abstract class Entity
{
    protected Entity(Guid id)
    {
        Id = id;
    }

    public Guid Id { get; init; }
}
```

## Domain Validations

### Throwing Exceptions

Create your own exception classes by deriving from the `Exception` class.

```csharp
public abstract class DomainExceptions : Exception
{
    protected DomainExceptions(string message)
    : base(message)
    {

    }

}
```

Throw custom exception if domain validation fails

```csharp
private void CalculateGatheringTypeDetails(int? maximumNumberOfAttendees,
    int? invitationsValidBeforeInHours)
    {
        switch (Type)
        {
            case GatheringType.WithFixedNumberOfAttendees:
                if (maximumNumberOfAttendees is null)
                {
                    throw new GatheringMaxNumberofAttendesNullDomainExceptions($"{nameof(maximumNumberOfAttendees)} can't be null");
                }
                MaximumNuberOfAttendees = maximumNumberOfAttendees;
                break;

            case GatheringType.WithExpirationForInvitations:
                if (invitationsValidBeforeInHours is null)
                {
                    throw new GatheringInvitationsValidBeforeInHoursIsNullDomainException($"{nameof(invitationsValidBeforeInHours)} can't be null");
                }

                InvitationsExpiredAtUtc = ScheduledAtUtc.AddHours(-invitationsValidBeforeInHours.Value);

                break;

            default:

                throw new ArgumentOutOfRangeException();
        }
    }
```

Advantages of throwing custom exceptions

- Defensive
- Stacktrace
- Easier debugging

Disadvantages

- Performance

### Result Object
