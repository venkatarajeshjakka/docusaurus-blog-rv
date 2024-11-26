---
title: Unraveling Block Scope and Shadowing
description: Unraveling Block Scope and Shadowing in JavaScript
sidebar_label: "Block Scope & Shadowing"
sidebar_position: 7
---

Block scope refers to the region within curly braces `{}` where variables are declared using `let` and `const`. Unlike `var`, which has function scope, variables declared with `let` and `const` are confined to the block they are defined in.

```javascript
if (true) {
  let blockScopedVariable = "I am block-scoped";
  console.log(blockScopedVariable); // Accessible within the block
}

console.log(blockScopedVariable); // Error: blockScopedVariable is not defined
```

## Shadowing

Shadowing occurs when a variable declared within a certain scope has the same name as a variable in an outer scope. The inner variable "shadows" the outer variable, temporarily taking precedence within its scope.

```javascript
let globalVariable = "I am a global variable";

function exampleFunction() {
  let globalVariable = "I am a local variable";
  console.log(globalVariable); // Accesses the local variable, not the global one
}

exampleFunction();
console.log(globalVariable); // Accesses the global variable
```

## Benefits of Block Scope and Shadowing

### Encapsulation

Block scope allows for better encapsulation of variables, preventing unintended variable hoisting and minimizing the risk of naming conflicts.

### Clarity

Shadowing can be used deliberately to create clear and self-contained code. It allows you to reuse variable names within different scopes without affecting the outer variables.

### Isolation

Block scope and shadowing contribute to isolating variables, reducing the likelihood of unintentional side effects or interference between different parts of your code.
