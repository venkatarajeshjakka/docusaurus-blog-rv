---
title: Domain Driven Design in Clean Architecture
description: Project Setup using Clean Architecture CQRS using MediatR
sidebar_label: "Domain Driven Design"
sidebar_position: 2
---

Domain-Driven Design (DDD) is a powerful approach to developing software that puts the focus on understanding and modeling the problem domain, leading to more maintainable, robust, and expressive code.

## Understanding DDD

Domain-Driven Design is an approach to software development that focuses on the domain, the problem you are trying to solve, rather than the technical aspects of the system. It encourages collaboration between domain experts and developers to create a shared understanding of the domain.

## Building Blocks of DDD

In Domain-Driven Design (DDD), aggregates, entities, and value objects are fundamental concepts used to model the domain of your application. These concepts help you create a rich, expressive, and maintainable domain model.

### Entity

An object primarily defined by its identity is called an **Entity**.

Entities are objects with a distinct identity and mutable state. They are the core building blocks of your domain model and are often part of aggregates.

#### init

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

### Aggregates

Aggregates are clusters of related domain objects that are treated as a single unit. They ensure consistency and transactionality within a specific part of the domain.

#### Aggregate Rules

- Reference other aggregates by id
- Changes are commited and rolled back as a whole
- Changes to an aggregate are done via the root

#### Aggregate modeling steps

- Define each of the entities is an aggregate
- Merge aggregates to enforce invariants
- Merge aggregates that cannot tolerate eventual consistancy

### Value objects

Value objects represent attributes or properties of domain objects and have no identity. They are immutable and help ensure strong typing and encapsulation.
