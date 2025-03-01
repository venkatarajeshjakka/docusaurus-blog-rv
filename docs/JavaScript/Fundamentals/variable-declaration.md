---
title: Variable Declaration
description: A Deep Dive into let, const, and var in JavaScript
sidebar_label: "Variable Declaration"
sidebar_position: 2
---

In JavaScript, variables can be declared using `let`, `const`, and `var`. With the introduction of ES6, `let` and `const` have largely replaced `var` due to their improved scoping rules and better practices.

## Declaring Variables with `let`

The `let` keyword allows you to declare a variable whose value can be changed (mutated) later.

```javascript
let age = 30;
age = 40; // The value of 'age' is now updated
```

## Declaring Constants with `const`

The `const` keyword is used to declare variables that should not be reassigned after their initial assignment.

```javascript
const birthYear = 1991;
// birthYear = 1992; // This will throw an error
```

## The Legacy `var` Keyword

The `var` keyword functions similarly to `let`, but with a key difference in scoping rules. `let` is block-scoped, whereas `var` is function-scoped. Due to potential issues like variable hoisting and unintended overwrites, it is recommended to avoid using `var`.

```javascript
var job = "programmer";
job = "teacher"; // The value of 'job' is updated
```

## Key Differences Between `let`, `const`, and `var`

| Feature       | `let`                                     | `const`                   | `var`                            |
| ------------- | ----------------------------------------- | ------------------------- | -------------------------------- |
| Scope         | Block-scoped                              | Block-scoped              | Function-scoped                  |
| Reassignment  | Allowed                                   | Not allowed               | Allowed                          |
| Hoisting      | Yes (not initialized)                     | Yes (not initialized)     | Yes (initialized as `undefined`) |
| Best Practice | Recommended for variables that can change | Recommended for constants | Avoid using                      |

Using `let` and `const` ensures cleaner, more predictable, and bug-free code compared to `var`.

---

By understanding these differences, you can write better, more maintainable JavaScript code.
