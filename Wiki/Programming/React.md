---
title: "React"
date: 2026-05-18
tags:
  - wiki
  - programming/javascript
  - programming/frontend
  - programming/library
aliases:
  - "React.js"
---

# React

## Table of Contents
- [[#Simple Explanation]]
- [[#Core Ideas]]
- [[#Component Examples]]
- [[#Deeper Component Patterns]]
- [[#State And Events]]
- [[#How We Use It Everywhere]]
- [[#Related Notes]]

## Simple Explanation

React is a library for building user interfaces out of reusable pieces called components. We use it to describe what the UI should look like for a given state, then let React update the screen when that state changes.

In practice, React is the layer that turns data into buttons, forms, cards, tables, menus, dashboards, and complete pages.

## Core Ideas

- **Components** are the basic building blocks of a React app
- **Props** pass data into a component from the outside
- **State** stores changing data inside a component
- **JSX** lets us write UI structure with HTML-like syntax inside JavaScript
- **Composition** means building bigger interfaces by combining smaller components

## Component Examples

```tsx
function Button({ label }: { label: string }) {
  return <button className="btn">{label}</button>
}

function UserCard({ name, role }: { name: string; role: string }) {
  return (
    <article className="card">
      <h2>{name}</h2>
      <p>{role}</p>
      <Button label="Message" />
    </article>
  )
}
```

This pattern scales because the same idea can represent tiny UI pieces or whole sections of an app:

- `Button` for one interactive control
- `Input` and `Form` for data entry
- `Card` for reusable content blocks
- `Navbar`, `Sidebar`, and `Footer` for layout structure
- `Dashboard`, `ProfilePage`, or `SettingsPage` for larger screen-level components

## Deeper Component Patterns

React apps usually end up with a few repeated patterns:

- **Presentational components** focus on how something looks
- **Container components** fetch data or manage state, then pass props down
- **Layout components** provide structure like sidebars, headers, and content regions
- **Compound components** work together as a small UI system, such as tabs or menus

This is why React is useful everywhere: the same component model works for a small button and for a full page shell.

## State And Events

React becomes interactive when components store state and respond to events.

```tsx
function Counter() {
  const [count, setCount] = useState(0)

  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  )
}
```

That pattern shows up constantly in real apps:

- form inputs update state as the user types
- filters update lists and tables
- dialogs open and close from button clicks
- dashboard widgets refresh when new data arrives

In a larger app, React usually sits at the boundary between user action and UI updates: events happen, state changes, and the interface re-renders from the new data.

## How We Use It Everywhere

React shows up anywhere the UI needs to stay consistent while data changes:

- **Design systems** use React components as the shared vocabulary for UI
- **Forms** use component state to track input, validation, and submission
- **Lists and tables** render rows from data instead of hard-coding markup
- **Dashboards** combine charts, filters, and summaries into one screen
- **Navigation** is often built from reusable menu and layout components
- **Next.js apps** use React for the UI layer while Next.js adds routing and server features
- **Canvas-style apps** usually combine React layout components, widget systems, form components, and data views in one shell

The practical mental model is: if a screen can be broken into parts, those parts are usually React components. The app then becomes a tree of components passing data downward and events upward.

## Related Notes

- [[The JavaScript Ecosystem]] — shows where React sits in the wider web stack
- [[Next.js]] — the framework that builds on React
- [[MOC - Programming]] — the programming index for this vault