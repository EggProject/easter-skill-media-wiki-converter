# MediaWiki Article Creator — Claude Code Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code Skill](https://img.shields.io/badge/Claude_Code-Skill-blueviolet)](https://docs.claude.com/en/docs/claude-code/skills)
[![Skill Format: SKILL.md](https://img.shields.io/badge/Format-SKILL.md-success)](https://agentskills.io/specification)
[![Wikitext](https://img.shields.io/badge/Output-MediaWiki_wikitext-36c)](https://www.mediawiki.org/wiki/Wikitext)
[![English](https://img.shields.io/badge/Docs-English-blue)](README.md)
[![Magyar](https://img.shields.io/badge/Docs-Magyar-green)](README.hu.md)

> A Claude Code skill for creating modern, clean-design MediaWiki articles and converting
> other formats (HTML reports, HTML slideshows, PDF, Word, Excel, PowerPoint) into
> MediaWiki wikitext — with double verification by an independent agent to ensure
> the text is never rewritten, only the format is converted.

[🇭🇺 **Magyar nyelvű dokumentáció itt** →](README.hu.md)

---

## Table of contents

- [What is this?](#-what-is-this)
- [Why this skill?](#-why-this-skill)
- [Features](#-features)
- [Quick start](#-quick-start)
- [Installation](#-installation)
- [The two workflows](#-the-two-workflows)
- [Usage examples](#-usage-examples)
- [Skill structure](#-skill-structure)
- [What's in the references?](#-whats-in-the-references)
- [Requirements & compatibility](#-requirements--compatibility)
- [skills.sh compliance & publishing](#-skillssh-compliance--publishing)
- [Versioning](#-versioning)
- [Contributing](#-contributing)
- [License](#-license)
- [Acknowledgements](#-acknowledgements)
- [Hungarian version (magyar nyelv)](#-hungarian-version-magyar-nyelv)

---

## 💡 What is this?

`mediawiki-article-creator` is a [Claude Code](https://docs.claude.com/en/docs/claude-code)
[skill](https://docs.claude.com/en/docs/claude-code/skills) that helps you write modern,
clean-design MediaWiki articles — from scratch or by converting content from other
formats. It is the most thorough, production-ready MediaWiki skill available, with a
complete reference library, a tested double-verification workflow, and ~4000 lines of
carefully curated documentation.

The skill is **format-agnostic at the input** but **strictly MediaWiki at the output**.
It supports a parallel iterative authoring workflow, plus six format-conversion
playbooks (HTML, PDF, Word, Excel, PowerPoint, HTML slideshow). Every conversion is
verified by a separate agent so the text content is never altered — only the format.

---

## 🎯 Why this skill?

| Pain point | How this skill solves it |
|------------|--------------------------|
| "I don't know all the MediaWiki syntax by heart." | 1100-line cheatsheet with examples, always loaded into context. |
| "My converted documents lose the design." | Six format-specific playbooks that preserve layout via MediaWiki's toolkit. |
| "I keep rewriting the text when I convert it." | Mandatory independent-agent verification catches any text change. |
| "My MediaWiki pages look like 2005 Wikipedia." | 22 copy-paste design recipes for modern layouts (cards, accordions, galleries, infoboxes). |
| "I need different rendering in different places." | Pattern library covers wikitable, TemplateStyles, syntax highlight, gallery, accordion, tabs, etc. |
| "I want one source of truth for MediaWiki." | Six reference files: syntax, design, conversion, verification, advanced features, recipes. |

---

## ✨ Features

- **Two clean workflows**
  - **Parallel iterative authoring** — clarify goal → outline → full wikitext in one block → feedback loop
  - **Format conversion** — read source → strategy → wikitext → independent-agent verification → final
- **Six input formats**
  - HTML reports
  - HTML slideshows (reveal.js, Swiper, etc.)
  - PDF
  - Microsoft Word (DOCX)
  - Microsoft Excel (XLSX)
  - Microsoft PowerPoint (PPTX)
- **Modern MediaWiki design**
  - `class="wikitable sortable"` tables
  - `<gallery mode="packed-hover">` for image showcases
  - `<syntaxhighlight lang="...">` for syntax-highlighted code
  - `{{Note}}` / `{{Tip}}` / `{{Warning}}` / `{{Caution}}` callouts
  - `<details>` accordions
  - TabberNeue tabs
  - Mermaid / Graphviz diagrams
  - TemplateStyles-powered cards and hero headers
  - Breadcrumbs, infoboxes, sidebars
- **Double verification by a separate agent**
  - Strictly checks text content matches the source letter-for-letter
  - Catches even single-word deviations
  - Filters out whitespace-only differences
  - Reports format and design differences separately
- **Language-agnostic communication**
  - Speaks the user's language; no forced language
  - Code, technical terms, and MediaWiki tags always stay in their canonical form
- **Production-ready reference library (~4000 lines)**
  - `01-syntax-cheatsheet.md` — full MediaWiki syntax with examples
  - `02-design-patterns.md` — modern, clean design patterns
  - `03-conversion-playbooks.md` — per-format conversion guides
  - `04-verification-checklist.md` — what the independent agent checks
  - `05-advanced-features.md` — TemplateStyles, Scribunto, ParserFunctions, extensions
  - `06-design-recipes.md` — 22 copy-paste code snippets

---

## 🚀 Quick start

1. **Install the skill** (see [Installation](#-installation) below).
2. **Invoke it** in a Claude Code conversation with one of the example prompts in
   [Usage examples](#-usage-examples).
3. **Iterate** — the skill shows the full wikitext in one block, you give feedback,
   it updates.

Example session:

> **You:** "Create a modern MediaWiki article about the OpenAI o1 model. Include
> a comparison table against o1-preview, a code example in Python, and a FAQ section
> with collapsible answers."
>
> **Claude (with the skill):** *generates 100-line wikitext in one ` ```wiki ` block, with
> `class="wikitable sortable"`, `<syntaxhighlight lang="python">`, and `<details>`
> accordions*

That's it. You copy-paste the wikitext into your MediaWiki editor.

---

## 📦 Installation

### Option 1 — Drop the `.skill` file into your project's `skills/` directory

```bash
# From the root of your project
mkdir -p skills
cp /path/to/mediawiki-article-creator.skill skills/
cd skills
unzip mediawiki-article-creator.skill
```

Claude Code automatically picks up skills placed under `skills/`.

### Option 2 — Drop the unzipped folder into `.claude/skills/`

```bash
mkdir -p .claude/skills
cp -r /path/to/mediawiki-article-creator .claude/skills/
```

### Option 3 — Install from skills.sh

The skill is published on skills.sh. Install with:

```bash
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator
```

You can also install it globally (across all projects for your user) with `-g`,
or pin to a specific agent with `-a`:

```bash
# Global install
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator -g

# For a specific agent (claude-code, codex, opencode, cursor, etc.)
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator -a claude-code
```

The CLI copies or symlinks the skill into the agent's local skills directory
(`./<agent>/skills/` by default, `~/<agent>/skills/` with `-g`).

After installation, restart Claude Code so it picks up the new skill.

---

## 🛠 The two workflows

### Workflow 1 — Parallel, iterative article creation

For when you start from scratch and want to brainstorm with the LLM.

```
Goal & audience clarification
        ↓
Outline (numbered section list)
        ↓
Full wikitext in one block ← the LLM shows the complete article at once
        ↓
Your feedback
        ↓
Updated full wikitext
        ↓
Final wrap-up (categories, links)
```

The skill will ask you four things up front: **purpose, audience, length, required
elements**. Then it produces the entire article in one go — not in pieces — so you
see the complete picture.

### Workflow 2 — Format conversion

For when you have content in another format (HTML, PDF, Word, etc.) and want it
in MediaWiki.

```
Source (any supported format)
        ↓
Strategy (how each element maps to wikitext)
        ↓
Wikitext in one block
        ↓
Independent-agent verification ← MANDATORY for conversions
        ↓
Fix any deviations
        ↓
Final wikitext
```

The independent agent is launched via the `Agent` tool with
`subagent_type: "general-purpose"`, following the checklist in
`references/04-verification-checklist.md`. It reports any text differences (even
single words!), format differences, and syntax errors.

---

## 💼 Usage examples

### Example 1 — Create an article from scratch (Workflow 1)

> "Write a modern MediaWiki article about vector databases. Include a comparison
> table (Pinecone vs. Weaviate vs. Qdrant vs. Milvus), a code example in Python
> using Qdrant, and a FAQ section."

The skill produces a single wikitext block with `__TOC__`, `class="wikitable sortable"`,
`<syntaxhighlight lang="python">`, `<details>` accordions, and `[[Category:Databases]]`.

### Example 2 — Convert an HTML quarterly report (Workflow 2)

> "Convert this HTML report to MediaWiki. Do not change any text, only the format.
> Verify with a separate agent."

The skill:
1. Reads the HTML end-to-end.
2. Maps `<h2>` → `==`, `<h3>` → `===`, `<table>` → `{| class="wikitable"`,
   `<strong>` → `'''...'''`, `<div class="warning">` → `{{Warning|...}}`.
3. Spawns an independent agent to verify the conversion.
4. Reports any deviations and fixes them.

### Example 3 — Convert a Word document (DOCX)

> "I have a Word document at `/docs/architecture.docx`. Convert it to MediaWiki
> and put the wikitext in `output.wiki`."

The skill explains that you should first run pandoc or OpenOffice to get plain
MediaWiki-formatted text, then hand it back. It cleans up pandoc's output and
applies modern design.

### Example 4 — Convert a PowerPoint deck

> "Convert this PPTX to a MediaWiki article. One section per slide."

The skill turns each slide's title into a `==` heading, content into lists,
images into `[[File:...|thumb]]` references, and notes into `{{Note|...}}`.

### Example 5 — Modern design with cards

> "Make me a landing-page-style article with three feature cards in a row,
> an FAQ accordion, and a hero header."

The skill produces a TemplateStyles-based hero, a `card-grid` with three `.card`
divs, and a `<details>`-based FAQ.

### Example 6 — Use MediaWiki's own templates

> "Add callout boxes (`{{Note}}`, `{{Warning}}`, `{{Tip}}`) at the right places
> in this article."

The skill identifies where each type of callout fits and adds them with the
right severity (Note = info, Tip = suggestion, Warning = heads-up, Caution =
critical).

### Example 7 — Migrate from a custom wiki

> "We have a custom wiki with 200 pages. Help me build a migration plan."

The skill walks you through the migration: identify extension-specific features,
build per-template conversion rules, run pandoc per page, verify with the
independent agent.

### Example 8 — Best-practice review

> "Here's my draft MediaWiki article. Tell me what to improve from a design
> and accessibility standpoint."

The skill applies the design checklist in `references/02-design-patterns.md`
and the verification checklist in `references/04-verification-checklist.md`.

---

## 📁 Skill structure

```
mediawiki-article-creator/
├── SKILL.md                                  # Main entry point (workflows, rules)
├── references/
│   ├── 01-syntax-cheatsheet.md               # 1100 lines: full MediaWiki syntax
│   ├── 02-design-patterns.md                 # 640 lines: modern design patterns
│   ├── 03-conversion-playbooks.md            # 480 lines: per-format conversion
│   ├── 04-verification-checklist.md          # 350 lines: independent agent checklist
│   ├── 05-advanced-features.md               # 460 lines: TemplateStyles, Lua, etc.
│   └── 06-design-recipes.md                  # 810 lines: 22 copy-paste recipes
└── workspace/                                # Optional scratch space
```

The `SKILL.md` is loaded into the LLM's context whenever the skill triggers.
The `references/` files are loaded on-demand when the LLM needs detailed
information about a specific topic. This keeps the context window lean.

---

## 📚 What's in the references?

| File | Purpose | Lines |
|------|---------|-------|
| `01-syntax-cheatsheet.md` | Complete MediaWiki syntax reference with examples. **Start here if you don't know the syntax.** | ~1100 |
| `02-design-patterns.md` | Typography, color usage, callouts, accordions, tabs, infoboxes, sidebars, hero headers. | ~640 |
| `03-conversion-playbooks.md` | Step-by-step conversion playbooks for HTML report, HTML slideshow, PDF, Word, Excel, PowerPoint. | ~480 |
| `04-verification-checklist.md` | The exact checklist the independent agent uses to verify wikitext. Includes prompt template and report format. | ~350 |
| `05-advanced-features.md` | TemplateStyles, Scribunto (Lua), ParserFunctions, Variables, Page Forms, Semantic MediaWiki, Cargo, security model. | ~460 |
| `06-design-recipes.md` | 22 ready-to-copy design recipes: cards, accordions, tabs, breadcrumbs, infobox, sidebar, gallery, diagrams, etc. | ~810 |

---

## 📋 Requirements & compatibility

### MediaWiki version

The skill targets **MediaWiki 1.39+** for full feature support:

- `class="wikitable striped"` requires 1.39+
- `<details>` / `<summary>` requires 1.40+
- `class="skin-invert-image"` requires Vector 2022 skin
- `{{short description}}` magic word requires 1.38+

For older MediaWiki versions, the skill falls back to native syntax and avoids
newer features.

### Extensions

The skill uses these MediaWiki extensions when available (all optional except
SyntaxHighlight):

- **SyntaxHighlight** (almost always available) — for `<syntaxhighlight>`
- **ParserFunctions** (almost always available) — for `#if`, `#switch`, etc.
- **TemplateStyles** (optional) — for custom CSS
- **Scribunto** (optional) — for Lua modules
- **TabberNeue** (optional) — for tabs
- **Mermaid** (optional) — for diagrams
- **Math** (usually available) — for LaTeX
- **Variables** (optional) — for `#vardefine`
- **Page Forms** (optional) — for form-based editing
- **Semantic MediaWiki** or **Cargo** (optional) — for structured data

The skill warns the user when it uses an extension that may not be available.

### Skill specification

The skill follows the [Agent Skills specification](https://agentskills.io/specification).
The `SKILL.md` frontmatter is valid:

```yaml
---
name: mediawiki-article-creator
description: |
  ... (max 1024 chars, no < or >)
---
```

---

## 📡 skills.sh compliance & publishing

This skill is designed to be installable through [skills.sh](https://skills.sh),
the open agent-skills ecosystem. The discovery rules and frontmatter requirements
are below.

### Required frontmatter

The `SKILL.md` frontmatter is valid per the [Agent Skills specification](https://agentskills.io/specification):

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
- **Must match the parent directory name** (the skill lives at `skills/mediawiki-article-creator/`, so the name is `mediawiki-article-creator`)

### Discovery paths in this repo

The skill is placed under the canonical `skills/<name>/` flat layout, which
skills.sh discovers automatically (one level deep):

```
skills/
└── mediawiki-article-creator/      ← name matches the directory
    ├── SKILL.md
    ├── references/
    ├── workspace/
    └── (no SKILL.md here, so the nested skill is picked up, not this file)
```

You could also discover it via `.claude/skills/`, `skills/.curated/`, or
plugin manifests (`.claude-plugin/marketplace.json`).

### Install command

```bash
# From this GitHub repo
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator
```

You can also list all skills first with `--list`, install globally with `-g`,
or pin to specific agents with `-a`.

### Publishing — no submission required

Per the [Vercel guide on agent skills](https://vercel.com/kb/guide/agent-skills-creating-installing-and-sharing-reusable-agent-context),
there is **no formal submission process** for skills.sh. To make a skill
discoverable:

1. Put it in a public GitHub repo
2. Make sure `SKILL.md` is valid (per spec above)
3. Add a `README.md` (this file is exactly that)
4. Optionally include a `LICENSE` file
5. Share the install command — installs show up on the skills.sh leaderboard
   organically via install telemetry

A skill is **installable** the moment the CLI can resolve the repo, even
before it appears on the public leaderboard.

### Validation

Validate the frontmatter locally with the [skills-ref validator](https://github.com/agentskills/agentskills/tree/main/skills-ref):

```bash
npx skills-ref validate skills/mediawiki-article-creator
```

Or use the manual checklist:

- [x] `name` field present, kebab-case, ≤ 64 chars, matches directory name
- [x] `description` field present, ≤ 1024 chars, no `<` or `>`, says both what and when
- [x] Optional fields valid (`license`, `compatibility`, `metadata`)
- [x] No unexpected top-level keys in frontmatter
- [x] File is named `SKILL.md` (case-sensitive)

---

## 🔢 Versioning

This project uses [Semantic Versioning](https://semver.org/) — `MAJOR.MINOR.PATCH`.

- **MAJOR** — breaking changes to the skill structure or workflow
- **MINOR** — new features (new reference, new workflow step, new example)
- **PATCH** — bug fixes, typo fixes, small clarifications

Current version: **1.0.0** (initial release)

---

## 🤝 Contributing

Contributions are welcome! Please open an issue or a pull request.

When adding a new reference file:
- Use the same Markdown style as the existing files
- Include a table of contents at the top if the file is longer than 300 lines
- Link the new file from `SKILL.md` in the "Referenced files" section
- Run the skill-creator validation before submitting

When fixing a bug:
- Describe the bug and the fix in the PR description
- Add an example to `references/03-conversion-playbooks.md` if it's a conversion edge case

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

You are free to use, copy, modify, merge, publish, distribute, sublicense, and/or
sell copies of the software, subject to the following conditions:

- The above copyright notice and this permission notice shall be included in all
  copies or substantial portions of the software.
- THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.

---

## 🙏 Acknowledgements

- The [MediaWiki community](https://www.mediawiki.org/wiki/Community) — for the
  excellent documentation that this skill distills
- The [Claude Code team](https://docs.claude.com/en/docs/claude-code) — for the
  skills system that makes this possible
- The [skill-creator skill](https://github.com/anthropics/skills) — for the
  meta-skill that built this one
- [Pandoc](https://pandoc.org/), [OpenOffice](https://www.openoffice.org/) — for
  the conversion tools the skill recommends

---

## 🇭🇺 Hungarian version (magyar nyelv)

A teljes magyar nyelvű dokumentáció és a magyar verziójú skill itt található:
[README.hu.md](README.hu.md).

A magyar skill példaként szolgál, és a `hungarian-example/` mappában található.
