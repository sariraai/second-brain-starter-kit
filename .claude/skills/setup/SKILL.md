---
name: setup
description: First-time setup wizard for the Second Brain Starter Kit. Walks a complete beginner through installing Obsidian and opening their vault.
trigger: /setup
---

# /setup — First-Time Setup

Walk the user through setting up their second brain. They may have never used Obsidian or a terminal before. Be patient, specific, and encouraging.

## Steps

### 1. Detect the vault path

```bash
if [ -d "vault-template" ]; then
  VAULT="vault-template"
elif [ -d "Inbox" ] && [ -d "Notes" ]; then
  VAULT="."
else
  VAULT="vault-template"
fi
echo "Vault path: $VAULT"
```

### 2. Create vault folders (safety net)

```bash
mkdir -p "$VAULT"/{Inbox,Notes,Daily,Projects,Archive,Templates}
```

### 3. Auto-install community plugins

Download and install the 4 required plugins BEFORE the user opens Obsidian. This way they just click "Trust" once and everything's ready.

```bash
PLUGINS_DIR="$VAULT/.obsidian/plugins"

# Calendar — daily notes sidebar
mkdir -p "$PLUGINS_DIR/calendar"
curl -sL "https://github.com/liamcain/obsidian-calendar-plugin/releases/latest/download/main.js" \
  -o "$PLUGINS_DIR/calendar/main.js"
curl -sL "https://github.com/liamcain/obsidian-calendar-plugin/releases/latest/download/manifest.json" \
  -o "$PLUGINS_DIR/calendar/manifest.json"
curl -sL "https://github.com/liamcain/obsidian-calendar-plugin/releases/latest/download/styles.css" \
  -o "$PLUGINS_DIR/calendar/styles.css"
echo "Installed: Calendar"

# Dataview — note queries
mkdir -p "$PLUGINS_DIR/dataview"
curl -sL "https://github.com/blacksmithgu/obsidian-dataview/releases/latest/download/main.js" \
  -o "$PLUGINS_DIR/dataview/main.js"
curl -sL "https://github.com/blacksmithgu/obsidian-dataview/releases/latest/download/manifest.json" \
  -o "$PLUGINS_DIR/dataview/manifest.json"
curl -sL "https://github.com/blacksmithgu/obsidian-dataview/releases/latest/download/styles.css" \
  -o "$PLUGINS_DIR/dataview/styles.css"
echo "Installed: Dataview"

# Templater — note templates
mkdir -p "$PLUGINS_DIR/templater-obsidian"
curl -sL "https://github.com/SilentVoid13/Templater/releases/latest/download/main.js" \
  -o "$PLUGINS_DIR/templater-obsidian/main.js"
curl -sL "https://github.com/SilentVoid13/Templater/releases/latest/download/manifest.json" \
  -o "$PLUGINS_DIR/templater-obsidian/manifest.json"
curl -sL "https://github.com/SilentVoid13/Templater/releases/latest/download/styles.css" \
  -o "$PLUGINS_DIR/templater-obsidian/styles.css"
echo "Installed: Templater"

# Importer — import from other apps
mkdir -p "$PLUGINS_DIR/obsidian-importer"
curl -sL "https://github.com/obsidianmd/obsidian-importer/releases/latest/download/main.js" \
  -o "$PLUGINS_DIR/obsidian-importer/main.js"
curl -sL "https://github.com/obsidianmd/obsidian-importer/releases/latest/download/manifest.json" \
  -o "$PLUGINS_DIR/obsidian-importer/manifest.json"
curl -sL "https://github.com/obsidianmd/obsidian-importer/releases/latest/download/styles.css" \
  -o "$PLUGINS_DIR/obsidian-importer/styles.css"
echo "Installed: Importer"

echo "All 4 plugins installed."
```

Update the community-plugins.json to include all 4:

```bash
echo '["calendar", "dataview", "templater-obsidian", "obsidian-importer"]' > "$VAULT/.obsidian/community-plugins.json"
```

Verify the downloads succeeded (check that main.js files are not empty):

```bash
for plugin in calendar dataview templater-obsidian obsidian-importer; do
  size=$(wc -c < "$PLUGINS_DIR/$plugin/main.js" 2>/dev/null || echo "0")
  if [ "$size" -lt 100 ]; then
    echo "WARNING: $plugin may not have downloaded correctly"
  else
    echo "OK: $plugin ($size bytes)"
  fi
done
```

If any plugin failed to download, fall back to manual install instructions for that specific plugin (see step 5 below).

### 4. Check if Obsidian is installed

```bash
ls /Applications/Obsidian.app 2>/dev/null || ls "$HOME/AppData/Local/Obsidian" 2>/dev/null || echo "NOT_FOUND"
```

If not found, tell them:

> "Now we need Obsidian — it's the free app that displays your notes as a beautiful connected graph. Go to **obsidian.md** in your browser and download it. Install it like any other app, then come back and tell me when it's ready."

Wait for confirmation before continuing.

### 5. Open the vault in Obsidian

Tell the user:

> "Open Obsidian. When it asks what to do, click **'Open folder as vault'**. Navigate to this folder and select it:"

Print the exact absolute path:

```bash
echo "$(cd "$VAULT" && pwd)"
```

> "Obsidian will show a message about community plugins and trusting the author. Click **'Trust author and enable plugins'**. That's it — I already downloaded and installed the plugins for you. You don't need to install anything manually."

If any plugins failed to download in step 3, walk the user through installing just those specific ones manually:

> "One plugin didn't download automatically. Let's install it manually. In Obsidian, go to **Settings** (gear icon, bottom-left) > **Community plugins** > **Browse**. Search for **[plugin name]**, click **Install**, then **Enable**."

### 6. Show them the graph

> "Click the **graph icon** in the left sidebar — it looks like a network of connected dots. That's your knowledge graph. Right now it's mostly empty. We're about to change that."

### 7. (Optional) Run Claude Code inside Obsidian

> "Want to talk to me from inside Obsidian so you don't have to switch apps? There's a plugin called **Terminal** that lets you do this.
>
> 1. In Obsidian, go to **Settings** > **Community plugins** > **Browse**
> 2. Search for **Terminal** by Polyipseity
> 3. Click **Install**, then **Enable**
> 4. Close Settings
> 5. Press **Cmd+P** (Mac) or **Ctrl+P** (Windows), type **'Terminal: Open'**, press Enter
> 6. In the terminal that opens, type `claude` and press Enter
>
> You're now running Claude Code inside Obsidian — notes on top, me at the bottom."

If they don't want to or it doesn't work:

> "No worries — you can keep using the Claude Code desktop app. Just make sure it's pointed at the vault folder. Everything works the same either way."

### 8. Ask about existing notes

> "Do you have notes from somewhere else you'd like to bring in? Apple Notes, Notion, Evernote, Google Docs, or just a folder of files on your computer? If yes, say `/import` and I'll walk you through it. If you're starting fresh, just drop any thought into the `Inbox` folder and say `/process` when you're ready."

### 9. Celebrate

> "You're set up! Your second brain is ready. Here's all you need to remember:
>
> - **Drop anything** into the Inbox folder — ideas, notes, links, whatever
> - **Say `/process`** and I'll organize it and create connections
> - **Say `/morning`** to start your day or `/digest` to end it
>
> That's the whole system. Capture and process. I handle the rest."
