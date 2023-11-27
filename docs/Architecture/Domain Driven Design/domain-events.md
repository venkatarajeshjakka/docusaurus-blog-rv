---
title: Domain Events in Domain Driven Design
description: Domain Events in Domain Driven Design
sidebar_label: "Domain Events"
sidebar_position: 4
---

Domain events are a Domain-Driven Design (DDD) tactical pattern that we can use to build loosely coupled systems.

You can raise a domain event from the domain, which represents a fact that has occurred. And other components in the system can subscribe to this event and handle it accordingly.

## Domain Events

An **event** is something that has happened in the past.

A domain event is something that happened in the domain, and other parts of the domain should be aware of it.

**Domain events** allow you to express side effects explicitly, and provide a better separation of concerns in the domain. They're an ideal way to trigger side effects across multiple aggregates inside the domain.

## Implementing Domain Events

Implement domain events by creating an `IDomainEvent` abstraction and implementing **MediatR** `INotification`.

The benefit is you can use MediatR's publish-subscribe support to publish a notification to one or multiple handlers.

```csharp
using MediatR;

public interface IDomainEvent : INotification
{

}
```

Implement a concrete domain event.

Here are a few constraints to consider when designing domain events:

- Immutability - domain events are facts, and should be immutable
- Use past tense for event naming

```csharp
public sealed record InvitationAcceptedDomainEvent(Guid InvitationId, Guid GatheringId)
                            : IDomainEvent
{

}
```

## Raising Domain Events

After you create your domain events, you want to raise them from the domain.

**Aggregate Root** calses are allowed to raise domain events

```csharp
public sealed class Gathering : AggregateRoot
{
    private readonly List<Invitation> _invitations = new();
    private readonly List<Attendee> _attendees = new();
    private Gathering(
        Guid id,
        Member creator,
        GatheringType type,
        DateTime scheduledAtUtc,
        string name,
        string? location) : base(id)
    {
        Creator = creator;
        Type = type;
        Name = name;
        Location = location;
        ScheduledAtUtc = scheduledAtUtc;
    }


    public Member Creator { get; private set; }

    public GatheringType Type { get; private set; }

    public DateTime ScheduledAtUtc { get; private set; }

    public string Name { get; private set; } = string.Empty;

    public string? Location { get; private set; }

    public int? MaximumNumberOfAttendees { get; private set; }

    public DateTime? InvitationsExpiredAtUtc { get; private set; }

    public int NumberOfAttendees { get; set; }

    public IReadOnlyCollection<Attendee> Attendees => _attendees;

    public IReadOnlyCollection<Invitation> Invitations => _invitations;

    public static Gathering Create(Guid id,
        Member creator,
        GatheringType type,
        DateTime scheduledAtUtc,
        string name,
        string? location,
        int? maximumNumberOfAttendees,
        int? invitationsValidBeforeInHours
        )
    {
        var gathering = new Gathering(
           Guid.NewGuid(),
           creator,
           type,
           scheduledAtUtc,
           name,
           location);


        //Calculate gathering type details

        gathering.CalculateGatheringTypeDetails(maximumNumberOfAttendees,
        invitationsValidBeforeInHours);

        return gathering;
    }


    public Result<Attendee> AcceptInvitation(Invitation invitation)
    {
        var reachedMaximumOfAttendees = Type == GatheringType.WithFixedNumberOfAttendees
         && NumberOfAttendees == MaximumNumberOfAttendees;

        var reachedInvitationExpiration = Type == GatheringType.WithExpirationForInvitations
        && InvitationsExpiredAtUtc < DateTime.UtcNow;

        var expired = reachedInvitationExpiration || reachedMaximumOfAttendees;

        if (expired)
        {
            invitation.Expire();

            return Result.Failure<Attendee>(new Error("Gathering.Expired", "Gathering Expired"));
        }

        var attendee = invitation.Accept();

        RaiseDomainEvent(new InvitationAcceptedDomainEvent(invitation.Id, Id));

        _attendees.Add(attendee);

        NumberOfAttendees++;
        return attendee;
    }
}
```

## Handler Domain Events

Define a class implementing `INotificationHandler<T>` and specify your domain event type as the generic argument.

```csharp
public sealed class InvitationAcceptedDomainEventHandler :
                INotificationHandler<InvitationAcceptedDomainEvent>
{

    private readonly IEmailService _emailRepository;

    private readonly IGatheringRepository _gatheringRepository;

    public InvitationAcceptedDomainEventHandler(IEmailService emailRepository,
     IGatheringRepository gatheringRepository)
    {
        _emailRepository = emailRepository;
        _gatheringRepository = gatheringRepository;
    }

    public async Task Handle(InvitationAcceptedDomainEvent notification, CancellationToken cancellationToken)
    {
        var gathering = await _gatheringRepository.GetByIdWithCreatorAsync(notification.GatheringId, cancellationToken);

        if (gathering is null)
        {
            return;
        }
        await _emailRepository.SendInvitationAcceptedEmailAsync(gathering, cancellationToken);
    }
}
```

**Domain events** can help you build a loosely coupled system. You can use them to separate the core domain logic from the side effects, which can be handled asynchronously.
