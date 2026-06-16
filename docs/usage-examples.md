# Usage examples

Eight realistic prompts and what the skill produces for each. These mirror
the examples in the [official skill-creator plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator)
test suite.

## Example 1 — Create an article from scratch (Workflow 1)

> "Write a modern MediaWiki article about vector databases. Include a
> comparison table (Pinecone vs. Weaviate vs. Qdrant vs. Milvus), a code
> example in Python using Qdrant, and a FAQ section."

The skill produces a single wikitext block with `__TOC__`,
`class="wikitable sortable"`, `<syntaxhighlight lang="python">`,
`<details>` accordions, and `[[Category:Databases]]`.

## Example 2 — Convert an HTML quarterly report (Workflow 2)

> "Convert this HTML report to MediaWiki. Do not change any text, only the
> format. Verify with a separate agent."

The skill:
1. Reads the HTML end-to-end.
2. Maps `<h2>` → `==`, `<h3>` → `===`, `<table>` → `{| class="wikitable"`,
   `<strong>` → `'''...'''`, `<div class="warning">` → `{{Warning|...}}`.
3. Spawns an independent agent to verify the conversion.
4. Reports any deviations and fixes them.

## Example 3 — Convert a Word document (DOCX)

> "I have a Word document at `/docs/architecture.docx`. Convert it to
> MediaWiki and put the wikitext in `output.wiki`."

The skill explains that you should first run `pandoc` to get plain
MediaWiki-formatted text, then hand it back. It cleans up pandoc's
output and applies modern design.

```bash
# Step 1 — extract raw MediaWiki text via pandoc
pandoc /docs/architecture.docx -t mediawiki -o /tmp/architecture.wiki

# Step 2 — hand the file to the skill
> "Here is the pandoc output: /tmp/architecture.wiki. Clean it up and
>  apply the modern design patterns from the references."
```

## Example 4 — Convert a PowerPoint deck

> "Convert this PPTX to a MediaWiki article. One section per slide."

The skill turns each slide's title into a `==` heading, content into lists,
images into `[[File:...|thumb]]` references, and notes into `{{Note|...}}`.

## Example 5 — Modern design with cards

> "Make me a landing-page-style article with three feature cards in a row,
> an FAQ accordion, and a hero header."

The skill produces a TemplateStyles-based hero, a `card-grid` with three
`.card` divs, and a `<details>`-based FAQ. See
[`references/06-design-recipes.md`](../skills/mediawiki-article-creator/references/06-design-recipes.md)
for the exact CSS.

## Example 6 — Use MediaWiki's own templates

> "Add callout boxes (`{{Note}}`, `{{Warning}}`, `{{Tip}}`) at the right
> places in this article."

The skill identifies where each type of callout fits and adds them with
the right severity:

- `{{Note}}` — info, supplementary detail
- `{{Tip}}` — best-practice suggestion
- `{{Warning}}` — heads-up, possible pitfall
- `{{Caution}}` — critical, will break things if ignored

## Example 7 — Migrate from a custom wiki

> "We have a custom wiki with 200 pages. Help me build a migration plan."

The skill walks you through the migration:

1. Inventory extension-specific features (custom tags, magic words, Lua
   modules) in the source wiki.
2. Build a per-template conversion map (what does `{{MyTemplate}}` become
   in standard MediaWiki?).
3. Run `pandoc` per page, then pass each output through the skill for
   cleanup.
4. Verify the first 5–10 converted pages with the independent agent.
5. Roll the rest out in batches.

## Example 8 — Best-practice review

> "Here's my draft MediaWiki article. Tell me what to improve from a
> design and accessibility standpoint."

The skill applies the design checklist in
[`references/02-design-patterns.md`](../skills/mediawiki-article-creator/references/02-design-patterns.md)
and the verification checklist in
[`references/04-verification-checklist.md`](../skills/mediawiki-article-creator/references/04-verification-checklist.md).
The output is a markdown report listing concrete edits, with line numbers
and proposed replacements.

---

**Next:** [Other platforms →](other-platforms.md) · [Back to README →](../README.md)
