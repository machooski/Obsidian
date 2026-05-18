---
name: obsidian-tech-concept
description: 'Create a personal wiki note explaining a technical concept (programming, AI, servers, networking, tools). Always includes a simple/plain-English explanation AND an in-depth section. USE FOR: documenting programming concepts; explaining AI/ML terms; describing server and infrastructure topics; breaking down tools and protocols; building a personal tech knowledge base. Trigger phrases: "explain", "what is", "tech note", "concept note", "wiki note", "document concept", "add to wiki".'
argument-hint: 'Name of the concept or technology to document'
---

# Obsidian Tech Concept Note

## When to Use
- Adding any technical topic to the personal wiki
- Explaining a programming language, pattern, or concept
- Documenting an AI/ML term, model, or framework
- Describing server infrastructure, protocols, or tools

---

## Folder Structure (Tech Wiki)

Place notes in the appropriate folder:

```
Wiki/
├── AI/               ← AI, ML, LLMs, agents, protocols
├── Programming/      ← Languages, patterns, paradigms, algorithms
├── Servers/          ← Servers, networking, protocols, infrastructure
├── Tools/            ← Dev tools, CLIs, editors, platforms
└── Concepts/         ← General CS concepts that span categories
```

---

## Note Template

```markdown
---
title: "Concept Name"
date: YYYY-MM-DD
tags:
  - wiki
  - domain/subtopic        # e.g. ai/protocol, programming/pattern, server/network
aliases:
  - "Alternative Name"
  - "Acronym"
---

# Concept Name

## Table of Contents
- [[#Simple Explanation]]
- [[#What Problem Does It Solve?]]
- [[#How It Works]]
- [[#Key Terms]]
- [[#In Depth]]
- [[#Quick Reference]]
- [[#Related Notes]]
- [[#References]]

---

## Simple Explanation
<!-- Explain it like the reader has no background. Use an analogy. 3–6 sentences max. -->

> **Analogy:** Compare it to something familiar and non-technical.

## What Problem Does It Solve?
<!-- Why does this exist? What was broken or hard before it? -->

## How It Works
<!-- Step-by-step, slightly more technical. Use numbered steps or a diagram description. -->

1. 
2. 
3. 

## Key Terms
| Term | What it means |
|------|---------------|
|      |               |

## In Depth
<!-- Detailed technical explanation: architecture, specs, edge cases, internals. -->

### Architecture / Structure

### How It's Used in Practice

### Common Gotchas / Misconceptions

## Quick Reference
<!-- Commands, code snippets, or a short cheat sheet if applicable -->

```code block here if needed```

## Related Notes
- [[Related Concept]] — reason for the link
- [[MOC - Domain]] — parent index

## References
- 
```

---

## Step-by-Step: Creating a Tech Concept Note

1. **Choose the folder** — AI, Programming, Servers, Tools, or Concepts
2. **Name the file** — use the canonical name of the concept (e.g., `MCP Server.md`)
3. **Write frontmatter** — set `tags` using `domain/subtopic` hierarchy
4. **Write the Table of Contents** — list all `##` sections using `[[#Heading Name]]` syntax
5. **Write "Simple Explanation" first** — plain English, analogy required
6. **Write "What Problem Does It Solve?"** — this anchors the concept in purpose
7. **Write "How It Works"** — numbered steps, conceptual flow
8. **Fill "Key Terms"** — define any jargon used in the in-depth section
9. **Write "In Depth"** — technical detail, architecture, internals
10. **Add "Quick Reference"** — commands or snippets if applicable
11. **Add "Related Notes"** — link to related concepts and the domain MOC
12. **Update the domain MOC** — add this note to the relevant `MOC - Domain.md`

---

## Tag Taxonomy

Use hierarchical tags for every tech wiki note:

| Domain | Tag examples |
|---|---|
| AI | `#ai/llm`, `#ai/protocol`, `#ai/agent`, `#ai/model` |
| Programming | `#programming/pattern`, `#programming/language`, `#programming/algorithm` |
| Servers | `#server/protocol`, `#server/network`, `#server/infrastructure` |
| Tools | `#tools/cli`, `#tools/editor`, `#tools/platform` |
| Concepts | `#concepts/cs`, `#concepts/security`, `#concepts/data` |

---

## Quality Checklist

Before finishing a tech concept note:
- [ ] Simple Explanation uses an analogy and is jargon-free
- [ ] "What Problem Does It Solve?" is filled out
- [ ] At least one `[[wiki-link]]` to a related concept
- [ ] Key terms table populated (if jargon is used)
- [ ] Domain MOC updated to include this note
