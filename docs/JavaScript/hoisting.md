---
title: Demystifying Hoisting
description: Demystifying Hoisting
sidebar_label: "Hoisting"
sidebar_position: 3
---

Hoisting is a JavaScript mechanism where variable and function declarations are moved to the top of their containing scope during the compilation phase. This allows you to use functions and variables even before they are declared in your code.

## Variables and Function Declarations:

```javascript
// Example 1: Variable Hoisting
console.log(x); // Outputs: undefined
var x = 5;

// Example 2: Function Hoisting
sayHello(); // Outputs: "Hello, World!"
function sayHello() {
  console.log("Hello, World!");
}
```

In Example 1, the variable `x` is hoisted to the top of the current scope, but its assignment (= 5) stays in place. So, when we log `x` before its declaration, we get undefined.

In Example 2, the function `sayHello` is hoisted along with its entire definition, allowing us to call the function before its declaration.

## The Hoisting Process

### 1. Variable Declaration:

- During the compilation phase, variable declarations are moved to the top of their scope.
- The initialization remains in place.

### 2. Function Declaration:

- Entire function declarations are hoisted, including their definition.

### 3. Function Expressions:

- Only the variable declaration is hoisted, not the function definition.
