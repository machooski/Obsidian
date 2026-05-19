---
title: "Canvas React Components"
date: 2026-05-18
tags:
  - wiki
  - programming/react
  - programming/webapp
aliases:
  - "Canvas Components"
  - "React Components in Canvas"
---

# Canvas React Components

## Table of Contents
- [[#Simple Explanation]]
- [[#Layout Components]]
- [[#Data And Widget Components]]
- [[#Form Components]]
- [[#Related Notes]]

## Simple Explanation

Canvas uses React as a component system: small pieces handle layout, widgets, forms, charts, and page composition, then those pieces are assembled into the app shell.

This note tracks the React components and component groups that are visible in the app structure.

## Layout Components

- `AppShell` — the top-level application frame
- `Sidebar` — primary navigation and screen switching
- `TopBar` — header controls and page-level actions
- `WidgetShell` — consistent container around dashboard widgets
- `SettingsModal` — modal surface for configuration and preferences

## Data And Widget Components

- `DraggableWidgetGrid` — grid layout and widget positioning
- `registry.js` — component registry that maps widget types to renderers
- `charts/` components — reusable chart views for dashboard metrics
- `widgets/` components — business widgets for systems like Jira, Smartsheet, and Supabase-backed panels
- `pages/` components — page-level composition pieces for different app sections

## Form Components

- `DynamicFormRenderer` — renders form flows from configuration
- `FormField` — field-level abstraction for inputs
- specialized controls in `src/components/forms/` — support for validation, versioning, and approval-driven workflows

## Related Notes

- [[React]] — the UI library behind the component model
- [[Canvas]] — the app these components belong to
- [[Recharts]] — the charting library used by the dashboard components
- [[MOC - Programming]] — the programming index for this vault
