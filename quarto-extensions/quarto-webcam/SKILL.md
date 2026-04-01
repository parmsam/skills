---
name: quarto-webcam
description: >
  Use when a user wants to embed a live webcam video feed into a Quarto RevealJS
  presentation using the quarto-webcam extension.
---

## Overview

**quarto-webcam** embeds a live webcam video overlay into RevealJS presentations. Useful for recording screencasts or presenting with a picture-in-picture camera feed.

Install with:
```bash
quarto add parmsam/quarto-webcam
```

Requires RevealJS output format and browser camera permission.

## Basic Setup

```yaml
title: "My Presentation"
format:
  revealjs: default
revealjs-plugins:
  - webcam
```

## Showing the Webcam on a Slide

Add the `data-video` attribute to a slide heading with size and position classes:

```markdown
# My Slide {data-video="small top-right"}
```

Click the video to cycle through available camera inputs.

## `data-video` Classes

**Size:**
| Class | Description |
|-------|-------------|
| `big` | ~90% of slide width |
| `small` | ~15% of slide width |

**Position:**
| Class | Description |
|-------|-------------|
| `top-left` | Top-left corner |
| `top-right` | Top-right corner |
| `bottom-left` | Bottom-left corner |
| `bottom-right` | Bottom-right corner |

Set `data-video="false"` to explicitly disable the webcam on a specific slide.

## Configuration

```yaml
format:
  revealjs:
    webcam:
      toggleKey: "c"
      enabled: false
      persistent: false
revealjs-plugins:
  - webcam
```

| Option | Default | Description |
|--------|---------|-------------|
| `toggleKey` | `"c"` | Key to toggle the webcam on/off |
| `enabled` | `false` | Start the camera stream on load |
| `persistent` | `false` | Keep camera stream active even when video is hidden |

## Custom Styling

Target `video.live-video.your-class` in CSS for custom styles. Example:

```css
video.live-video.small {
  border-radius: 50%;
  border: 3px solid white;
}
```

## Notes

- Combine size and position in `data-video`, e.g. `"big bottom-left"`.
- `persistent: true` avoids the camera re-initializing when toggling visibility, which can reduce flicker.
- Browser will prompt for camera permission on first use.
