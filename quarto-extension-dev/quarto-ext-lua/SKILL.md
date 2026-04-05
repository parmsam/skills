---
name: quarto-ext-lua
description: >
  Use when a user is writing a Quarto filter or shortcode extension in Lua,
  working with the Pandoc AST, using the Quarto Lua API, or debugging
  Lua-based extension behavior.
---

## Overview

Quarto uses **Lua** as its extension language for filters and shortcodes. Lua runs inside Pandoc's embedded interpreter — no external dependencies needed.

## Filters vs. Shortcodes

| | Filter | Shortcode |
|---|---|---|
| Triggered by | Every matching AST node | `{{< name args >}}` in markdown |
| Returns | Modified AST node (or nil to remove) | Pandoc inline/block node(s) |
| Entry point | Named function matching node type | Function named after shortcode |
| Common use | Transform existing content | Generate new content |

## Writing a Filter

A filter is a Lua file with functions named after Pandoc AST node types:

```lua
-- Italicize all headings
function Header(el)
  el.content = pandoc.Emph(el.content)
  return el
end

-- Remove all horizontal rules
function HorizontalRule()
  return {}  -- returning empty list removes the node
end

-- Process code blocks by language
function CodeBlock(el)
  if el.classes[1] == "mermaid" then
    -- transform or wrap the block
    return el
  end
end
```

Activate in document YAML:

```yaml
filters:
  - my-filter.lua        # local file
  - my-org/my-extension  # installed extension
```

Control filter order relative to Quarto's built-ins:

```yaml
filters:
  - my-filter.lua
  - quarto              # quarto's built-in filters run here
  - my-post-filter.lua
```

## Writing a Shortcode

A shortcode returns Pandoc inline or block content:

```lua
-- {{< greeting name="World" >}}
function greeting(args, kwargs, meta, raw_args, context)
  local name = pandoc.utils.stringify(kwargs["name"] or "World")
  return pandoc.Str("Hello, " .. name .. "!")
end
```

Shortcode function parameters:
- `args` — positional arguments as `pandoc.List` of inlines
- `kwargs` — named arguments as table of inlines
- `meta` — document metadata
- `raw_args` — raw inline stream (Quarto ≥ 1.3)
- `context` — `"block"`, `"inline"`, or `"text"` (Quarto ≥ 1.5)

Convert inlines to string with `pandoc.utils.stringify(arg)`.

## Common Pandoc AST Nodes

```lua
pandoc.Str("text")                    -- plain text inline
pandoc.Emph(inlines)                  -- italic
pandoc.Strong(inlines)                -- bold
pandoc.Code(attr, "code")             -- inline code
pandoc.RawInline("html", "<br/>")     -- raw inline content
pandoc.Para(inlines)                  -- paragraph block
pandoc.Div(blocks, attr)              -- div block
pandoc.CodeBlock(attr, "code")        -- code block
pandoc.RawBlock("html", "<div>...")   -- raw block content
pandoc.Header(level, inlines, attr)   -- heading
pandoc.BulletList({{block1}, {block2}}) -- bulleted list
```

Attr constructor: `pandoc.Attr("id", {"class1","class2"}, {key="val"})`

## Format Detection

```lua
if quarto.doc.is_format("html") then
  return pandoc.RawBlock("html", "<div class='custom'>...</div>")
elseif quarto.doc.is_format("latex") then
  return pandoc.RawBlock("latex", "\\begin{custom}...")
end
```

Format aliases: `"html"`, `"latex"`, `"pdf"`, `"epub"`, `"markdown"`, `"html:js"`.

## Useful Utilities

```lua
pandoc.utils.stringify(el)            -- convert any inline/block to plain string
quarto.log.output(obj)                -- print any object for debugging
quarto.utils.resolve_path("file.css") -- absolute path to resource next to the .lua file
quarto.metadata.get("key")            -- read document metadata
quarto.variables.get("name")          -- read _variables.yml value
```

## Injecting Resources

```lua
-- Add CSS/JS to HTML output
quarto.doc.add_html_dependency({
  name = "my-styles",
  version = "1.0.0",
  stylesheets = { "styles.css" },
  scripts = { "script.js" }
})

-- Inject raw content into <head>
quarto.doc.include_text("in-header", "<style>.foo { color: red; }</style>")
quarto.doc.include_file("after-body", "footer.html")
```

## Module Organization

Split larger extensions into modules using `require()`:

```lua
-- In my-filter.lua:
local utils = require('./utils')
local parsing = require('./utils/parsing')
```

## Debugging

- `quarto.log.output(el)` — inspect any object
- `quarto render example.qmd --to native` — see the raw Pandoc AST
- `quarto preview` — auto-reloads on Lua file changes

For VS Code / Positron: install the Quarto extension and Lua LSP extension for type checking and autocomplete.

## Full API Reference

Read `references/lua-api.md` for the complete Quarto and Pandoc Lua API, including all custom node constructors (`quarto.Callout`, `quarto.Tabset`), HTML dependency fields, JSON/Base64 encoding, and path utilities.

## Further Reading

- [Lua Development](https://quarto.org/docs/extensions/lua.html)
- [Lua API Reference](https://quarto.org/docs/extensions/lua-api.html)
- [Shortcodes](https://quarto.org/docs/extensions/shortcodes.html)
- [Filters](https://quarto.org/docs/extensions/filters.html)
- [Pandoc Lua Filters](https://pandoc.org/lua-filters.html)
