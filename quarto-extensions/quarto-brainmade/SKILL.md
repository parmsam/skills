---
name: quarto-brainmade
description: >
  Use when a user wants to add the Brainmade mark (human-made badge) to a
  Quarto document using the quarto-brainmade extension.
---

## Overview

**quarto-brainmade** adds the [Brainmade mark](https://brainmade.org) — a badge created by [Tristram Oaten (No Boilerplate)](https://github.com/0atman) to signal that a work is primarily human-made. AI assistance is permitted as long as the majority of the work is human-created.

Install with:
```bash
quarto add parmsam/quarto-brainmade
```

Works with any Quarto output format that supports shortcodes (HTML, RevealJS, etc.).

## Usage

Use shortcodes anywhere in `.qmd` files:

```
{{< brainmade-dark >}}
{{< brainmade-light >}}
```

Button variants:
```
{{< brainmade-dark-btn >}}
{{< brainmade-light-btn >}}
```

With custom size:
```
{{< brainmade-dark width="52px" >}}
{{< brainmade-dark height="52px" >}}
```

## Shortcode Reference

| Shortcode | Description |
|-----------|-------------|
| `brainmade-dark` | Dark version of the Brainmade mark |
| `brainmade-light` | Light version of the Brainmade mark |
| `brainmade-dark-btn` | Dark button-style variant |
| `brainmade-light-btn` | Light button-style variant |

## Notes

- Use `brainmade-light` on dark backgrounds and `brainmade-dark` on light backgrounds.
- `width` and `height` accept any CSS size value (`px`, `em`, `%`, etc.).
- Typically placed in a footer, byline, or acknowledgment section of a document.
