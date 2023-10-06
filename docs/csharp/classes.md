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

## Reference types and value types

### Reference types

A reference type is a type which has as its value a reference to the appropriate data rather than the data itself.

Common reference types in C# include:

- `string`
- `class` (user-defined reference types)
- `Arrays` (e.g., `int[]`, `string[]`)
- delegates
- Collections and containers (e.g., `List<T>`, `Dictionary<TKey, TValue>`)
- Interfaces

```csharp
static void Main(string[] args)
{
    StringBuilder str = new();
    str.Append("Hello");

    StringBuilder str1 = str;
    str.Append(" world!");

    Console.WriteLine(str);
    Console.WriteLine(str1); // Prints Hello world!, as both variables refer to the same StringBuilder

    Console.ReadLine();
}

```

### Value types

Variables of a value type directly contain the data. Assignment if a value type involves the actual data being copied.

Some common value types in C# include:

- `int`
- `float`
- `char`
- `bool`
- `struct` (user-defined value types)

Create a `struct`

```csharp
struct IntContainer
{
    public int i;
}
```

When you assign a value type variable to another variable or pass it as a parameter to a method, a copy of the data is created, and modifications to one variable do not affect the other.

```csharp
static void Main(string[] args)
{
    IntContainer first = new();
    first.i = 5;
    IntContainer second = first;
    second.i = 10;
    Console.WriteLine(first.i); //Print 5
    Console.WriteLine(second.i); //Print 10
    Console.ReadLine();
}
```

## Boxing, unboxing and type conversions

### Boxing

Boxing is the process of converting a value type to the type `object` (reference type).

Boxing a value type allocates an object instance on the heal and copies the value into the new object.

```csharp
 static void Main(string[] args)
{

    //Boxing
    int i = 123;
    object o = i;
    Console.WriteLine(o); //Print 123
    Console.ReadLine();
}
```

### Unboxing

Unboxing is an explicit conversion from the type `object` to the value type

An unboxing operation consists of two steps:

- Checking the object instance to make sure that it is a boxed value of the given value type
- Copying the value from the instance into the value-type variable

```csharp
static void Main(string[] args)
{

    //Boxing
    int i = 123;
    object o = i;
    Console.WriteLine(o); //Print 123

    //unboxing
    int j = (int)o;
    Console.WriteLine(j); //Print 123
    Console.ReadLine();

}
```

### Type Conversions

After a variable is declared, it cannot be declared again or assigned a value of another type unless that type is implicitly convertible to the variable's type

For example, a `string` cannot be implicitly converted to `int`.Therefore, after you declare a variable `i` as an `int`, you cannot assign a `string` to it.

- **Implict conversions**: No Special syntax is required because the conversion is type safe and no data will be lost. Example include conversion from smaller to larger integer types.

```csharp
static void Main(string[] args)
{

    //Implicit conversion
    int a = 123456;
    long b = a;
    Console.WriteLine(b); //Print 123456

}
```

- **Explicit conversions**: Explicit conversions require a cast operator. Casting is required when information might be lost in the conversion, or when the conversion might not succeed for other reasons. Typical example include numeric conversion to a type that has less precision or a smaller range.

```csharp
static void Main(string[] args)
{

    //Explicit conversion
    double a = 10.45;
    int b = (int)a;
    Console.WriteLine(b); //Print 10

}
```

- **User-defined conversions**: User-defined conversions are performed by special methods that you can define to enable explicit and implicit conversions between custom types.
- **Conversion with helper classes**: To convert between non-compatiable types, such as integers and `System.DateTime` objects, or hexadecimal string and byte arrays, you use the `System.BitConverter` class, the `System.Convert` class, and the Parse methods of the built-in numeric types.
