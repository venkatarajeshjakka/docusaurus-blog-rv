---
title: Global Error Handling
description: Global Error Handling in .Net
sidebar_label: "Introduction"
sidebar_position: 1
---

Handle errors and customize error handling with ASP.NET Core web API's

- Middleware
- Exception Filter Attribute
- Error endpoint

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
