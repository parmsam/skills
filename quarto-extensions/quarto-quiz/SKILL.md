---
name: quarto-quiz
description: >
  Use when a user wants to add interactive multiple-choice quiz slides to a
  Quarto RevealJS presentation using the quarto-quiz extension.
---

## Overview

**quarto-quiz** adds interactive multiple-choice quiz slides to Quarto RevealJS presentations. Users get immediate visual feedback, optional per-answer explanations, and optional scoring.

Install with:
```bash
quarto add parmsam/quarto-quiz
```

Requires Quarto >= 1.4.0 and RevealJS output format.

## Document Setup

Add to the YAML header:

```yaml
format:
  revealjs:
    quiz:
      checkKey: 'c'
      resetKey: 'q'
      shuffleKey: 't'
      allowNumberKeys: true
      shuffleOptions: true
      includeScore: false
revealjs-plugins:
  - quiz
```

## Writing Quiz Slides

**Single-choice question** — one correct answer:
```markdown
## What is the capital of France? {.quiz-question}

- Berlin
- [Paris]{.correct}
- Madrid
- Rome
```

**Multiple-choice question** — select all that apply:
```markdown
## Which of these are programming languages? {.quiz-question .quiz-multiple}

- [Python]{.correct}
- [JavaScript]{.correct}
- [HTML]{data-explanation="HTML is a markup language, not a programming language."}
- CSS
```

**With explanations** — shown after checking the answer:
```markdown
## What is the area of a circle? {.quiz-question}

- [$2\pi r$]{data-explanation="That's the circumference, not the area."}
- [$[\pi r^2$]{.correct}
- [$\pi d$]{data-explanation="That's the circumference using diameter."}
```

**Question in the slide body, not the title** — use when the question is too long for a heading, needs code/content before it, or you'd rather leave the title blank or generic:
```markdown
## {.quiz-question}
**What is the name of Ross and Carol's son in the TV show *"Friends"?***

- [Jack]{data-explanation="Jack is the name of Monica and Ross's father."}
- [Ben]{.correct}
- [Chandler]{data-explanation="Chandler is another main character."}
```

The title can also stay short/generic while the real question sits in the body — e.g. a `## Python lists {.quiz-question}` heading followed by a code block and then "What is the value of x[2]?" as body text above the options. The `{.quiz-question}` class on the heading is what makes the slide a quiz slide either way — the heading text itself is just a label and can be empty, generic, or the full question.

## Key Rules

- Slide must have `{.quiz-question}` class to be treated as a quiz slide — the heading text is optional; the question can instead (or also) live in the slide body
- Wrap the correct answer(s) with `[text]{.correct}`
- Add `{.quiz-multiple}` for select-all-that-apply questions
- Add `{data-explanation="..."}` to any option to show a hint after checking
- Options support images (`![](url){width=90px}`) and LaTeX math (`$\pi r^2$`) as well as plain text
- Non-quiz slides in the same deck work normally

## Configuration Options

| Option | Default | Description |
|--------|---------|-------------|
| `checkKey` | `'c'` | Keyboard shortcut to check answer |
| `resetKey` | `'q'` | Keyboard shortcut to reset question |
| `shuffleKey` | `'t'` | Keyboard shortcut to shuffle slide order |
| `allowNumberKeys` | `true` | Press 1–4 to select options |
| `disableOnCheck` | `false` | Lock options after checking |
| `disableReset` | `false` | Hide the reset button |
| `shuffleOptions` | `true` | Randomize option order on load |
| `defaultCorrect` | `""` | Custom correct feedback message |
| `defaultIncorrect` | `""` | Custom incorrect feedback message |
| `includeScore` | `false` | Show running score on slides |
