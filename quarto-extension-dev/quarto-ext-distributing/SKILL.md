---
name: quarto-ext-distributing
description: >
  Use when a user wants to publish a Quarto extension to GitHub, understand
  quarto add install commands, use version tags, create starter templates,
  or distribute extensions via archives.
---

## Overview

Extensions are distributed either via **GitHub** (recommended) or **archives** (.zip / .tar.gz). GitHub distribution is the standard path — it gives users a simple install command and supports versioning via tags.

## GitHub Distribution

Host your `_extensions/` directory in a public GitHub repo. Users install with:

```bash
quarto add your-org/your-repo
```

Target a specific version tag or branch:

```bash
quarto add your-org/your-repo@v1.2.0
quarto add your-org/your-repo@main
```

If your repo contains multiple extensions in subdirectories:

```bash
quarto add your-org/your-repo/path/to/ext
```

## Versioning

Use semantic versioning in `_extension.yml`:

```yaml
version: 1.0.0
quarto-required: ">=1.3.0"
```

Create a GitHub release + tag (e.g., `v1.0.0`) for each version. Tags let users pin to specific releases and make `quarto update` work correctly.

## Starter Templates

A **starter template** bundles the extension with a `template.qmd`. When a user installs via `quarto use template`, the `template.qmd` is copied and renamed to their project name — useful for formats and project types.

```bash
# For formats/projects with a template:
quarto use template your-org/your-repo
```

vs.

```bash
# Extension only (no template):
quarto add your-org/your-repo
```

Include a `template.qmd` at the repo root when you want `quarto use template` to work. The file is renamed to match the user's directory on install.

## Archive Distribution

For non-GitHub hosting (GitLab, internal servers, local files):

```bash
# GitLab archive
quarto add https://gitlab.com/org/repo/-/archive/main/repo-main.zip

# Web-hosted archive
quarto add https://example.com/extensions/my-ext.zip

# Local archive or directory
quarto add ~/Downloads/my-ext.zip
quarto add /shared/extensions/my-ext
```

Note: archive-distributed extensions share a flat namespace (no org prefix), so name collisions are possible.

## What Gets Installed

Only the `_extensions/` subdirectory is installed. `README.md`, `LICENSE`, `example.qmd`, and other root-level files are **not** copied to the user's project.

## Recommended Repo Layout

```
README.md          # GitHub landing page
LICENSE
CHANGELOG.md       # optional but helpful
example.qmd        # runnable demo
template.qmd       # optional: enables quarto use template
_extensions/
  <ext-name>/
    _extension.yml
    <ext-name>.lua
```

## Reference Files

For deeper detail, read on demand:
- `references/managing-and-templates.md` — `quarto list/update/remove` commands, committing `_extensions/` to version control, `.quartoignore` syntax, `template.qmd` renaming behavior, subdirectory multi-extension repos

## Further Reading

- [Distributing Extensions](https://quarto.org/docs/extensions/distributing.html)
- [Managing Extensions](https://quarto.org/docs/extensions/managing.html)
- [Starter Templates](https://quarto.org/docs/extensions/starter-templates.html)
