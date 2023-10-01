---
title: Classes in C#
description: C# Basic concepts | Classes
sidebar_label: "Classes in C#"
sidebar_position: 2
---

Classes are the fundamental concepts of **Object Oriented Programming**

Modern real-world objects and define:

- **Attributes** (state,properties,fields)
- **Behaviour** (methods)

Classes describe the **structure of objects**

- Objects describe a **particular instance** of the class

Properties hold information about the model and object relevant to the problem

Create `Car` class

```csharp
    public class Car
    {
        public Car(string color, int year, int numberOfDoors)
        {
            Color = color;
            Year = year;
            NumberOfDoors = numberOfDoors;
        }

        public Car()
        {

        }
        public string Color { get; set; } = string.Empty;  //Properties

        public int Year { get; set; }

        public int NumberOfDoors { get; set; }

    }
```

Create `object` of `car` class

```csharp
    class Program
    {
        static void Main(string[] args)
        {
            Car redCar = new("red", 2024, 4);
            Console.WriteLine(redCar.Color);
            Console.ReadLine();
        }
    }
```
