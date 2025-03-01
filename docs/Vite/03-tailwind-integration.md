---
title: Tailwind Integration in Vite
description: Introduction to Tailwind Integration in Vite
sidebar_label: "Tailwind Integration"
sidebar_position: 3
---

# 🌟 Tailwind Integration in Vite

## 🚀 Introduction

Tailwind CSS is a utility-first CSS framework that helps you build modern and responsive user interfaces with ease. This guide will show you how to integrate Tailwind CSS into a **Vanilla JavaScript Application** and **React Application** using **Vite**. 🎨✨

---

## 🔧 Integration with Vanilla JavaScript

Follow these steps to integrate Tailwind CSS into your **Vite-powered** JavaScript project. 📌

### 📦 Install Tailwind CSS

First, install Tailwind CSS and its dependencies:

```sh
npm i -D tailwindcss postcss autoprefixer @tailwindcss/postcss
```

Then, initialize Tailwind configuration:

```sh
npx tailwindcss init -p
```

### ⚙️ Configure PostCSS

Update `postcss.config.js` with the following content:

```js
module.exports = {
  plugins: {
    "@tailwindcss/postcss": {},
    autoprefixer: {},
  },
};
```

### 🎨 Configure Tailwind

Update `tailwind.config.js`:

```js
module.exports = {
  content: ["index.html", "./*.js"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

### 🎨 Import Tailwind CSS

Add the following **@import** statement to your main CSS file to include Tailwind:

```css
@import "tailwindcss";
```

### 🚀 Start the Build Process

Run your build process using Vite:

```sh
npm run dev
```

---

## ⚛️ Integration with React

Follow these steps to integrate Tailwind CSS into your **React Application** using **Vite**. 🔥

### 📦 Install Tailwind CSS

Install `tailwindcss` and `@tailwindcss/vite` via npm:

```sh
npm install tailwindcss @tailwindcss/vite
```

### ⚙️ Configure the Vite Plugin

Add the `@tailwindcss/vite` plugin to your Vite configuration.

📌 **Update `vite.config.ts`:**

```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tailwindcss()],
});
```

### 🎨 Import Tailwind CSS

Add an `@import` statement to your CSS file that imports Tailwind CSS:

```css
@import "tailwindcss";
```

### 🚀 Start the Build Process

Run your build process with `npm run dev` or whatever command is configured in your `package.json` file:

```sh
npm run dev
```

🎉 **Congratulations!** You have successfully integrated Tailwind CSS with both Vanilla JavaScript and React applications using Vite! 🚀🔥

Happy coding! 💻😊
