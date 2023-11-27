---
title: Single responsibility principle
description: Single responsibility principle using C#
sidebar_label: "Single Responsibility Principle"
sidebar_position: 1
---

The Single Responsibility Principle (SRP) is a programming principle that states that every module, class or function should have one and only one reason to change.

In C#, the following code snippet demonstrates the SRP principle:

```csharp
public class UserRepository
{
    public void AddUserToDb()
    {
        SqlConnection connection = new SqlConnection();

        connection.Open();

        SqlCommand command = new SqlCommand("INSERT INTO [...]");
    }
}

public class EmailSender
{
    public void SendEmail()
    {
        SmtpClient client = new SmtpClient("smtp.myhost.com");

        client.Send(new MailMessage());
    }
}
```

In the above code, we have two classes `UserRepository` and `EmailSender`. The `UserRepository` class is responsible for only one thing - adding user to database. The `EmailSender` class is responsible for only one thing - sending email.

By following the SRP principle, we can write classes that are focused, maintainable, and easy to test.
