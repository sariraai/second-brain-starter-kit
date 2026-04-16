---
name: setup
description: First-time setup wizard for the Second Brain Starter Kit. Walks a complete beginner through installing Obsidian and opening their vault.
trigger: /setup
---

# /setup — First-Time Setup

Walk the user through setting up their second brain. They may have never used Obsidian or a terminal before. Be patient, specific, and encouraging.

The repo root IS the vault. All folders (Inbox/, Notes/, Daily/, etc.) are at the top level.

## Steps

### 1. Create vault folders (safety net)

```bash
mkdir -p Inbox Notes Daily Projects Archive Templates
```

### 2. Auto-install community plugins

Download and install all 5 community plugins BEFORE the user opens Obsidian. This way they just click "Trust" once and everything's ready.

**Important:** Calendar MUST be pinned to release 1.5.10 — the "latest" release is a beta with plugin ID `calendar-beta` which doesn't match the `community-plugins.json` entry.

```bash
PLUGINS_DIR=".obsidian/plugins"

# Calendar — pinned to stable 1.5.10 (latest is beta with different plugin ID)
mkdir -p "$PLUGINS_DIR/calendar"
curl -sL "https://github.com/liamcain/obsidian-calendar-plugin/releases/download/1.5.10/main.js" -o "$PLUGINS_DIR/calendar/main.js"
curl -sL "https://github.com/liamcain/obsidian-calendar-plugin/releases/download/1.5.10/manifest.json" -o "$PLUGINS_DIR/calendar/manifest.json"
curl -sL "https://github.com/liamcain/obsidian-calendar-plugin/releases/download/1.5.10/styles.css" -o "$PLUGINS_DIR/calendar/styles.css"
echo "Installed: Calendar"

# Dataview
mkdir -p "$PLUGINS_DIR/dataview"
curl -sL "https://github.com/blacksmithgu/obsidian-dataview/releases/latest/download/main.js" -o "$PLUGINS_DIR/dataview/main.js"
curl -sL "https://github.com/blacksmithgu/obsidian-dataview/releases/latest/download/manifest.json" -o "$PLUGINS_DIR/dataview/manifest.json"
curl -sL "https://github.com/blacksmithgu/obsidian-dataview/releases/latest/download/styles.css" -o "$PLUGINS_DIR/dataview/styles.css"
echo "Installed: Dataview"

# Templater
mkdir -p "$PLUGINS_DIR/templater-obsidian"
curl -sL "https://github.com/SilentVoid13/Templater/releases/latest/download/main.js" -o "$PLUGINS_DIR/templater-obsidian/main.js"
curl -sL "https://github.com/SilentVoid13/Templater/releases/latest/download/manifest.json" -o "$PLUGINS_DIR/templater-obsidian/manifest.json"
curl -sL "https://github.com/SilentVoid13/Templater/releases/latest/download/styles.css" -o "$PLUGINS_DIR/templater-obsidian/styles.css"
echo "Installed: Templater"

# Importer
mkdir -p "$PLUGINS_DIR/obsidian-importer"
curl -sL "https://github.com/obsidianmd/obsidian-importer/releases/latest/download/main.js" -o "$PLUGINS_DIR/obsidian-importer/main.js"
curl -sL "https://github.com/obsidianmd/obsidian-importer/releases/latest/download/manifest.json" -o "$PLUGINS_DIR/obsidian-importer/manifest.json"
curl -sL "https://github.com/obsidianmd/obsidian-importer/releases/latest/download/styles.css" -o "$PLUGINS_DIR/obsidian-importer/styles.css"
echo "Installed: Importer"

# Terminal — run Claude Code inside Obsidian
mkdir -p "$PLUGINS_DIR/terminal"
curl -sL "https://github.com/polyipseity/obsidian-terminal/releases/latest/download/main.js" -o "$PLUGINS_DIR/terminal/main.js"
curl -sL "https://github.com/polyipseity/obsidian-terminal/releases/latest/download/manifest.json" -o "$PLUGINS_DIR/terminal/manifest.json"
curl -sL "https://github.com/polyipseity/obsidian-terminal/releases/latest/download/styles.css" -o "$PLUGINS_DIR/terminal/styles.css"
echo "Installed: Terminal"

echo "All 5 plugins installed."
```

Update the community-plugins.json to include all 5:

```bash
echo '["calendar", "dataview", "templater-obsidian", "obsidian-importer", "terminal"]' > .obsidian/community-plugins.json
```

Verify the downloads succeeded (check that main.js files are not empty):

```bash
for plugin in calendar dataview templater-obsidian obsidian-importer terminal; do
  size=$(wc -c < "$PLUGINS_DIR/$plugin/main.js" 2>/dev/null || echo "0")
  if [ "$size" -lt 100 ]; then
    echo "WARNING: $plugin may not have downloaded correctly"
  else
    echo "OK: $plugin ($size bytes)"
  fi
done
```

