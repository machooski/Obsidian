---
name: obsidian-note-creation
description: 'Create well-structured, informative Obsidian markdown notes with proper YAML frontmatter, wiki-links, tags, and backlinks. USE FOR: creating new notes in the vault; writing permanent notes; adding internal links between related notes; structuring notes with proper headings and metadata; building a connected knowledge graph. Trigger phrases: "create a note", "write a note about", "add a note", "new note", "document this".'
argument-hint: 'Topic or title of the note to create'
---

# Obsidian Note Creation

## When to Use
- Creating any new note in the vault
- Documenting knowledge, ideas, or concepts
- Linking new information to existing notes
- Building out the knowledge graph with connected content

## Note Anatomy

Every note must include:

### 1. YAML Frontmatter (top of file)
```yaml
---
title: "Note Title"
date: YYYY-MM-DD
tags:
  - topic/subtopic
  - category
aliases:
  - "Alternative Name"
  - "Short Name"
---
```

### 2. Table of Contents
After the title, add a TOC using Obsidian's native heading link syntax:
```markdown
## Table of Contents
- [[#Section Name]]
- [[#Another Section]]
  - [[#Subsection]]
```
Use `[[#Heading Name]]` — works in both Live Preview and Reading View, and Obsidian auto-updates links when headings are renamed.

### 3. Opening Summary
Start with 1–3 sentences that answer: *What is this note about and why does it matter?*

### 4. Body Content
Use clear headings (`##`, `###`) to organize. Keep each section focused on a single idea.

### 5. Connections Section (at the bottom)
```markdown
## Related Notes
- [[Related Note Title]] — brief reason for the link
- [[Another Note]] — brief reason for the link

## References
- Source or citation if applicable
```

---

## Linking Procedure

### When to Create a Wiki-Link
- Any concept, person, place, or idea that *could* have its own note → `[[Note Title]]`
- Use the exact target note's title to ensure Obsidian resolves the link
- If the note doesn't exist yet, create it (even as a stub) to keep the graph connected

### Link Syntax
| Purpose | Syntax | Example |
|---|---|---|
| Basic link | `[[Note Title]]` | `[[Atomic Habits]]` |
| Aliased link | `[[Note Title\|Display Text]]` | `[[Atomic Habits\|the book]]` |
| Section link | `[[Note Title#Heading]]` | `[[Atomic Habits#Key Ideas]]` |
| Embed content | `![[Note Title]]` | `![[Quote Block]]` |

### Backlink Strategy
- After creating a note, **update at least 2 existing related notes** to link back to the new note
- This ensures the graph stays bidirectionally connected and new notes aren't orphaned

---

## Step-by-Step: Creating a New Note

1. **Determine the title** — Use a clear, specific noun phrase (e.g., `Spaced Repetition`, not `Study Tip`)
2. **Write frontmatter** — Add `title`, `date`, `tags`, and `aliases` as needed
3. **Write the opening summary** — 1–3 sentences, plain language
4. **Structure the body** — Use `##` headings; each section = one idea
5. **Add wiki-links inline** — As you write, wrap any concept that merits its own note in `[[...]]`
6. **Add a "Related Notes" section** — List 2–5 links with a one-line reason for each
7. **Update existing notes** — Add a link to the new note from at least 2 relevant existing notes
8. **Apply tags** — Use hierarchical tags like `#topic/subtopic` for discoverability

---

## Tagging Guidelines

- Prefer **hierarchical tags**: `#concept/memory`, `#project/health`, `#book/nonfiction`
- Keep tags broad enough to reuse (avoid one-off tags)
- Use tags to represent *type* or *category*, not content (content lives in links)

---

## Stub Notes

If a concept is referenced but doesn't yet have its own note, create a minimal stub:

```markdown
---
title: "Concept Name"
date: YYYY-MM-DD
tags:
  - stub
---

> [!info] Stub
> This note is a placeholder. Expand it when ready.
```

This keeps the graph intact without requiring full content immediately.

---

## Quality Checklist

Before finishing a note, verify:
- [ ] Frontmatter is valid YAML (no missing quotes, correct indentation)
- [ ] At least 2–5 `[[wiki-links]]` are present in the body
- [ ] A "Related Notes" section exists at the bottom
- [ ] Tags are applied (at least 1)
- [ ] Existing notes have been updated to link back here
