---
name: process
description: Process the inbox — read unorganized notes, add structure and metadata, create connections between related notes, and move them to the proper folders.
trigger: /process
---

# /process — Organize Your Inbox

This is the core skill. Read everything in the Inbox, add structure, create connections, and move notes to their proper home. This is what makes the graph grow.

## Steps

### 1. Scan the inbox

List all files in `Inbox/`:

```bash
ls "Inbox/" 2>/dev/null
```

If the inbox is empty:

> "Your inbox is empty — nothing to process. Drop some notes, ideas, or captures into the Inbox folder and run `/process` again when you're ready."

### 2. Read all inbox files

Use the Read tool to read each file. Keep track of:
- The original filename
- The content
- Key concepts, topics, people, or projects mentioned

### 3. Read existing notes for context

Scan titles and frontmatter of existing notes in `Notes/` and `Projects/` to find connection opportunities:

```bash
ls "Notes/"*.md 2>/dev/null
```

Read frontmatter (first 10 lines) of each to understand what topics already exist.

### 4. Process each inbox file

For each file:

**a) Determine the type:**
- Quick idea or shower thought → `type: idea`
- Book highlights or reading notes → `type: book`
- Meeting notes → `type: meeting`
- Personal reflection or journal entry → `type: note`
- Project-related → `type: note` with project link
- Recipe, list, or reference → `type: note`

**b) Create a clean title** from the content (not the messy filename).

**c) Add YAML frontmatter:**
```yaml
---
title: Clean Descriptive Title
type: note
date: YYYY-MM-DD
tags: [topic1, topic2, topic3]
---
```

Use today's date if no date is apparent from the content or filename.

**d) Clean up formatting:**
- Fix inconsistent line breaks
- Remove app-specific metadata or formatting artifacts
- Standardize bullet points and headings
- **Never change the user's words** — only fix formatting

**e) Add wiki-links:**
- Look for concepts that appear in other notes (existing or being processed in this batch)
- Create `[[wiki-links]]` inline where natural
- Add a `## Related` section at the bottom:
  ```
  ## Related
  - [[Connected Note Title]]
  - [[Another Related Note]]
  ```

**f) Rename the file** to clean kebab-case: `book-notes-atomic-habits.md`

**g) Move the file** to the appropriate folder:
- Most notes → `Notes/`
- Project-related → `Projects/[project-name]/`
- Daily entry → append to today's daily note in `Daily/`

### 5. Create topic notes if patterns emerge

If a concept appears in 3 or more notes and doesn't have its own note yet, create a brief topic note:

```yaml
---
title: [Topic Name]
type: note
date: YYYY-MM-DD
tags: [topic]
---

# [Topic Name]

*This topic appears across several of your notes.*

## Related
- [[Note 1]]
- [[Note 2]]
- [[Note 3]]
```

### 6. Report what you did

> "Processed **X notes** from your inbox:
>
> | Note | Type | Connections |
> |------|------|-------------|
> | [[Note Title]] | idea | → [[Other Note]], [[Another]] |
> | [[Note Title]] | book | → [[Related Note]] |
> | ... | ... | ... |
>
> **New connections:** Y links created
> **Themes:** [list 2-3 themes]
>
> Open Obsidian's graph view to see the new connections."

Be encouraging. Point out interesting connections:

> "I noticed your note about [X] connects to something you wrote about [Y] — there might be a pattern forming there."
