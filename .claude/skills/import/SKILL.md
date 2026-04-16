---
name: import
description: Import notes from other apps (Apple Notes, Notion, Evernote, Google Docs, or a folder of files) into the second brain vault.
trigger: /import
---

# /import — Bring In Your Notes

Help the user import existing notes into their vault. Ask where their notes are, give export instructions, then process the imported files.

## Step 1: Ask where their notes are

> "Where are your notes right now? Pick one:
>
> 1. **Apple Notes**
> 2. **Notion**
> 3. **Evernote**
> 4. **Google Docs**
> 5. **A folder of files** on my computer (.md, .txt, .docx, etc.)
> 6. **I'm starting fresh** — no existing notes"

## Step 2: Give export instructions

Based on their answer, guide them through the right import path. There are two tools working together:

- **Obsidian Importer plugin** — handles format conversion (binary formats like .enex, Notion databases, Bear exports). It turns foreign formats into markdown files Obsidian can read.
- **Claude Code** (/process) — handles structuring (adds frontmatter, creates wiki-links, finds connections). It turns plain markdown into connected knowledge.

### Import paths by source:

**Apple Notes:**
- Obsidian Importer supports Apple Notes directly. Walk the user through: Settings > Community plugins > Importer > Open Importer > select "Apple Notes" > choose notes > import to Inbox/.
- Then run `/process` to add structure and connections.

**Notion:**
- Two options. For simple pages: export as Markdown & CSV, put in Inbox.
- For databases and complex pages: use Obsidian Importer (Settings > Importer > select "Notion" > choose export file).
- Then run `/process`.

**Evernote:**
- Export from Evernote as .enex (File > Export Notes).
- Use Obsidian Importer (Settings > Importer > select "Evernote" > choose .enex file > import to Inbox/).
- Then run `/process`.

**Google Docs:**
- Google Takeout (takeout.google.com) to download as .docx files.
- Put them in the Inbox/ folder.
- Claude Code can read .docx — `/process` handles the rest.

**Bear, Day One, other apps:**
- Obsidian Importer supports many formats. Check Settings > Importer for the full list.
- Import to Inbox/, then `/process`.

**Folder of .md or .txt files:**
- Just copy/move them into `Inbox/`. No conversion needed.
- Run `/process`.

**Starting fresh:**
- No import needed. Just start capturing to Inbox/ and run `/process` when ready.

> "Once your notes land in the Inbox folder, tell me and I'll start processing them — adding structure, tags, and connections between related ideas."

## Step 3: Process the imported files

Once files are in Inbox/, scan and process them:

1. **List everything in Inbox/**

```bash
ls "Inbox/"
```

2. **Read each file** using the Read tool

3. **For each file, add structure:**
   - Add YAML frontmatter: `title`, `type`, `date`, `tags`
   - Determine the type: `note`, `book`, `meeting`, `idea`, `project`
   - Identify key concepts and topics for tags
   - Clean up formatting (remove app-specific junk, fix line breaks)
   - **Preserve the user's original words** — never rewrite their content

4. **Create connections across the batch:**
   - After reading all files, identify themes that appear in multiple notes
   - Add `[[wiki-links]]` connecting related notes
   - Add a `## Related` section at the bottom of each note with links to other notes
   - If a concept appears in 3+ notes, consider creating a dedicated topic note for it

5. **Rename files** to clean kebab-case: `book-notes-atomic-habits.md`, `meeting-q2-planning.md`

6. **Move processed files** from `Inbox/` to `Notes/` (or `Projects/` if project-related)

## Step 4: Batch handling for large imports

If there are more than 50 files:

- Process in batches of 50
- After each batch, report progress:
  > "Processed 50 of 312 notes. Found 23 connections so far. Themes emerging: productivity, health, project management. Continuing..."
- After all batches, give a final summary

## Step 5: Summary

When done, report:

> "Import complete! Here's what I found:
>
> - **X notes** processed and organized
> - **Y connections** created between related notes
> - **Top themes:** [list 3-5 themes]
>
> Open Obsidian and click the graph icon in the sidebar — you should see your notes connected in a web now.
>
> From here, just keep dropping new notes into Inbox/ and say `/process` whenever you want me to organize them."
