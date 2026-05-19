---
title: "SQL"
date: 2026-05-18
tags:
  - wiki
  - programming/database
  - programming/sql
aliases:
  - "Structured Query Language"
---

# SQL

## Table of Contents
- [[#Simple Explanation]]
- [[#Core Concepts]]
- [[#Common Query Shapes]]
- [[#Related Notes]]

## Simple Explanation

SQL is the language used to work with relational databases. We use it to create tables, insert data, query records, update rows, and enforce relationships between entities.

If an application stores structured data, SQL is usually the vocabulary behind that data layer.

## Core Concepts

- **Tables** store related rows of data
- **Rows** are individual records
- **Columns** describe the fields in each record
- **Primary keys** uniquely identify a row
- **Foreign keys** connect related tables
- **Indexes** speed up lookups and joins
- **Constraints** keep data valid

## Common Query Shapes

- `SELECT` reads data
- `INSERT` adds data
- `UPDATE` changes data
- `DELETE` removes data
- `JOIN` combines tables
- `GROUP BY` aggregates rows
- `ORDER BY` sorts results
- `WHERE` filters rows

SQL is usually most useful when you think in terms of questions:

- Which rows match this filter?
- Which related records should be joined?
- What should be grouped or counted?
- Which rows should be inserted, changed, or removed?

## Related Notes

- [[SQL Commands]] — command examples and usage patterns
- [[Supabase]] — the backend platform we use around SQL
- [[MOC - Programming]] — the programming index for this vault
