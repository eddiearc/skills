# Markmap Configuration Reference

## JSON Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `color` | function/string[] | d3.schemeCategory10 | Branch color function or array |
| `colorFreezeLevel` | number | 0 | Freeze color at this branch depth |
| `lineWidth` | number | - | Stroke width for connecting lines (v0.18.8+) |
| `spacingHorizontal` | number | 80 | Horizontal spacing between nodes |
| `spacingVertical` | number | 5 | Vertical spacing between nodes |
| `maxWidth` | number | 0 | Max node content width (0 = no limit) |
| `duration` | number | 500 | Animation duration in ms |
| `zoom` | boolean | true | Enable mouse wheel zoom |
| `pan` | boolean | true | Enable drag to pan |
| `initialExpandLevel` | number | -1 | Initial expand depth (-1 = all) |
| `autoFit` | boolean | true | Auto-fit diagram to viewport |

## Programmatic API

```javascript
// Create markmap
const mm = markmap.Markmap.create('svg#mindmap', options, rootData);

// Destroy instance (important before recreating)
mm.destroy();

// Root data structure
const root = {
  content: "Node text",
  children: [
    { content: "Child", children: [] }
  ]
};
// Every node MUST have a children array (empty [] for leaves)
```

## Color Schemes

Custom colors are passed as an array to `d3.scaleOrdinal()`:

```javascript
// Morandi palette
['#B4A7D6','#A4C2A5','#D4A574','#7BA7BC','#C49A9A','#8DB596']

// Tech Blue palette
['#1890ff','#36cfc9','#597ef7','#9254de','#f759ab','#faad14']
```

## CLI Usage

```bash
# Generate HTML from markdown
npx markmap-cli input.md -o output.html --offline --no-open

# Options
--offline     # Embed all assets for standalone use
--no-open     # Don't auto-open browser
--no-toolbar  # Hide the toolbar
-w, --watch   # Watch mode for development
```
