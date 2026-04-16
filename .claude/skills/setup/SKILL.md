---
name: setup
description: First-time setup wizard for the Second Brain Starter Kit. Walks a complete beginner through installing Obsidian and opening their vault.
trigger: /setup
---

# /setup — First-Time Setup

Walk the user through setting up their second brain. They may have never used Obsidian or a terminal before. Be patient, specific, and encouraging.

## Steps

### 1. Check if Obsidian is installed

```bash
ls /Applications/Obsidian.app 2>/dev/null || ls "$HOME/AppData/Local/Obsidian" 2>/dev/null
```

If not found, tell them:

> "First, we need Obsidian — it's the app that displays your notes as a beautiful connected graph. Go to **obsidian.md** in your browser and download it. It's free. Install it like any other app, then come back and tell me when it's ready."

Wait for confirmation before continuing.

### 2. Open the vault

Tell the user:

> "Great! Now open Obsidian. When it asks what to do, click **'Open folder as vault'**. Navigate to the `vault-template` folder inside this starter kit and select it."

Give them the exact path based on where the project is located. Use the Bash tool to find the absolute path:

```bash
echo "$(pwd)/vault-template"
```

### 3. Enable community plugins

> "Obsidian might show a message about community plugins. Click **'Trust author and enable plugins'**. This turns on the starter plugins that make the system work."

Now walk them through installing the essential plugins. Go to **Settings** (gear icon, bottom-left) > **Community plugins** > **Browse**:

1. **Calendar** — search "Calendar" by Liam Cain. Install and enable. Shows a calendar in the sidebar for navigating daily notes.
2. **Dataview** — search "Dataview" by Michael Brenan. Install and enable. Lets the system query your notes.
3. **Templater** — search "Templater" by SilentVoid. Install and enable. Powers the note templates.
4. **Terminal** — search "Terminal" by Polyipseity. Install and enable. This lets you run Claude Code directly inside Obsidian — no separate terminal window needed.
5. **Importer** — search "Importer" by Obsidian. Install and enable. Converts notes from Evernote, Notion, Bear, and other apps into Obsidian format.

Give click-by-click instructions for each. After all 5 are installed:

> "All plugins are set up. The **Terminal** plugin is especially useful — it lets you talk to me (Claude Code) without leaving Obsidian. To open it, use the command palette (Cmd+P on Mac, Ctrl+P on Windows) and type 'Terminal'. You can also find it in the ribbon on the left."

### 4. Show them the graph

> "Click the **graph icon** in the left sidebar — it looks like a network of connected dots. That's your knowledge graph. Right now it's mostly empty. We're about to change that."

### 5. Switch to running Claude Code inside Obsidian

Now walk the user through opening Claude Code in the Terminal plugin so they can work from inside Obsidian going forward:

> "Now let's set you up to talk to me from inside Obsidian — that way you never have to switch between apps.
>
> 1. In Obsidian, press **Cmd+P** (Mac) or **Ctrl+P** (Windows) to open the command palette
> 2. Type **'Terminal: Open'** and press Enter
> 3. A terminal panel will open at the bottom of Obsidian
> 4. Type `claude` and press Enter
>
> You're now running Claude Code inside Obsidian. From here on, you can talk to me right alongside your notes. You can close the Claude Code desktop app — you won't need it anymore."

If the Terminal plugin isn't working or the user has trouble, give them the fallback:

> "If the terminal isn't working, you can always use the Claude Code desktop app pointing at the `vault-template` folder. The Terminal plugin is convenient but not required — everything works either way."

### 6. Ask about existing notes

> "Do you have notes from somewhere else you'd like to bring in? Apple Notes, Notion, Evernote, Google Docs, or just a folder of files on your computer? If yes, say `/import` and I'll walk you through it. If you're starting fresh, just drop any thought into the `Inbox` folder and say `/process` when you're ready."

### 7. Celebrate

> "You're set up! Your second brain is ready. Here's all you need to remember:
>
> - **Drop anything** into the Inbox folder — ideas, notes, links, whatever
> - **Say `/process`** and I'll organize it and create connections
> - **Say `/morning`** to start your day or `/digest` to end it
>
> That's the whole system. Capture and process. I handle the rest."
