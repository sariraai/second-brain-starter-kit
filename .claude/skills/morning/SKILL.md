---
name: morning
description: Daily morning start — creates today's daily note, shows inbox status and recent activity.
trigger: /morning
---

# /morning — Start Your Day

Create today's daily note and give a brief morning overview.

## Steps

### 1. Get today's date

```bash
date "+%Y-%m-%d"
```

Also get the day of the week and a human-friendly date:

```bash
date "+%A, %B %d, %Y"
```

### 2. Create today's daily note (if it doesn't exist)

Check if `Daily/YYYY-MM-DD.md` exists. If not, create it using the template in `Templates/daily-note.md`. Replace `{{date}}` with today's date and `{{day}}` with the day of the week.

Write the file to `Daily/YYYY-MM-DD.md`.

### 3. Check the inbox

```bash
ls "Inbox/" 2>/dev/null | wc -l
```

### 4. Check recent notes

Look at the last 5 notes created or modified in `Notes/`:

```bash
ls -t "Notes/"*.md 2>/dev/null | head -5
```

### 5. Check for open tasks

Search recent daily notes and notes for unchecked tasks:

```bash
grep -r "\- \[ \]" "Daily/" "Notes/" 2>/dev/null | head -10
```

### 6. Roll forward incomplete tasks

Check yesterday's daily note for unchecked tasks (`- [ ]`). If found, list them in today's note under Tasks so they carry forward automatically. Don't nag — just move them.

### 7. Present the morning briefing

> "Good morning! It's **[Day, Date]**.
>
> **Inbox:** [N] items waiting to be processed. [Say `/process` to organize them, or leave them for later.]
>
> **Recent notes:** [List last 3-5 notes by title]
>
> **Open tasks:**
> - [ ] [task 1]
> - [ ] [task 2]
> - [ ] [carried from yesterday: task 3]
>
> Today's daily note is ready at `Daily/YYYY-MM-DD.md`.
>
> **What's your one thing today?** If you could only accomplish one thing, what would it be?"

If the inbox is empty and there are no tasks, keep it simple:

> "Good morning! It's **[Day, Date]**. Your inbox is clear and you have no open tasks. Today's note is ready. What's on your mind?"

### 8. Write the user's focus to the daily note

If they answer the "one thing" question, add it to the top of today's daily note under Intentions. This becomes reviewable in `/digest` and `/weekly`.
