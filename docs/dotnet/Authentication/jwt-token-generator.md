---
title: Generating JWT Tokens
description: Generating JWT Tokens in .Net
sidebar_label: "Generating JWT Tokens"
sidebar_position: 1
---

**JSON Web Token (JWT)** is an open standard used to share information between two parties. The information of JWT is encoded as JSON containing claims or signatures.

Install package

```csharp
System.IdentityModel.Tokens.Jwt
```

- We will set default values for “ExpireMinutes” which will be 20 minutes which is 20 minutes token expiration date.
- We will use default encryption algorithm for “SecurityAlgorithm” which will be “_HmacSha256_” and we get it from SecurityAlgorithms class.

```csharp
    public string GenerateToken(Guid userId, string firstName, string lastName)
    {
        var signedCredentials = new SigningCredentials(
            new SymmetricSecurityKey(Encoding.UTF8.GetBytes("super-secret-key")),
            SecurityAlgorithms.HmacSha256);

        var claims = new[]
        {
            new Claim(JwtRegisteredClaimNames.Sub,userId.ToString()),
            new Claim(JwtRegisteredClaimNames.Jti,Guid.NewGuid().ToString()),
            new Claim(JwtRegisteredClaimNames.GivenName,firstName),
            new Claim(JwtRegisteredClaimNames.FamilyName, lastName)
        };

        var securityToken = new JwtSecurityToken(
            issuer: "BuberDinner",
            expires: DateTime.Now.AddMinutes(20),
            claims: claims,
            signingCredentials: signedCredentials
        );

        return new JwtSecurityTokenHandler().WriteToken(securityToken);
    }
```

Resolve dependency by registering in the IoC container

```csharp
public static IServiceCollection AddInfrastructure(this IServiceCollection services)
{
    services.AddSingleton<IJwtTokenGenerator, JwtTokenGenerator>();
    return services;
}
```
