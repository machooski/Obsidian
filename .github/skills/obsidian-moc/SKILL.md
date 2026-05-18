---
name: obsidian-moc
description: 'Create a Map of Content (MOC) note in Obsidian — an index hub note that organizes and links a cluster of related notes on a topic. USE FOR: building topic indexes; organizing a group of notes under one hub; creating entry points into a subject area; preventing orphaned notes. Trigger phrases: "map of content", "MOC", "index note", "hub note", "organize notes on", "create an index for".'
argument-hint: 'Topic or subject area for the MOC'
---

# Obsidian Map of Content (MOC)

## What Is a MOC?

A Map of Content is a **hub note** that doesn't contain deep content itself — it *organizes and links* to other notes on a topic. MOCs are the backbone of a well-connected vault.

**When to create one:**
- You have 5+ notes on a related topic and navigation is getting hard
- You want an entry point into a subject area
- A topic has multiple subtopics that each deserve their own cluster

---

## MOC Template

Name the file: `MOC - Topic Name.md` or `Topic Name MOC.md`

```markdown
---
title: "MOC - Topic Name"
date: YYYY-MM-DD
tags:
  - moc
  - topic/name
aliases:
  - "Topic Name Index"
---

# Topic Name — Map of Content

> Brief 1–2 sentence description of what this MOC covers and why it exists.

---

## Core Concepts
- [[Concept A]] — one-line description
- [[Concept B]] — one-line description
- [[Concept C]] — one-line description

## Subtopics
### Subtopic 1
- [[Note 1]] — one-line description
- [[Note 2]] — one-line description

### Subtopic 2
- [[Note 3]] — one-line description
- [[Note 4]] — one-line description

## Processes & How-Tos
- [[How to Do X]] 
- [[Step-by-Step Guide for Y]]

## Resources & References
- [[Book Note: Title]] 
- [[Article: Title]]

## Related MOCs
- [[MOC - Related Topic A]]
- [[MOC - Related Topic B]]
```

---

## Step-by-Step: Building a MOC

1. **Identify the topic cluster** — search the vault for notes that belong together
2. **Name the MOC file** — use `MOC - Topic.md` convention for easy filtering
3. **Write a one-paragraph overview** — what is this topic and why does it matter?
4. **Group notes into sections** — organize by concept, subtopic, or note type
5. **Add a one-line description for each link** — so the MOC is scannable at a glance
6. **Link to related MOCs** — connect hubs together to build the meta-graph
7. **Update all linked notes** — add `[[MOC - Topic Name]]` to their "Related Notes" section
8. **Tag the MOC with `#moc`** — so all MOCs are easily filterable

---

## MOC vs. Regular Note

| | Regular Note | MOC |
|---|---|---|
| Content | Deep dive on one idea | Links and brief descriptions |
| Purpose | Capture knowledge | Navigate knowledge |
| Length | As long as needed | Short and scannable |
| Tag | `#topic/subtopic` | `#moc` |
| Update frequency | When idea evolves | When new related notes are added |

---

## Keeping MOCs Alive

- When creating a new note on a topic, **immediately add it to the relevant MOC**
- Review MOCs periodically — add new notes, remove broken links
- If a MOC section grows beyond ~10 items, consider splitting it into a sub-MOC
