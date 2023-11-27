---
title: Asynchronous Programming in C#
description: C# Advanced concepts | Asynchronous Programming
sidebar_label: "Asynchronous Programming in C#"
sidebar_position: 7
---

Using asynchronous programming, the application can work on other tasks with out waiting for the task to be completed.

If any synchronous process is blocked, the entire application is blocked, and our application stops responding untill the task is completed.

## Async and Await keyword

### Async Keyword

- Asynchronous methods are created by using the async modfier on the method.
- An async method run synchronously until it reached its first await operator, at which point it suspends.

### Await Keyword

- while the asynchronous operation is running, the await operator suspends the async method evaluation.
- when the asynchronous operation completed, the await operator returns the result.

```csharp
public class Program
{
    static void Main(string[] args)
    {
        Method1();
        Method2();
        Console.ReadKey();

    }

    public static async Task Method1()
    {
        await Task.Run(() =>
        {
            for (int i = 0; i < 100; i++)
            {
                Console.WriteLine("Method 1 is running" + i);
                Task.Delay(100).Wait();
            }
        });
    }

    public static void Method2()
    {
        for (int i = 0; i < 25; i++)
        {
            Console.WriteLine("Method 2 is running" + i);
            Task.Delay(100).Wait();
        }
    }

}
```

Asynchronous programming is used for two kinds of tasks: I/O bound tasks and CPU-bound tasks.
