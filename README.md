# Claude Skills

A personal collection of Claude Skills for using my open-source projects.

Claude Skills extend Claude's capabilities with specialized knowledge and workflows. Skills are automatically activated by Claude based on your task and can be used in Claude.ai, Claude Code, or via the Claude API. Learn more at the [Claude Skills documentation](https://support.claude.com/en/articles/12512180-using-skills-in-claude).

## Available Skills

### Quarto Extensions

Skills for using parmsam's Quarto extensions in your documents and presentations.

- **[quarto-quiz](./quarto-extensions/quarto-quiz/)** - Add interactive multiple-choice quiz slides to Quarto RevealJS presentations
- **[quarto-flashcards](./quarto-extensions/quarto-flashcards/)** - Create interactive flip-card study presentations in Quarto RevealJS
- **[quarto-quizdown](./quarto-extensions/quarto-quizdown/)** - Embed interactive quizzes in Quarto HTML documents using quizdown syntax
- **[quarto-excalidraw](./quarto-extensions/quarto-excalidraw/)** - Embed an Excalidraw whiteboard canvas in Quarto RevealJS presentations
- **[quarto-github-corner](./quarto-extensions/quarto-github-corner/)** - Add a GitHub corner SVG badge to Quarto HTML documents
- **[quarto-brainmade](./quarto-extensions/quarto-brainmade/)** - Add the Brainmade (human-made) mark via shortcode
- **[quarto-tts](./quarto-extensions/quarto-tts/)** - Text-to-speech narration for RevealJS using the Web Speech API
- **[quarto-subtitles](./quarto-extensions/quarto-subtitles/)** - Live speech-recognition subtitles for RevealJS presentations
- **[quarto-speech](./quarto-extensions/quarto-speech/)** - Voice-controlled slide navigation for RevealJS presentations
- **[quarto-webcam](./quarto-extensions/quarto-webcam/)** - Embed a live webcam video feed in RevealJS slides

## Installation

### Using `npx skills add` (Any Agent)

Install skills from this repository into any supported coding agent (Claude Code, Codex, Cursor, Cline, and [many more](https://github.com/vercel-labs/skills)) using the `npx skills add` CLI:

```bash
# List available skills without installing
npx skills add parmsam/skills --list

# Install skills via an interactive menu
npx skills add parmsam/skills --all

# Install specific skills by category name
npx skills add parmsam/skills --skill quarto-quiz --skill quarto-flashcards

# Install to Claude Code only, globally
npx skills add parmsam/skills --agent claude-code --global
```

### Claude Code

#### Method 1: Add Marketplace

Add this repository as a plugin marketplace in Claude Code:

```
/plugin marketplace add parmsam/skills
```

Then browse and install skill categories through the Claude Code UI.

#### Method 2: Direct Installation

Install specific skill categories directly:

```
/plugin install quarto-extensions@parmsam-skills
```

#### Method 3: Manual Installation

1. Clone this repository:

   ```bash
   git clone https://github.com/parmsam/skills.git
   cd skills
   ```

2. Copy an individual skill to your Claude Code skills directory:

   ```bash
   cp -r quarto-extensions/<skill-name> ~/.config/claude-code/skills/
   ```

3. Or install all skills from a category:

   ```bash
   for skill in quarto-extensions/*/; do
     cp -r "$skill" ~/.config/claude-code/skills/
   done
   ```

### Claude.ai

Skills can be uploaded to Claude.ai following the [Creating Custom Skills guide](https://support.claude.com/en/articles/12512198-creating-custom-skills).

### Claude API

Use the [Skills API](https://docs.claude.com/en/api/skills-guide) to programmatically load and manage skills in your applications.

## Using Skills

Once installed, Claude will automatically activate relevant skills based on your task — no explicit invocation needed.

## Skill Categories

| Category | Description |
|----------|-------------|
| **quarto-extensions** | Skills for using parmsam's Quarto extensions (quiz, flashcards, quizdown, excalidraw, github-corner, brainmade, tts, subtitles, speech, webcam) |

## Resources

- [Claude Skills Overview](https://www.anthropic.com/news/skills)
- [Using Skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)
- [Creating Custom Skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)
- [Skills API Documentation](https://docs.claude.com/en/api/skills-guide)
- [Anthropic's Official Skills Repository](https://github.com/anthropics/skills)
