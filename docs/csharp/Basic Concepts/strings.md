---
title: String in C#
description: C# Basic concepts | Strings
sidebar_label: "Strings in C#"
sidebar_position: 5
---

`Strings` are a reference type but behave similar to value types.

- `Strings` objects contain an **immutable** (read-only) sequence of characters
- `Strings` use **Unicode** to support mutilple language and alphabets
- `Strings` are stored in the dynamic memory (managed heap)
- `String` objects are **arrays of characters (char[])**
- Have fixed length (String.Length)

```csharp

string s = "Hello!";
int len = s.Length; //len = 6
char ch = s[1];

```

### Equality Comparison

You can compare strings for equality using the `==` operator or the `Equals` method. The `==` operator compares the content of the strings, while `Equals` can be used to specify comparison rules (e.g., case-insensitive comparison).

```csharp
string str1 = "Hello";
string str2 = "hello";

bool areEqual = (str1 == str2);           // false
bool areEqualIgnoreCase = str1.Equals(str2, StringComparison.OrdinalIgnoreCase);  // true
```

### String Comparison

You can perform case-sensitive or case-insensitive string comparisons using the `String.Compare` method or its overloads.

```csharp
int result = string.Compare(str1, str2, StringComparison.OrdinalIgnoreCase);
```

### Substring

To extract a portion of a string, you can use the `Substring` method.

```csharp
string substring = str1.Substring(0, 3); // "Hel"
```

### Whitespace Check

To check if a string consists only of whitespace characters, you can use the `string.IsNullOrWhiteSpace` method.

```csharp
bool isNullOrWhiteSpace = string.IsNullOrWhiteSpace("  \t  "); // true
```

### String interpolation

String interpolation allows you to embed expressions within string literals using the `$` character.

```csharp
int age = 30;
string message = $"My age is {age} years old.";

```

### String Modification

Strings are immutable, which means you cannot change their contents. Instead, you create a new string with the desired modifications.

```csharp
string modifiedStr = str1.Replace("l", "L"); // "HeLLo"
```
