---
title: Open closed principle
description: Open closed principle using C#
sidebar_label: "Open Closed Principle"
sidebar_position: 2
---

The Open-Closed Principle (OCP) states that software entities should be open for extension but closed for modification.

That means adding new functionality should involve adding new code, instead of changing existing code. This ensures that any change made to the application in the future do not break the existing code.

Here is an implementation of the Open-Closed Principle in C# using abstract:

```csharp
public abstract class Shape
{
    public abstract double Area();

}
```

`Circle` & `Rectangle` class implements the area method.

```csharp
public  class Square : Shape
{
    public int Size { get; set; }

    public override double Area()
    {
       return Size * Size;
    }
}

public class Rectangle : Shape
{
    public int Width { get; internal set; }
    public int Height { get; internal set; }

    public override double Area()
    {
        return Height * Width;
    }
}
```
