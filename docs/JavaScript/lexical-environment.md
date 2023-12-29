---
title: Understanding Scope, Lexical Environment, and the Scope Chain
description: Understanding Scope, Lexical Environment, and the Scope Chain
sidebar_label: "Lexical Environment"
sidebar_position: 4
---

## Scope: The Containment of Variables

Scope in JavaScript refers to the context in which variables are declared and defined. It determines the visibility and accessibility of variables throughout your code. JavaScript has two primary types of scope: global scope and local scope.

### Global Scope

Variables declared outside of any function or block have global scope, meaning they can be accessed from any part of the code.

```javascript
var globalVariable = "I am a global variable";

function exampleFunction() {
  console.log(globalVariable); // Accessible within the function
}

exampleFunction();
console.log(globalVariable); // Accessible outside the function
```

### Local Scope

Variables declared within a function or a block have local scope, making them accessible only within that specific function or block.

```javascript
function exampleFunction() {
  var localVariable = "I am a local variable";
  console.log(localVariable); // Accessible within the function
}

exampleFunction();
console.log(localVariable); // Error: localVariable is not defined
```

## Lexical Environment: The Contextual Container

Lexical environment refers to the environment in which a piece of code is written. It consists of the variables that are in scope at that particular moment. Every time a function is invoked, a new lexical environment is created, forming a chain of nested environments.

```javascript
function outerFunction() {
  var outerVariable = "I am in the outer function";

  function innerFunction() {
    var innerVariable = "I am in the inner function";
    console.log(outerVariable); // Accessible due to lexical scoping
  }

  innerFunction();
}

outerFunction();
```

## The Scope Chain: Connecting Lexical Environments

The scope chain is the linkage between different lexical environments. It enables inner functions to access variables from their outer functions. When a variable is not found in the current lexical environment, JavaScript looks up the scope chain until it reaches the global scope.

```javascript
var globalVariable = "I am a global variable";

function outerFunction() {
  var outerVariable = "I am in the outer function";

  function innerFunction() {
    console.log(outerVariable); // Accessible through the scope chain
    console.log(globalVariable); // Also accessible through the scope chain
  }

  innerFunction();
}

outerFunction();
```
