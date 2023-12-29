---
title: Clousers
description: A Deep Dive into Lexical Scoping
sidebar_label: "Clousers"
sidebar_position: 7
---

Closures are a powerful and often misunderstood feature in JavaScript that leverages lexical scoping to create functions with retained access to their outer scope variables.

## What are Closures?

A closure is created in JavaScript when a function is defined within another function, allowing the inner function to access variables from the outer (enclosing) function, even after the outer function has finished executing. This behavior is possible due to the preservation of the lexical scope at the time of function creation.

```javascript
function outerFunction() {
  let outerVariable = "I am from the outer function";

  function innerFunction() {
    console.log(outerVariable);
  }

  return innerFunction;
}

const closureExample = outerFunction();
closureExample(); // Outputs: I am from the outer function
```
