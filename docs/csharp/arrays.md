---
title: Arrays in C#
description: C# Basic concepts | Arrays
sidebar_label: "Arrays in C#"
sidebar_position: 4
---

An `array` is a sequence of elements

- All elements are of the same type
- The order of the elements is fixed
- Has fixed size(`Array.Length`)

## Multidimensional arrays

- Have more than one dimensions
- The most important multidimensional arrays are the 2 dimensional
- Know as matrices or tables

Declaring multidimensional arrays

```csharp

int[,] intMatrix;
string[,,] strCube;

```

Creating multidimensional arrays

```csharp

 class Program
{
    static void Main(string[] args)
    {

        int[,] myArray = new int[2, 3];

        myArray[0, 0] = 1;
        myArray[0, 1] = 2;
        myArray[0, 2] = 3;

        myArray[1, 0] = 4;
        myArray[1, 1] = 5;
        myArray[1, 2] = 6;

        for (int row = 0; row < myArray.GetLength(0); row++)
        {
            for (int col = 0; col < myArray.GetLength(1); col++)
            {
                Console.WriteLine($"myArray[{row},{col}] = {myArray[row, col]}");
            }
        }
        Console.ReadLine();
    }
}

```
