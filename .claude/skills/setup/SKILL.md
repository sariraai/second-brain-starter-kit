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

### 3. Create vault folders (safety net)

Before anything else, make sure all the vault folders exist:

```bash
VAULT="vault-template"
if [ -d "Inbox" ] && [ -d "Notes" ]; then VAULT="."; fi
mkdir -p "$VAULT"/{Inbox,Notes,Daily,Projects,Archive,Templates}
```

### 4. Enable community plugins

> "Obsidian might show a message about community plugins. Click **'Trust author and enable plugins'**."

Now walk them through installing plugins one at a time. Go to **Settings** (gear icon, bottom-left) > **Community plugins** > **Browse**:

**Required plugins (install these first):**

1. **Calendar** — search "Calendar" by Liam Cain. Click Install, then Enable. This shows a calendar in the sidebar for navigating daily notes.
2. **Dataview** — search "Dataview" by Michael Brenan. Click Install, then Enable. This lets the system query your notes.
3. **Templater** — search "Templater" by SilentVoid. Click Install, then Enable. This powers the note templates.
4. **Importer** — search "Importer" by Obsidian. Click Install, then Enable. This converts notes from Evernote, Notion, Bear, and other apps into Obsidian format.

**Optional but recommended:**

5. **Terminal** — search "Terminal" by Polyipseity. Click Install, then Enable. This lets you run Claude Code directly inside Obsidian so you don't have to switch apps. *This is optional — you can always use the Claude Code desktop app instead.*

> "All plugins are set up! You installed [4 or 5] plugins. Let's move on."

### 4. Show them the graph

> "Click the **graph icon** in the left sidebar — it looks like a network of connected dots. That's your knowledge graph. Right now it's mostly empty. We're about to change that."

### 5. (Optional) Switch to running Claude Code inside Obsidian

If the user installed the Terminal plugin, offer this option:

> "Want to run Claude Code inside Obsidian so you don't have to switch between apps? Here's how:
>
> 1. In Obsidian, press **Cmd+P** (Mac) or **Ctrl+P** (Windows) to open the command palette
> 2. Type **'Terminal: Open'** and press Enter
> 3. A terminal panel will open at the bottom of Obsidian
> 4. Type `claude` and press Enter
>
> You're now running Claude Code inside Obsidian — notes on top, me at the bottom."

If they didn't install Terminal, or if it doesn't work, reassure them:

> "You can keep using the Claude Code desktop app — just make sure it's pointed at the `vault-template` folder. Everything works the same either way. The Terminal plugin is a convenience, not a requirement."

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
