---
name: mediawiki-article-creator
description: |
  Creates modern, clean-design MediaWiki articles and converts other formats (HTML
  reports, slideshows, PDF, Word, Excel, PowerPoint) into MediaWiki wikitext.

  USE THIS SKILL when the user wants to:
  - Write a MediaWiki article, template, description, table, or gallery
  - Ask for "wiki syntax", "wikitext", or "mediawiki markup"
  - Convert any other format (PDF, Word, Excel, PPT, HTML, markdown, rich text) to MediaWiki
  - Ask about MediaWiki design (cards, boxes, code blocks, tabs, infoboxes, sidebars)
  - Ask for MediaWiki design tips, layout recipes, or skin setup
  - Ask about Wikipedia-style article structure or conventions

  Do not use for plain Markdown or HTML output (not MediaWiki).

  Two workflows:
  1. Parallel, iterative authoring: LLM shows code while the user negotiates.
  2. Format conversion: preserves design with the MediaWiki toolkit; every conversion
     MUST be verified by a separate agent to ensure the text was not rewritten.
---

# MediaWiki Article Creator

This skill is for creating modern, clean-design MediaWiki articles and for converting
other formats into MediaWiki.

Always split the work into **two phases**:

1. **Phase one: wikitext generation.** Based on the source (free conversation, PDF,
   Word, HTML, etc.), produce the MediaWiki wikitext with design and style adapted to
   MediaWiki's toolkit. See: `references/01-syntax-cheatsheet.md`,
   `references/02-design-patterns.md`, `references/05-advanced-features.md`,
   `references/06-design-recipes.md`.
2. **Phase two: double verification.** A **separate agent** (spawned with the `Agent`
   tool, using `isolation: "worktree"` if files are written) renders the wikitext into
   HTML (or runs a syntax check) and compares it against the source. We ONLY convert the
   format — the text must not change. See: `references/04-verification-checklist.md`.

> In the conversion workflow, NEVER edit the text, only the format and the visual
> structure. If you find a sentence too verbose or could be worded more elegantly, do
> not touch it — that is data loss. The double-check agent's job is to catch this.

---

## Workflow 1 — Parallel, iterative article creation from free conversation

### Steps

1. **Clarify goal and audience.** Before writing anything, ask (or confirm) the following:
   - What is the article's purpose? (documentation, description, reference, tutorial, project report, etc.)
   - Who will read it? (developers, internal staff, general public, subject matter experts)
   - How long should it be? (one or two sections, or a long, structured page)
   - Do you need an infobox, table, code, image, or gallery?
2. **Outline.** Propose a short, numbered outline of the article's sections, in line
   with MediaWiki style: overview (lead), table of contents (automatic), sections,
   sources, categories.
3. **Parallel code generation.** After the user agrees, write the entire article at
   once, in one block — do not piece it together section by section. This way the user
   sees the whole article at once.
4. **Apply design tips.** Read `references/02-design-patterns.md` and apply modern,
   clean-design principles:
   - Simple, clean heading hierarchy (==, ===, ====)
   - `class="wikitable"` on tables
   - `class="wikitable sortable"` when columns should be sortable
   - `<gallery mode="packed-hover">` for image showcases
   - `<syntaxhighlight lang="...">` for code blocks
   - `{{Note}}`, `{{Tip}}`, `{{Warning}}` for emphasis (MediaWiki's own templates)
   - `__TOC__` for the table of contents, if it does not appear automatically
   - Categories at the bottom of the page: `[[Category:...]]`
5. **Iteration.** Modify based on the user's feedback. In every iteration, show the
   **entire updated wikitext**, not just the diff — the user wants the full picture.
6. **Final package.** When done, offer:
   - `{{Infobox}}` or infobox template for highlight summaries
   - Categories that fit the article
   - Checking internal links (`[[Other page]]`)

### Example opening question

> "Let's start by discussing: what is the article's purpose, who will read it, and how
> long should it be? If you have a favorite reference (e.g., an existing MediaWiki page
> you like), paste the link and we'll work from that."

---

## Workflow 2 — Format conversion into MediaWiki

### General rules

- **NEVER edit the text.** Only convert the format and the visual structure.
- **Preserve design.** If the source has design (colors, boxes, tables, icons), try to
  reproduce it using MediaWiki's toolkit (TemplateStyles, wikitable, gallery, syntax
  highlight).
- **Two phases, always.** Phase one is wikitext generation, phase two is verification
  by an independent agent.
- **Source attribution.** Optionally put a `{{Source|...}}` or similar template at the
  top of the finished article to mark where it came from.

### Supported source formats

Detailed conversion playbooks for each format live in
`references/03-conversion-playbooks.md`.

1. **HTML report** (handwritten or generated): the richest source. `class` attributes,
   `<table>`, `<pre>`, `<code>`, `<blockquote>`, `<h1>–h6>` map almost 1:1. See the
   "HTML report" section in `03-conversion-playbooks.md`.
2. **HTML slideshow** (reveal.js, Swiper, etc.): slides become MediaWiki sections, the
   progress bar is dropped, `__TOC__` is used instead.
3. **PDF**: a bit more limited because we lose the layout. Text + table + image can
   usually be extracted. See the "PDF" section in `03-conversion-playbooks.md`.
4. **Word (DOCX)**: Open Office "Export as MediaWiki" or pandoc can be a starting
   point, but the output always needs manual fixes for MediaWiki-specific things
   (TemplateStyles, gallery, syntaxhighlight, categories).
5. **Excel (XLSX)**: only the table and cell text are extracted. Charts, conditional
   formatting, cell colors are lost. The extracted table goes in as
   `class="wikitable sortable"`.
