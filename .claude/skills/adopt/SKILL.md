---
name: adopt
description: Adopt an existing Obsidian vault — scan its structure, detect its methodology, and wire up the Second Brain skills to work with what's already there.
trigger: /adopt
---

# /adopt — Adopt an Existing Vault

For people who already have an Obsidian vault. Scan what they've built, understand their structure, and adapt the Second Brain skills to work with it — without breaking anything they already have.

**Critical rule: NEVER move, rename, or delete existing files.** This skill only ADDS structure. The user's vault is sacred — treat it that way.

## Steps

### 1. Ask for the vault location

> "You already have an Obsidian vault — great! Where is it on your computer? Give me the folder path, or just tell me roughly where it is (like 'on my Desktop' or 'in Documents') and I'll help find it."

If they're not sure, search common locations:

```bash
find ~/Documents ~/Desktop ~ -maxdepth 3 -name ".obsidian" -type d 2>/dev/null
```

Each result's parent folder is a vault.

### 2. Scan the vault structure

Once you have the path, map what's there:

```bash
# Top-level folders
ls -d "$VAULT_PATH"/*/

# Total file counts
echo -n "Total .md files: "; find "$VAULT_PATH" -name "*.md" | wc -l
echo -n "Total folders: "; find "$VAULT_PATH" -type d | wc -l

# Check for common PKM structures
ls "$VAULT_PATH"/{Inbox,inbox,0-Inbox,"00 Inbox","00-Inbox"} 2>/dev/null
ls "$VAULT_PATH"/{Daily,daily,"Daily Notes","daily notes",Journal,journal,journals} 2>/dev/null
ls "$VAULT_PATH"/{Projects,projects,"01-Projects","1-Projects"} 2>/dev/null
ls "$VAULT_PATH"/{Archive,archive,"04-Archive","4-Archive"} 2>/dev/null
ls "$VAULT_PATH"/{Templates,templates} 2>/dev/null
ls "$VAULT_PATH"/{Resources,Atlas,Sources,References,Areas,Zettelkasten} 2>/dev/null
```

### 3. Detect the methodology

Based on folder names and structure, identify what system they're using (or trying to use):

| Pattern | Methodology |
|---------|------------|
| Inbox, Projects, Areas, Resources, Archive | **PARA** (Tiago Forte) |
| Atlas, Calendar, Cards, Sources, Spaces | **LYT / ACCESS** (Nick Milo) |
| Zettelkasten, Permanent, Literature, Fleeting | **Zettelkasten** (Ahrens/Luhmann) |
| Numbered folders (00-, 01-, 02-...) | **Johnny Decimal** |
| Daily Notes / Journal + loose files | **Casual / organic** |
| No clear structure | **Unstructured** |

Tell the user what you found:

> "Your vault looks like it's based on [PARA / LYT / Zettelkasten / a custom structure]. You have X notes across Y folders. Here's what I see: [brief description]."

### 4. Assess vault health

Run a quick health scan (similar to `/health`):

```bash
# Notes with frontmatter
grep -rl "^---" "$VAULT_PATH"/*.md "$VAULT_PATH"/**/*.md 2>/dev/null | wc -l

# Notes with wiki-links
grep -rl "\[\[" "$VAULT_PATH"/*.md "$VAULT_PATH"/**/*.md 2>/dev/null | wc -l

# Orphan notes (no wiki-links)
grep -rL "\[\[" "$VAULT_PATH"/*.md "$VAULT_PATH"/**/*.md 2>/dev/null | wc -l
```

Report:

> "Quick scan: X of Y notes have frontmatter. Z have wiki-links. W are orphans (no connections). Your graph connectivity is [strong / moderate / sparse]."

### 5. Map their folders to our skills

Create a mapping between their existing structure and our skills:

```
Their folder          → Our skill
─────────────────────────────────
[Inbox folder]        → /process reads from here
[Daily folder]        → /morning creates notes here
[Notes/main folder]   → /process moves organized notes here
[Projects folder]     → /process moves project notes here
[Archive folder]      → completed projects go here
[Templates folder]    → /morning uses daily template from here
```

If they have folders we don't map to, leave them alone:

> "I'll leave your [Resources / Sources / Areas] folder untouched — those are yours to manage. I'll focus on your inbox and daily notes."

### 6. Create an adapted CLAUDE.md

Write a new `CLAUDE.md` in their vault root (or update the existing one if they have one). This CLAUDE.md is customized to their vault:

```markdown
# Second Brain — [Vault Name]

## Vault Structure

This vault uses [detected methodology]. The skills are adapted to work with your existing folders:

- **Inbox:** `[their inbox path]` — `/process` reads from here
- **Notes:** `[their notes path]` — processed notes go here
- **Daily:** `[their daily path]` — `/morning` creates daily notes here
- **Projects:** `[their projects path]` — project notes go here
- **Archive:** `[their archive path]` — completed work goes here
- **Templates:** `[their templates path]` — note templates live here

[Include the rest of the standard CLAUDE.md content, adapted to their paths]
```

### 7. Install the skills

Copy the skill files from the starter kit into their vault's `.claude/skills/` directory:

```bash
mkdir -p "$VAULT_PATH/.claude/skills"
```

Copy each skill, updating folder paths in the SKILL.md files to match their vault structure. For example, if their inbox is at `00-Inbox/` instead of `Inbox/`, update all references.

### 8. Handle edge cases

**No inbox folder:**
> "You don't have an Inbox folder. Want me to create one? It's where you'll drop quick captures for me to process."

**No daily notes folder:**
> "I don't see a daily notes folder. Want me to create a `Daily/` folder and set up daily notes? You can use `/morning` to start each day."

**No templates:**
> "You don't have a Templates folder. I'll create one with a daily note template and a general note template."

**Thousands of notes with no frontmatter:**
> "You have X notes without frontmatter (the metadata header). I can add it gradually — want me to start with your most recent 50 notes? I won't change your content, just add structure on top."

**Existing CLAUDE.md:**
> "You already have a CLAUDE.md. I'll add the Second Brain skills alongside your existing instructions rather than replacing them. Want me to show you the merged version before saving?"

### 9. Test the integration

Run a quick test:
- Try `/morning` — does it create a daily note in the right place?
- Check if `/process` can find their inbox
- Verify `/health` scans the right folders

### 10. Present the adoption report

> "Your vault is adopted! Here's what I set up:
>
> **Vault:** [name] ([X] notes, [Y] folders)
> **Methodology detected:** [PARA / LYT / etc.]
> **Skills installed:** /setup, /import, /morning, /process, /digest, /weekly, /health
>
> **Folder mapping:**
> - Inbox: `[path]`
> - Notes: `[path]`
> - Daily: `[path]`
> - Projects: `[path]`
>
> **Quick health:** [X] orphan notes, [Y]% have frontmatter, [Z] total connections
>
> **What's next:**
> - Say `/morning` to start using daily notes
> - Say `/process` to organize anything in your inbox
> - Say `/health` to get a full vault health report
> - Drop new captures into `[inbox path]` anytime
>
> I didn't move or rename anything in your existing vault. Everything is where you left it — I just added the tools to help you keep building."

### Important: What /adopt NEVER does

- Never moves existing files
- Never renames existing files
- Never deletes anything
- Never changes existing note content
- Never overwrites an existing CLAUDE.md without showing the user first
- Never assumes a folder's purpose without confirming

The user's vault is their work. /adopt adds to it. It never subtracts.
