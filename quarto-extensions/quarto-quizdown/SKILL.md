---
name: quarto-quizdown
description: >
  Use when a user wants to embed interactive quizzes in a Quarto HTML document
  using the quarto-quizdown extension and quizdown markdown syntax.
---

## Overview

**quarto-quizdown** embeds interactive quizzes in Quarto HTML documents using [quizdown-js](https://github.com/bonartm/quizdown-js). Quizzes are written in fenced code blocks with class `quizdown`. Supports multiple-choice, single-choice, and sequencing question types.

Install with:
```bash
quarto add parmsam/quarto-quizdown
```

Requires Quarto >= 1.6.0 and an HTML-based output format. Works with standard HTML documents and RevealJS presentations (both are `html:js` formats). Does not work with PDF or other non-HTML formats.

## Document Setup

Add to the YAML header:

```yaml
filters:
  - quizdown
```

## Writing Quizzes

Wrap quiz content in a `quizdown` fenced code block. Each question starts with a `#` heading.

**Multiple-choice** (bullet list, one correct answer):
````markdown
```quizdown
# What is the capital of France?

- [ ] Berlin
- [x] Paris
- [ ] Madrid
- [ ] Rome
```
````

**Single-choice** (numbered list, one correct answer):
````markdown
```quizdown
# Which planet is closest to the Sun?

1. [ ] Earth
2. [x] Mercury
3. [ ] Venus
```
````

**Sequencing** (put items in the correct order):
````markdown
```quizdown
# Order the steps of the scientific method.

1. Ask a question
2. Form a hypothesis
3. Conduct an experiment
4. Analyze results
```
````

## Quiz-level Configuration

Add a YAML block at the top of the code block to configure appearance and behavior:

````markdown
```quizdown
---
primary_color: orange
secondary_color: lightgray
text_color: black
shuffle_questions: false
shuffle_answers: true
---

# Question goes here...
```
````

## Hints and Rich Content

**Hints** — shown to the user on demand, using blockquote syntax:
```
# What does CPU stand for?

> Think about what a processor does in a computer.

- [x] Central Processing Unit
- [ ] Core Performance Unit
```

**Rich content** supported inside questions:
- Inline code: `` `code` ``
- Code blocks: triple tildes ` ~~~python `
- Images: `![alt](url)`
- LaTeX math: `$$E = mc^2$$`

## Multiple Quizzes in One Document

Use multiple `quizdown` code blocks — each renders as a separate independent quiz:

````markdown
```quizdown
# Quiz 1, Question 1
...
```

Some prose in between.

```quizdown
# Quiz 2, Question 1
...
```
````
