---
title: C# Fundamentals
description: C# Basic concepts
sidebar_label: "C# Fundamentals"
sidebar_position: 1
---

# C# Fundamentals

Let'd discover **C# in depth**.

## Getting Started

What we'll learn

- Data types, variables and constants
- Operators
- decision statements
- Loops
- Classes and methods
- Value types and reference types
- Working with collections
- Interfaces
- Introduction to generics
- OOP principles

## Variables and data types

```csharp
string hello = "hello world!";
int number = 6;
double number2 = 7.1;
bool isTrue = true;
```

## Operators

- Primary operators

  - Member access operator
  - Array indexer

    ```csharp
     int[] numbers = {2,3,4};
     Console.WriteLine(numbers[0]);
    ```

  - Unary
    ```csharp
     int number = 5;
     int x = ++number;
    ```

- Relational operators

```csharp
int x = 17;
int y = 19;
bool isGreater = x > y;
bool isSmaller = x < y;
bool areEqual = x == y;
```

- Conditional operators

```csharp
bool x= true;
bool y = false;
bool k = true;
bool a = x && y;
bool b = x || y;
```

## Conditional statements

- if

- switch

## Loops

- Loops with initital test

```csharp

while(condition){}

```

- Loops with final test
