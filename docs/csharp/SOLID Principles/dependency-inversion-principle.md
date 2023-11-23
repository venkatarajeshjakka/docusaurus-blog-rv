---
title: Dependency Inversion Principle
description: Dependency Inversion Principle using C#
sidebar_label: "Dependency Inversion Principle"
sidebar_position: 5
---

The Dependency Inversion Principle (DIP) is one of the SOLID principles of object-oriented programming. The Dependency Inversion Principle consists of two key points:

1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
2. Abstractions should not depend on details. Details should depend on abstractions.

It helps in loose coupling.

```csharp
public class Student
{
    public int StudentId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public DateTime DoB { get; set; }

    private IStudentRepository _stdRepo;

    public Student(IStudentRepository stdRepo)
    {
        _stdRepo = stdRepo;
    }

    public void Save()
    {
        _stdRepo.AddStudent(this);
    }
}

public interface IStudentRepository
{
    void AddStudent(Student std);
    void EditStudent(Student std);
    void DeleteStudent(Student std);

    IList<Student> GetAllStudents();
}

public class StudentRepository : IStudentRepository
{
    public void AddStudent(Student std)
    {
        //code removed for clarity
    }

    public void DeleteStudent(Student std)
    {
        //code removed for clarity
    }

    public void EditStudent(Student std)
    {
        //code removed for clarity
    }

    public IList<Student> GetAllStudents()
    {
        //code removed for clarity
    }
}
```
