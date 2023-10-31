---
title: Jwt Bearer Authentication
description: Jwt Bearer Authentication in .Net
sidebar_label: "Jwt Bearer Authentication"
sidebar_position: 2
---

**JWT (JSON web token)** has become more and more popular in web development. It is an open standard that allows transmitting data between parties as a JSON object in a secure and compact way. The data transmitted using JWT between parties are digitally signed so that it can be easily verified and trusted.

Register a JWT authentication schema by using `AddAuthentication` method and specifying `JwtBearerDefaults.AuthenticationScheme`. Here, we configure the authentication schema with JWT bearer options.

```csharp

 public static IServiceCollection AddAuth(this IServiceCollection services,
    ConfigurationManager configuration)
    {
        var jwtSettings = new JwtSettings();
        configuration.Bind(JwtSettings.SectionName, jwtSettings);

        services.AddSingleton(Options.Create(jwtSettings));
        services.AddSingleton<IJwtTokenGenerator, JwtTokenGenerator>();

        services.AddAuthentication(defaultScheme: JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateAudience = true,
            ValidateIssuer = true,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            ValidIssuer = jwtSettings.Issuer,
            ValidAudience = jwtSettings.Audience,
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(jwtSettings.Secret))
        });

        return services;
    }
```

**AppSetting.Json**

```json
"JwtSetting": {
    "Secret": "super-secret-key",
    "ExpiryMinutes": 20,
    "Issuer": "BuberDinner",
    "Audience": "BuberDinner"
  }
```

Authentication Middleware `(UseAuthentication)` attempts to authenticate the user before they're allowed access to secure resources.

Authorization Middleware `(UseAuthorization)` authorizes a user to access secure resources.

```csharp
var app = builder.Build();
{
    app.UseExceptionHandler("/error");

    app.UseHttpsRedirection();
    app.UseAuthentication();
    app.UseAuthorization();
    app.MapControllers();

    app.Run();
}
```

Authorization in ASP.NET Core is controlled with `AuthorizeAttribute` and its various parameters. In its most basic form, applying the `[Authorize]` attribute to a controller, action limits access to that component to authenticated users.

```csharp
[ApiController]
[Authorize]
public class ApiController : ControllerBase
{
    //...
}
```

You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.

```csharp
[Route("auth")]
[AllowAnonymous]
public class AuthenticationController : ApiController
{
    //...
}
```
