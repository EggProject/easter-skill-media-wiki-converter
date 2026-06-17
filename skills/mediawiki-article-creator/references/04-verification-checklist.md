# 04 — Verification checklist (task description for the independent agent)

> This file describes how the independent agent (launched by the `Agent` tool)
> should verify the MediaWiki wikitext against the source.

> **HARD RULE — native-only enforcement.** Every check in this file assumes
> the wikitext was produced with the **native MediaWiki toolbox only** —
> no extensions, no custom templates. The verification agent MUST additionally
> enforce section 6 below. If the agent finds a banned token (e.g. `{{Note|...}}`,
> `<syntaxhighlight>`, `<templatestyles>`, `<mermaid>`, `<tabber>`,
> `{{cite web|...}}`, `<math>`, `{{#invoke:...}}`, `{{Infobox|...}}`),
> the report is an immediate FAIL with the fix being: replace with the
> native equivalent from `references/00-native-only-mapping.md`.

---

## Launching the agent

The main agent (which generated the wikitext) launches the independent
verification agent as follows:

```
Agent tool call:
  subagent_type: "general-purpose" (or "claude")
  isolation: "worktree" (optional, only if the wikitext is written to a file)
  prompt: |
    Your task is to verify the MediaWiki wikitext conversion.

    ## Source (the original format)
    <INSERT THE SOURCE HERE>

    ## Completed MediaWiki wikitext
    <INSERT THE WIKITEXT HERE>

    ## The wikitext's source file
    <PATH TO THE WIKITEXT FILES, if any>

    ## Your task
    Read the `references/04-verification-checklist.md` file (the full
    checklist), and based on it, produce a DETAILED REPORT on whether:

    1. The text in the wikitext matches the source text VERBATIM.
    2. ONLY the format has been converted (no content changes).
    3. The design elements have been preserved, or where they could not be,
       the MediaWiki equivalent has been used.
    4. The MediaWiki syntax is correct (cite `references/01-syntax-cheatsheet.md`).

    ## Critical rule
    NEVER accept text differences (not even a SINGLE WORD!).
    Report every difference and quote both versions.

    Produce a structured report:
    - Summary (pass/fail)
    - List of text differences (source vs. wikitext, at the line level)
    - List of format/design differences
    - List of syntax errors
    - Suggestions for fixes

    The report must be written in Hungarian.
```

---

## The checklist (used by the agent)

### 1. Text matching (MOST CRITICAL)

**Goal:** The text in the wikitext must match the source text VERBATIM.

#### 1.1. Word-level check

- [ ] Are all words in the wikitext found in the source?
- [ ] Are all words in the source found in the wikitext?
- [ ] No missing words, sentences, or paragraphs?
- [ ] No added words, sentences, or paragraphs?
- [ ] No rephrased text segments?

#### 1.2. Specific text elements

- [ ] Are numbers (prices, dates, percentages) unchanged?
- [ ] Are proper nouns, company names, product names unchanged?
- [ ] Is link text unchanged?
- [ ] Are image captions unchanged?
- [ ] Is footnote text unchanged?
- [ ] Is code (program code) unchanged (syntax highlighting is NOT considered a modification)?

#### 1.3. Specific escapes

