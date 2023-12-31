---
title: Inheritance & access modifiers in C#
description: C# Basic concepts | Classes
sidebar_label: "Inheritance in C#"
sidebar_position: 4
---

## Inheritance

Inheritance enables you to create new classes that reuse, extends, and modify the behaviour that is defined in other classes

The class whose members are inherited is called the **base class**, and the class that inherits those members is called the **derived class**

A derived class have only one direct base class.However, inheritance is transitive.

When you define a clss to derive from another class, the derived class implicitly gains all the members of the base calls, except for its constructor.The derived class can therby reuse the code in the base class without having to re-implement it.

Inheritance represents an "is-a" relationship and not a "has-a" relationship.

## Access modifiers

Access modifiers are keywords used to specify the declared accessibility of a member or a type.

- **public**: Access is not resticted
- **protected**: Access is limited to the containing class or types derived from the containing class.
- **internal**: Access is limited to the current assembly.
- **protected internal**: Access is limited to the current assembly or types derived from the containing class
- **private**: Access is limited to the containing type.
- **private protected**: Access is limited to the containing class or types derived from the containing class with the current assembly.

## Polymorphism

You can use polymorphism if you want to have multiple forms of one or more methods of a class with the same name.

In C#, polymorphism can be achieved in two ways:

1. Compile-time Polymorphism
2. Run-time Polymorphism

### Method Overloading

Compile-time polymorphism is also known as method overloading. C# allows us to define more than one method with the same name but with different signatures. This is called method overloading.

```csharp
class Printer
{
    public void Print(string str){
        Console.WriteLine(str);
    }

    public void Print(int a){
        Console.WriteLine($"Integer {a}");
    }
}
```

### Method Overriding

This is achieved through inheritance and allows a subclass to provide a specific implementation of a method that is already defined in its parent class. The method to be executed is determined at runtime based on the actual type of the object.

```csharp
public class Shape
{
    public int Width { get; set; }
    public int Height { get; set; }

    public virtual double GetArea()
    {
        return Width * Height;
    }
}

public class Square : Shape
{
    public Square(int width)
    {
        Width = width;
    }

    public override double GetArea()
    {
        return Width * Width;
    }

}

```

Use the `virtual` keyword with a member of the base class to make it overridable, and use the `override` keyword in the derived class to indicate that this member of the base class is being redefined in the derived class
