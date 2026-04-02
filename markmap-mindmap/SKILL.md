---
name: markmap-mindmap
description: Convert text content into interactive markmap mindmaps with style playground. Use when asked to create a mindmap, visualize content structure, or generate an interactive thinking map from text, files, or outlines.
---

# Markmap Mindmap

Convert text content into beautiful, interactive mindmap HTML files using [markmap](https://markmap.js.org/).

## Prerequisites

- **Node.js 18+**: `node --version`
- **markmap-cli**: auto-installed via `npx markmap-cli` on first run

## Workflow

```text
Mindmap progress:
- [ ] Step 1: Determine input source
- [ ] Step 2: Extract structure into markmap markdown
- [ ] Step 3: Generate HTML with markmap-cli
- [ ] Step 4: Offer style playground (if user wants to pick a style)
- [ ] Step 5: Present output
```

### Step 1: Determine input source

| Source | Action |
|--------|--------|
| Local file (.md / .txt) | Read the file |
| User-provided text or outline | Use directly |

This skill does NOT fetch URLs or scrape websites. Content acquisition is the caller's responsibility.

### Step 2: Extract structure into markmap markdown

Convert the content into a markdown file using **heading hierarchy only**:

```markdown
# Root Topic

## Branch 1
### Sub-branch 1a
- Leaf item
- Leaf item
### Sub-branch 1b
- Leaf item

## Branch 2
### Sub-branch 2a
- Leaf item
```

**Constraints:**
- Max 4 levels deep (`#` → `##` → `###` → `####`, plus `-` list items as leaves)
- Each node text: max 15 Chinese characters or 30 English characters
- 3-7 main branches (`##`), 2-5 sub-nodes per branch
- No Mermaid syntax — just plain markdown headings and lists
- No blank lines between items in the same branch (markmap treats them as breaks)

Save to `{output_dir}/{name}.md`.

### Step 3: Generate HTML with markmap-cli

```bash
npx markmap-cli "{name}.md" -o "{name}.html" --offline --no-open
```

- `--offline`: embeds all JS/CSS for standalone use
- `--no-open`: don't auto-open browser (we control when to open)

### Step 4: Offer style playground

Ask the user:

> "Want to pick a visual style? I can open a playground with 8 preset themes to preview."

If yes, generate the playground HTML using the template:

```bash
# Read the template
cat "$SKILL_DIR/templates/playground.html"
```

The playground template needs the mindmap data injected as a JSON tree. Convert the markdown structure to a JSON tree where each node has `{"content": "text", "children": [...]}`. Every node must have a `children` array (empty `[]` for leaves).

**How to build the JSON tree from the markdown:**

1. Parse the markdown heading hierarchy
2. `#` heading → root node
3. `##` headings → root's children
4. `###` headings → children of their parent `##`
5. `####` headings → children of their parent `###`
6. `- list items` → children of their parent heading
7. Every leaf node must have `"children": []`

Replace the `__MARKMAP_DATA__` placeholder in the template with the JSON tree, and `__MARKMAP_TITLE__` with the root topic.

Save to `{output_dir}/{name}-playground.html` and open in browser.

After the user picks a style from the playground, they can click "Export HTML" to download the final styled version. No further action needed from the skill.

### Step 5: Present output

If playground was skipped (user just wants the default style), open the HTML from Step 3:

```bash
open "{name}.html"
```

Report the output files:
- `{name}.md` — markmap source (editable markdown)
- `{name}.html` — interactive mindmap (default style)
- `{name}-playground.html` — style playground (if generated)

## Available Styles in Playground

| Style | Description |
|-------|-------------|
| Default Rainbow | markmap native d3.schemeCategory10 |
| Morandi | Low saturation, warm background, serif font |
| Tech Blue | Dark background, blue-purple palette, monospace |
| Forest Green | Fresh green gradient, light background |
| Warm Orange | Warm tones, energetic |
| Premium Gray | Minimalist business style |
| Macaron | Candy colors, playful |
| Black Gold | Dark background, gold accents, luxury feel |

## Anti-patterns

- Do NOT use Mermaid mindmap syntax — markmap uses plain markdown
- Do NOT write long sentences as node labels — split into parent + children
- Do NOT exceed 4 levels of depth — becomes unreadable
- Do NOT fetch URLs or scrape websites — that's the caller's job
- Do NOT generate custom HTML manually — always use markmap-cli or the playground template
