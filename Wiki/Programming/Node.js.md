---
title: "Node.js"
date: 2026-05-17
tags:
  - wiki
  - programming/javascript
  - programming/runtime
  - programming/backend
aliases:
  - "NodeJS"
  - "Node"
---

# Node.js

## Table of Contents
- [[#Simple Explanation]]
- [[#What Problem Does It Solve?]]
- [[#How It Works]]
- [[#Key Terms]]
- [[#In Depth]]
  - [[#The V8 Engine]]
  - [[#The Event Loop]]
  - [[#npm — The Package Manager]]
  - [[#CommonJS vs ES Modules]]
- [[#What You Build With Node.js]]
- [[#Quick Reference]]
- [[#Common Gotchas / Misconceptions]]
- [[#Related Notes]]

---

## Simple Explanation

JavaScript was invented to run *inside web browsers* — it's how websites respond when you click a button. For years, that was its only job.

**Node.js breaks JavaScript out of the browser.** It lets you run JavaScript on a server, on your laptop, anywhere. It's a runtime — a program that executes JavaScript code outside the browser environment.

> **Analogy:** JavaScript is a language. A browser is one place you can speak it. Node.js is another place — the server room, the terminal, the cloud. Same language, completely different room.

---

## What Problem Does It Solve?

Before Node.js (2009), if you built a web app, you needed:
- **JavaScript** for frontend (browser interactions)
- **PHP, Ruby, Python, Java, etc.** for the backend server

This meant learning two languages, two ecosystems, two sets of tools, and constantly switching mental contexts.

Node.js also solved a technical bottleneck: traditional servers handled each request by spinning up a new thread. Under heavy load, you'd run out of threads and grind to a halt. Node's **non-blocking I/O** model handles thousands of simultaneous connections on a single thread — it just doesn't *wait* for slow operations (database reads, file reads, API calls) to finish before handling the next request.

---

## How It Works

1. **You write JavaScript** — same syntax you'd use in a browser
2. **Node.js feeds it to the V8 engine** — Google's JavaScript engine (the same one Chrome uses) compiles and executes your code
3. **Requests come in** — e.g., someone hits your API endpoint
4. **The event loop takes over** — instead of waiting for a database query to finish, Node registers a callback ("call me when the data is ready") and immediately moves on to handle the next request
5. **When I/O completes**, the callback fires and the response is sent
6. **npm manages dependencies** — you install libraries (packages) that other people wrote, rather than building everything from scratch

---

## Key Terms

| Term | What it means |
|------|---------------|
| **Runtime** | A program that executes code. Node.js is a JavaScript runtime (like how a browser is also a JS runtime, just a different one) |
| **V8** | Google's high-performance JavaScript engine. Both Chrome and Node.js use it to actually run JS code |
| **Event loop** | Node's mechanism for handling many operations concurrently without blocking — the core of what makes Node fast |
| **Non-blocking I/O** | When Node makes a file read or database call, it doesn't freeze and wait — it moves on and comes back when the result is ready |
| **npm** | Node Package Manager — the tool for installing, sharing, and managing JavaScript libraries (packages) |
| **package.json** | The manifest file for your Node project — lists your dependencies, scripts, and project metadata |
| **module** | A reusable piece of code. Everything in Node can be organized into modules that export and import from each other |

---

## In Depth

### The V8 Engine

V8 is a C++ program written by Google. Its job: take JavaScript source code and compile it to fast machine code. Node.js is essentially V8 + a set of extra APIs that don't exist in browsers (file system access, network sockets, OS interaction, etc.).

When Google updates V8 for Chrome performance, Node.js benefits from the same improvements.

### The Event Loop

This is the heart of Node.js and the hardest thing to really understand:

```
   ┌──────────────────────────────┐
   │         Event Loop           │
   │                              │
   │  1. Execute synchronous code  │
   │  2. Check for I/O callbacks  │
   │  3. Check timers             │
   │  4. Check setImmediate       │
   │  5. Close events             │
   │                              │
   │  ← loops forever while       │
   │    there is work to do →     │
   └──────────────────────────────┘
```

**The key insight:** When you do something slow (read a file, hit a database, call an API), Node doesn't sit and wait. It says "tell me when you're done" and goes on to handle other work. This is why Node can handle thousands of simultaneous connections on one thread.

**What Node is NOT good at:** CPU-heavy work (video encoding, complex math, machine learning). Because there's only one thread, if you block it with computation, *everything* waits. Use Python or Go for CPU-bound tasks.

### npm — The Package Manager

`npm` comes bundled with Node. It connects to the npm registry (npmjs.com) — a public library of over 2 million packages.

```bash
npm install express        # adds to node_modules/ and package.json
npm install -D typescript  # dev dependency (only used during development)
npm run dev                # runs the "dev" script defined in package.json
npm init                   # creates a new package.json
```

**Alternative package managers** that are faster and/or more reliable than npm:
| Tool | Notes |
|------|-------|
| **pnpm** | Fastest; uses a shared content-addressable store; saves disk space |
| **yarn** | Meta's alternative; yarn workspaces good for monorepos |
| **bun** | Extremely fast; also a full JS runtime (Node alternative) |

### CommonJS vs ES Modules

Two different ways to import/export code in Node. You'll see both in the wild:

```javascript
// CommonJS (older, .js files, require/module.exports)
const express = require('express')
module.exports = { myFunction }

// ES Modules (modern, .mjs files or "type": "module" in package.json)
import express from 'express'
export { myFunction }
```

Most modern projects use ES Modules. Next.js uses ES Modules.

---

## What You Build With Node.js

| Use case | Common tools |
|---|---|
| **Web API / REST server** | Express, Fastify, Hono |
| **Full-stack web app** | Next.js, Remix, Nuxt (all run on Node) |
| **CLI tools** | Commander.js, Inquirer — e.g., `npm`, `git` wrappers |
| **Build tools** | Webpack, Vite, esbuild (all run on Node) |
| **Real-time apps** | Socket.io (chat, live collaboration) |
| **Scripting / automation** | Anything you'd use Python scripts for |
| **MCP servers** | Most MCP servers are Node.js programs |

---

## Quick Reference

```bash
# Check Node version
node --version

# Run a JS file
node script.js

# Start a REPL (interactive Node shell)
node

# Initialize a new project
npm init -y

# Install a package
npm install <package-name>

# Install dev dependency
npm install -D <package-name>

# Run a script from package.json
npm run <script-name>

# Install all dependencies from package.json
npm install
```

---

## Common Gotchas / Misconceptions

- **"Node.js is a framework"** — It's a runtime. Express is a framework that *runs on* Node. Next.js is a framework that *runs on* Node.
- **"Node.js is only for servers"** — Build tools (Vite, webpack), CLIs, and scripts all use Node. When you run `npm install`, that's Node.
- **"JavaScript is slow"** — V8 is heavily optimized. Node.js performance is competitive with Go and Java for I/O-heavy workloads.
- **"async/await is Node-specific"** — It's standard JavaScript. Node just makes it essential because almost everything is async.
- **"node_modules can be deleted"** — Yes. Running `npm install` recreates it from `package.json`. Never commit `node_modules` to git.

---

## Related Notes
- [[Next.js]] — the React framework that runs on Node.js
- [[The JavaScript Ecosystem]] — how Node, npm, React, and Next.js connect
- [[MOC - Programming]] — parent index
