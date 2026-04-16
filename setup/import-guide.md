# Importing Notes from Other Apps

Already have notes somewhere? Here's how to get them into your second brain.

## From Apple Notes

Apple Notes doesn't have a great bulk export. Your options:

**Option A (fastest for a few notes):** Open each note in Apple Notes, select all (Cmd+A), copy (Cmd+C), then create a new file in the Inbox folder and paste.

**Option B (for many notes):** Use an app like [Exporter](https://apps.apple.com/app/exporter/id1099120373) (free) to export your Apple Notes as markdown files. Then move those files into the Inbox folder.

## From Notion

1. Open Notion
2. Click **Settings & Members** in the left sidebar
3. Click **Settings** at the top
4. Scroll down to **Export all workspace content**
5. Choose **Markdown & CSV** format
6. Click **Export**
7. Unzip the downloaded file
8. Move the markdown (.md) files into your Inbox folder

Note: Notion exports can be messy — lots of extra folders and IDs in filenames. Don't worry about that; Claude Code will clean everything up when you run `/process`.

## From Evernote

1. Open Evernote
2. Select the notes you want to export (or Cmd+A for all)
3. Go to **File > Export Notes**
4. Choose **ENEX format**
5. Save the file
6. Open Obsidian, go to **Settings > Community Plugins > Browse**
7. Search for "Importer" and install it
8. Go to **Settings > Importer** and select your .enex file
9. Import into the Inbox folder

## From Google Docs

1. Go to [takeout.google.com](https://takeout.google.com)
2. Click **Deselect all**
3. Scroll down and check **Google Docs** (or Drive if you want everything)
4. Click **Next step** and **Create export**
5. Download the zip file when ready
6. Unzip it — your docs will be in .docx format
7. Move them into the Inbox folder

Claude Code can read .docx files, so no need to convert them first.

## From a Folder of Files

Already have notes as .md, .txt, or other text files on your computer?

Just copy or move them into the Inbox folder. That's it.

## Starting Fresh

No existing notes? No problem. Just start using the system:

1. Say `/morning` to create today's daily note
2. Throughout the day, create notes in the Inbox folder for any thoughts or ideas
3. At the end of the day, say `/process` to organize everything

The system grows with you. Even one note per day adds up to 365 notes in a year — that's a meaningful knowledge base.

## After Importing

Once your files are in the Inbox folder, go back to Claude Code and say:

> **"/import"**

Claude Code will read everything, add structure, create connections, and organize your notes automatically.
