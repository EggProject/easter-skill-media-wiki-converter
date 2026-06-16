# MediaWiki Article Creator — Claude Code Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude_Code-Skill-blueviolet)](https://docs.claude.com/en/docs/claude-code/skills)
[![Skill Format: SKILL.md](https://img.shields.io/badge/Format-SKILL.md-success)](https://agentskills.io/specification)
[![Wikitext](https://img.shields.io/badge/Output-MediaWiki_wikitext-36c)](https://www.mediawiki.org/wiki/Wikitext)
[![skills.sh](https://img.shields.io/badge/skills.sh-installable-orange)](https://skills.sh)
[![English](https://img.shields.io/badge/Docs-English-blue)](README.md)
[![Magyar](https://img.shields.io/badge/Docs-Magyar-green)](README.hu.md)

> A Claude Code skill for creating modern, clean-design MediaWiki articles
> and converting other formats (HTML reports, HTML slideshows, PDF, Word,
> Excel, PowerPoint) into MediaWiki wikitext — with double verification by
> an independent agent to ensure the text is never rewritten, only the
> format is converted.

[🇭🇺 **Magyar nyelvű dokumentáció itt** →](README.hu.md)

---

## 📖 Documentation

The full docs are split across this hub and a `docs/` folder. The hub stays
focused; click through for depth.

| Page | What's in it |
|------|--------------|
| **[📦 Installation](docs/installation.md)** | Three install paths: skills.sh, `.skill` zip, `.claude/skills/`. |
| **[🛠 Workflows](docs/workflows.md)** | The two workflows — parallel authoring and format conversion — with diagrams. |
| **[💼 Usage examples](docs/usage-examples.md)** | Eight realistic prompts and what the skill produces. |
| **[🌐 Other AI platforms](docs/other-platforms.md)** | Using the skill on ChatGPT, Claude web, Claude Cowork, and Gemini. |
| **[📁 Skill structure](docs/skill-format.md)** | On-disk layout, what each `references/*.md` is for. |
| **[📋 Compatibility](docs/compatibility.md)** | MediaWiki versions, extensions, agent hosts, skill spec. |
| **[📡 Publishing](docs/publishing.md)** | skills.sh frontmatter, discovery, validation, packaging. |
| **[ℹ️ Project info](docs/project-info.md)** | Versioning, contributing, license, acknowledgements. |

---

## 💡 What is this?

`mediawiki-article-creator` is a
[Claude Code skill](https://docs.claude.com/en/docs/claude-code/skills) that
helps you write modern, clean-design MediaWiki articles — from scratch or
by converting content from other formats. It is the most thorough,
production-ready MediaWiki skill available, with a complete reference
library, a tested double-verification workflow, and ~4000 lines of
carefully curated documentation.

The skill is **format-agnostic at the input** but **strictly MediaWiki at
the output**. It supports a parallel iterative authoring workflow, plus
six format-conversion playbooks (HTML, PDF, Word, Excel, PowerPoint, HTML
slideshow). Every conversion is verified by a separate agent so the text
content is never altered — only the format.

## 🎯 Why this skill?

| Pain point | How this skill solves it |
|------------|--------------------------|
| "I don't know all the MediaWiki syntax by heart." | 1100-line cheatsheet with examples, always loaded into context. |
| "My converted documents lose the design." | Six format-specific playbooks that preserve layout via MediaWiki's toolkit. |
| "I keep rewriting the text when I convert it." | Mandatory independent-agent verification catches any text change. |
| "My MediaWiki pages look like 2005 Wikipedia." | 22 copy-paste design recipes for modern layouts (cards, accordions, galleries, infoboxes). |
| "I need different rendering in different places." | Pattern library covers wikitable, TemplateStyles, syntax highlight, gallery, accordion, tabs, etc. |
| "I want one source of truth for MediaWiki." | Six reference files: syntax, design, conversion, verification, advanced features, recipes. |

## ✨ Features

- **Two clean workflows** — parallel iterative authoring, and format
  conversion with mandatory independent-agent verification
- **Six input formats** — HTML reports, HTML slideshows, PDF, Word, Excel,
  PowerPoint
- **Modern MediaWiki design** — `wikitable sortable`, `<gallery>`, syntax
  highlight, `{{Note}}` / `{{Tip}}` / `{{Warning}}` / `{{Caution}}` callouts,
  `<details>` accordions, TabberNeue tabs, Mermaid diagrams,
  TemplateStyles-powered cards and hero headers
- **Double verification by a separate agent** — letter-for-letter text
  check, single-word deviation detection, whitespace filter, format and
  design diff reported separately
- **Language-agnostic communication** — speaks the user's language;
  code, technical terms, and MediaWiki tags stay in their canonical form
- **Production-ready reference library** — ~4000 lines across six files

## 🚀 Quick start

1. **Install the skill** — see [📦 Installation](docs/installation.md).
2. **Invoke it** with one of the prompts in
   [💼 Usage examples](docs/usage-examples.md).
3. **Iterate** — the skill shows the full wikitext in one block, you give
   feedback, it updates.

Example session:

> **You:** "Create a modern MediaWiki article about the OpenAI o1 model.
> Include a comparison table against o1-preview, a code example in
> Python, and a FAQ section with collapsible answers."
>
> **Claude (with the skill):** *generates a 100-line wikitext in one
> ` ```wiki ` block, with `class="wikitable sortable"`,
> `<syntaxhighlight lang="python">`, and `<details>` accordions*

That's it. You copy-paste the wikitext into your MediaWiki editor.

For other AI platforms (ChatGPT, Claude web, Gemini, Claude Cowork), see
[🌐 Other AI platforms](docs/other-platforms.md).

## 📄 License

This project is licensed under the [MIT License](LICENSE) (Copyright ©
2026 EggProject).

The MIT License grants **every person** who obtains a copy of this
software the right to **use, copy, modify, merge, publish, distribute,
sublicense, and/or sell** copies of it, free of charge. The only
condition is that the copyright notice and permission notice must be
preserved in all copies or substantial portions of the software, and the
software is provided **"as is"** without warranty of any kind — see the
full text in [`LICENSE`](LICENSE) for details.

## 🇭🇺 Hungarian version (magyar nyelv)

A teljes magyar nyelvű dokumentáció és a magyar verziójú skill itt
található: [README.hu.md](README.hu.md).

A magyar skill példaként szolgál, és a `hungarian-example/` mappában
található.
