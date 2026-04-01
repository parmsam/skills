---
name: lzstring
description: >
  Use when a user wants to compress or decompress strings in R using LZ-based
  compression via the lzstring R package, including Shinylive URL encoding.
---

## Overview

**lzstring** is an R wrapper for the [lz-string C++ library](https://github.com/andykras/lz-string-cpp), which implements LZ-based string compression. Commonly used for compressing JSON payloads and generating Shinylive share URLs.

Install with:
```r
install.packages("lzstring")

# Development version:
devtools::install_github("parmsam/lzstring-r")
```

## Functions

| Function | Description |
|----------|-------------|
| `compressToBase64(string)` | Compress a string to Base64 |
| `decompressFromBase64(string)` | Decompress a Base64-compressed string |
| `compressToEncodedURIComponent(string)` | Compress to URL-safe encoded string |
| `decompressFromEncodedURIComponent(string)` | Decompress a URI-encoded compressed string |

## Examples

**Basic compression:**
```r
library(lzstring)

msg <- "The quick brown fox jumps over the lazy dog!"
compressed <- compressToBase64(msg)
decompressFromBase64(compressed)
#> [1] "The quick brown fox jumps over the lazy dog!"
```

**Compress JSON:**
```r
library(jsonlite)

json <- toJSON(list(name = "Alice", age = 30))
compressed <- compressToBase64(as.character(json))
decompressFromBase64(compressed)
```

**Round-trip for R objects:**
```r
obj <- list(a = 1, b = "text", c = list(x = 1:3))
lz <- compressToBase64(serializeJSON(obj))
obj2 <- unserializeJSON(decompressFromBase64(lz))
identical(obj, obj2)
#> [1] TRUE
```

**Generate a Shinylive share URL:**
```r
code <- 'library(shiny)
ui <- fluidPage("Hello!")
server <- function(input, output, session) {}
shinyApp(ui, server)'

files_json <- toJSON(list(list(name = unbox("app.R"), content = unbox(code))))
hash <- compressToEncodedURIComponent(as.character(files_json))
cat(paste0("https://shinylive.io/r/app/#code=", hash))
```

**Decompress a Shinylive URL hash:**
```r
x <- decompressFromEncodedURIComponent("<hash from URL>")
fromJSON(x)
```

## Notes

- lzstring operates on strings only — encode binary data as base64 or hex before compressing.
- Always ensure input strings are UTF-8 encoded.
- Serialize complex R objects with `jsonlite::serializeJSON()` before compressing; use `jsonlite::unserializeJSON()` to restore.
- Use `compressToEncodedURIComponent` / `decompressFromEncodedURIComponent` for URL-safe output (e.g., Shinylive links).
- See [pkgdown site](https://parmsam.github.io/lzstring-r/) for full reference.
