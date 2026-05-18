---
name: obsidian-daily-note
description: 'Create a daily note in Obsidian with date-stamped frontmatter, journal prompts, task tracking, and links to related notes. USE FOR: daily journaling; logging tasks and wins; end-of-day reflections; capturing fleeting thoughts for later processing. Trigger phrases: "daily note", "journal entry", "today note", "log today", "daily log".'
argument-hint: 'Date for the daily note (defaults to today)'
---

# Obsidian Daily Note

## When to Use
- Starting a new day's note
- Logging tasks, events, or reflections for a specific date
- Capturing fleeting thoughts that need processing later

---

## Daily Note Template

Create a file named `YYYY-MM-DD.md` (e.g., `2026-05-17.md`) in a `Daily Notes/` folder.

```markdown
---
title: "YYYY-MM-DD"
date: YYYY-MM-DD
tags:
  - daily-note
---

# YYYY-MM-DD — Day of Week

## Focus
> What is the one thing that would make today a success?

## Tasks
- [ ] 
- [ ] 
- [ ] 

## Notes & Thoughts
<!-- Capture ideas, observations, and links here -->

## What I Learned
- 

## Reflection
- **Win of the day:** 
- **What I'd do differently:** 

## Links to Process
<!-- Fleeting notes that should become permanent notes -->
- 

---
← [[YYYY-MM-DD]] | [[YYYY-MM-DD]] →
```

---

## Step-by-Step

1. **Name the file** using `YYYY-MM-DD` format and place it in `Daily Notes/`
2. **Fill the frontmatter** — set `date` to today's date
3. **Set the Focus** — one sentence on the day's priority
4. **List tasks** — use `- [ ]` checkboxes
5. **Log throughout the day** — add notes, ideas, and `[[wiki-links]]` as they come
6. **End-of-day reflection** — fill in "What I Learned" and "Reflection"
7. **Process "Links to Process"** — create permanent notes for any idea worth keeping
8. **Add navigation links** — link to yesterday's and tomorrow's note using `[[YYYY-MM-DD]]`

---

## Fleeting → Permanent Note Pipeline

Daily notes are *inboxes*, not permanent storage. Any idea captured in a daily note that has lasting value should be:
1. Turned into its own permanent note (use the `obsidian-note-creation` skill)
2. Tagged appropriately
3. Linked back from the daily note with a brief note: `→ [[Permanent Note Title]]`
