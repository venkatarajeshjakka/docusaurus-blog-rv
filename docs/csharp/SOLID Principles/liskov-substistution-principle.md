---
title: Liskov Substitution Principle
description: Open closed principle using C#
sidebar_label: "Liskov Substitution Principle"
sidebar_position: 3
---

Liskov Subsitution Principle guides how to use inheritance in object oriented programming. How to correctly derive a type a from a base type.

A derived class must be correctly substitutable for its base class. When you derived a class from a base class then the derived class should correctly implement all the methods of the base class. It should not remove some methods by throwing NotImplementedException.

The principle states that objects of a superclass should be able to be replaced with objects of a subclass without affecting the correctness of the program.

Here's a simple example in C# demonstrating Liskov Substitution Principle:

```csharp
class Shape
{
    public virtual void Draw()
    {
        Console.WriteLine("Drawing a shape");
    }
}

class Circle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a circle");
    }
}

class Rectangle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a rectangle");
    }
}

class Program
{
    static void DrawShape(Shape shape)
    {
        shape.Draw();
    }

    static void Main()
    {
        Shape circle = new Circle();
        Shape rectangle = new Rectangle();

        DrawShape(circle);    // Draws a circle
        DrawShape(rectangle); // Draws a rectangle
    }
}
```
