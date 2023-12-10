---
title: Demystifying Execution Context
description: Demystifying Execution Context
sidebar_label: "Execution Context"
sidebar_position: 1
---

At its core, Execution Context can be envisioned as the space or environment where a piece of JavaScript code is evaluated and executed. Understanding this concept is key to unraveling the intricacies of how JavaScript code operates.

## Global Execution Context

### The Foundation:

The global execution context is the default space where code not within any function is executed. It serves as the bedrock of the execution process, created before any other code begins to run. Variables and functions declared in the global scope become attached to the global object (e.g., `window` in browsers).

## Functional Execution Context

### In the Function Zone:

Functional execution contexts are created each time a function is invoked. These contexts have their own space for variables and functions, distinct from the global context. As functions are called, a new execution context is formed, stacking up in what is known as the "Call Stack."

## The Execution Phases

### Creation Phase

In the two-phase process of code execution, the Creation Phase sets up the global context and hoists variables and functions.

### Execution Phase

The engine then moves on to the Execution Phase, where the code is executed line by line. Function calls trigger the creation of new functional execution contexts, forming the call stack.

## Stack Mechanism

The Call Stack is crucial in managing the order of execution contexts. When a function is called, a new context is pushed onto the stack; when the function completes, its context is popped off the stack.

## Context Linking

Execution contexts are not isolated islands. They are linked to form a chain, known as the "Scope Chain." This chain facilitates the access of variables from both the current and outer (enclosing) contexts.
