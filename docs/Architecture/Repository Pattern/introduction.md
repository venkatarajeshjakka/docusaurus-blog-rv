---
title: Repository Pattern
description: Repository Pattern
sidebar_label: "Introduction"
sidebar_position: 1
---

_Repositories are classes or components that encapsulate the logic required to access the data sources._

The repository pattern is a widely adopted architectural design pattern used in C# and .NET applications to facilitate the separation of concerns between the data access layer and the business logic layer. By abstracting data persistence and retrieval, the repository pattern promotes code maintainability, testability, and scalability.

The repository pattern is a software design pattern that acts as an intermediary between the business logic and the data storage. It provides a consistent interface for performing CRUD (Create, Read, Update, Delete) operations on data entities while encapsulating the complexities of data access.

## Benefits of the Repository Pattern

### Seperation of Concerns:

The repository pattern separates the data access logic from the rest of the application, allowing for a modular and maintainable codebase. It isolates the business logic from the underlying data storage implementation, enabling changes to either component without affecting the other.

### Testability

The repository pattern simplifies unit testing by allowing the data access layer to be easily mocked. With well-defined interfaces, you can replace the actual repository implementation with a mock repository during testing, enabling thorough testing of the business logic without the need for a physical database.

### Code Reusability

By abstracting the data access logic into repositories, you can reuse these repositories across multiple parts of the application. This reduces code duplication and promotes a consistent approach to data access throughout the system.

### Scalability

The repository pattern provides a structured approach to managing data access, facilitating scalability. You can introduce caching mechanisms, database sharding, or other performance optimizations within the repository layer to improve the application’s scalability as require.

## Repository Pattern Implementation

### Define the Repository Interface

First,Create interface `IUserRespository` that defines the contract for data access operations.

```csharp
public interface IUserRespository
{
    void Add(User user);

    User? GetUserByEmail(string email);

}

```

### Implement the Repository

Next, create concrete repository classes that implement the repository interface. These classes interact with the underlying data storage and provide the necessary operations for data retrieval and manipulation.

```csharp
public class UserRespository : IUserRespository
{
    private readonly List<User> _users = new();

    public void Add(User user)
    {
        _users.Add(user);
    }

    public User? GetUserByEmail(string email)
    {
        var user = _users.SingleOrDefault(x => x.Email == email);

        return user;
    }
}
```

### Dependency Injection

To utilize the repository within the business logic layer, employ dependency injection. By injecting the repository interface into your business logic classes, you decouple them from the specific repository implementation, making it easier to switch repositories or mock them during testing

```csharp
public class AuthenticationService : IAuthenticationService
{
    private readonly IJwtTokenGenerator _jwtTokenGenerator;
    private readonly IUserRespository _userRepository;
    public AuthenticationService(IJwtTokenGenerator jwtTokenGenerator,
    IUserRespository userRepository)
    {
        _jwtTokenGenerator = jwtTokenGenerator;
        _userRepository = userRepository;
    }

}
```
