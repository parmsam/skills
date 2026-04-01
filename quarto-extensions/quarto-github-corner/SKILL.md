---
name: quarto-github-corner
description: >
  Use when a user wants to add a GitHub corner SVG badge linking to a repo URL
  in the corner of a Quarto HTML document using the quarto-github-corner extension.
---

## Overview

**quarto-github-corner** adds a [GitHub corner](https://tholman.com/github-corners/) SVG badge to any Quarto HTML output. The badge links to a URL (typically a GitHub repo) and renders in a corner of the page.

Install with:
```bash
quarto add parmsam/quarto-github-corner
```

Works with any Quarto HTML output format.

## Setup

Add the filter and options to YAML front matter:

```yaml
filters:
  - github-corner
github-corner:
  url: "https://github.com/username/repo"
  logo-color: "white"
  background-color: "#1c84e5"
  size: 150
  position: "right"
```

## Options

| Option | Default | Description |
|--------|---------|-------------|
| `url` | *(required)* | Full URL including `https://` |
| `logo-color` | `"#fff"` | CSS color for the octocat logo |
| `background-color` | `"#151513"` | CSS color for the corner background |
| `size` | `80` | Size in pixels |
| `position` | `"right"` | `"left"` or `"right"` |

## Notes

- The `url` field is required — the badge won't render without it.
- Colors accept any valid CSS color (named colors, hex, rgb, etc.).
- Works on `html`, `revealjs`, and other HTML-based output formats.
