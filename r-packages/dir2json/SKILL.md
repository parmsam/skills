---
name: dir2json
description: >
  Use when a user wants to convert a directory to JSON or decode JSON back into
  a directory structure using the dir2json R package.
---

## Overview

**dir2json** converts a directory structure (including file contents) into JSON and back. Useful for archiving, sharing codebases with LLMs, or round-tripping directory contents through text-based formats.

- Text files are stored as plain strings.
- Binary files (images, PDFs, etc.) are base64-encoded.

Install with:
```r
install.packages("dir2json")

# Development version:
devtools::install_github("parmsam/dir2json-r")
```

## Core Functions

| Function | Description |
|----------|-------------|
| `json_encode_dir(path)` | Encode a directory to a JSON string |
| `json_decode_dir(json, path)` | Decode JSON back into a directory |
| `validate_dir_json(json)` | Validate a directory JSON string |
| `as_file_list(path)` | Convert a file path to a list representation |
| `write_files(files, path)` | Write a list of files to a directory |
| `text_file_extensions()` | Return the list of recognized text file extensions |

## JSON Schema

Each file is an object with two fields:
- `name` — file or directory name (string)
- `content` — plain text for text files; base64-encoded string for binary files

```json
[
  {"name": "README.md", "content": "# My project\n"},
  {"name": "logo.png", "content": "<base64 string>"}
]
```

## Examples

**Encode a directory:**
```r
library(dir2json)

json_data <- json_encode_dir("path/to/my-project")
cat(json_data)
```

**Decode JSON back to disk:**
```r
json_decode_dir(json_data, "path/to/output-dir")
```

**Round-trip:**
```r
tmp <- tempfile()
dir.create(tmp)
writeLines("Hello!", file.path(tmp, "hello.txt"))

json <- json_encode_dir(tmp)
out <- tempfile()
json_decode_dir(json, out)
readLines(file.path(out, "hello.txt"))
#> [1] "Hello!"
```

**Validate JSON structure:**
```r
validate_dir_json(json_data)
```

## Notes

- Not a replacement for ZIP — JSON is a text interchange format, not a compression format.
- Binary files are base64-encoded, so the JSON will be larger than the originals.
- Well-suited for passing multi-file codebases to LLMs that accept text input.
- See [pkgdown site](https://parmsam.github.io/dir2json-r/) for full reference.
