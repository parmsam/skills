# Quarto & Pandoc Lua API Reference

Read this file when you need specifics on Lua API functions, custom AST node constructors, HTML dependency fields, or utility functions.

## Lua Standard Libraries

| Library | Purpose |
|---|---|
| `string` | Pattern matching, substrings, find/replace |
| `utf8` | UTF-8 encoding support |
| `table` | Table manipulation (insert, remove, sort, concat) |
| `math` | Mathematical functions |
| `io` / `file` | File I/O with implicit or explicit handles |
| `os` | Dates, locales, environment variables (`os.getenv()`) |

## Pandoc Lua API

### AST Constructors (`pandoc.*`)

```lua
-- Inlines
pandoc.Str(text)
pandoc.Space()
pandoc.LineBreak()
pandoc.SoftBreak()
pandoc.Emph(inlines)
pandoc.Strong(inlines)
pandoc.Strikeout(inlines)
pandoc.Underline(inlines)
pandoc.Subscript(inlines)
pandoc.Superscript(inlines)
pandoc.SmallCaps(inlines)
pandoc.Quoted(type, inlines)        -- type: "SingleQuote" | "DoubleQuote"
pandoc.Cite(citations, inlines)
pandoc.Code(attr, text)
pandoc.Math(type, text)             -- type: "InlineMath" | "DisplayMath"
pandoc.RawInline(format, text)
pandoc.Link(inlines, target, title, attr)
pandoc.Image(inlines, src, title, attr)
pandoc.Note(blocks)
pandoc.Span(inlines, attr)

-- Blocks
pandoc.Para(inlines)
pandoc.Plain(inlines)
pandoc.Header(level, inlines, attr)
pandoc.CodeBlock(attr, text)
pandoc.RawBlock(format, text)
pandoc.BlockQuote(blocks)
pandoc.BulletList(items)            -- items: list of {block, ...}
pandoc.OrderedList(items, listattr)
pandoc.DefinitionList(items)
pandoc.HorizontalRule()
pandoc.Div(blocks, attr)
pandoc.Figure(blocks, caption, attr)
pandoc.Table(...)

-- Attr
pandoc.Attr(id, classes, attributes)
-- e.g. pandoc.Attr("my-id", {"class1","class2"}, {key="value"})
```

### Pandoc Functions

```lua
pandoc.read(text, format)           -- parse text as Pandoc document
pandoc.write(doc, format)           -- render Pandoc document to string
pandoc.pipe(cmd, args, input)       -- run subprocess, return output
pandoc.walk_block(block, filter)    -- apply filter to block tree
pandoc.walk_inline(inline, filter)  -- apply filter to inline tree
```

### pandoc.utils

```lua
pandoc.utils.stringify(el)          -- convert AST node to plain string
pandoc.utils.blocks_to_inlines(blocks, sep)
pandoc.utils.make_sections(numbered, base_level, blocks)
pandoc.utils.normalize_date(str)
pandoc.utils.references(doc)        -- get all references from doc
pandoc.utils.citeproc(doc)          -- process citations
pandoc.utils.type(val)              -- pandoc type name of value
pandoc.utils.Version(str)           -- parse version string
```

### pandoc.List

List type with extra methods:

```lua
local l = pandoc.List({1, 2, 3})
l:find(val)                         -- returns value and index, or nil
l:find_if(pred)                     -- first element matching predicate
l:filter(pred)                      -- new list of matching elements
l:map(fn)                           -- new list applying fn to each
l:includes(val)                     -- boolean membership test
l:extend(list2)                     -- append list2 in place
l:insert([pos,] val)                -- insert element
l:remove([pos])                     -- remove element
```

### pandoc.text (UTF-8 aware)

```lua
pandoc.text.upper(s)
pandoc.text.lower(s)
pandoc.text.len(s)
pandoc.text.sub(s, i [, j])
pandoc.text.reverse(s)
pandoc.text.find(s, pattern [, init [, plain]])
pandoc.text.match(s, pattern [, init])
```

### pandoc.path

```lua
pandoc.path.join(parts)             -- join path components
pandoc.path.split(path)             -- split into directory and filename
pandoc.path.directory(path)         -- directory component
pandoc.path.filename(path)          -- filename component
pandoc.path.split_extension(path)   -- filename and extension
pandoc.path.is_absolute(path)
pandoc.path.normalize(path)
```

### pandoc.system

```lua
pandoc.system.get_working_directory()
pandoc.system.list_directory(path)
pandoc.system.make_directory(path [, create_parent])
pandoc.system.os                    -- "linux", "macos", "windows"
pandoc.system.arch                  -- architecture string
pandoc.system.with_temporary_directory(dir, fn)
pandoc.system.with_working_directory(dir, fn)
pandoc.system.environment()         -- table of env vars
```