6. **PowerPoint (PPTX)**: slides become headings (== Slide title ==), content blocks
   become lists, images go in `[[File:...|thumb]]` form. Per-slide animations and
   transitions are lost — note this to the user.

### The 7 conversion steps

1. **Read the source thoroughly.** Read the source all the way through before writing
   anything. If the source is incomplete, ask the user.
2. **Plan the strategy.** Decide how to map each source element:
   - Titles → `==`/`===`
   - Paragraphs → plain text
   - Lists → `*` / `#`
   - Tables → `class="wikitable"`
   - Images → `[[File:...|thumb|...]]`
   - Code → `<syntaxhighlight lang="...">`
   - Quotes → `{{blockquote|...}}` or `>`
   - Boxes, callouts → `{{Note}}`, `{{Tip}}`, `{{Warning}}`, `{{Caution}}`
3. **Generate wikitext.** In one block, the entire article. Do NOT break it into pieces
   — the user wants to see the whole thing.
4. **Spawn the independent agent.** Hand over the source + the finished wikitext to an
   `Agent` (preferably with `isolation: "worktree"` if files are writable). Have it
   verify using `references/04-verification-checklist.md`:
   - The text content matches letter-for-letter
   - Only the format was converted
   - The design elements survived, or where impossible, the closest MediaWiki equivalent
     is in use
5. **Process the feedback.** If the agent finds differences:
   - **Text differences** (even a single word!) → fix immediately, without waiting for
     user consent
   - **Format/design differences** → if a better MediaWiki solution exists, apply it;
     if not, tell the user what was dropped and why
6. **Final wikitext.** After the fixes, show the full final wikitext to the user.
7. **Source attribution.** If the user asks, add `{{Source|...}}` or a similar
   template at the top of the article.

### Example: HTML report conversion

Source (HTML):
```html
<h1>Q4 2025 Analysis</h1>
<p>This report summarizes the Q4 2025 results.</p>
<table class="data">
  <thead><tr><th>Metric</th><th>Value</th></tr></thead>
  <tbody>
    <tr><td>Revenue</td><td>€4.2M</td></tr>
    <tr><td>Growth</td><td>+18%</td></tr>
  </tbody>
</table>
<p><strong>Conclusion:</strong> Q4 was a strong quarter.</p>
```

Result (wikitext):
```wiki
== Q4 2025 Analysis ==

This report summarizes the Q4 2025 results.

{| class="wikitable sortable"
! Metric !! Value
|-
| Revenue || €4.2M
|-
| Growth || +18%
|-
| colspan="2" | <small>Source: internal financial report, 2026-01-15</small>
|}

'''Conclusion:''' Q4 was a strong quarter.

[[Category:Reports]][[Category:2025]]
```

---

## Double verification: when it is MANDATORY and how

**ALWAYS** spawn an independent agent (using the `Agent` tool) when:

- The user asks for conversion from another format (Workflow 2)
- The wikitext is longer than ~30 lines
- The user explicitly asks for "double-checking"
- There is any doubt that the text may have been altered

Send the following message to the independent agent:

> "Verify the MediaWiki wikitext below against the source. ONLY the format may be
> converted — the text must match letter-for-letter. Read
> `references/04-verification-checklist.md` and produce a detailed report based on it.
> NEVER accept text differences (not even a single word): report every deviation."

Always spawn the independent agent with the `general-purpose` (or `claude`) subagent
type, and pass it the full content of `references/04-verification-checklist.md` in the
task assignment.

---

## Referenced files (read whenever the topic is relevant)

- `references/01-syntax-cheatsheet.md` — Complete MediaWiki syntax reference with
  examples (formatting, headings, lists, links, images, tables, code, categories, magic
  words).
- `references/02-design-patterns.md` — Modern, clean design patterns for MediaWiki
  (typography, colors, boxes, cards, section separators, emphasis).
- `references/03-conversion-playbooks.md` — Step-by-step guides for HTML report, HTML
  slideshow, PDF, Word, Excel, PowerPoint → MediaWiki conversion.
- `references/04-verification-checklist.md` — The checklist the independent agent uses
  to verify wikitext.
- `references/05-advanced-features.md` — Advanced MediaWiki features: TemplateStyles,
  ParserFunctions, magic words, Scribunto/Lua, extensions (Infobox, Sidebar, Tabber).
- `references/06-design-recipes.md` — Ready-to-copy code snippets: infobox, navigation
  box, breadcrumbs, accordion, tabber, code syntax highlighting, slideshow, accordion,
  category list, etc.

---

## Style and behavior rules

- **Language.** Communicate in the user's language. Do not force a different
  language on the user than the one they are using. Code and technical identifiers
  stay in their original form.
- **Code block format.** Every wikitext output goes in a ` ```wiki ` block (NOT plain
  markdown ` ``` `) so the user can paste it straight into the MediaWiki editor.
- **Whole article, not fragments.** If the user did not ask for a specific section, show
  the full article.
- **Iterative feedback.** Always ask at the end what the next step is: edit, different
  design, different section, done?
- **Stepwise approach.** Workflow 1: goal → outline → wikitext → feedback. Workflow 2:
  source → strategy → wikitext → independent verification → final.
- **DO NOT use MediaWiki-specific extensions that may not be available.** If you use
  TemplateStyles, Scribunto, or other extensions, add a note at the top of the article
  that the MediaWiki admin needs to install them.
- **NEVER commit, push, or delete existing files on the user's behalf.** The skill's
  purpose is to produce the wikitext; uploading it is the user's job.
