---
title: Value Objects in Domain Driven Design
description: Value Objects in Domain Driven Design
sidebar_label: "Value Objects"
sidebar_position: 3
---

There are two main characteristics for value objects:

- They have no identity.
- They are immutable.

The values of a value object must be immutable once the object is created. Therefore, when the object is constructed, you must provide the required values, but you must not allow them to change during the object's lifetime.

## Value object implementation in C#

Value object base class that has basic utility methods like equality based on the comparison between all the attributes (since a value object must not be based on identity) and other fundamental characteristics.

```csharp
public abstract class ValueObject : IEquatable<ValueObject>
{
    public abstract IEnumerable<object> GetEqualityComponents();

    public override bool Equals(object? obj)
    {
        if (obj is null || obj.GetType() != GetType())
        {
            return false;
        }

        var valueObject = (ValueObject)obj;

        return GetEqualityComponents()
        .SequenceEqual(valueObject.GetEqualityComponents());
    }

    public static bool operator ==(ValueObject left, ValueObject right)
    {
        return Equals(left, right);
    }

    public static bool operator !=(ValueObject left, ValueObject right)
    {
        return !Equals(left, right);
    }

    public override int GetHashCode()
    {
        return GetEqualityComponents()
       .Select(x => x?.GetHashCode() ?? 0)
       .Aggregate((x, y) => x ^ y);

    }

    public bool Equals(ValueObject? other)
    {
        return Equals((object?)other);
    }
}

```

Implementing `ValueObject`

```csharp
public sealed class MenuId : ValueObject
{
    public Guid Value { get; }

    private MenuId(Guid value)
    {
        Value = value;
    }

    public static MenuId CreateUnique()
    {
        return new(Guid.NewGuid());
    }
    public override IEnumerable<object> GetEqualityComponents()
    {
        yield return Value;
    }
}
```
