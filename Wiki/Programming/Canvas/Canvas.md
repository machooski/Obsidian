---
title: "Canvas"
date: 2026-05-18
tags:
  - wiki
  - programming/webapp
  - programming/react
  - programming/nextjs
aliases:
  - "Trigger Canvas"
  - "Canvas App"
---

# Canvas

## Table of Contents
- [[#Simple Explanation]]
- [[#Main Stack]]
- [[#How The App Is Structured]]
- [[#UI Components In Use]]
- [[#Related Notes]]

## Simple Explanation

Canvas is the web application in `C:\Users\vinng\Desktop\VS\Trigger\Canvas`. It is a Next.js app built around React components, shared layouts, widget-driven screens, and form-heavy workflows.

This note tracks the tools and component patterns used in the app so the stack is documented in one place.

## Main Stack

- **Next.js** for the app shell, routing, server-side features, and API routes
- **React** for reusable UI components and interactive screen state
- **Tailwind CSS** for styling and layout utility classes
- **Supabase** for backend data and database access
- **NextAuth** for authentication and session handling
- **Framer Motion** for animation and motion polish
- **Recharts** for charting and data visualization
- **react-grid-layout** for draggable dashboard grids
- **react-hook-form** plus **zod** for form state and validation
- **react-markdown** for rendering markdown content
- **@phosphor-icons/react** for iconography
- **jspdf** for PDF generation and export flows

## How The App Is Structured

The app uses the Next.js App Router and a component-oriented layout:

- `src/app/` contains routes, layouts, and API endpoints
- `src/app/AppShell.jsx` provides the main application shell
- `src/app/providers.tsx` wires global providers and shared state
- `src/components/layout/` holds the shared frame like sidebar, top bar, and modal surfaces
- `src/components/widgets/` contains dashboard widgets for external systems and data sources
- `src/components/forms/` contains the dynamic form system and field-level building blocks
- `src/components/charts/` contains reusable chart wrappers
- `src/components/pages/` holds page-specific UI composition
- `src/components/DraggableWidgetGrid.jsx` controls the dashboard grid behavior

The practical pattern is: the app shell sets the frame, pages assemble the view, widgets render data, and forms/charts provide reusable UI subsystems.

## UI Components In Use

The component set suggests a few major UI families:

- **Layout components** like `Sidebar`, `TopBar`, `WidgetShell`, and `SettingsModal`
- **Grid and dashboard components** built around `DraggableWidgetGrid`
- **Form components** such as `DynamicFormRenderer`, `FormField`, and specialized input controls
- **Chart components** that wrap Recharts for standard dashboard visuals
- **Business widgets** for app-specific integrations like Jira, Smartsheet, and Supabase-backed panels

The design is component-first: small UI pieces are composed into pages, and pages are composed into the full shell.

## Related Notes

- [[React]] — the UI library that powers the component model
- [[Canvas React Components]] — the component inventory for this app
- [[Supabase]] — backend platform used in Canvas
- [[NextAuth]] — authentication layer used in Canvas
- [[Recharts]] — dashboard charting library used in Canvas
- [[Next.js]] — the framework Canvas is built on
- [[The JavaScript Ecosystem]] — broader context for the stack
- [[MOC - Programming]] — the programming index for this vault