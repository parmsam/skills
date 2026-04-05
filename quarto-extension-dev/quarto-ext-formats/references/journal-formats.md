# Journal Article Formats

Read this when helping a user build a Quarto journal article format extension.

## Scaffold

```bash
quarto create extension journal
```

Generates:

```
_extensions/
  <name>/
    _extension.yml
    header.tex          # LaTeX preamble customizations
    styles.css          # HTML styling
    <name>.lua          # Lua filter for author/affiliation transforms etc.
template.qmd            # example article; renamed on quarto use template
```

## _extension.yml Structure

Journal formats typically support both PDF and HTML outputs:

```yaml
title: ACS Journal Format
author: Your Name
version: 1.0.0
quarto-required: ">=1.3.0"
contributes:
  formats:
    pdf:
      shift-heading-level-by: -1
      toc: false
      documentclass: article
      template: partials/title-block.tex   # Pandoc .tex template partial
      filters:
        - acs.lua
      format-resources:
        - acs.bst                          # bibliography style file
      header-includes: |
        \usepackage{acs}
    html:
      css: styles.css
      filters:
        - acs.lua
    common:
      number-sections: true
      fig-cap-location: bottom
```

Key fields:
- `format-resources` — files needed during rendering (e.g., `.bst`, `.sty`) that must be copied alongside the output
- `template` — Pandoc `.tex` partial template for title block, author list, etc.
- `filters` — Lua filters applied when this format is used

## header.tex

Used for LaTeX preamble additions:

```tex
% In header.tex, referenced via header-includes in _extension.yml
\usepackage{amsmath}
\usepackage{booktabs}
\newcommand{\journal}{My Journal}
```

Or reference it directly:

```yaml
contributes:
  formats:
    pdf:
      include-in-header: header.tex
```

## Lua Filters in Journal Formats

Journal formats typically include a Lua filter to handle author affiliations, abstract formatting, or metadata transforms:

```lua
-- acs.lua: transform author metadata for LaTeX output
function Meta(meta)
  -- process author/affiliation data from YAML frontmatter
  -- inject into LaTeX template variables
  return meta
end
```

Authors are typically specified in the `template.qmd` frontmatter:

```yaml
author:
  - name: Jane Smith
    affiliations:
      - id: uni1
        name: University of Science
        department: Chemistry
    corresponding: true
    email: jane@uni.edu
```

Quarto has built-in author/affiliation normalization — filters receive structured metadata.

## Pandoc Template Partials

For fine-grained LaTeX control, override specific Pandoc template partials rather than the full template:

```yaml
contributes:
  formats:
    pdf:
      template-partials:
        - partials/title.tex
        - partials/before-body.tex
```

Partials use Pandoc's `$variable$` syntax and can reference Quarto-specific variables.

## Distribution

- `quarto use template org/repo` — installs format + copies `template.qmd`
- `quarto add org/repo` — installs format only (for existing projects)

## Examples

See the [Quarto Journals GitHub org](https://github.com/quarto-journals) for real implementations:
- `quarto-journals/acm` — ACM format
- `quarto-journals/acs` — American Chemical Society
- `quarto-journals/plos` — PLOS journals
- `quarto-journals/elsevier` — Elsevier journals
