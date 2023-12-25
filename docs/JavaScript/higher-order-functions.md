---
title: Higher-Order Function
description: A Deep Dive into Higher-Order Function in Javascript
sidebar_label: "Higher-Order Function"
sidebar_position: 12
---

Higher Order Functions (HOFs) are functions that take other functions as arguments or return functions as results. They enable functional programming paradigms and are a powerful concept in JavaScript.

### Examples of Higher Order Functions:

1. **map():**

   ```javascript
   const numbers = [1, 2, 3, 4];
   const doubled = numbers.map((num) => num * 2);
   // doubled: [2, 4, 6, 8]
   ```

2. **filter():**

   ```javascript
   const evenNumbers = numbers.filter((num) => num % 2 === 0);
   // evenNumbers: [2, 4]
   ```

3. **reduce():**
   ```javascript
   const sum = numbers.reduce((acc, num) => acc + num, 0);
   // sum: 10
   ```

## Functional Programming: A Paradigm Shift

Functional programming is a programming paradigm that treats computation as the evaluation of mathematical functions and avoids changing state and mutable data. It emphasizes immutability, pure functions, and higher order functions.

### Key Principles of Functional Programming:

1. **Immutability:** Avoiding the mutation of data.
2. **Pure Functions:** Functions with no side effects.
3. **First-Class Functions:** Functions as first-class citizens.
4. **Higher Order Functions:** Functions that accept or return functions.

## Polyfill for the `map` Function in JavaScript

A polyfill is a piece of code that provides the functionality that is not natively supported in the browser or environment. Here's a basic polyfill for the `map` function:

```javascript
// Polyfill for the map function
if (!Array.prototype.map) {
  Array.prototype.map = function (callback, thisArg) {
    const newArray = [];
    for (let i = 0; i < this.length; i++) {
      newArray.push(callback.call(thisArg, this[i], i, this));
    }
    return newArray;
  };
}

// Using the polyfill
const numbers = [1, 2, 3, 4];
const squared = numbers.map((num) => num ** 2);
// squared: [1, 4, 9, 16]
```

This polyfill checks if the `map` function is already defined and, if not, provides an implementation that mimics the native behavior.

## Benefits of the `map` Polyfill:

1. **Cross-Browser Compatibility:** Ensures consistent behavior across different browsers.
2. **Environment Adaptability:** Works in environments that may not have native support.

## Conclusion

Higher order functions and functional programming concepts like immutability and pure functions empower developers to write more modular, readable, and maintainable code in JavaScript. Understanding and implementing polyfills for missing functions further extends the capabilities of JavaScript, making it adaptable to various environments and ensuring a consistent experience for users.
