---
title: Abstraction & Encapsulation in C#
description: C# Basic concepts | Classes
sidebar_label: "Abstraction & Encapsulation in C#"
sidebar_position: 3
---

Abstraction is the process of simplifying complex systems by modeling classes based on the essential properties and behaviors they possess. In other words, it involves creating abstract representations of real-world objects and their interactions.

In C#, you achieve abstraction through:

## Abstract classes:

Abstract classes allow you to declare methods without providing an implementation. These methods are meant to be implemented by derived classes.

```csharp
public abstract class Shape
{
    public abstract void Draw();
}
```

## Interfaces:

Interfaces define a contract that classes must adhere to by implementing the specified methods and properties.

```csharp
public interface IDrawable
{
    void Draw();
}
```

## Encapsulation

Encapsulation is a technique to implement abstraction in code. Create classes and their members with appropriate access modifiers to show or hide details and complexity.

Encapsulation helps in hiding the internal details of an object and exposing only what is necessary. It contributes to the concept of information hiding and makes the implementation details of a class less prone to accidental misuse. Together with abstraction, encapsulation is a key principle in creating modular, maintainable, and extensible software systems.

Most object-oriented programming languages allow you to create classes and their properties and methods along with the access modifiers such as public, private, protected, and internal to show or hide data members and implementation details. Interfaces and abstract classes can also be used for encapsulation.

In C#, you achieve encapsulation through:

```csharp
public class Student
{
    private string _firstName;

    public string FirstName
    {
        get { return _firstName; }
        set { _firstName = value; }
    }

    private string _middleName;

    public string MiddleName
    {
        get { return _middleName; }
        set { _middleName = value; }
    }


    private string _lastName;

    public string LastName
    {
        get { return _lastName; }
        set { _lastName = value; }
    }


    public string FullName
    {
        get { return _firstName + " " + _lastName; }
    }

}
```
