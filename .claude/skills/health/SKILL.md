---
name: health
description: Vault health check — finds orphan notes, dead links, missing frontmatter, duplicate topics, and reports growth stats. Keeps the vault alive.
trigger: /health
---

# /health — Vault Health Check

Scan the entire vault for problems and opportunities. Report what's healthy, what needs attention, and how the vault is growing. This skill is what keeps the system alive after the initial setup.

## Steps

### 1. Gather vault stats

Count total files in each folder:

```bash
echo "=== Vault Stats ==="
echo -n "Notes: "; find "vault-template/Notes/" -name "*.md" 2>/dev/null | wc -l
echo -n "Daily: "; find "vault-template/Daily/" -name "*.md" 2>/dev/null | wc -l
echo -n "Projects: "; find "vault-template/Projects/" -name "*.md" 2>/dev/null | wc -l
echo -n "Archive: "; find "vault-template/Archive/" -name "*.md" 2>/dev/null | wc -l
echo -n "Inbox: "; find "vault-template/Inbox/" -type f 2>/dev/null | wc -l
echo -n "Total wiki-links: "; grep -r "\[\[" "vault-template/Notes/" "vault-template/Projects/" "vault-template/Daily/" 2>/dev/null | wc -l
```

### 2. Find orphan notes (no outgoing links)

Notes that contain zero `[[wiki-links]]` — isolated dots in the graph:

```bash
grep -rL "\[\[" "vault-template/Notes/"*.md 2>/dev/null
```

For each orphan, read the note and suggest 2-3 connections it could have to existing notes. Don't just list them — actually suggest the link:

> "**Orphan: [[Recipe Mom's Pasta Sauce]]** — This could connect to [[Gift Ideas]] (you mentioned cooking for people) and [[Trip Portland]] (you mentioned food there too)."

### 3. Find dead links (links pointing to notes that don't exist)

Search for `[[wiki-links]]` across all notes, then check if the target file exists:

```bash
grep -roh "\[\[[^]]*\]\]" "vault-template/Notes/" "vault-template/Projects/" "vault-template/Daily/" 2>/dev/null | sort -u
```

For each unique link found, check if a matching .md file exists anywhere in the vault. Report any dead links:

> "**Dead link: [[Productivity System]]** — referenced in 3 notes but the note doesn't exist. Want me to create it?"

### 4. Find notes missing frontmatter

Check for .md files that don't start with `---`:

```bash
for f in "vault-template/Notes/"*.md; do
  head -1 "$f" 2>/dev/null | grep -q "^---" || echo "$f"
done
```

Offer to add frontmatter to any notes missing it.

### 5. Find duplicate or near-duplicate topics

Read all note titles and tags. Look for notes that might be about the same thing but aren't linked:

- Same tags but no link between them
- Similar titles (e.g., "workout notes" and "exercise log")
- Same person/concept mentioned in content but not linked

Suggest merges or links.

### 6. Check tag consistency

List all tags used across the vault:

```bash
grep -roh "tags: \[.*\]" "vault-template/Notes/" "vault-template/Projects/" 2>/dev/null
```

Look for:
- Inconsistent naming (e.g., `health` vs `wellness` vs `fitness`)
- Tags used only once (might be a typo or could be consolidated)
- Suggest standardization

### 7. Calculate connection density

For each note, count how many wiki-links it has. Rank notes by connectivity:

- **Hub notes** (5+ links) — these are well-connected
- **Lightly connected** (1-2 links) — could use more connections
- **Orphans** (0 links) — need attention

Report the distribution:

> "Your vault has X notes. Y are well-connected hubs, Z have light connections, and W are orphans."

### 8. Present the health report

> "## Vault Health Report
>
> **Overall:** [Healthy / Needs Attention / Just Getting Started]
>
> ### Stats
> - **Total notes:** X
> - **Total connections:** Y wiki-links
> - **Connection density:** Z links per note (average)
> - **Inbox:** N items waiting
>
> ### What's Working
> - [Positive observations — hubs, well-connected areas, growth trends]
>
> ### Needs Attention
> - **W orphan notes** with no connections [list top 5 with suggestions]
> - **N dead links** pointing to notes that don't exist [list them]
> - **N notes** missing frontmatter [list them]
> - **Tag inconsistencies:** [list any]
>
> ### Suggestions
> 1. [Most impactful action — e.g., "Connect your 3 book notes to each other"]
> 2. [Second action — e.g., "Create a [[Health]] topic note to link your 4 health-related notes"]
> 3. [Third action]
>
> Want me to fix any of these automatically?"

### 9. Offer to fix

If the user says yes, fix the issues:
- Add frontmatter to notes missing it
- Create stub notes for dead links
- Add suggested connections to orphan notes
- Standardize inconsistent tags

Report what was fixed:

> "Fixed 8 issues: added frontmatter to 3 notes, created 2 stub notes, connected 3 orphans. Run `/health` again anytime to check your vault."
