---
title: JavaScript Data Types
description: A Deep Dive into Data Types in JavaScript
sidebar_label: "Data Types"
sidebar_position: 1
---

JavaScript has seven **primitive** data types, which are immutable and stored by value.

### 1. Number

A floating-point number used for both integers and decimals.

```javascript
let age = 23;
```

### 2. String

A sequence of characters used to represent text.

```javascript
let firstName = "Rajesh";
```

### 3. Boolean

A logical type that can only be `true` or `false`, commonly used for decision-making.

```javascript
let fullAge = true;
```

### 4. Undefined

A variable that has been declared but not assigned a value.

```javascript
let children;
```

### 5. Null

Represents an explicitly empty value.

### 6. Symbol (ES6)

A unique and immutable value, primarily used for object property keys.

### 7. BigInt (ES2020)

Used for integers larger than the `Number` type can safely represent.

```javascript
let largeNumber = 9007199254740991n;
```

### Dynamic Typing in JavaScript

💡 **JavaScript uses dynamic typing**, meaning we do not need to explicitly define the data type of a variable. The type is determined automatically based on the assigned value.