---

## Quarto Lua API

### Version & Utilities

```lua
quarto.version                              -- pandoc.Version of current Quarto

quarto.utils.resolve_path(path)             -- absolute path relative to calling .lua file
quarto.utils.string_to_inlines(str, sep)    -- parse string to pandoc.List of Inlines
quarto.utils.string_to_blocks(str)          -- parse string to pandoc.List of Blocks
```

### Logging

```lua
quarto.log.output(obj)       -- print object to stdout (always shown)
quarto.log.warning(obj)      -- print suppressible warning
quarto.log.debug(obj)        -- print only when --trace is active
```

### Format Detection

```lua
quarto.doc.is_format(name)   -- returns true/false
```

Format aliases:

| Alias | Matches |
|---|---|
| `"html"` | html*, epub*, revealjs, dashboard, email |
| `"html:js"` | HTML formats that can run JavaScript |
| `"latex"` | latex, pdf, beamer |
| `"pdf"` | pdf |
| `"epub"` | epub* |
| `"markdown"` | markdown*, commonmark*, gfm, markua |

Additional format info:

```lua
quarto.doc.cite_method()     -- "citeproc" | "natbib" | "biblatex"
quarto.doc.pdf_engine()      -- "pdflatex" | "xelatex" | "lualatex" | "tectonic"
```

### Content Inclusion

```lua
-- location: "in-header" | "before-body" | "after-body"
quarto.doc.include_text(location, text)
quarto.doc.include_file(location, filepath)
```

### HTML Dependencies

```lua
quarto.doc.add_html_dependency({
  name = "my-dep",           -- required: unique identifier
  version = "1.0.0",         -- required: version string
  scripts = {                -- JS files
    "script.js",             -- simple path
    {                        -- or script object:
      path = "script.js",
      attribs = { defer = "true" },
      afterBody = false
    }
  },
  stylesheets = {            -- CSS files
    "styles.css",            -- simple path
    { path = "styles.css", attribs = { media = "print" } }
  },
  links = {
    { rel = "preconnect", href = "https://fonts.googleapis.com" }
  },
  resources = {              -- additional files to copy
    { name = "data.json", path = "data.json" }
  },
  meta = {                   -- <meta> tags
    { name = "theme-color", content = "#ffffff" }
  },
  head = "<style>/* inline css */</style>"
})

quarto.doc.attach_to_dependency(name, { name = "file.js", path = "file.js" })
```

### LaTeX Dependencies

```lua
quarto.doc.use_latex_package("tikz", "arrows,shapes")
quarto.doc.add_format_resource("my-style.sty")
```

### Resources

```lua
quarto.doc.add_resource(path)     -- add resource file to output bundle
quarto.doc.add_supporting(path)   -- add supporting file
```

### Custom AST Node Constructors

```lua
-- Callout block
quarto.Callout({
  type = "note",          -- required: "note"|"tip"|"warning"|"caution"|"important"
  title = "My Title",     -- optional
  content = blocks,       -- optional: pandoc.List of Blocks
  collapse = false,       -- optional
  icon = true             -- optional
})

-- Tabset
quarto.Tabset({
  tabs = {
    quarto.Tab({ title = "Tab 1", content = blocks }),
    quarto.Tab({ title = "Tab 2", content = blocks })
  },
  level = 2,              -- heading level for tab labels
  attr = pandoc.Attr()
})

-- Conditional block
quarto.ConditionalBlock({
  node = div,             -- the pandoc.Div to wrap
  behavior = "visible"    -- required
})

-- Float/cross-reference target
quarto.FloatRefTarget({
  type = "Figure",
  content = blocks,
  identifier = "fig-my-fig"
})
```

### JSON & Base64

```lua
quarto.json.encode(table)     -- Lua table → JSON string
quarto.json.decode(str)       -- JSON string → Lua table

quarto.base64.encode(str)     -- string → Base64
quarto.base64.decode(b64)     -- Base64 → string
```

### Metadata & Variables

```lua
quarto.metadata.get("key")           -- returns pandoc.MetaValue or nil
quarto.metadata.get("section.key")   -- supports dot notation for nesting
-- Use pandoc.utils.stringify() to convert MetaValue to string

quarto.variables.get("name")         -- returns string from _variables.yml or nil
```

### Shortcode Helpers

```lua
quarto.shortcode.read_arg(args, n)   -- get nth positional arg (1-indexed), returns inlines or nil
quarto.shortcode.error_output(name, message_or_args, context)  -- structured error output
```

### Paths

```lua
quarto.paths.rscript()           -- path to Rscript binary
quarto.paths.tinytex_bin_dir()   -- path to TinyTeX bin dir, or nil
quarto.paths.typst()             -- path to typst binary
```
