---
title: Factory Design Pattern in C#
description: Design Patterns | Factory
sidebar_label: "Factory Design Pattern"
sidebar_position: 1
---

The Factory Design Pattern is a creational pattern that provides an interface for creating instances of a class, but allows subclasses to alter the type of instances that will be created. This pattern is useful when a class cannot anticipate the class of objects it needs to create, or when a class wants its subclasses to specify the objects it creates.

Here's an example of implementing the Factory Design Pattern in C#:

Create interface `IEmployee`

```csharp
public interface IEmployee
{
    int Id { get; set; }

    string FirstName { get; set; }

    string LastName { get; set; }

    decimal Salary { get; set; }

}
```

Create Abstract class that implements interface `IEmployee`

```csharp
public class EmployeeBase : IEmployee
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public virtual decimal Salary { get; set; }
}
```

Create generic `FactoryPattern` class and can be used to create instances of any class `(T)` that is a subclass of a specified base class or interface (K) and has a default parameterless constructor (`new()` constraint).

```csharp
public static class FactoryPattern<K, T> where T : class, K, new()
{
    public static K GetInstance()
    {
        K objK;
        objK = new T();
        return objK;
    }

}
```

The `Program` class demonstrates how to use the factory to instantiate an object of a specific type (DerivedClass) while treating it as the base type (SomeBaseClass). This provides flexibility in creating objects of different types while maintaining a common interface or base type.

```csharp
public enum EmployeeType
{
    Teacher,
    HeadOfDepartment,
    DeputyHeadMaster,
    HeadMaster,

}
public class Program
{

    static void Main(string[] args)
    {

        List<IEmployee> employees = new();
        SeedData(employees);

        Console.WriteLine($"Total Annual Salaries (including bonus): {employees.Sum(e => e.Salary)}");
    }

    public static void SeedData(List<IEmployee> employees)
    {
        IEmployee teacher1 = EmployeeFactory.GetEmployeeInstance(EmployeeType.Teacher, 1, "Bob", "Fisher", 40000);
        employees.Add(teacher1);

        IEmployee teacher2 = EmployeeFactory.GetEmployeeInstance(EmployeeType.Teacher, 2, "Jenny", "Thomas", 40000);
        employees.Add(teacher2);

        IEmployee headOfDepartment = EmployeeFactory.GetEmployeeInstance(EmployeeType.HeadOfDepartment, 3, "Brenda", "Mullins", 50000);
        employees.Add(headOfDepartment);

        IEmployee deputyHeadMaster = EmployeeFactory.GetEmployeeInstance(EmployeeType.DeputyHeadMaster, 4, "Devlin", "Brown", 60000);
        employees.Add(deputyHeadMaster);

        IEmployee headMaster = EmployeeFactory.GetEmployeeInstance(EmployeeType.HeadMaster, 5, "Damien", "Jones", 80000);
        employees.Add(headMaster);
    }
}

public class Teacher : EmployeeBase
{
    public override decimal Salary { get => base.Salary + (base.Salary * 0.02m); }
}

public class HeadOfDepartment : EmployeeBase
{
    public override decimal Salary { get => base.Salary + (base.Salary * 0.03m); }
}

public class DeputyHeadMaster : EmployeeBase
{
    public override decimal Salary { get => base.Salary + (base.Salary * 0.04m); }
}

public class HeadMaster : EmployeeBase
{
    public override decimal Salary { get => base.Salary + (base.Salary * 0.05m); }
}

public static class EmployeeFactory
{
    public static IEmployee GetEmployeeInstance(EmployeeType employeeType, int id, string firstName, string lastName, decimal salary)
    {
        IEmployee employee = null;

        switch (employeeType)
        {
            case EmployeeType.Teacher:
                employee = FactoryPattern<IEmployee, Teacher>.GetInstance();
                break;
            case EmployeeType.HeadOfDepartment:
                employee = FactoryPattern<IEmployee, HeadOfDepartment>.GetInstance();
                break;
            case EmployeeType.DeputyHeadMaster:
                employee = FactoryPattern<IEmployee, DeputyHeadMaster>.GetInstance();
                break;
            case EmployeeType.HeadMaster:
                employee = FactoryPattern<IEmployee, HeadMaster>.GetInstance();
                break;
            default:
                break;
        }
        if (employee != null)
        {
            employee.Id = id;
            employee.FirstName = firstName;
            employee.LastName = lastName;
            employee.Salary = salary;
        }
        else
        {
            throw new NullReferenceException();
        }

        return employee;
    }
}
```
