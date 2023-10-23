---
title: Introduction
description: Introduction .Net
sidebar_label: "Introduction"
sidebar_position: 1
---

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

## Run Project

```csharp
dotnet run --project ./BuberDinner.Api/
```

Run using `https` profile

```csharp
dotnet run --project ./BuberDinner.Api/ --launch-profile https
```
