# The two workflows

The skill supports two distinct authoring flows. Choose one based on your
starting point.

## Workflow 1 — Parallel, iterative article creation

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

The skill will ask you four things up front: **purpose, audience, length,
required elements**. Then it produces the entire article in one go — not in
pieces — so you see the complete picture and can give holistic feedback.

This mirrors the [skill-creator plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator)
methodology: write the whole thing first, then iterate. The agent rewrites
the entire block on each revision (rather than patching in place), so the
wikitext stays consistent end-to-end.

## Workflow 2 — Format conversion

For when you have content in another format (HTML, PDF, Word, etc.) and
want it in MediaWiki.

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
[`references/04-verification-checklist.md`](../skills/mediawiki-article-creator/references/04-verification-checklist.md).
It reports any text differences (even single words!), format differences,
and syntax errors.

**Why the independent verification matters:** when an LLM rewrites text, it
will subtly paraphrase — even when explicitly told not to. A fresh agent
with a strict checklist catches these and forces a fix before the article
is published.

## Which workflow when?

| You have... | Use |
|-------------|-----|
| A blank page and a topic in mind | **Workflow 1** — iterate from scratch |
| An existing document (HTML, PDF, DOCX, PPTX, XLSX) | **Workflow 2** — convert |
| A draft MediaWiki article you want to polish | **Workflow 1** style — review and iterate |
| A custom wiki to migrate | **Workflow 2** — convert page-by-page |

---

**Next:** [Usage examples →](usage-examples.md) · [Back to README →](../README.md)
