---
name: quarto-ext-overview
description: >
  Use when a user wants to create a Quarto extension, understand extension
  types, scaffold a new extension, or learn the _extensions/ directory
  structure and _extension.yml format.
---

## Extension Types

| Type | What it does | Needs Lua? |
|---|---|---|
| **Shortcode** | Custom markdown directive that generates content | Yes |
| **Filter** | Modifies the Pandoc AST during render | Yes |
| **Custom Format** | New output format bundling options/templates/CSS | No |
| **Journal Article** | PDF + HTML article format for academic publishing | Sometimes |
| **RevealJS Plugin** | Extends HTML presentation capabilities | Sometimes |
| **Project Type** | Standardized project structure | No |
| **Starter Template** | Scaffolded project with example content | No |
| **Metadata** | YAML config merged into existing projects | No |
| **Brand** | Distributes `brand.yml` and associated assets | No |
| **Engine** | Custom code execution engine | Yes |

**Quick rule of thumb**: if you need to transform document content → Filter; if you need a custom `{{< >}}` tag → Shortcode; if you need a new output style → Custom Format.

## Scaffolding a New Extension

Use `quarto create extension <type>` to scaffold. Types: `filter`, `shortcode`, `format:html`, `format:pdf`, `format:revealjs`, `journal`, `project`.

```bash
quarto create extension filter       # prompts for name
quarto create extension shortcode
quarto create extension format:html
```

This generates the `_extensions/<name>/` directory with `_extension.yml` and starter files.

## Directory Structure

```
README.md            # shown on GitHub; NOT installed with the extension
LICENSE
example.qmd          # demo document for development/testing
_extensions/
  <ext-name>/
    _extension.yml   # required: metadata + contributes
    <ext-name>.lua   # Lua filter or shortcode (if applicable)
    styles.css       # optional styles
    template.qmd     # optional template (for formats)
```

Supporting files (`README.md`, `LICENSE`, `example.qmd`) live at the repo root — they are **not** installed when a user runs `quarto add`.

## _extension.yml

Every extension requires this file. Minimal example:

```yaml
title: My Extension
author: Your Name
version: 1.0.0
quarto-required: ">=1.3.0"
contributes:
  filters:
    - my-filter.lua
```

The `contributes` key declares what the extension provides. Common values:

```yaml
contributes:
  shortcodes:
    - my-shortcode.lua
  filters:
    - my-filter.lua
  formats:
    html:
      toc: true
      theme: cosmo
  revealjs-plugins:
    - my-plugin
```

Use semantic versioning for `version` (e.g., `1.0.0`, `1.2.3-beta`).

## Development Workflow

1. Scaffold with `quarto create extension <type>`
2. Edit `_extensions/<name>/<name>.lua` (for Lua-based extensions)
3. Run `quarto preview example.qmd` — live reloads on Lua file changes
4. Inspect the Pandoc AST with `quarto render example.qmd --to native`
5. Test edge cases, then publish to GitHub

## Further Reading

- [Shortcodes](https://quarto.org/docs/extensions/shortcodes.html)
- [Filters](https://quarto.org/docs/extensions/filters.html)
- [Custom Formats](https://quarto.org/docs/extensions/formats.html)
- [Lua Development](https://quarto.org/docs/extensions/lua.html)
- [Lua API Reference](https://quarto.org/docs/extensions/lua-api.html)
- [Distributing Extensions](https://quarto.org/docs/extensions/distributing.html)
- [Quarto Extensions org (examples)](https://github.com/quarto-ext)
