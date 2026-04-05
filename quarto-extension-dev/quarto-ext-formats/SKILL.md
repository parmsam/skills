---
name: quarto-ext-formats
description: >
  Use when a user wants to create a Quarto custom format extension, journal
  article format, RevealJS plugin extension, or bundle format options and
  templates into a distributable extension.
---

## Overview

Format extensions create new output formats by bundling Quarto options, templates, CSS/SCSS, and other extensions. All custom formats derive from a **base format** — the base format name becomes the output suffix.

## Scaffold

```bash
quarto create extension format:html    # HTML-based format
quarto create extension format:pdf     # PDF/LaTeX format
quarto create extension format:revealjs
quarto create extension journal        # journal article (PDF + HTML)
```

This generates `_extensions/<name>/`, a `template.qmd`, and an `_extension.yml`.

## _extension.yml for Formats

```yaml
title: My HTML Format
author: Your Name
version: 1.0.0
quarto-required: ">=1.3.0"
contributes:
  formats:
    html:
      toc: true
      theme: [default, custom.scss]
      css: styles.css
      grid:
        sidebar-width: 300px
```

Support **multiple base formats** in one extension:

```yaml
contributes:
  formats:
    html:
      toc: true
      theme: cosmo
    pdf:
      documentclass: article
      geometry: margin=1in
    common:           # applied to all formats
      number-sections: true
      fig-cap-location: bottom
```

Users choose which to render: `format: my-format-html` or `format: my-format-pdf`.

## Using the Format

Users add to their document YAML:

```yaml
format: my-org/my-repo-html    # or just the format name if installed
```

Or after `quarto use template my-org/my-repo`, the `template.qmd` is pre-configured.

## Custom Templates (Pandoc)

For deep control over output structure, include a Pandoc template. Reference it in `_extension.yml`:

```yaml
contributes:
  formats:
    html:
      template: template.html
    pdf:
      template: template.tex
```

Templates use Pandoc's `$variable$` syntax. See [Pandoc templates](https://pandoc.org/MANUAL.html#templates) for variables.

## SCSS Theming (HTML formats)

```yaml
contributes:
  formats:
    html:
      theme: [default, custom.scss]
```

In `custom.scss`:

```scss
/*-- scss:defaults --*/
$font-size-root: 18px;
$link-color: #0066cc;

/*-- scss:rules --*/
.my-callout {
  border-left: 4px solid $link-color;
}
```

## Embedding Dependencies

If your format depends on other extensions, embed them to avoid version conflicts:

```bash
quarto add some-org/some-ext --embed my-format-name
```

This copies the dependency into your `_extensions/` directory.

## RevealJS Plugin Extensions

RevealJS plugins bundle JavaScript plugins for HTML presentations:

```yaml
contributes:
  revealjs-plugins:
    - name: my-plugin
      script: my-plugin.js
      stylesheet: my-plugin.css
```

Users activate with:

```yaml
format:
  revealjs: default
revealjs-plugins:
  - my-org/my-plugin
```

## Journal Article Formats

Journal formats produce both PDF (LaTeX) and HTML. Scaffold with `quarto create extension journal`. Key additions:

- LaTeX template (`.tex`) for PDF output
- HTML template and CSS for web output
- CSL file for citation formatting (optional)
- `_extension.yml` with both `pdf` and `html` format contributions

See [Quarto Journals GitHub org](https://github.com/quarto-journals) for real examples.

## Distribution

- `quarto add org/repo` — installs the format
- `quarto use template org/repo` — installs format + copies `template.qmd`

Prefer `quarto use template` for formats so users get a pre-configured starting document.

## Reference Files

For deeper detail, read these on demand:
- `references/journal-formats.md` — journal scaffold files (`header.tex`, `aps.lua`, `format-resources`, Pandoc template partials, author/affiliation metadata)
- `references/revealjs-plugins.md` — full Reveal.js plugin API, `deck.getConfig()`, CSS patterns, packaging 3rd-party plugins

## Further Reading

- [Custom Formats](https://quarto.org/docs/extensions/formats.html)
- [Journal Articles](https://quarto.org/docs/journals/formats.html)
- [RevealJS Plugins](https://quarto.org/docs/extensions/revealjs.html)
- [Quarto Journals org (examples)](https://github.com/quarto-journals)
