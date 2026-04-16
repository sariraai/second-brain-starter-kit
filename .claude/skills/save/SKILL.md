---
name: save
description: Save a conversation insight, analysis, or idea as a permanent note in the vault. Captures valuable thinking before it disappears.
trigger: /save
---

# /save — Save This to Your Vault

When a conversation produces something worth keeping — an insight, an analysis, a decision, a plan — save it as a permanent note in the vault. Conversations disappear; vault notes compound.

## When to use

- The user says "save this" or "remember this" or "that's worth keeping"
- An analysis or synthesis was produced that the user might want to reference later
- A decision was made that should be documented
- A plan or framework was created

## Steps

### 1. Identify what to save

Ask the user if it's not obvious:

> "What would you like me to save? I can capture:
> - The last analysis/insight I shared
> - A specific idea or decision from our conversation
> - A summary of what we discussed
>
> Or just tell me what to save in your own words."

### 2. Determine the note type and title

Based on the content:

| Content | Type | Example title |
|---------|------|---------------|
| An idea or insight | `idea` | "Ideas for Morning Routine Redesign" |
| A decision made | `note` | "Decision: Switch to Weekly Batch Cooking" |
| An analysis | `note` | "Analysis: Sleep Quality Factors" |
| A plan or framework | `project` | "Plan: Learn Spanish in 6 Months" |
| A useful reference | `note` | "How Compound Interest Works" |
| A reflection | `note` | "Reflection: What I Learned From the Project" |

### 3. Create the note

Write to `Notes/` (or `Projects/` for plans):

```markdown
---
title: [Title]
type: [type]
date: [today's date]
tags: [relevant tags]
source: conversation
---

# [Title]

[Content — structured cleanly, preserving the substance of what was discussed]

## Context
*Saved from a conversation on [date]. [Brief note about what prompted this.]*

## Related
- [[any relevant existing notes]]
```

**Important:** Rewrite for clarity as a standalone note. A conversation excerpt won't make sense in 6 months. The note should read as if someone wrote it fresh, not as a chat transcript.

### 4. Find connections

Check existing notes in the vault for related content. Add wiki-links in the Related section.

### 5. Confirm

> "Saved as **[[Note Title]]** in your Notes folder. Connected to [[Related Note 1]] and [[Related Note 2]].
>
> This will show up in your graph and in future `/weekly` reviews."
