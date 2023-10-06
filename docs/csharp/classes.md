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
        private string _manufacturer = "Ford"; //Fields

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

        public string GetManufacturer() //Methods
        {
            return _manufacturer;
        }

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

## Static Vs Non-Static

The terms static and non-static are used to describe the behavior and usage of members (variables, methods, properties, etc.) within classes. These concepts are crucial for understanding how to design and use classes effectively.

### Static Variables

A static variable (also known as a class variable) is shared among all instances of a class. It belongs to the class itself rather than any specific object. You can access static variables using the class name without creating an instance of the class. They are typically used for data that should be common across all instances of a class.

```csharp
public class MyClass
{
    public static int staticVariable = 10;
}

// Accessing a static variable
int value = MyClass.staticVariable;
```

### Static Methods

Static methods belong to the class and not to any particular instance. They can be called using the class name without creating an instance. These methods are often used for utility functions that don't rely on instance-specific data.

```csharp
public class MyClass
{
    public static void MyStaticMethod()
    {
        // Static method logic
    }
}

// Calling a static method
MyClass.MyStaticMethod();
```

### Instance Variables

Non-static variables (also known as instance variables) are associated with specific instances of a class. Each instance of the class has its own copy of these variables. They are used to store data that is unique to each instance of the class.

```csharp
public class MyClass
{
    public int instanceVariable = 10;
}

// Creating instances and accessing instance variables
MyClass obj1 = new MyClass();
MyClass obj2 = new MyClass();
int value1 = obj1.instanceVariable; // 10
int value2 = obj2.instanceVariable; // 10
```

### Instance Methods

Instance methods are associated with objects created from the class. They can access and manipulate instance variables and perform operations specific to individual instances.

```csharp
public class MyClass
{
    public int instanceVariable = 10;

    public void MyInstanceMethod()
    {
        // Instance method logic
        Console.WriteLine(instanceVariable);
    }
}

// Creating instances and calling instance methods
MyClass obj1 = new MyClass();
obj1.MyInstanceMethod(); // Prints 10
```
