---
title: Introduction
description: Introduction .Net
sidebar_label: "Introduction"
sidebar_position: 1
---

## Project Setup

### Create Solution

```csharp
dotnet new sln -o BuberDinner
```

### Create Project

Create Web Api

```csharp
dotnet new webapi -o BuberDinner.Api
```

Create `class` library

```csharp
dotnet new classlib -o BuberDinner.Contracts
```

### Add Projects to Solution

```csharp
dotnet sln add (ls -r **\*.csproj)
```

### Adding dependencies

```csharp
dotnet add ./BuberDinner.Api/ reference ./BuberDinner.Contracts/ ./BuberDinner.Application/
```

### Run Project

```csharp
dotnet run --project ./BuberDinner.Api/
```

Run using `https` profile

```csharp
dotnet run --project ./BuberDinner.Api/ --launch-profile https
```

## Safe storage of app secrets

Never store passwords or other sensitive data in source code. Production secrets shouldn't be used for development or test. Secrets shouldn't be deployed with the app.

### Secret Manager

The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project. In this context, a piece of sensitive data is an app secret. App secrets are stored in a separate location from the project tree. The app secrets are associated with a specific project or shared across several projects. The app secrets aren't checked into source control.

### Enable secret storage

The Secret Manager tool operates on project-specific configuration settings stored in your user profile.

The Secret Manager tool includes an `init` command. To use user secrets, run the following command in the project directory:

```csharp
dotnet user-secrets init --project ./BuberDinner.Api
```

The preceding command adds a `UserSecretsId` element within a PropertyGroup of the project file. By default, the inner text of `UserSecretsId` is a GUID. The inner text is arbitrary, but is unique to the project.

```csharp
 <PropertyGroup>
    <TargetFramework>net7.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <UserSecretsId>12dcb1fe-0754-40fe-8688-e9dd2217a575</UserSecretsId>
  </PropertyGroup>
```

### Set a secret

Define an app secret consisting of a key and its value

```csharp
dotnet user-secrets set "JwtSetting:Secret" "super-secret-key" --project ./BuberDinner.Api
```

### List the secrets

```csharp
dotnet user-secrets list --project ./BuberDinner.Api/
```

### Remove a single secret

```csharp
dotnet user-secrets remove "JwtSetting:Secret" --project ./BuberDinner.Api/
```

### Remove all secrets

```csharp
dotnet user-secrets clear
```
