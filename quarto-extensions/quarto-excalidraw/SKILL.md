---
name: quarto-excalidraw
description: >
  Use when a user wants to embed an Excalidraw whiteboard canvas inside a
  Quarto RevealJS presentation using the quarto-excalidraw extension.
---

## Overview

**quarto-excalidraw** embeds a live [Excalidraw](https://excalidraw.com) whiteboard canvas into RevealJS presentations. It runs locally in the browser — no external service required. Useful for live sketching, diagrams, and freehand annotation during presentations.

Install with:
```bash
quarto add parmsam/quarto-excalidraw
```

Requires RevealJS output format.

## Basic Setup

```yaml
title: "My Presentation"
format:
  revealjs: default
revealjs-plugins:
  - excalidraw
```

Toggle the canvas with the 🎨 toolbar button or the backtick (`` ` ``) shortcut key.

## Configuration

```yaml
format:
  revealjs:
    excalidraw:
      button: true
      shortcut: "`"
      template: "template.excalidraw"
      library: "library.excalidrawlib"
      langCode: "en"
      viewModeEnabled: false
      zenModeEnabled: false
      gridModeEnabled: false
      theme: "light"
      autoFocus: false
      useLocalStorage: true
revealjs-plugins:
  - excalidraw
resources:
  - template.excalidraw
  - library.excalidrawlib
```

| Option | Default | Description |
|--------|---------|-------------|
| `button` | `true` | Show toggle button in toolbar |
| `shortcut` | `` ` `` | Keyboard shortcut to toggle canvas |
| `template` | — | Path to `.excalidraw` file to preload |
| `library` | — | Path to `.excalidrawlib` file (e.g. Shinydraw) |
| `useLocalStorage` | `true` | Auto-save/restore drawings via browser localStorage |
| `theme` | `"light"` | `"light"` or `"dark"` |
| `langCode` | `"en"` | Language code for the UI |
| `viewModeEnabled` | `false` | Read-only view mode |
| `zenModeEnabled` | `false` | Minimal UI mode |
| `gridModeEnabled` | `false` | Show grid |
| `autoFocus` | `false` | Auto-focus canvas on open |

## Notes

- `useLocalStorage: true` keys drawings by the URL path, so drawings persist across reloads.
- Preload a drawing by pointing `template` at a `.excalidraw` JSON file and listing it under `resources`.
- Libraries (`.excalidrawlib`) let you import shape libraries like [Shinydraw](https://shinydraw.com).
- All Excalidraw props are documented at [docs.excalidraw.com](https://docs.excalidraw.com/docs/@excalidraw/excalidraw/api/props/).
