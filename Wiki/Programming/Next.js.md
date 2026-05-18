---
title: "Next.js"
date: 2026-05-17
tags:
  - wiki
  - programming/javascript
  - programming/framework
  - programming/frontend
  - programming/fullstack
aliases:
  - "NextJS"
  - "Next"
---

# Next.js

## Table of Contents
- [[#Simple Explanation]]
- [[#What Problem Does It Solve?]]
- [[#How It Works]]
- [[#Key Terms]]
- [[#In Depth]]
  - [[#Rendering Models — The Most Important Concept]]
  - [[#App Router vs Pages Router]]
  - [[#Server Components vs Client Components]]
  - [[#File-Based Routing]]
  - [[#API Routes / Route Handlers]]
  - [[#The Next.js Stack]]
- [[#When to Use Next.js vs Alternatives]]
- [[#Quick Reference]]
- [[#Common Gotchas / Misconceptions]]
- [[#Related Notes]]

---

## Simple Explanation

React is a library for building UI — buttons, forms, pages. But plain React gives you no routing, no server, no way to pre-render pages for SEO, and no opinions on project structure. **Next.js is a framework built on React that fills all those gaps.**

It adds: routing, server-side rendering, static generation, image optimization, font optimization, API endpoints, and a folder structure that drives the whole app. And because it runs on [[Node.js]], you can do backend work and frontend work in the same project with the same language.

> **Analogy:** React is an engine. Next.js is the whole car — same engine inside, but now you also have a steering wheel, brakes, seats, and GPS.

---

## What Problem Does It Solve?

Plain React has three major gaps for real production websites:

**1. SEO problem**
React renders in the browser (client-side rendering). When a search engine bot visits your page, it sees a nearly empty HTML file — the content hasn't loaded yet. Next.js can pre-render pages on the server so bots (and users) get full HTML immediately.

**2. Performance problem**
Users on slow connections see a blank page until JavaScript loads and runs. Server-rendered pages from Next.js deliver visible content instantly.

**3. Routing problem**
React has no built-in router. You have to install `react-router` and configure it manually. Next.js gives you file-based routing — create a file, get a route.

---

## How It Works

1. **You write React components** — same JSX syntax you already know
2. **Next.js figures out where each component runs** — some render on the server (Server Components), some in the browser (Client Components)
3. **At build time or request time**, Next.js renders pages to HTML — either ahead of time (Static Generation) or on each request (Server-Side Rendering)
4. **The router is your folder structure** — `app/about/page.tsx` becomes the `/about` route automatically
5. **API routes live in the same project** — `app/api/users/route.ts` handles `GET /api/users`
6. **Deployment is optimized for Vercel** (made by the same company) but works on any Node.js host

---

## Key Terms

| Term | What it means |
|------|---------------|
| **React** | The UI library Next.js is built on — you write components in React |
| **SSR** | Server-Side Rendering — HTML is generated on the server for each request |
| **SSG** | Static Site Generation — HTML is generated once at build time, served as a file |
| **ISR** | Incremental Static Regeneration — SSG pages that can be silently updated in the background |
| **CSR** | Client-Side Rendering — the default React behavior; browser downloads JS and renders the page |
| **App Router** | The modern Next.js router (v13+), based on the `app/` folder |
| **Pages Router** | The older Next.js router, based on the `pages/` folder — still supported |
| **Server Component** | A React component that renders on the server; can fetch data directly; sends no JS to the browser |
| **Client Component** | A React component that runs in the browser; needed for interactivity, state, events |
| **Route Handler** | An API endpoint file inside the `app/` folder (`route.ts`) |
| **Hydration** | The process of attaching React's JavaScript to server-rendered HTML so it becomes interactive |
| **Vercel** | The company that builds Next.js; also the recommended hosting platform |

---

## In Depth

### Rendering Models — The Most Important Concept

Understanding when and where a page is rendered is the core of Next.js. Getting this wrong causes SEO issues, slow pages, or stale data.

| Mode | When HTML is made | Good for |
|---|---|---|
| **SSG** (Static) | At build time, once | Marketing pages, blogs, docs — content that rarely changes |
| **ISR** | At build time + silently refreshed on a schedule | Product pages, news — content that changes but can be slightly stale |
| **SSR** | On every request, on the server | Dashboards, personalized content, real-time data |
| **CSR** | In the browser after page load | Highly interactive UIs where SEO doesn't matter |

In the App Router, you control this with `fetch()` options and `export const revalidate`:

```typescript
// Static (SSG) — cached forever
const data = await fetch('https://api.example.com/data')

// ISR — re-fetch every 60 seconds
const data = await fetch('https://api.example.com/data', {
  next: { revalidate: 60 }
})

// SSR — never cache, always fresh
const data = await fetch('https://api.example.com/data', {
  cache: 'no-store'
})
```

---

### App Router vs Pages Router

Next.js has two routing systems. **New projects should use App Router.** If you're working on an older codebase, you may encounter Pages Router.

| | App Router (`app/`) | Pages Router (`pages/`) |
|---|---|---|
| **Introduced** | Next.js 13 (2022) | Original Next.js |
| **Components** | Server Components by default | Client Components by default |
| **Data fetching** | `async` components + `fetch()` | `getStaticProps`, `getServerSideProps` |
| **Layouts** | Nested `layout.tsx` files | Custom `_app.tsx` |
| **API routes** | `route.ts` files | `pages/api/*.ts` files |
| **Status** | Current standard | Still supported, not deprecated |

---

### Server Components vs Client Components

This is the biggest mental shift in the App Router.

**Server Components (default):**
- Render on the server
- Can `async/await` directly — no `useEffect`, no loading states
- Can access databases, environment variables, secrets
- **Send zero JavaScript to the browser**
- Cannot use `useState`, `useEffect`, event handlers, or browser APIs

**Client Components (`'use client'` at top of file):**
- Render in the browser (also pre-rendered on server for initial HTML)
- Can use `useState`, `useEffect`, `onClick`, etc.
- Can access browser APIs (`window`, `localStorage`)
- Their JavaScript is sent to the browser

```typescript
// Server Component (no directive needed)
export default async function ProductPage({ params }) {
  const product = await db.product.findById(params.id) // direct DB access!
  return <div>{product.name}</div>
}

// Client Component
'use client'
import { useState } from 'react'

export default function Counter() {
  const [count, setCount] = useState(0)
  return <button onClick={() => setCount(count + 1)}>{count}</button>
}
```

**Rule of thumb:** Keep components as Server Components until you need interactivity, then add `'use client'`.

---

### File-Based Routing

Your folder structure *is* your routes. In the `app/` directory:

```
app/
├── page.tsx           →  /
├── about/
│   └── page.tsx       →  /about
├── blog/
│   ├── page.tsx       →  /blog
│   └── [slug]/
│       └── page.tsx   →  /blog/anything  (dynamic route)
├── layout.tsx         →  wraps every page
└── api/
    └── users/
        └── route.ts   →  GET/POST /api/users
```

Special files:
| File | Purpose |
|---|---|
| `page.tsx` | The UI for a route |
| `layout.tsx` | Shared wrapper (nav, footer) — persists across navigations |
| `loading.tsx` | Shown while the page is loading (automatic Suspense) |
| `error.tsx` | Shown when an error occurs in that route segment |
| `route.ts` | API endpoint (no UI) |
| `not-found.tsx` | Custom 404 page |

---

### API Routes / Route Handlers

You can build a full backend API inside a Next.js project. Each `route.ts` file exports functions named after HTTP methods:

```typescript
// app/api/users/route.ts

export async function GET() {
  const users = await db.user.findMany()
  return Response.json(users)
}

export async function POST(request: Request) {
  const body = await request.json()
  const user = await db.user.create({ data: body })
  return Response.json(user, { status: 201 })
}
```

---

### The Next.js Stack

Next.js is often used with:

| Layer | Common choices |
|---|---|
| **Language** | TypeScript (strongly recommended) |
| **Styling** | Tailwind CSS, CSS Modules, shadcn/ui |
| **Database ORM** | Prisma, Drizzle |
| **Auth** | NextAuth.js / Auth.js, Clerk, Lucia |
| **State management** | Zustand, Jotai (client state); React Query / SWR (server state) |
| **Deployment** | Vercel (easiest), Railway, Fly.io, self-hosted |

---

## When to Use Next.js vs Alternatives

| If you need... | Consider... |
|---|---|
| Full-stack app, SEO matters, React | **Next.js** ✓ |
| Full-stack app, fine-grained data loading control | **Remix** |
| Mostly static site (blog, docs, marketing) | **Astro** |
| Pure SPA, no SEO concerns, no server | **Vite + React** |
| Vue.js equivalent of Next.js | **Nuxt** |
| Maximum simplicity, landing page | **Astro** or plain HTML |

---

## Quick Reference

```bash
# Create a new Next.js project
npx create-next-app@latest my-app

# Start development server
npm run dev         # → http://localhost:3000

# Build for production
npm run build

# Start production server
npm run start

# Check Next.js version
npx next --version
```

**Useful `next.config.ts` options:**
```typescript
const nextConfig = {
  images: { domains: ['example.com'] },  // allow external images
  experimental: { serverActions: true },  // server actions (forms)
}
```

---

## Common Gotchas / Misconceptions

- **"Next.js is just React"** — React is the UI layer. Next.js adds routing, server rendering, build optimization, image handling, API routes, and a full deployment model. It's a full framework.
- **"I need 'use client' on all components"** — Most components don't need it. Only add it when you need `useState`, `useEffect`, or browser events. Keeping components as Server Components improves performance.
- **"SSG is always better than SSR"** — SSG is faster but can serve stale data. Use SSR or ISR for anything that changes. Use SSG for truly static content.
- **"My API keys are safe in client components"** — They are NOT. Environment variables in Client Components are exposed to the browser. Only use secrets in Server Components, API routes, and server actions.
- **"The `app/` and `pages/` directories can coexist"** — Yes, Next.js supports both in one project during migration.
- **"Vercel is required"** — Vercel is easiest, but Next.js runs on any Node.js host. Self-hosting is fully supported.

---

## Related Notes
- [[Node.js]] — the runtime Next.js runs on
- [[The JavaScript Ecosystem]] — how React, Node, npm, and Next.js fit together
- [[MOC - Programming]] — parent index
