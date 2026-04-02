# markmap-mindmap

An AI agent skill that converts text content into beautiful, interactive mindmap HTML files.

Powered by [markmap](https://markmap.js.org/) with a built-in style playground featuring 8 preset themes.

## Features

- **Plain markdown input** — just headings and lists, no special syntax
- **Interactive HTML output** — zoom, pan, expand/collapse nodes
- **8 preset themes** — Rainbow, Morandi, Tech Blue, Forest Green, Warm Orange, Premium Gray, Macaron, Black Gold
- **Style playground** — preview all themes side-by-side, adjust expand level and node width, export with one click
- **Offline support** — generated HTML works without internet

## Install

```bash
npx skills add https://github.com/eddiearc/skills --skill markmap-mindmap
```

Or manually symlink the skill directory to `.claude/skills/markmap-mindmap`.

## Usage

In Claude Code, just ask:

```
帮我把这段内容生成思维导图
Create a mindmap from this outline
```

The skill will:
1. Convert your content into markmap-compatible markdown
2. Generate an interactive HTML mindmap
3. Optionally open a style playground to pick a theme

## Playground Preview

The playground lets you:
- Switch between 8 visual themes in real-time
- Adjust node expand level (1-3 levels or all)
- Control max node width
- Export the final styled HTML with one click

## Input Format

Just plain markdown:

```markdown
# Root Topic

## Branch 1
### Sub-branch
- Leaf item
- Leaf item

## Branch 2
### Sub-branch
- Leaf item
```

## Requirements

- Node.js 18+
- markmap-cli (auto-installed via npx)

## License

MIT
