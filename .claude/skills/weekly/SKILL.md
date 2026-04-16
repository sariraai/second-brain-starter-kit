---
name: weekly
description: Weekly review — summarize the past 7 days of notes, connections, patterns, and growth.
trigger: /weekly
---

# /weekly — Weekly Review

Summarize the past week. Show what grew, what connected, and what patterns are emerging. Create a weekly summary note.

## Steps

### 1. Get the date range

```bash
date "+%Y-%m-%d"
date -v-7d "+%Y-%m-%d" 2>/dev/null || date -d "7 days ago" "+%Y-%m-%d"
```

### 2. Gather the week's activity

**Notes created this week:**

Find all notes in `vault-template/Notes/` and `vault-template/Projects/` with dates in the past 7 days. Check frontmatter `date:` fields.

```bash
find "vault-template/Notes/" "vault-template/Projects/" -name "*.md" -mtime -7 2>/dev/null
```

**Daily notes from this week:**

Read the 7 daily notes from `vault-template/Daily/` for this week (some may not exist if the user skipped days — that's fine).

### 3. Analyze the week

For each note created this week, read it and track:
- **Topics and tags** — what subjects came up most?
- **Connections** — which notes link to each other?
- **Types** — how many ideas vs. meetings vs. book notes vs. reflections?

### 4. Find orphan notes

Look for notes in `vault-template/Notes/` that have NO wiki-links (no `[[` in their content) and are not linked FROM any other note:

```bash
grep -rL "\[\[" "vault-template/Notes/"*.md 2>/dev/null
```

These are orphan notes — isolated dots in the graph. Suggest connections for the top 3-5 orphans.

### 5. Calculate growth stats

- Total notes in `vault-template/Notes/`
- Total notes in `vault-template/Projects/`
- Number of daily notes in `vault-template/Daily/`
- Approximate total wiki-links across all notes

```bash
find "vault-template/Notes/" "vault-template/Projects/" -name "*.md" | wc -l
grep -r "\[\[" "vault-template/Notes/" "vault-template/Projects/" 2>/dev/null | wc -l
```

### 6. Create the weekly summary note

Write to `vault-template/Daily/weekly-YYYY-MM-DD.md`:

```markdown
---
title: "Week of [Start Date] — [End Date]"
type: weekly
date: YYYY-MM-DD
---

# Week of [Start Date] — [End Date]

## This Week in Numbers
- **Notes created:** X
- **Connections made:** Y
- **Days captured:** Z of 7
- **Total vault size:** N notes

## What You Captured
- [[Note 1]] — brief description
- [[Note 2]] — brief description
- ...

## Top Themes
1. **[Theme]** — appeared in X notes
2. **[Theme]** — appeared in Y notes
3. **[Theme]** — appeared in Z notes

## Connections That Formed
- [[Note A]] ↔ [[Note B]] — [why they connect]
- [[Note C]] ↔ [[Note D]] — [why they connect]

## Orphan Notes (could use connections)
- [[Orphan 1]] — might connect to [[Suggestion]]
- [[Orphan 2]] — might connect to [[Suggestion]]

## Patterns
[1-2 sentences about what patterns or themes are emerging across the vault as a whole, not just this week.]
```

### 7. Present the review

> "Here's your week:
>
> **[X] notes** captured across **[Z] days**. **[Y] connections** made.
>
> **Top themes:** [themes]
>
> **Interesting pattern:** [observation about what's emerging]
>
> **[N] orphan notes** could use connections — want me to suggest some links?
>
> Your vault now has **[total] notes**. Keep capturing — the graph gets smarter every week."

Be encouraging about growth, even if it's small:

> "Even 3 notes a week is 150 a year. You're building something."
