---
name: quarto-tts
description: >
  Use when a user wants to add text-to-speech narration to a Quarto RevealJS
  presentation using the quarto-tts extension and the Web Speech API.
---

## Overview

**quarto-tts** adds browser-based text-to-speech narration to RevealJS presentations using the Web Speech API. Slide content is read aloud automatically on each slide change.

Install with:
```bash
quarto add parmsam/quarto-tts
```

Requires RevealJS output format and a browser that supports the Web Speech API (Chrome recommended).

## Basic Setup

```yaml
title: "My Presentation"
format:
  revealjs: default
revealjs-plugins:
  - tts
```

Default keyboard shortcuts: `t` toggles TTS on/off, `p` pauses/resumes, `q` cancels.

## Configuration

```yaml
format:
  revealjs:
    tts:
      cancelKey: "q"
      onOffKey: "t"
      playPauseKey: "p"
      dvIndex: 0
      dvRate: 0.85
      ttsOn: true
      cancel: true
      readVisElmts: true
      readFrags: false
      readNotes: false
revealjs-plugins:
  - tts
```

| Option | Default | Description |
|--------|---------|-------------|
| `cancelKey` | `"q"` | Key to cancel speech immediately |
| `onOffKey` | `"t"` | Key to toggle TTS on/off |
| `playPauseKey` | `"p"` | Key to pause/resume speech |
| `dvIndex` | `0` | Default voice index (browser voice list) |
| `dvRate` | `0.85` | Speech rate (0–2; 1 = normal speed) |
| `ttsOn` | `true` | Start with TTS enabled |
| `cancel` | `true` | Stop current speech on slide change |
| `readVisElmts` | `true` | Read visible slide elements |
| `readFrags` | `false` | Read fragment text as fragments appear |
| `readNotes` | `false` | Read speaker notes (`<aside class="notes">`) |

## Notes

- Voice index (`dvIndex`) maps to the browser's built-in voice list, which varies by OS and browser.
- Setting `dvRate` below 1 slows speech; above 1 speeds it up.
- `readFrags: true` is useful for step-by-step reveals where each bullet point should be narrated.
- `readNotes: true` reads speaker notes instead of (or in addition to) slide content.
