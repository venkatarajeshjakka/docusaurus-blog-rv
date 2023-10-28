---
title: Global Error Handling
description: Global Error Handling in .Net
sidebar_label: "Introduction"
sidebar_position: 1
---

Handle errors and customize error handling with ASP.NET Core web API's

- Middleware
- Exception Filter Attribute
- Exception Handler

Unhandled exception, it generates a default plain-text response

## Middleware

Create `ErrorHandlingMiddleware.cs` middleware `class` in API project

```csharp
public class ErrorHandlingMiddleware
{
    private readonly RequestDelegate _next;

    public ErrorHandlingMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            await HandleExecptionAsync(context, ex);
        }
    }

    private static Task HandleExecptionAsync(HttpContext context, Exception exception)
    {
        var code = HttpStatusCode.InternalServerError;

        var result = JsonSerializer.Serialize(new
        {
            error = "an error occured while processing your request"
        });

        context.Response.ContentType = "application/json";
        context.Response.StatusCode = (int)code;

        return context.Response.WriteAsync(result);
    }
}
```

Add middleware in the request pipeline in `program.cs` class

```csharp
app.UseMiddleware<ErrorHandlingMiddleware>();
```

## Exception Filter

An exception filter is executed when a controller method throws any unhandled exception that is not an `HttpResponseException` exception.

Exception filters implement the `System.Web.Http.Filters.IExceptionFilter` interface. The simplest way to write an exception filter is to derive from the `System.Web.Http.Filters.ExceptionFilterAttribute` class and override the `OnException` method.

Here is a filter that handles exceptions into HTTP status code 500:

```csharp
public class ErrorHandlingFilterAttribute : ExceptionFilterAttribute
{

    public override void OnException(ExceptionContext context)
    {
        var exception = context.Exception;

        var problemDetails = new ProblemDetails
        {
            Type = "https://datatracker.ietf.org/doc/html/rfc7231#section-6.6.1",
            Instance = context.HttpContext.Request.Path,
            Status = (int)HttpStatusCode.InternalServerError,
            Title = "An error occured while processing your request."
        };

        context.Result = new OkObjectResult(problemDetails);

        context.ExceptionHandled = true;
    }
}
```

### Registering Exception Filters

There are several ways to register a Web API exception filter:

- By action
- By controller
- Globally

To apply the filter to a specific action, add the filter as an attribute to the action:

```csharp
[HttpPost("login")]
[ErrorHandlingFilter]
public IActionResult Login(LoginRequest loginRequest)
{
    var authResult = _authenticationService.Login(
            loginRequest.Email,
            loginRequest.Password);

     var response = new AuthenticationResponse(
            authResult.User.Id,
            authResult.User.FirstName,
            authResult.User.LastName,
            authResult.User.Email,
            authResult.Token);

    return Ok(response);
}
```

To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:

```csharp

[ApiController]
[Route("auth")]
[ErrorHandlingFilter]
public class AuthenticationController : ControllerBase
{
   //....
}
```

To apply the filter globally to all Web API controllers. In `Program.cs`, add the action filter to the filters collection:

```csharp

 builder.Services.AddControllers(options =>
    options.Filters.Add<ErrorHandlingFilterAttribute>());

```

## Exception handler

use Exception Handling Middleware to produce an error payload

In `Program.cs`, call `UseExceptionHandler` to add the Exception Handling Middleware:

```csharp
var app = builder.Build();
{

    //app.UseMiddleware<ErrorHandlingMiddleware>();
    app.UseExceptionHandler("/error");

    app.UseHttpsRedirection();

    app.MapControllers();

    app.Run();
}
```

Configure a controller action to respond to the `/error` route:

```csharp
[Route("/error")]
public IActionResult Error()
{
    Exception? exception = HttpContext.Features
        .Get<IExceptionHandlerFeature>()?.Error;

    return Problem(title: exception?.Message);
}
```

## Implement Custom Problem Details Factory

Create class `BuberDinnerProblemDetailsFactory` that implements `ProblemDetailsFactory`.

Add custom properties in the method `ApplyProblemDetailsDefaults`.

```csharp
public class BuberDinnerProblemDetailsFactory : ProblemDetailsFactory
{
    private readonly ApiBehaviorOptions _options;

    public BuberDinnerProblemDetailsFactory(
        IOptions<ApiBehaviorOptions> options)
    {
        _options = options?.Value ?? throw new ArgumentNullException(nameof(options));

    }

    public override ProblemDetails CreateProblemDetails(
        HttpContext httpContext,
        int? statusCode = null,
        string? title = null,
        string? type = null,
        string? detail = null,
        string? instance = null)
    {
        statusCode ??= 500;

        var problemDetails = new ProblemDetails
        {
            Status = statusCode,
            Title = title,
            Type = type,
            Detail = detail,
            Instance = instance,
        };

        ApplyProblemDetailsDefaults(httpContext, problemDetails, statusCode.Value);

        return problemDetails;
    }

    public override ValidationProblemDetails CreateValidationProblemDetails(
        HttpContext httpContext,
        ModelStateDictionary modelStateDictionary,
        int? statusCode = null,
        string? title = null,
        string? type = null,
        string? detail = null,
        string? instance = null)
    {
        ArgumentNullException.ThrowIfNull(modelStateDictionary);

        statusCode ??= 400;

        var problemDetails = new ValidationProblemDetails(modelStateDictionary)
        {
            Status = statusCode,
            Type = type,
            Detail = detail,
            Instance = instance,
        };

        if (title != null)
        {
            // For validation problem details, don't overwrite the default title with null.
            problemDetails.Title = title;
        }

        ApplyProblemDetailsDefaults(httpContext, problemDetails, statusCode.Value);

        return problemDetails;
    }

    private void ApplyProblemDetailsDefaults(HttpContext httpContext, ProblemDetails problemDetails, int statusCode)
    {
        problemDetails.Status ??= statusCode;

        if (_options.ClientErrorMapping.TryGetValue(statusCode, out var clientErrorData))
        {
            problemDetails.Title ??= clientErrorData.Title;
            problemDetails.Type ??= clientErrorData.Link;
        }

        var traceId = Activity.Current?.Id ?? httpContext?.TraceIdentifier;
        if (traceId != null)
        {
            problemDetails.Extensions["traceId"] = traceId;
        }

        problemDetails.Extensions.Add("CustomProperty", "customValue");
    }
}
```

Configure in `program.cs`

```csharp
builder.Services.AddSingleton<ProblemDetailsFactory, BuberDinnerProblemDetailsFactory>();
```
