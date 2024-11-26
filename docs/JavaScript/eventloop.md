---
title: Asynchronous Javascript & Eventloop
description: A Deep Dive into Asynchronous Javascript & Eventloop
sidebar_label: "Eventloop"
sidebar_position: 11
---

## Call Stack: Tracking the Execution

The JavaScript engine employs a call stack to manage the execution of code. It keeps track of function calls and their execution context. When a function is called, it is pushed onto the call stack, and when it completes, it is popped off.

```javascript
function greet(name) {
  console.log(`Hello, ${name}!`);
}

function welcome() {
  console.log("Welcome!");
}

greet("Alice");
welcome();
```

In this example, `greet("Alice")` and `welcome()` are pushed onto the call stack and executed in sequence.

## Main Job of the Call Stack

The primary job of the call stack is to maintain the execution context of the program. It ensures that function calls are executed in the correct order and prevents the stack from overflowing by managing the push and pop operations.

```javascript
function recursiveFunction() {
  recursiveFunction(); // Causes a stack overflow
}

recursiveFunction();
```

In this scenario, the call stack would eventually overflow due to the infinite recursion.

## Asynchronous Tasks in JavaScript

JavaScript is single-threaded and non-blocking, meaning it can handle asynchronous tasks efficiently. Tasks like I/O operations, timers, and events are managed through the event loop and Web APIs.

## Web APIs in JS: Delegating Heavy Tasks

Web APIs provide functionality beyond the capabilities of the JavaScript runtime. For example, the `setTimeout` function is not part of the JavaScript language itself but is provided by the browser's Web API.

```javascript
setTimeout(() => {
  console.log("Delayed log");
}, 1000);
```

Here, `setTimeout` delegates the timer-related task to the browser's Web API.

## Event Loop & Callback Queue: Keeping it Responsive

The event loop continuously checks the call stack and the callback queue. When the call stack is empty, it picks up tasks from the callback queue and pushes them onto the call stack for execution.

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Inside setTimeout");
}, 0);

console.log("End");
```

In this example, "Start" and "End" will be logged before "Inside setTimeout" because the `setTimeout` callback is moved to the callback queue, allowing the call stack to finish its current execution.

## How Event Listeners Work in JS

Event listeners are asynchronous. When an event occurs, the associated callback is added to the callback queue and processed by the event loop.

```javascript
const button = document.getElementById("myButton");

button.addEventListener("click", () => {
  console.log("Button clicked!");
});
```

When the button is clicked, the associated callback is added to the callback queue and processed asynchronously.

## How fetch() Function Works

The `fetch` function initiates network requests. It returns a Promise, allowing for asynchronous handling.

```javascript
fetch("https://api.example.com/data")
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((error) => console.error(error));
```

`fetch` returns a Promise that resolves with the `Response` to the request. The `then` and `catch` methods handle the asynchronous response or error.

## MicroTask Queue in JS

Microtasks are tasks with higher priority than regular tasks. Promises and mutation observers are examples of microtasks.

```javascript
console.log("Start");

Promise.resolve().then(() => console.log("Microtask"));

console.log("End");
```

In this example, "Microtask" will be logged before "End" due to the higher priority of microtasks in the event loop.

## Starvation of Functions in Callback Queue

Starvation occurs when the call stack is continuously occupied by long-running synchronous tasks, preventing other functions from being processed in the callback queue. This can lead to delayed execution of asynchronous tasks.

```javascript
function syncTask() {
  // Long-running synchronous task
}

setTimeout(() => {
  console.log("Async task");
}, 0);

syncTask(); // May cause starvation
```

In this scenario, the asynchronous task may be delayed due to the synchronous task occupying the call stack.

## Conclusion

Understanding the interplay between the call stack, event loop, callback queue, and Web APIs is crucial for effective JavaScript development. By embracing asynchronous patterns and leveraging the event-driven nature of JavaScript, you can build responsive and efficient applications. Keep the dance of execution in mind, and your code will gracefully navigate the complexities of asynchronous programming in the JavaScript environment.
