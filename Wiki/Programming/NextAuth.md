---
title: "NextAuth"
date: 2026-05-18
tags:
  - wiki
  - programming/auth
  - programming/nextjs
aliases:
  - "NextAuth.js"
  - "Auth.js"
---

# NextAuth

## Table of Contents
- [[#Simple Explanation]]
- [[#Core Ideas]]
- [[#How We Use It]]
- [[#Related Notes]]

## Simple Explanation

NextAuth is the authentication layer used in a Next.js app. It handles login flows, sessions, providers, and server-side auth checks so the app can know who the current user is.

## Core Ideas

- **Providers** connect external sign-in systems or credential flows
- **Sessions** represent the authenticated user state
- **Callbacks** let you shape what data gets attached to the session or token
- **Server checks** protect routes and data access on the backend

## How We Use It

In practice, NextAuth is used to:

- sign users in and out
- keep session state available across the app
- protect pages and API routes
- attach user identity to data-access logic

In a Next.js app, this often means the login page, auth routes, and server components all work together around the same session model.

## Related Notes

- [[Supabase]] — the backend/data platform alongside auth
- [[Next.js]] — the framework NextAuth is running inside
- [[Canvas]] — the app that uses this auth setup
- [[MOC - Programming]] — the programming index for this vault
