---
title: CQRS Validation with MediatR Pipeline and FluentValidation
description: CQRS Validation with MediatR Pipeline and FluentValidation in .Net
sidebar_label: "Validation Behavior"
sidebar_position: 2
---

Validation is an essential cross-cutting concern that you need to solve in your application. You want to ensure the request is valid before you consider processing it.

## MediatR Validation Pipeline

`ValidationBehavior` using `FluentValidation` and MediatR's `IPipelineBehavior`.

The `ValidationBehavior` acts as a middleware for the request pipeline and performs validation. If the validation fails, it will return error with a collection of `Error` objects.

`ValidateAsync` which allows you to define asynchronous validation rules.

```csharp
public class ValidationBehavior<TRequest, TResponse> : IPipelineBehavior<TRequest, TResponse>
    where TRequest : IRequest<TResponse>
    where TResponse : IErrorOr
{
    private readonly IValidator<TRequest>? _validator;

    public ValidationBehavior(IValidator<TRequest>? validator = null)
    {
        _validator = validator;
    }

    public async Task<TResponse> Handle(
        TRequest request,
        RequestHandlerDelegate<TResponse> next,
        CancellationToken cancellationToken)
    {
        //before the handler
        if (_validator is null)
        {
            return await next();
        }

        var validationResult = await _validator.ValidateAsync(request, cancellationToken);

        if (validationResult.IsValid)
        {
            return await next();
        }

        var errors = validationResult.Errors
        .ConvertAll(validationError => Error.Validation(
            validationError.PropertyName,
            validationError.ErrorMessage));
        // after the handler
        return (dynamic)errors;
    }
}
```

Register the `ValidationBehavior` in `program.cs`

```csharp
services.AddMediatR(typeof(DependencyInjection).Assembly);

services.AddScoped(
            typeof(IPipelineBehavior<,>),
            typeof(ValidationBehavior<,>));

services.AddValidatorsFromAssemblyContaining<IAssemblyMarker>();

```

## Handling Validation Error

Errors are handled in **Exception Handler**, It converts the exception to a ProblemDetails response and includes any validation errors.

```csharp
[ApiController]
public class ApiController : ControllerBase
{
    protected IActionResult Problem(List<Error> errors)
    {
        if (errors.Count is 0)
        {
            return Problem();
        }

        if (errors.All(error => error.Type == ErrorType.Validation))
        {
            return ValidationProblem(errors);
        }

        HttpContext.Items[HttpContextItemKeys.Errors] = errors;
        var firstError = errors[0];

        return Problem(firstError);
    }

    private IActionResult Problem(Error error)
    {
        var statusCode = error.Type switch
        {
            ErrorType.Conflict => StatusCodes.Status409Conflict,
            ErrorType.Validation => StatusCodes.Status400BadRequest,
            ErrorType.NotFound => StatusCodes.Status404NotFound,
            _ => StatusCodes.Status500InternalServerError
        };
        return Problem(statusCode: statusCode, title: error.Description);
    }

    private IActionResult ValidationProblem(List<Error> errors)
    {
        var modelStateDictionary = new ModelStateDictionary();

        foreach (var error in errors)
        {
            modelStateDictionary.AddModelError(error.Code, error.Description);
        }
        return ValidationProblem(modelStateDictionary);
    }
}
```
