# Compatibility & requirements

The skill targets **MediaWiki 1.39+** for full feature support, and works
with any agent that follows the
[Agent Skills specification](https://agentskills.io/specification).

## MediaWiki version

| Feature | Minimum MediaWiki version |
|---------|---------------------------|
| `class="wikitable sortable"` | 1.27+ |
| `class="wikitable striped"` | 1.39+ |
| `<details>` / `<summary>` | 1.40+ |
| `class="skin-invert-image"` | Vector 2022 skin |
| `{{short description}}` magic word | 1.38+ |

For older MediaWiki versions, the skill falls back to native syntax and
avoids newer features. The skill checks the version it's talking to (you
can pass it explicitly) and downgrades gracefully.

## MediaWiki extensions

The skill uses these extensions when available. Only **SyntaxHighlight**
is assumed to be present everywhere; the rest are flagged if the skill
uses them in a given output.

| Extension | Required? | Used for |
|-----------|-----------|----------|
| **SyntaxHighlight** | Almost always available | `<syntaxhighlight>` blocks |
| **ParserFunctions** | Almost always available | `#if`, `#switch`, etc. |
| **TemplateStyles** | Optional | Custom CSS for cards, hero headers |
| **Scribunto** | Optional | Lua modules |
| **TabberNeue** | Optional | Tabs |
| **Mermaid** | Optional | Diagrams |
| **Math** | Usually available | LaTeX |
| **Variables** | Optional | `#vardefine` |
| **Page Forms** | Optional | Form-based editing |
| **Semantic MediaWiki** or **Cargo** | Optional | Structured data |

The skill warns you when it uses an extension that may not be available,
and offers a fallback (e.g. a plain wikitable instead of a TabberNeue tab,
a code block instead of syntax highlighting).

## Supported agent hosts

| Agent | Install method | Notes |
|-------|----------------|-------|
| Claude Code | `npx skills add` or `.claude/skills/` | Full feature support, including `Agent` tool for verification. |
| Claude Cowork | `npx skills add -a claude-code` | Same as Claude Code. |
| Claude web (Projects) | Paste SKILL.md into Project instructions | No `Agent` tool — verification is done by the main model. |
| OpenAI Codex | `npx skills add -a codex` | |
| OpenCode | `npx skills add -a opencode` | |
| Cursor | `npx skills add -a cursor` | |
| ChatGPT (Custom GPT) | Paste SKILL.md into Instructions | No `Agent` tool. |
| Gemini (Gems) | Paste SKILL.md into Instructions | No `Agent` tool. |

## Skill specification

The `SKILL.md` frontmatter is valid per the
[Agent Skills specification](https://agentskills.io/specification):

```yaml
---
name: mediawiki-article-creator
description: |
  ... (max 1024 chars, no < or >)
---
```

| Field | Required | Value |
|-------|----------|-------|
| `name` | yes | `mediawiki-article-creator` — must match the parent directory name |
| `description` | yes | ≤ 1024 chars, no `<` or `>`, describes what the skill does **and when to use it** |
| `license` | no | `MIT` (this project) |
| `compatibility` | no | `Designed for Claude Code (or similar products).` |
| `metadata` | no | `author: eggp`, `version: 1.0.0` |
| `allowed-tools` | no | not set (skill uses default tool set) |

### Name rules

- Lowercase letters, digits, and hyphens only
- 1-64 characters
- Must not start or end with a hyphen
- Must not contain consecutive hyphens
- **Must match the parent directory name** (the skill lives at
  `skills/mediawiki-article-creator/`, so the name is `mediawiki-article-creator`)

---

**Next:** [Publishing →](publishing.md) · [Back to README →](../README.md)
