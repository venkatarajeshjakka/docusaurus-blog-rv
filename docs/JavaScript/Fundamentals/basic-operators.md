---
title: Basic Operators
description: A Deep Dive into Basic Operators in JavaScript
sidebar_label: "Basic Operators"
sidebar_position: 3
---

Operators allow us to manipulate values and perform computations in JavaScript. They enable value transformation and combination for various operations.

### Types of Operators

1. **Arithmetic Operators** - Perform mathematical operations.
2. **Comparison Operators** - Compare values and return a boolean result.
3. **Logical Operators** - Combine multiple boolean expressions.
4. **Assignment Operators** - Assign values to variables.

## Arithmetic Operators

Arithmetic operators perform mathematical calculations on numeric values.

```javascript
const now = 2037;
const ageJonas = now - 1991; // Subtraction
const ageSarah = now - 2018; // Subtraction

const sum = 10 + 5; // Addition
const product = 10 * 5; // Multiplication
const quotient = 10 / 2; // Division
const remainder = 10 % 3; // Modulus
const exponent = 2 ** 3; // Exponentiation (2 to the power of 3)
```

## Comparison Operators

Comparison operators are used to compare two values and return a boolean (`true` or `false`).

```javascript
console.log(10 > 5); // true (greater than)
console.log(10 >= 10); // true (greater than or equal to)
console.log(5 < 10); // true (less than)
console.log(5 <= 5); // true (less than or equal to)
console.log(10 === "10"); // false (strict equality, checks both value and type)
console.log(10 == "10"); // true (loose equality, checks value only)
```

## Logical Operators

Logical operators are used to combine multiple boolean expressions.

```javascript
const hasDriversLicense = true;
const hasGoodVision = false;

console.log(hasDriversLicense && hasGoodVision); // false (AND operator)
console.log(hasDriversLicense || hasGoodVision); // true (OR operator)
console.log(!hasDriversLicense); // false (NOT operator)
```

## Assignment Operators

Assignment operators are used to assign values to variables.

```javascript
let x = 10;
x += 5; // x = x + 5 (Addition assignment)
x -= 3; // x = x - 3 (Subtraction assignment)
x *= 2; // x = x * 2 (Multiplication assignment)
x /= 2; // x = x / 2 (Division assignment)
x %= 3; // x = x % 3 (Modulus assignment)
x **= 2; // x = x ** 2 (Exponentiation assignment)
```

These operators form the foundation of working with data in JavaScript, allowing developers to perform calculations, comparisons, and logical operations efficiently.
