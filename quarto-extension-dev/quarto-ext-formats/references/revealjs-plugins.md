# RevealJS Plugin Extensions

Read this when helping a user build a Quarto RevealJS plugin extension.

## Scaffold

```bash
quarto create extension revealjs-plugin
```

Generates:

```
_extensions/
  <name>/
    _extension.yml
    <name>.js           # plugin JavaScript
    <name>.css          # plugin styles (optional)
example.qmd             # demo presentation
```

## _extension.yml

```yaml
title: My Plugin
author: Your Name
version: 1.0.0
quarto-required: ">=1.2.0"
contributes:
  revealjs-plugins:
    - name: MyPlugin
      script: my-plugin.js
      stylesheet: my-plugin.css   # optional
```

The `name` under `revealjs-plugins` must match the plugin name registered in the JavaScript.

## Plugin JavaScript Structure

Quarto RevealJS plugins follow the Reveal.js plugin API:

```javascript
window.MyPlugin = {
  id: 'MyPlugin',

  init: function(deck) {
    // deck is the Reveal.js instance
    // access config options:
    var config = deck.getConfig().myPlugin || {};
    var myOption = config.myOption || 'default';

    // listen to Reveal events:
    deck.on('slidechanged', function(event) {
      // event.currentSlide, event.previousSlide, event.indexh, event.indexv
    });

    deck.on('ready', function(event) {
      // presentation is fully initialized
    });
  }
};
```

Key Reveal.js deck methods available in plugins:

```javascript
deck.getConfig()              // all config options including custom keys
deck.getCurrentSlide()        // current slide DOM element
deck.getSlides()              // all slide elements
deck.getIndices()             // { h, v, f } current position
deck.on(event, callback)      // subscribe to events
deck.off(event, callback)     // unsubscribe
deck.setState(state)          // navigate programmatically
deck.slide(h, v, f)           // navigate to position
deck.next() / deck.prev()     // navigate
deck.isReady()                // boolean
```

Common Reveal events: `ready`, `slidechanged`, `slidetransitionend`, `fragmentshown`, `fragmenthidden`, `overviewshown`, `overviewhidden`, `paused`, `resumed`.

## User Configuration

Expose configurable options via a distinct key in the presentation YAML:

```yaml
# In user's document:
format:
  revealjs: default
revealjs-plugins:
  - my-org/my-plugin

# Plugin options go under format.revealjs:
format:
  revealjs:
    my-plugin:
      myOption: true
      speed: 0.5
```

Access in JS via `deck.getConfig().myPlugin`.

## CSS Approach

Position overlays and decorations with CSS fixed/absolute positioning:

```css
/* my-plugin.css */
.my-plugin-overlay {
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 100;
  pointer-events: none;
}

/* Respect Reveal's print/PDF mode */
@media print {
  .my-plugin-overlay {
    display: none;
  }
}
```

## Packaging Existing Reveal.js Plugins

To wrap an existing 3rd-party Reveal.js plugin as a Quarto extension:

1. Copy the plugin JS (and CSS) into `_extensions/<name>/`
2. Ensure the plugin registers itself on `window` (e.g., `window.RevealHighlight = ...`)
3. Reference it in `_extension.yml` under `revealjs-plugins`
4. The `name` field must match the plugin's registered name

See [awesome-reveal](https://github.com/hakimel/reveal.js/wiki/Plugins,-Tools-and-Hardware) for community plugins worth wrapping.

## Examples

- [quarto-ext/pointer](https://github.com/quarto-ext/pointer) — cursor switcher (simple, good reference)
- [quarto-ext/attribution](https://github.com/quarto-ext/attribution) — slide edge text (CSS + JS)
- [quarto-ext/fontawesome](https://github.com/quarto-ext/fontawesome) — icon shortcode (not RevealJS-specific but shows multi-type extension)
