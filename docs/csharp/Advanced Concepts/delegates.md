---
title: Delegates in C#
description: C# Advanced concepts | Delegates
sidebar_label: "Delegates in C#"
sidebar_position: 1
---

A delegates is a type safe function pointer.A delegate is a type that represents references to methods with a particular parameter list and return type.

When you instantiate a delegate, you can associate its instance with any method with a compatible signature and return type. You can invoke (or call) the method through the delegate instance.

Any method from any accessible class or struct that matches the delegate type can be assigned to the delegate. The method can be either static or an instance method.

This ability to refer to a method as a parameter makes delegates ideal for defining callback methods.

Delegates are commonly used to implement events and call back methods. All delegates are derived implicitly from the System.

### Static Method

```csharp
public class Program
{

    delegate void LogDel(string text);
    static void Main(string[] args)
    {
        LogDel logDel = new LogDel(LogTextToScreen);

        logDel("text");

    }
    static void LogTextToScreen(string text)
    {
        Console.WriteLine($"{DateTime.Now}: {text}");
    }

}
```

### Instance Method

```csharp
public class Program
{

    delegate void LogDel(string text);
    static void Main(string[] args)
    {
        Log log = new();
        LogDel logDel = new(log.LogTextToScreen);

        logDel("text");

    }


}

public class Log
{
    public void LogTextToScreen(string text)
    {
        Console.WriteLine($"{DateTime.Now}: {text}");
    }
}
```

## Multicast Delegates

A useful property of delegate objects is that multiple objects can be assigned to one delegate instance by using the `+` operator. The multicast delegate contains a list of the assigned delegates. When the multicast delegate is called, it invokes the delegates in the list, in order. Only delegates of the same type can be combined.

The `-` operator can be used to remove a component delegate from a multicast delegate.

```csharp
public class Program
{

    delegate void LogDel(string text);
    static void Main(string[] args)
    {
        //Declare instances of the custom delegate.

        LogDel logScreen, goodBye, multiDel;
        Log log = new();
        logScreen = new(log.LogTextToScreen);
        goodBye = new(log.Goodbye);
        multiDel = logScreen + goodBye;

        multiDel("rajesh");

    }


}

public class Log
{
    public void LogTextToScreen(string text)
    {
        Console.WriteLine($"{DateTime.Now}: {text}");
    }

    public void Goodbye(string text)
    {
        Console.WriteLine($"  Goodbye, {text}!");
    }
}
```

## Func, Action and Predicate

These generic delegates are available in the system namespace

### Func Delegate

`Func` is a generic delegate included in the System namespace. It has zero or more **input** parameters and one **out** parameter. The last parameter is considered as an out parameter.

The `Func` delegate that takes one input parameter and one out parameter is defined in the System namespace, as shown below:

```csharp
public delegate TResult Func<in T, out TResult>(T arg);
```

The following `Func` delegate takes two input parameters of int type and returns a value of int type:

```csharp
public class Program
{
    static void Main(string[] args)
    {
        MathClass mathClass = new MathClass();

        Func<int, int, int> calc = mathClass.Sum;

        int result = calc(1, 2);
        Console.WriteLine(result);
    }

}

public class MathClass
{
    public int Sum(int a, int b)
    {
        return a + b;
    }
}
```

### Func with an Anonymous Method

You can assign an anonymous method to the `Func` delegate by using the delegate keyword.

```csharp
static void Main(string[] args)
{
    MathClass mathClass = new MathClass();

    Func<int, int, int> calc = delegate (int a, int b) { return a + b; };

    int result = calc(1, 2);
    Console.WriteLine(result);
}
```

### Func with Lambda Expression

Shorter way of expressing **Anonymous Method**

```csharp
static void Main(string[] args)
{
    MathClass mathClass = new MathClass();

    Func<int, int, int> calc = (a, b) => a+b;

    int result = calc(1, 2);
    Console.WriteLine(result);
}
```

### Action Delegate

An `Action` type delegate is the same as `Func` delegate except that the Action delegate doesn't return a value.

```csharp
public delegate void Action<in T>(T obj);
```

The following `Action` delegate takes three input parameters of `int` & `string` type and with out return type:

```csharp
static void Main(string[] args)
{
    Action<int, string, string> displayEmployeeRecords = (arg1, arg2, arg3) =>
    Console.WriteLine($"Id: {arg1}{Environment.NewLine}First Name: {arg2}{Environment.NewLine} LastName: {arg3}");

    displayEmployeeRecords(1, "Sam", "Curry");
}
```

### Predicate Delegate

`Predicate` is the delegate like `Func` and `Action` delegates. It represents a method containing a set of criteria and checks whether the passed parameter meets those criteria. A predicate delegate methods must take one input parameter and return a boolean - true or false.

```csharp
public delegate bool Predicate<in T>(T obj);
```

Below is the example of implementing `Predicate`.

```csharp
public class Program
{
    static void Main(string[] args)
    {
        Action<int, string, string, decimal, char, bool> displayEmployeeDetails = (arg1, arg2, arg3, arg4, arg5, arg6) => Console.WriteLine($"Id: {arg1}{Environment.NewLine}First Name: {arg2}{Environment.NewLine}Last Name: {arg3}{Environment.NewLine}Annual Salary: {arg4}{Environment.NewLine}Gender: {arg5}{Environment.NewLine}Manager: {arg6}");

        //***********Predicate
        List<Employee> employees = new();

        employees.Add(new Employee { Id = 1, FirstName = "Sarah", LastName = "Jones", AnnualSalary = 60000, Gender = 'f', IsManager = true });
        employees.Add(new Employee { Id = 2, FirstName = "Andrew", LastName = "Brown", AnnualSalary = 40000, Gender = 'm', IsManager = false });
        employees.Add(new Employee { Id = 3, FirstName = "John", LastName = "Henderson", AnnualSalary = 58000, Gender = 'm', IsManager = true });
        employees.Add(new Employee { Id = 4, FirstName = "Jane", LastName = "May", AnnualSalary = 30000, Gender = 'f', IsManager = false });

        List<Employee> employeesFiltered = FilterEmployees(employees, e => e.Gender == 'm');

        foreach (Employee employee in employeesFiltered)
        {
            displayEmployeeDetails(employee.Id, employee.FirstName, employee.LastName, employee.AnnualSalary, employee.Gender, employee.IsManager);
            Console.WriteLine();
        }
    }

    static List<Employee> FilterEmployees(List<Employee> employees, Predicate<Employee> predicate)
    {
        List<Employee> employeesFiltered = new();

        foreach (Employee employee in employees)
        {
            if (predicate(employee))
            {
                employeesFiltered.Add(employee);
            }
        }

        return employeesFiltered;
    }

}

public class Employee
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public decimal AnnualSalary { get; set; }
    public char Gender { get; set; }
    public bool IsManager { get; set; }
}
```
