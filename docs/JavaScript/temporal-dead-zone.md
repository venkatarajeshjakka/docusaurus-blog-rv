---
title: Exploring let, const, and the Temporal Dead Zone
description: Exploring let, const, and the Temporal Dead Zone in JavaScript
sidebar_label: "Temporal Dead Zone"
sidebar_position: 6
---

JavaScript has evolved over the years, introducing new variable declaration keywords like `let` and `const` to enhance the language's functionality. We'll explore the nuances of `let` and `const`, and understand a phenomenon called the **Temporal Dead Zone (TDZ)**, which plays a crucial role in how these declarations work.

## `let` and Block Scoping

The `let` keyword was introduced in ECMAScript 6 (ES6) to address some of the issues associated with variable scoping using `var`. Unlike `var`, variables declared with `let` are block-scoped, meaning they exist only within the block, statement, or expression they are declared in.

```javascript
if (true) {
  let blockVariable = "I am a block-scoped variable";
  console.log(blockVariable); // Accessible within the block
}

console.log(blockVariable); // Error: blockVariable is not defined
```

This helps in preventing unintended variable hoisting and enhances the predictability of variable scope.

## `const` for Constants

The `const` keyword is used to declare constants in JavaScript. Once a variable is declared with `const`, its value cannot be reassigned.

```javascript
const pi = 3.14;
pi = 22 / 7; // Error: Assignment to constant variable
```

Constants are block-scoped like variables declared with `let`. While the variable itself cannot be reassigned, it's important to note that for objects and arrays declared with `const`, their properties or elements can still be modified.

## Temporal Dead Zone (TDZ)

The Temporal Dead Zone is a concept associated with the use of `let` and `const`. It refers to the time between entering a scope and the actual declaration of a variable. During this period, attempting to access the variable results in a `ReferenceError`.

```javascript
console.log(tempVariable); // ReferenceError: tempVariable is not defined
let tempVariable = "I am in the Temporal Dead Zone";
```
