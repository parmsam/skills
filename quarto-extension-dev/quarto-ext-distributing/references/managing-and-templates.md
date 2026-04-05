# Managing Extensions & Starter Template Details

Read this for details on extension lifecycle commands, version control practices, `.quartoignore`, and starter template behavior.

## Extension Lifecycle Commands

```bash
# Install
quarto add quarto-ext/fontawesome
quarto add org/repo@v1.2.0          # pin to version tag
quarto add org/repo@branch-name     # pin to branch

# List installed extensions
quarto list extensions

# Update (prompts with current → target version)
quarto update quarto-ext/fontawesome
quarto update org/repo

# Remove
quarto remove quarto-ext/fontawesome
quarto remove                        # interactive list if no arg given
```

## Version Control

**Always commit `_extensions/` to your git repo.** This makes projects reproducible without depending on external registries or specific versions remaining available.

The `_extensions/` directory is treated as vendored source code — not an artifact to be gitignored.

## Local Extension Development

Extensions don't require GitHub hosting to work. You can develop and test locally:

```bash
# Test from a local directory
quarto add /path/to/your/extension

# Or just work directly inside a project — place your _extensions/ folder
# alongside your .qmd files and it will be picked up automatically
```

A full project structure isn't required — any directory with an `_extensions/` folder adjacent to `.qmd` files works.

## Security Note for Documentation

When documenting your extension, note that extensions can execute code during rendering. Users should only install extensions from trusted sources. Mention this in your README.

## Starter Templates: What Gets Copied

When a user runs `quarto use template org/repo`, Quarto clones the repo and copies **most** files — with these automatic exclusions:

- Hidden files (`.git`, `.github`, `.gitignore`, etc.)
- `README.md`, `LICENSE`
- AI assistant config files

### .quartoignore

Create a `.quartoignore` at the repo root to exclude additional files from template installation. Uses glob syntax like `.gitignore`:

```
# .quartoignore
*.Rproj
dev/
tests/
scripts/
_freeze/
```

Useful for excluding development artifacts, test files, or CI configs that don't belong in a user's new project.

### template.qmd Renaming

The `template.qmd` file at the repo root is special — it gets **renamed** to match the user's directory/project name on install. This gives users a sensibly named starting document.

For website/book templates, `index.qmd` serves this purpose instead — no `template.qmd` needed.

## Subdirectory Extensions

If your repo contains multiple extensions:

```
_extensions/
  ext-one/
    _extension.yml
  ext-two/
    _extension.yml
```

Users can install individual extensions:

```bash
quarto add org/repo/ext-one
quarto add org/repo/ext-two
```

Or install all at once (installs everything in `_extensions/`):

```bash
quarto add org/repo
```
