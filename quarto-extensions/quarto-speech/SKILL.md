---
name: quarto-speech
description: >
  Use when a user wants to add voice-controlled slide navigation to a Quarto
  RevealJS presentation using the quarto-speech extension.
---

## Overview

**quarto-speech** adds voice-command navigation to RevealJS presentations. Say built-in phrases (or custom keywords) to move between slides hands-free.

Install with:
```bash
quarto add parmsam/quarto-speech
```

Requires RevealJS output format. **Browser support**: Chrome or Safari (Web Speech API). Microphone permission required.

## Basic Setup

```yaml
title: "My Presentation"
format:
  revealjs: default
revealjs-plugins:
  - speech
```

## Default Voice Commands

| Phrase | Action |
|--------|--------|
| "Next slide" | Go to next slide |
| "Previous slide" | Go to previous slide |
| "Last slide" | Go to last slide |
| "First slide" | Go to first slide |
| "Go to slide number" | Prompt for slide number |

## Custom Keywords

Keywords must be **single words** (no spaces):

```yaml
format:
  revealjs:
    speech:
      nextKeyword: "gotonext"
      prevKeyword: "gotoprevious"
      lastKeyword: "gotolast"
      firstKeyword: "gotofirst"
      numberKeyword: "gotoslidenumber"
revealjs-plugins:
  - speech
```

## Per-Slide Custom Phrase

Use `data-speech-next` on a slide to trigger "next" when a specific word is said:

```html
<section data-speech-next="movingalong">
  ...
</section>
```

## Fragment Control

Use `data-speech` on a fragment element to reveal it when a specific word is recognized:

```html
<li class="fragment" data-speech="spaghetti">Spaghetti</li>
```

## Notes

- Firefox does not support the Web Speech API.
- "Go to slide number" uses 1-based numbering.
- Custom keywords replace the default phrases — users must say the new word, not the original phrase.
