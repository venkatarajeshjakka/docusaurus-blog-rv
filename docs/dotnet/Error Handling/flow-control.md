---
title: Flow control
description: Flow control in .Net
sidebar_label: "Flow control"
sidebar_position: 2
---

## Throwing Exceptions

Using exceptions for flow control is an approach to implement the **fail-fast** principle.

As soon as you encounter an error in the code, you throw an exception — effectively terminating the method, and making the caller responsible for handling the exception.

The problem is the caller must know which exceptions to handle. And this isn't obvious from the method signature alone.

Another common use case is throwing exceptions for validation errors.

Create `IServiceException` interface

```csharp
public interface IServiceException
{
    public HttpStatusCode StatusCode { get; }

    public string ErrorMessage { get; }
}
```

Create your own exception classes by deriving from the `Exception` class and implement `IServiceException` interface.

```csharp
public class DuplicationEmailException : Exception, IServiceException
{
    public HttpStatusCode StatusCode => HttpStatusCode.Conflict;

    public string ErrorMessage => "Email already Exists";
}
```

Throw custom exception if validation fails

```csharp
 public AuthenticationResult Register(
        string firstName,
        string lastName,
        string email,
        string password)
    {

        //Check if user already exists

        if (_userRepository.GetUserByEmail(email) is not null)
        {
            throw new DuplicationEmailException();
        }
        //...
    }
```

Show appropriate message and status code based upon the exception

```csharp
[Route("/error")]
public IActionResult Error()
{
    Exception? exception = HttpContext.Features
        .Get<IExceptionHandlerFeature>()?.Error;

    var (statusCode, message) = exception switch
    {
        IServiceException serviceException => ((int)serviceException.StatusCode, serviceException.ErrorMessage),
            _ => (StatusCodes.Status500InternalServerError, "An unexpected error occured.")
    };
    return Problem(statusCode: statusCode, title: message);
}
```

Advantages of throwing custom exceptions

- Defensive
- Stacktrace
- Easier debugging

Disadvantages

- Performance

## Using OneOf package

Install latest version of **OneOf** package

```csharp
Install-Package OneOf
```

Return `OneOf<T0,T1>` where `T0` exptected result, `T1` error result.

```csharp
OneOf<AuthenticationResult, DuplicateEmailError> Register(string firstName,
    string lastName, string email, string password);
```

In the implemetation either return the actual respose or error response.

```csharp
public OneOf<AuthenticationResult, DuplicateEmailError> Register(
        string firstName,
        string lastName,
        string email,
        string password)
    {

        //Check if user already exists

        if (_userRepository.GetUserByEmail(email) is not null)
        {
            return new DuplicateEmailError();
        }
        //Create user ( generate unique ID)

        var user = new User()
        {
            Email = email,
            FirstName = firstName,
            LastName = lastName,
            Password = password
        };

        _userRepository.Add(user);
        //Create JWT token

        var token = _jwtTokenGenerator.GenerateToken(user);

        return new AuthenticationResult(
            user,
            token);
    }
```

In the controller action method return based up on the response

```csharp
[HttpPost("register")]
public IActionResult Register(RegisterRequest registerRequest)
{
    OneOf<AuthenticationResult, DuplicateEmailError> registeredResult = _authenticationService.Register(
            registerRequest.FirstName,
            registerRequest.LastName,
            registerRequest.Email,
            registerRequest.Password);

    return registeredResult.Match(
        authResult => Ok(MapAuthResult(authResult)),
        _ => Problem(
                statusCode: StatusCodes.Status409Conflict,
                title: "Email already exisits."));

    }
```

Drawback of this approach is can not handle multiple error or list of errors.

## Using FluentResults package

Install latest version of **FluentResults** package

```csharp
Install-Package FluentResults
```

A `Result<T>` can store multiple Error and Success messages.

```csharp
public Result<AuthenticationResult> Register(
        string firstName,
        string lastName,
        string email,
        string password)
{

        //Check if user already exists

    if (_userRepository.GetUserByEmail(email) is not null)
    {
        return Result.Fail<AuthenticationResult>(new[] { new DuplicateEmailError() });
    }
        //Create user ( generate unique ID)

    var user = new User()
    {
        Email = email,
        FirstName = firstName,
        LastName = lastName,
        Password = password
    };

     _userRepository.Add(user);
        //Create JWT token

    var token = _jwtTokenGenerator.GenerateToken(user);

    return new AuthenticationResult(
            user,
            token);
}
```

After you get a Result object from a method you have to process it. This means, you have to check if the operation was completed successfully or not. The properties IsSuccess and IsFailed in the Result object indicate success or failure. The value of a `Result<T>` can be accessed via the properties `Value` and `ValueOrDefault`.

```csharp
[HttpPost("register")]
public IActionResult Register(RegisterRequest registerRequest)
{
    Result<AuthenticationResult> registeredResult = _authenticationService.Register(
            registerRequest.FirstName,
            registerRequest.LastName,
            registerRequest.Email,
            registerRequest.Password);

    if (registeredResult.IsSuccess)
    {
        return Ok(MapAuthResult(registeredResult.Value));
    }

    var firstError = registeredResult.Errors[0];

    if (firstError is DuplicateEmailError)
    {
        return Problem(statusCode: StatusCodes.Status409Conflict, detail: "Email already exisits.");
    }
    return Problem();
}
```

## Using ErrorOr & Domain Errors

Install latest version of **ErrorOr** package in domain layer

```csharp
Install-Package ErrorOr
```

Create a static class with the expected errors. For example:

```csharp
public static partial class Errors
{
    public static class Authentication
    {
        public static Error InValidCredentials = Error.Validation(
            code: "Auth.InvalidCred",
            description: "Invalid Credentials.");
    }
}
```

Which can later be used as following

```csharp
public ErrorOr<AuthenticationResult> Login(string email, string password)
{
    if (_userRepository.GetUserByEmail(email) is not User user)
    {
        return Errors.Authentication.InValidCredentials;
    }

    if (user.Password != password)
    {
        return Errors.Authentication.InValidCredentials;
    }

    var token = _jwtTokenGenerator.GenerateToken(user);

    return new AuthenticationResult(
            user,
            token);
    }
```

Actions that return a value on the value or list of errors

```csharp
[HttpPost("login")]
public IActionResult Login(LoginRequest loginRequest)
{
    ErrorOr<AuthenticationResult> authResult = _authenticationService.Login(
            loginRequest.Email,
            loginRequest.Password);

    return authResult.Match(
            result => Ok(MapAuthResult(result)),
            errors => Problem(errors));
}
```
