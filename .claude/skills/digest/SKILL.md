---
name: digest
description: End-of-day summary — appends a digest to today's daily note showing what was captured and connected.
trigger: /digest
---

# /digest — End-of-Day Summary

Summarize what was captured and connected today. Append the summary to today's daily note.

## Steps

### 1. Get today's date

```bash
date "+%Y-%m-%d"
```

### 2. Check if today's daily note exists

Look for `vault-template/Daily/YYYY-MM-DD.md`. If it doesn't exist, create it from the template first (same as `/morning` step 2).

### 3. Find what was created or modified today

```bash
find "vault-template/Notes/" "vault-template/Projects/" -name "*.md" -newer "vault-template/Daily/$(date +%Y-%m-%d).md" 2>/dev/null
```

Or check file modification times for today's date. Read the frontmatter of recent files to find notes dated today.

### 4. Count connections

For notes created today, count how many `[[wiki-links]]` they contain:

```bash
grep -c "\[\[" "vault-template/Notes/"*.md 2>/dev/null | sort -t: -k2 -nr | head -10
```

### 5. Check inbox status

```bash
ls "vault-template/Inbox/" 2>/dev/null | wc -l
```

### 6. Build the digest

Append to today's daily note (`vault-template/Daily/YYYY-MM-DD.md`):

```markdown

---

## Daily Digest

**Notes created today:** X
**Connections made:** Y
**Inbox remaining:** Z items

### What was captured:
- [[Note Title 1]] — brief description
- [[Note Title 2]] — brief description

### Patterns noticed:
- [Any themes or connections that emerged today]

### Tomorrow:
- [Inbox items remaining, if any]
- [Any open tasks carried forward]
```

### 7. Check the morning's intention

Read today's daily note. Did the user set a "one thing" in the Intentions section (from `/morning`)? If so, ask:

> "This morning you said your one thing was: **[intention]**. Did you get to it?"

Don't judge either way — just capture the answer in the daily note.

### 8. Quick reflection prompts

Ask briefly (the user can skip any of these):

> "Two quick questions to close the day — skip any you want:
>
> 1. **What went well today?**
> 2. **What's your #1 priority tomorrow?**"

Append their answers to the daily note under a `## Reflection` section.

### 9. Present the digest to the user

> "Here's your day in notes:
>
> - **X notes** created or updated
> - **Y connections** made
> - **Z items** still in your inbox
>
> [Brief observation about themes or connections]
>
> I've added the full digest to today's daily note. See you tomorrow — say `/morning` to start fresh."

If nothing was captured today, keep it light:

> "Quiet day — no new notes captured. That's fine. The system is here when you need it. Say `/morning` tomorrow to start fresh."
