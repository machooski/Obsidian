---
title: "Supabase"
date: 2026-05-18
tags:
  - wiki
  - programming/backend
  - programming/database
  - programming/auth
aliases:
  - "Supabase Platform"
---

# Supabase

## Table of Contents
- [[#Simple Explanation]]
- [[#What It Gives Us]]
- [[#How We Use It]]
- [[#Why It Fits This Stack]]
- [[#Related Notes]]

## Simple Explanation

Supabase is the backend platform we use around our database layer. It combines Postgres with tools for auth, APIs, storage, and client access so the app can talk to data without building every backend piece from scratch.

## What It Gives Us

- **Postgres** as the relational database
- **Auth** for user sign-in and session-related flows
- **Client libraries** for talking to the backend from the app
- **Storage** for files and uploads
- **Auto-generated APIs** around the database
- **Row-level security** to control what users can access

## How We Use It

In a Next.js app, Supabase usually shows up in a few places:

- reading application data from tables
- saving form submissions and workflow state
- validating user access through auth and session state
- connecting server-side routes to the database
- enforcing permissions close to the data with RLS

The important idea is that Supabase lets the app stay SQL-first while still having a practical backend surface for auth and data access.

## Why It Fits This Stack

- It works naturally with SQL and Postgres
- It pairs well with React and Next.js because the app can fetch data from the client or server
- It reduces the amount of custom backend code needed for CRUD and auth

## Related Notes

- [[SQL]] — the database language underneath Supabase
- [[SQL Commands]] — practical query usage
- [[NextAuth]] — the app’s authentication layer
- [[Canvas]] — the app where Supabase is being used
- [[MOC - Programming]] — the programming index for this vault
