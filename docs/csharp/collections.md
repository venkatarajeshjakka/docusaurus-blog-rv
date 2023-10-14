---
title: Collections  in C#
description: C# Basic concepts | Collections
sidebar_label: "Collections in C#"
sidebar_position: 6
---

## Collections

- Provide a more flexible way to work with groups of objects
- The group of objects you work which can grow and shrink
- For some collections, you can assign a key to any object that you put into the collection so that you can quickly retrieve the object by using the key
- If your collection contains elements of only one data type, you can use one of the classes in the `System.Collections.Generic` namespace
- A generic collection enforces `type safety` so that no other data type can be added to it
- When you retrieve an element from a generic collection, you do not have to determine its data type or convert it

| Class                   | Description                                                                                                              |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Dictionary<TKey,TValue> | Represents a collection of key/value pairs that are organized based on the key.                                          |
| List                    | Represents a list of objects that can be accessed by index. Provides methods to search, sort, and modify lists.          |
| Queue                   | Represents a first in, first out (FIFO) collection of objects.                                                           |
| SortedList<TKey,TValue> | Represents a collection of key/value pairs that are sorted by key based on the associated `IComparer<T>` implementation. |
| Stack                   | Represents a last in, first out (LIFO) collection of objects                                                             |

## List

### Creating a List

To create a list, you first need to instantiate it with the type of elements it will hold

```csharp
List<int> integerList = new List<int>();
```

### Adding Items

```csharp
integerList.Add(42);
```

### Count of Items

```csharp
int itemCount = integerList.Count; // Returns 3 in this case.
```

### Removing Items

```csharp
integerList.Remove(56); // Removes the item with the value 56.
integerList.RemoveAt(0); // Removes the first item (42).
```

## Stack

### Creating a Stack

```csharp
Stack<int> integerStack = new Stack<int>();
```

### Pushing items onto the Stack

```csharp
integerStack.Push(42);
integerStack.Push(56);
integerStack.Push(73);
```

### Popping Items from the Stack

You can remove and retrieve the item from the top of the stack using the `Pop` method

```csharp
int topItem = integerStack.Pop(); // Removes and returns the top item (73).
```

### Peeking at the Top Item

You can inspect the item at the top of the stack without removing it using the `Peek`

```csharp
int topItem = integerStack.Peek(); // Returns the top item without removing it (56).
```

## Queues

Any time we need processing in a FIFO order

```csharp
Queue<string> queue = new Queue<string>();

queue.Enqueue("First Item");
queue.Enqueue("Second Item");
queue.Enqueue("Third Item");

foreach (var item in queue)
{
     Console.WriteLine(item);
}

Console.WriteLine($"Number if items in the queue:{queue.Count}");

Console.WriteLine($"First item in the queue is:{queue.Dequeue()}");

Console.WriteLine($"Current number of items in the queue:{queue.Count}");

Console.WriteLine($"Current first item in the queue:{queue.Peek()}");

```

## Dictionary

- Provides a mapping from a set of keys to a set of values
- Each addition to the dictionary consists of a value and its associated key
- Retrieving a value by using its key is very fast, because the `Dictionary<TKey,TValue>` class is implemented as a hash table

```csharp
 Dictionary<string, string> dictionary = new Dictionary<string, string>()
    {
        {"home", "A place to live"},
        {"fruit", "apple"}
    };

dictionary.Add("java", "programming");

string definition;
bool isPresent = dictionary.TryGetValue("java", out definition);

Console.WriteLine(definition);
Console.WriteLine(isPresent);

foreach (var item in dictionary)
{
    Console.WriteLine($"Key: {item.Key}, Value: {item.Value}");

}
```

## HashTable

- `HashTable` is a class that stores data in key/value pairs and it is optimized for fast lookups
- It computes a `hash` of each key you add
- It then uses its hash code to look up the element very quickly
