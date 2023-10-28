---
title: Flow control
description: Flow control in .Net
sidebar_label: "Flow control"
sidebar_position: 2
---

## Throwing Exceptions

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
<ItemGroup>

    <PackageReference Include="OneOf" Version="3.0.263" />
  </ItemGroup>
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
