---
title: Introduction
description: Introduction to Vite
sidebar_label: "Introduction"
sidebar_position: 1
---

# Introduction to Vite

Vite (French for "fast") is a modern frontend build tool that provides a lightning-fast development experience. It is designed to enhance performance by optimizing development and build processes.

![Vite Logo](https://vitejs.dev/logo.svg)

## What is Vite?

Vite is a next-generation frontend tooling framework that offers:

- **Instant Server Start**: Uses native ES modules to speed up loading.
- **Lightning Fast HMR (Hot Module Replacement)**: Updates modules in milliseconds without refreshing the page.
- **Optimized Build**: Uses Rollup for fast production builds with automatic code-splitting.
- **Out-of-the-Box Support**: Works seamlessly with Vue, React, Svelte, and other modern frameworks.

## What Makes Vite Fast?

### 1. Bundle-Based Development Server

Traditional bundlers like Webpack analyze and bundle the entire project before serving it. Vite improves this by serving source files as native ES modules, reducing unnecessary processing.

### 2. Native ESM-Based Development Server

Instead of bundling all files together, Vite leverages the browser's native support for ES modules. This means it only processes and serves files when requested, making it significantly faster.

## Setting Up a Static Server with Vite

To get started with Vite, follow these simple steps:

### 1. Install Vite

Run the following command to install Vite as a development dependency:

```sh
npm install --save-dev vite
```

### 2. Create `index.html`

Create an `index.html` file at the root of your project with the following content:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
  </head>
  <body>
    <h1>Hello Vite!</h1>
    <script type="module" src="/main.js"></script>
  </body>
</html>
```

### 3. Start the Development Server

Update your `package.json` file with the following script:

```json
{
  "scripts": {
    "dev": "vite dev"
  },
  "devDependencies": {
    "vite": "^6.2.0"
  }
}
```

Run the following command to start the server:

```sh
npm run dev
```
