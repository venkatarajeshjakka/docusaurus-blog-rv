---
title: undefined vs not defined
description: undefined vs not defined in Javascript
sidebar_label: "Undefined vs Not Defined"
sidebar_position: 4
---

## Undefined:

- `undefined` is a primitive value in JavaScript.
- It represents the lack of a value or the absence of a value in a variable.
- Variables that are declared but not assigned a value are automatically assigned the value `undefined`.

```javascript
let x;
console.log(x); // Outputs: undefined
```

`x` is declared but not assigned a value, so its default value is `undefined`

## Not Defined:

- "Not defined" refers to a situation where a variable is not declared in the current scope.
- If you try to access a variable that has not been declared using `var`, `let`, or `const`, a "ReferenceError" will occur.

```javascript
console.log(z); // Throws a ReferenceError: z is not defined

// Checking if a variable is declared before accessing it
if (typeof a === "undefined") {
  console.log("a is not defined");
} else {
  console.log("a is defined");
}
```

In the example, attempting to log the value of `z` will result in a ReferenceError because z has not been declared. In the second example, we use the typeof operator to check if a variable is defined before attempting to access it.

## Key Takeaways:

### Undefined:

- Represents the absence of a value or the default value of a variable.
- Can be explicitly assigned to a variable.

### Not Defined:

- Occurs when attempting to use a variable that has not been declared.
- Results in a ReferenceError.