If any plugin failed to download, fall back to manual install instructions when the user opens Obsidian (step 5).

### 3. Install the Claude Code CLI

The Claude Code desktop app does NOT install the `claude` command-line tool. We need it so the user can type `claude` inside Obsidian's Terminal plugin. Install it now, before the user opens Obsidian, so everything just works when they get there.

```bash
if ! command -v claude &>/dev/null; then
  echo "Installing Claude Code CLI..."
  curl -fsSL https://claude.ai/install.sh | bash
fi
```

Tell the user while it runs:

> "I'm installing a small helper so you can talk to me from inside Obsidian. This takes about 30 seconds..."

After it completes, verify:

```bash
command -v claude &>/dev/null && echo "CLI ready" || echo "CLI_FAILED"
```

If it fails, don't block the flow — note it and come back to it at step 7:

> "The helper didn't install automatically — we'll sort that out in a minute. Let's keep going."

### 4. Check if Obsidian is installed

```bash
ls /Applications/Obsidian.app 2>/dev/null || ls "$HOME/AppData/Local/Obsidian" 2>/dev/null || echo "NOT_FOUND"
```

If not found, tell them:

> "Now we need Obsidian — it's the free app that displays your notes as a beautiful connected graph. Go to **obsidian.md** in your browser and download it. It's free. Install it like any other app, then come back and tell me when it's ready."

Wait for confirmation before continuing.

### 5. Open the vault in Obsidian

Tell the user:

> "Open Obsidian. When it asks what to do, click **'Open folder as vault'**. Navigate to this folder and select it:"

Print the exact absolute path:

```bash
pwd
```

> "Obsidian will show a message about community plugins and trusting the author. Click **'Trust author and enable plugins'**. That's it — I already downloaded and installed the plugins for you. You don't need to install anything manually."

If any plugins failed to download in step 2, walk the user through installing just those specific ones manually:

> "One plugin didn't download automatically. Let's install it manually. In Obsidian, go to **Settings** (gear icon, bottom-left) > **Community plugins** > **Browse**. Search for **[plugin name]**, click **Install**, then **Enable**."

### 6. Show them the graph

> "Click the **graph icon** in the left sidebar — it looks like a network of connected dots. That's your knowledge graph. Right now it's mostly empty. We're about to change that."

### 7. Switch to Claude Code inside Obsidian

The Terminal plugin and CLI are both installed from earlier steps. Now show them how to open it. **Important:** Use the sidebar icon, not the command palette.

> "Now let's get me running inside Obsidian so you don't have to switch between apps.
>
> 1. Look at the **left sidebar** in Obsidian. Find the **Terminal icon** — it looks like a small square with a `>_` inside it
> 2. Click it
> 3. It will ask which terminal to open — choose **Integrated**
> 4. A terminal panel will open at the bottom of Obsidian
> 5. Type `claude` and press Enter
>
> Claude Code will start up. Here's what to expect:
>
> - **It will ask you to log in.** A browser window will open — sign in with the same account you use for Claude. Once you log in, come back to Obsidian.
> - **It may ask you to accept terms.** Type `yes` and press Enter.
> - **It will ask you to choose a project folder.** Just press Enter to accept the current folder — it's already the right one.
> - Once it loads, you'll see a chat prompt. **That's me!** Try saying hello, or type `/morning` to start your first daily note.
>
> *(If you've already used Claude Code before and are already logged in, the login and terms steps will be skipped automatically.)*
>
> One more thing: you're switching from talking to me in the desktop app to talking to me here in Obsidian. It's the same me — I can read the same files and I know the same commands. You can close the desktop app now. From here on, just open Obsidian and I'm right there at the bottom.
>
> **Your vault is ready and waiting.** The first thing to do in Obsidian is bring in your existing notes — type `/import` and watch your knowledge graph come to life.
>
> If you're starting completely fresh with no existing notes, drop any thought into the Inbox folder and say `/process` to see how it works."

If `claude` returns "command not found" (the CLI install failed in step 3):

> "Looks like the Claude Code helper didn't install correctly earlier. Let's try again. In the terminal that just opened, paste this command:
>
> `curl -fsSL https://claude.ai/install.sh | bash`
>
> When it finishes, type `claude` and press Enter."

If they have trouble or prefer not to:

> "If the terminal isn't working, you can keep using the Claude Code desktop app — just make sure it's pointed at this folder. Everything works the same either way."

### 8. End of setup

This is the last step. Do NOT continue with steps 8-9 from the old flow (ask about existing notes, celebrate) — those would happen in the desktop app, but the user is about to switch to Obsidian. The "what to type next" menu above IS the celebration and the handoff. The new Obsidian session will greet them fresh (see CLAUDE.md "First Interaction" section).
