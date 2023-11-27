---
title: Interfact Segregation Principle
description: Interface Segregation Principle using C#
sidebar_label: "Interface Segregation Principle"
sidebar_position: 4
---

The Interface Segregation Principle (ISP) is one of the SOLID principles of object-oriented design, and it states that a class should not be forced to implement interfaces it does not use. In other words, a client should not be forced to depend on interfaces it does not use.

```csharp
public interface IStudentRepository
{
    void AddStudent(Student std);
    void EditStudent(Student std);
    void DeleteStudent(Student std);

    void AddCourse(Course cs);
    void EditCourse(Course cs);
    void DeleteCourse(Course cs);

    bool SubscribeCourse(Course cs);
    bool UnSubscribeCourse(Course cs);

    IList<Student> GetAllStudents();
    IList<Student> GetAllStudents(Course cs);

    IList<Course> GetAllCourse();
    IList<Course> GetAllCourses(Student std);
}

public class StudentRepository : IStudentRepository
{
    public void AddCourse(Course cs)
    {
        //implementation code removed for better clarity
    }

    public void AddStudent(Student std)
    {
        //implementation code removed for better clarity
    }

    public void DeleteCourse(Course cs)
    {
        //implementation code removed for better clarity
    }

    public void DeleteStudent(Student std)
    {
        //implementation code removed for better clarity
    }

    public void EditCourse(Course cs)
    {
        //implementation code removed for better clarity
    }

    public void EditStudent(Student std)
    {
        //implementation code removed for better clarity
    }

    public IList<Course> GetAllCourse()
    {
        //implementation code removed for better clarity
    }

    public IList<Course> GetAllCourses(Student std)
    {
        //implementation code removed for better clarity
    }

    public IList<Student> GetAllStudents()
    {
        //implementation code removed for better clarity
    }

    public IList<Student> GetAllStudents(Course cs)
    {
        //implementation code removed for better clarity
    }

    public bool SubscribeCourse(Course cs)
    {
        //implementation code removed for better clarity
    }

    public bool UnSubscribeCourse(Course cs)
    {
        //implementation code removed for better clarity
    }
}
```

The above `IStudentRepository` interface contains 12 methods for different purposes. The `StudentRepository` class implements the `IStudentRepository` interface.

we can split our large interface `IStudentRepository` and create another interface `ICourseRepository` with all course-related methods, as shown below.

```csharp
public interface IStudentRepository
{
    void AddStudent(Student std);
    void EditStudent(Student std);
    void DeleteStudent(Student std);

    bool SubscribeCourse(Course cs);
    bool UnSubscribeCourse(Course cs);
    IList<Student> GetAllStudents();
    IList<Student> GetAllStudents(Course cs);
}

public interface ICourseRepository
{
    void AddCourse(Course cs);
    void EditCourse(Course cs);
    void DeleteCourse(Course cs);

    IList<Course> GetAllCourse();
    IList<Course> GetAllCourses(Student std);
}
```

Now, we can create two concrete classes that implement the above two interfaces. This will automatically support SRP and increase cohesion.