- [ ] Are apostrophes (') preserved in the wiki as well? (`''italic''`, `'''bold'''`)
- [ ] Are quotation marks (", ') consistent?
- [ ] Are special characters (&, <, >) properly escaped?
- [ ] Are pipe characters (|) in tables escaped with `{{!}}`?

#### 1.4. Emphasis

- [ ] Are bold texts from the source in the wiki in `'''...'''` form?
- [ ] Are italic texts from the source in the wiki in `''...''` form?
- [ ] Are underlined texts from the source in the wiki in `<u>...</u>` form?
- [ ] Are strikethrough texts from the source in the wiki in `<s>...</s>` form?

#### 1.5. Text context

- [ ] Are heading texts unchanged?
- [ ] Are table cell texts unchanged?
- [ ] Are list item texts unchanged?

**If you find a difference in any of these points → the entire report is an immediate PASS-FAIL.**

---

### 2. Format conversion (IMPORTANT, but lower priority than text)

**Goal:** The visual structure of the source should also appear in MediaWiki, using
MediaWiki's own toolset.

#### 2.1. Headings

- [ ] Are headings from the source in the wiki in `==`, `===`, `====` form?
- [ ] Is the hierarchy preserved (level 1 = never, level 2 is the default)?
- [ ] No level 1 (`=`) headings?
- [ ] Are heading texts UNCHANGED (only the format changed)?

#### 2.2. Paragraphs

- [ ] Are paragraphs separated by blank lines?
- [ ] No merged paragraphs?
- [ ] Are line breaks in `<br />` form where needed?

#### 2.3. Lists

- [ ] Do unordered lists start with `*`?
- [ ] Do ordered lists start with `#`?
- [ ] For nested lists, does the number of `*`/`#` increase?
- [ ] Are definition lists in `;` and `:` form?

#### 2.4. Tables

- [ ] Does every table have `class="wikitable"`?
- [ ] Are headers in `!` form?
- [ ] Are data cells in `|` form?
- [ ] Is the `|+` caption in place where needed?
- [ ] Are `|-` rows placed correctly?
- [ ] Are `colspan`/`rowspan` attributes preserved?
- [ ] Is cell alignment (`style="text-align:..."`) present where needed?
- [ ] Is the table text UNCHANGED?

#### 2.5. Images

- [ ] Are images in `[[File:...|thumb|...]]` format?
- [ ] Is the `alt` attribute present where it was in the source?
- [ ] Is the image caption UNCHANGED?
- [ ] Is the image position (left/right/center) preserved where it was?

#### 2.6. Code

- [ ] Is code in native `<pre>` format? (NEVER `<syntaxhighlight>` — requires extension)
- [ ] Is the code text UNCHANGED?

#### 2.7. Quotes

- [ ] Are `>` lines or `<blockquote>` format used?
- [ ] Is the quote text UNCHANGED?
- [ ] Is source attribution present where it was?

#### 2.8. Highlights, boxes

- [ ] Is a "Note"-type box from the source in **inline-CSS `<div style="...">`** form?
      (NEVER `{{Note|...}}`, `{{Tip|...}}`, `{{Warning|...}}`,
      `{{Caution|...}}`, `{{ambox|...}}`, `{{Notice|...}}`)
- [ ] Is the box text UNCHANGED?
- [ ] Does the box use one of the color palette tokens from
      `references/00-native-only-mapping.md` section 3.1?

#### 2.9. Links

- [ ] Are internal links in `[[...]]` form?
- [ ] Are external links in `[...]` form?
- [ ] Are piped links in `[[target|label]]` form?
- [ ] Is link text UNCHANGED?

#### 2.10. Footnotes

- [ ] Are footnotes in `<ref>...</ref>` form?
- [ ] Is the `<references />` tag at the bottom of the page?
- [ ] Is footnote text UNCHANGED?

---

### 3. Design preservation (native MediaWiki only)

**Goal:** Where the source's design elements can be preserved, they should be —
using ONLY the native MediaWiki toolbox. NEVER use TemplateStyles, Scribunto,
or any extension-based styling. Inline CSS on `<div>` and `<table>` is the only
permitted styling surface.

#### 3.1. Colors

- [ ] Are colors expressed via inline `style="..."` attributes on `<div>` or
      `<table>` (or via the standard `class="wikitable"`)?
- [ ] Do the colors come from the palette tokens in
      `references/00-native-only-mapping.md` section 3.1?
- [ ] If the source's color palette was consistent, is it so in the wiki as well?
- [ ] **NO `<templatestyles>` tag anywhere** (instant FAIL if present).

#### 3.2. Boxes, cards

- [ ] Are "card"-type boxes from the source present in the wiki as
      `class="wikitable"` tables with inline `style="..."` on cells?
- [ ] Are callout boxes inline-CSS `<div style="...">` per the recipes in
      `references/00-native-only-mapping.md` section 3.1?
- [ ] **NO `{{Note|...}}` / `{{Tip|...}}` / `{{Warning|...}}` /
      `{{Caution|...}}` / `{{Important|...}}` / `{{ambox|...}}` anywhere**
      (instant FAIL if present).

#### 3.3. Accordion

- [ ] Are "expand/collapse" elements from the source in the wiki in `mw-collapsible` or
      `<details>` form?

#### 3.4. Galleries

- [ ] If the source had multiple images, is there a `<gallery>` in the wiki?
- [ ] Is the gallery `mode` appropriate (packed-hover, slideshow, etc.)?

---

### 4. Correctness of MediaWiki syntax

**Goal:** The wikitext should be syntactically correct and render properly in
the MediaWiki engine.

#### 4.1. Basic syntax

- [ ] No `=` level headings?
- [ ] Are pipe characters in tables escaped with `{{!}}` where needed?
- [ ] Are equals signs in headings escaped with `{{=}}` where needed?
- [ ] Are HTML entities correct (`&amp;`, `&lt;`, `&gt;`, etc.)?

#### 4.2. Templates

- [ ] Are template calls in the correct format (`{{name|param=value}}`)?
- [ ] Are the template parameters correct?
- [ ] Are ONLY native magic-word templates used (`{{PAGENAME}}`, `{{CURRENTYEAR}}`,
      `{{FULLPAGENAME}}`, `{{NAMESPACE}}`, `{{REVISIONID}}`, `{{#time:}}` etc.)?
- [ ] **NO custom templates** like `{{Note}}`, `{{Tip}}`, `{{Warning}}`,
      `{{Caution}}`, `{{Infobox}}`, `{{Sidebar}}`, `{{cite ...}}`, `{{blockquote}}`,
      `{{pull quote}}`, `{{Quotation}}`, `{{Codesample}}`, `{{Key press}}`,
      `{{Button}}`, `{{Forrás}}`, `{{Source}}`, `{{Main}}`, `{{See also}}`,
      `{{For}}`, `{{Breadcrumb}}`, `{{#breadcrumb}}`, `{{#invoke:...}}`,
      `{{ambox}}`, `{{Notice}}`, `{{Outdated}}`, `{{Historical}}`, `{{Fixme}}`,
      `{{Todo}}`, `{{Update}}`, `{{Graph:Chart}}` — instant FAIL if present.
- [ ] **NO ParserFunctions** (`{{#if:}}`, `{{#switch:}}`, `{{#expr:}}`,
      `{{#time:}}` from the ParserFunctions extension) — instant FAIL.
      Use only the core `{{#time:}}` from `Help:Magic_words` if a time
      expression is required.

#### 4.3. Links

- [ ] Are external links in `[URL label]` format (space before the label)?
- [ ] Are email links in `[mailto:... address]` format?

#### 4.4. Categories

- [ ] Are categories at the bottom of the page?
- [ ] Are category names with correct diacritics and consistent?

#### 4.5. Magic words

- [ ] Are `__TOC__`, `__NOTOC__` magic words used correctly?
- [ ] Are `{{PAGENAME}}`, `{{CURRENTYEAR}}` etc. magic words correct?

---

### 5. Accessibility and responsiveness

- [ ] Do table headers have a `scope="col"` attribute?
- [ ] Do images have an `alt` attribute?
- [ ] Is link text descriptive (not "click here")?
- [ ] Are code blocks readable on mobile (`overflow-x: auto` for long lines)?
- [ ] Do tables not break apart on mobile?

---

## Report format

The report produced by the independent agent should be structured as follows:

```markdown
# Verification report

## Summary
- **Status:** PASS / FAIL
- **Number of text differences:** X
- **Number of format differences:** Y
- **Number of syntax errors:** Z
- **Overall impression:** ...

## 1. Text differences (CRITICAL)

### 1.1. Paragraph X
- **Source:** "This is the exact text."
- **Wikitext:** "This is the modified text."
- **Line:** line X of the wiki file
- **Fix:** Replace the wikitext with the exact text from the source.

### 1.2. ...

## 2. Format differences

### 2.1. Table Y
- **Problem:** The table does not have `class="wikitable"`.
- **Fix:** Add `class="wikitable"` to the table.

### 2.2. ...

## 3. Syntax errors

### 3.1. Pipe character in a table
- **Line:** line X of the wiki
- **Problem:** The `|` character is not escaped.
- **Fix:** Replace `|` with `{{!}}`.

## 4. Design / appearance

- The source had a "card"-type box, in the wiki this...
- ...

## 5. Suggestions

1. ...
2. ...
```

---

## Main agent's tasks after the report

1. **If the report is PASS** → the wikitext is final, show it to the user.
2. **If the report is FAIL** (there are text differences):
   - Fix the text differences based on the source (NOT from your own head!).
   - Fix the syntax errors.
   - Fix the design differences where MediaWiki's toolset allows.
   - **Launch ANOTHER independent agent** to verify the fixes.
   - Repeat until it becomes PASS.
3. **If the design element CANNOT be reproduced in MediaWiki** → inform the user
   about what was left out and why.

---

## Common errors the agent should look for

1. **Adding stopwords** (e.g. "It is important to note that..." — this was NOT in the source).
2. **Removing or merging headings** (the user split them up, the LLM merged them).
3. **Rounding numbers** (12.5% → ~13%).
4. **Currency conversion** (€4.2M → 4 200 000 Ft).
5. **Reversing name order** (Anna Kovács → Kovács Anna — THIS IS A DIFFERENCE!).
6. **Losing quotes** (the source had a "..." quote, the wiki does not).
7. **Changing link text** ("here" → "at this link").
8. **Losing line breaks** (the source had a line break, the wiki does not).
9. **Whitespace changes** (the source had 2 spaces, the wiki has 1 — this is technical, NOT
   a content difference, but should be flagged).
10. **Removing "TODO" comments from the source** (these are needed in the wiki too).

---

## When the user MUST be asked

The independent agent should NOT decide on its own in the following cases — it should
notify the user:

1. **If a design element in the source cannot be reproduced in MediaWiki** (e.g. JS-based
   animation, custom CSS that requires TemplateStyles).
2. **If an image in the source has not been uploaded** to the wiki by the user.
3. **If the categories in the source are not clear** (which category should the article go in?).
4. **If the footnote format in the source is not clear** (reference, citation, etc.).
5. **If the source language is not Hungarian** and the user has not specified whether to translate.

---

### 6. Native-only enforcement (CRITICAL — instant FAIL)

**Goal:** the wikitext MUST work on a stock MediaWiki install with only the
core parser. NO extensions and NO custom templates may be used.

#### 6.1. Banned extension tags

If ANY of the following appear in the wikitext, the report is an **immediate
FAIL**. Replace with the native equivalent from
`references/00-native-only-mapping.md`.

| Banned token | Native replacement |
|--------------|--------------------|
| `<syntaxhighlight lang="...">` | `<pre>` |
| `<templatestyles src="..." />` | inline `style="..."` on `<div>` / `<table>` |
| `<math>...</math>` | `<span class="texhtml">` + HTML `<sup>` / `<sub>` |
| `<mermaid>...</mermaid>` | ASCII diagram in `<pre>` or `<table>` box diagram |
| `<graphviz>...</graphviz>` | ASCII diagram in `<pre>` or `<table>` box diagram |
| `<tabber>...</tabber>` | multi-column `class="wikitable"` |
| `<categorytree>` / `{{#categorytree:...}}` | native `<gallery>` + `[[:Category:...]]` |
| `<poem>...</poem>` | simple `<br />` lines |
| `<ref name="..." />` inside `{{cite ...}}` | manual citation string |
| `<inputbox>` | native search-form wikitext |
| `<section>` / `<references responsive>` outside core | core equivalent |
| `__HIDDENCAT__` inside `<noinclude>` | acceptable (core magic word) |

#### 6.2. Banned custom templates

If ANY of the following appear in the wikitext, the report is an **immediate
FAIL**. Replace with the native equivalent from
`references/00-native-only-mapping.md`.

| Banned template | Native replacement |
|-----------------|--------------------|
| `{{Note\|...}}` | inline-CSS `<div>` (note style) |
| `{{Tip\|...}}` | inline-CSS `<div>` (tip style) |
| `{{Warning\|...}}` | inline-CSS `<div>` (warning style) |
| `{{Caution\|...}}` | inline-CSS `<div>` (caution style) |
| `{{Important\|...}}` | inline-CSS `<div>` (important style) |
| `{{NoteTip\|...}}` | inline-CSS `<div>` |
| `{{ambox\|...}}` | inline-CSS `<div>` |
| `{{Notice\|...}}` | inline-CSS `<div>` |
| `{{Outdated\|...}}` | inline-CSS `<div>` (yellow banner) |
| `{{Historical\|...}}` | inline-CSS `<div>` (gray banner) |
| `{{Fixme\|...}}` | inline-CSS `<div>` (warning style) |
| `{{Todo\|...}}` | inline-CSS `<div>` (note style) |
| `{{Update\|...}}` | inline-CSS `<div>` (warning style) |
| `{{Codesample\|...}}` | wikitable with `<pre>` cell |
| `{{Key press\|...}}` | native `<kbd>` |
| `{{Button\|...}}` | native `<kbd>` |
| `{{blockquote\|...}}` | native `<blockquote>` |
| `{{pull quote\|...}}` | native `<blockquote>` |
| `{{Quotation\|...}}` | native `<blockquote>` |
| `{{cite web\|...}}` | native `<ref>` with manual citation string |
| `{{cite book\|...}}` | native `<ref>` with manual citation string |
| `{{cite journal\|...}}` | native `<ref>` with manual citation string |
| `{{cite news\|...}}` | native `<ref>` with manual citation string |
| `{{Graph:Chart\|...}}` | ASCII in `<pre>` or `<table>` box diagram |
| `{{Sidebar\|...}}` | wikitable floated right |
| `{{Infobox\|...}}` | wikitable floated right |
| `{{Source\|...}}` | inline-CSS `<div>` at top of article |
| `{{Forrás\|...}}` | inline-CSS `<div>` at top of article |
| `{{Main\|...}}` | native hatnote `<div>` |
| `{{See also\|...}}` | native hatnote `<div>` |
| `{{For\|...}}` | native hatnote `<div>` |
| `{{Breadcrumb\|...}}` | wikilink chain in inline-CSS `<div>` |
| `{{#breadcrumb:...}}` | wikilink chain in inline-CSS `<div>` |
| `{{#invoke:Module:...}}` | plain wikitext (no scripting) |
| `{{CategoryTree\|...}}` | `<gallery>` + `[[:Category:...]]` |
| `{{#if:...}}` / `{{#switch:...}}` / `{{#expr:...}}` (ParserFunctions) | rewrite in plain wikitext |

#### 6.3. Banned ParserFunctions / Scribunto

- [ ] No `{{#if:}}`, `{{#ifeq:}}`, `{{#switch:}}`, `{{#expr:}}`, `{{#time:}}`
      (ParserFunctions extension). `{{#time:}}` is acceptable ONLY if it is
      the core `Help:Magic_words` `{{#time:` — confirm by reading the wiki
      docs. Otherwise, rewrite the conditional in plain wikitext.
- [ ] No `{{#invoke:Module:Foo|...}}` (Scribunto). All Lua logic must be
      inlined as plain wikitext.
- [ ] No `{{#tag:...}}` with extension namespaces (e.g. `{{#tag:tabber|...}}`).

#### 6.4. Native toolbox whitelist

The verification agent MUST confirm that ONLY the following elements appear
in the wikitext:

- **Wikitext markup:** `[[...]]`, `[...]`, `[[...|...]]`, `[[:Category:...]]`,
  `[[File:...]]`, `{{PAGENAME}}`, `{{CURRENTYEAR}}`, `{{FULLPAGENAME}}`,
  `{{NAMESPACE}}`, `{{REVISIONID}}`, `__TOC__`, `__NOTOC__`, `__FORCETOC__`,
  `__HIDDENCAT__`, `__NOEDITSECTION__`, `__NEWSECTIONLINK__`,
  `{{#time:Y}}` (core, if needed).
- **Native HTML elements:** `<div>`, `<span>`, `<p>`, `<pre>`, `<code>`,
  `<kbd>`, `<var>`, `<abbr>`, `<s>`, `<u>`, `<sup>`, `<sub>`, `<small>`,
  `<big>`, `<br />`, `<hr />`, `<blockquote>`, `<q>`, `<cite>`,
  `<table>`, `<tr>`, `<th>`, `<td>`, `<caption>`, `<colgroup>`, `<col>`,
  `<thead>`, `<tbody>`, `<tfoot>`, `<details>`, `<summary>`,
  `<a href="...">` (external only).
- **Native MediaWiki tags:** `<gallery>`, `<ref>`, `<references />`,
  `<includeonly>`, `<noinclude>`, `<onlyinclude>`, `<templatedata>`
  (for documentation only, not styling).
- **Native CSS classes:** `wikitable`, `wikitable sortable`, `wikitable striped`,
  `mw-collapsible`, `mw-collapsed`, `mw-headline`, `mw-parser-output`,
  `mw-code`, `center`, `TexHTML`.
- **Inline CSS:** minimal, on `<div>` and `<table>` only, palette tokens from
  `references/00-native-only-mapping.md` section 3.1.

Anything outside this whitelist is a violation that must be reported and
fixed.

#### 6.5. Why this matters

If the wikitext uses `{{Note|...}}` and the target wiki doesn't have
`Template:Note` defined, the wikitext renders as raw `Sablon:Note` text
(the literal template name appears on the page). The user has already hit
this bug — see `references/00-native-only-mapping.md` section 1 for the
list of native replacements that always render correctly on stock
MediaWiki.

---

Sources:
- Detailed MediaWiki syntax reference: `references/01-syntax-cheatsheet.md`
- MediaWiki design patterns: `references/02-design-patterns.md`
- Conversion playbooks: `references/03-conversion-playbooks.md`
