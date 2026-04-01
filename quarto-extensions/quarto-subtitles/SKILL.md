---
name: quarto-subtitles
description: >
  Use when a user wants to add live speech-recognition subtitles to a Quarto
  RevealJS presentation using the quarto-subtitles extension.
---

## Overview

**quarto-subtitles** adds real-time speech-recognition subtitles to RevealJS presentations. Recognized speech appears in a subtitle bar at the bottom of each slide, useful for accessibility and live presentations.

Install with:
```bash
quarto add parmsam/quarto-subtitles
```

Requires RevealJS output format. **Browser support**: Chrome or Safari (Web Speech API). Microphone permission required.

## Basic Setup

```yaml
title: "My Presentation"
format:
  revealjs: default
revealjs-plugins:
  - subtitles
```

Default toggle key: `t`.

## Configuration

```yaml
format:
  revealjs:
    subtitles:
      toggleKey: "s"
      isVisible: true
revealjs-plugins:
  - subtitles
```

| Option | Default | Description |
|--------|---------|-------------|
| `toggleKey` | `"t"` | Key to show/hide the subtitle bar |
| `isVisible` | `true` | Whether subtitles are visible on load |

## Downloading Subtitles

Add a button with class `subtitles-dl-btn` anywhere in the presentation to let users download the recognized transcript as a text file:

```html
<button type="button" class="subtitles-dl-btn">Download Subtitles</button>
```

## Notes

- Firefox does not support the Web Speech API and will not work.
- Subtitles are recognized continuously — they do not reset between slides.
- The transcript download captures everything recognized during the session.
