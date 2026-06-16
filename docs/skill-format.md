# Skill structure & references

The skill follows the open
[Agent Skills specification](https://agentskills.io/specification). This
page documents its on-disk layout and what each reference file is for.

## On-disk layout

```
skills/
└── mediawiki-article-creator/      ← name matches the directory
    ├── SKILL.md                    # Main entry point (workflows, rules)
    ├── references/                 # On-demand documentation
    │   ├── 01-syntax-cheatsheet.md
    │   ├── 02-design-patterns.md
    │   ├── 03-conversion-playbooks.md
    │   ├── 04-verification-checklist.md
    │   ├── 05-advanced-features.md
    │   └── 06-design-recipes.md
    └── workspace/                  # Optional scratch space (initially empty)
```

The `SKILL.md` is loaded into the LLM's context whenever the skill
triggers. The `references/` files are loaded **on-demand** when the LLM
needs detailed information about a specific topic. This keeps the context
window lean and is part of the
[progressive disclosure](https://agentskills.io/specification) pattern the
spec recommends.

## What's in the references?

| File | Purpose | Lines |
|------|---------|-------|
| `01-syntax-cheatsheet.md` | Complete MediaWiki syntax reference with examples. **Start here if you don't know the syntax.** | ~1100 |
| `02-design-patterns.md` | Typography, color usage, callouts, accordions, tabs, infoboxes, sidebars, hero headers. | ~640 |
| `03-conversion-playbooks.md` | Step-by-step conversion playbooks for HTML report, HTML slideshow, PDF, Word, Excel, PowerPoint. | ~480 |
| `04-verification-checklist.md` | The exact checklist the independent agent uses to verify wikitext. Includes prompt template and report format. | ~350 |
| `05-advanced-features.md` | TemplateStyles, Scribunto (Lua), ParserFunctions, Variables, Page Forms, Semantic MediaWiki, Cargo, security model. | ~460 |
| `06-design-recipes.md` | 22 ready-to-copy design recipes: cards, accordions, tabs, breadcrumbs, infobox, sidebar, gallery, diagrams, etc. | ~810 |

## Where to find what

- *"I don't know how to do X in wikitext"* → `01-syntax-cheatsheet.md`
- *"What does a modern callout look like?"* → `02-design-patterns.md`
- *"How do I convert an HTML table to wikitext?"* → `03-conversion-playbooks.md`
- *"What does the verification agent actually check?"* → `04-verification-checklist.md`
- *"Can I use Lua / TemplateStyles?"* → `05-advanced-features.md`
- *"Give me a copy-paste component."* → `06-design-recipes.md`

## The `workspace/` folder

The `workspace/` folder is included in the skill as an empty scratch
space. The skill may use it for temporary intermediate files when
converting large documents. It is safe to keep it empty (and to add
`workspace/**/*` to your `.gitignore` if you fork the skill).

## Hungarian mirror

A Hungarian-language version of the skill is included as a bilingual
example at [`hungarian-example/`](../hungarian-example/). It has the same
structure and the same six reference files, all in Hungarian.

---

**Next:** [Compatibility →](compatibility.md) · [Back to README →](../README.md)
