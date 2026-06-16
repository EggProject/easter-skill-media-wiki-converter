# 03 — Conversion playbooks (HTML / PDF / Word / Excel / PPT → MediaWiki)

> Sources: <https://www.mediawiki.org/wiki/Extension:PandocUltimateConverter>,
> <https://www.mediawiki.org/wiki/Extension:ConvertPDF2Wiki>,
> <https://stackoverflow.com/questions/2833263/convert-from-microsoft-word-to-media-wiki-markup-style>,
> <https://openwetware.org/wiki/Converting_documents_to_mediawiki_markup>

> **IMPORTANT:** This file describes the principles of conversion, it does NOT produce scripts. The actual
> conversion is always performed manually or by an LLM, so that the design and style are preserved.

---

## General rules (for all formats)

1. **NEVER modify the text.** Only the format and the visual structure are transformed.
2. **Try to preserve design elements** — if the source contains color, boxes, tables, icons,
   find their MediaWiki equivalents.
3. **Double-check is MANDATORY.** After every conversion, launch an independent agent
   (see: `references/04-verification-checklist.md`).
4. **Source attribution.** Optionally place a `{{Forrás|...}}` template at the top of the finished article.
5. **Show the full article, not excerpts.** The user wants to see the entire conversion.

---

## 1. HTML report → MediaWiki

### The easiest conversion, because HTML elements can be mapped 1:1.

#### Step by step

1. **Headings**: `<h1>` → `=` (but NEVER use it), `<h2>` → `==`, `<h3>` → `===`, etc.
2. **Paragraphs**: `<p>` → text, separated by a double line break.
3. **Lists**: `<ul><li>` → `*`, `<ol><li>` → `#`, for nested lists increase the number of `*`/`#`.
4. **Tables**: `<table>` → `{|`, `<tr>` → `|-`, `<th>` → `!`, `<td>` → `|`.
   Add `class="wikitable"`.
5. **Links**: `<a href="...">` internal → `[[...]]`, external → `[...]`.
6. **Images**: `<img src="...">` → `[[File:...|thumb|...]]`.
7. **Code**: `<pre>` → `<syntaxhighlight>` (if a language is specified), or `<pre>` if not.
8. **Quotes**: `<blockquote>` → `{{blockquote|...}}` or `<blockquote>`.
9. **Boxes, callouts**: `<div class="note">` → `{{Note|...}}`.
10. **HTML entities**: `&amp;` `&lt;` `&gt;` — keep these as is (MediaWiki supports them too).

#### HTML attribute → MediaWiki attribute

| HTML | MediaWiki | Note |
|------|-----------|------------|
| `class="wikitable"` | `class="wikitable"` | unchanged |
| `class="wikitable sortable"` | `class="wikitable sortable"` | unchanged |
| `style="text-align: center"` | `style="text-align: center"` | unchanged |
| `style="color: red"` | `style="color: red"` | unchanged (rare) |
| `colspan="2"` | `colspan="2"` | unchanged |
| `rowspan="2"` | `rowspan="2"` | unchanged |
| `align="center"` | `style="text-align: center"` | deprecated in HTML5, replace |
| `bgcolor="..."` | `style="background: ..."` | deprecated in HTML5, replace |

#### Example: HTML → Wiki

**Source (HTML):**
```html
<h2>Q4 2025 Analysis</h2>
<p>This report summarizes the <strong>Q4 2025</strong> results.</p>
<table class="data">
  <thead>
    <tr><th>Metric</th><th>Value</th></tr>
  </thead>
  <tbody>
    <tr><td>Revenue</td><td>€4.2M</td></tr>
    <tr><td>Growth</td><td>+18%</td></tr>
  </tbody>
</table>
<p><strong>Conclusion:</strong> Q4 was a strong quarter.</p>
<ul>
  <li>First item</li>
  <li>Second item</li>
</ul>
```

**Result (MediaWiki):**
```wiki
== Q4 2025 Analysis ==

This report summarizes the '''Q4 2025''' results.

{| class="wikitable sortable"
! Metric !! Value
|-
| Revenue || €4.2M
|-
| Growth || +18%
|}

'''Conclusion:''' Q4 was a strong quarter.

* First item
* Second item

[[Category:Reports]][[Category:2025]]
```

#### Tips

- HTML classes may be lost if the MediaWiki skin styles them differently. Always
  replace with `class="wikitable"`.
- Try to replace `<div>`-based layouts with tables or TemplateStyles.
- If the HTML contains many nested `<div>` tags, the page becomes complex. Consider
  which elements of the source are worth keeping.

---

## 2. HTML slideshow (reveal.js, Swiper, etc.) → MediaWiki

### Challenge: a slideshow is structural, but MediaWiki is linear.

#### Step by step

1. **Each slide → a section (`==` heading).** The slide's title becomes the section's title.
2. **The slide's content** becomes the section's content.
3. **The progress bar** is omitted; use `__TOC__` at the top of the page instead.
4. **Instead of navigation arrows**, use section links in the text, or `[[#Next section]]`.
5. **Images** go into `[[File:...|thumb|...]]` format.
6. **Code** goes into `<syntaxhighlight>` format.
7. **Fragments** (content that appears step by step in reveal.js) → "Collapsible sections"
   (`mw-collapsible`) or "Details" (`<details>`).

#### Example: reveal.js slide → Wiki section

**Source (HTML):**
```html
<section>
  <h2>1. Introduction</h2>
  <p>This presentation is about MediaWiki.</p>
  <ul>
    <li class="fragment">On wiki syntax</li>
    <li class="fragment">On design options</li>
    <li class="fragment">On conversion methods</li>
  </ul>
</section>
<section>
  <h2>2. Wiki syntax</h2>
  <pre><code class="language-python">def hello(): return "Hello"</code></pre>
</section>
```

**Result (MediaWiki):**
```wiki
__TOC__

== 1. Introduction ==

This presentation is about MediaWiki.

{| class="mw-collapsible mw-collapsed"
! Topics (click to expand)
|-
* On wiki syntax
* On design options
* On conversion methods
|}

== 2. Wiki syntax ==

<syntaxhighlight lang="python">
def hello():
    return "Hello"
</syntaxhighlight>
```

#### Tips

- The "fragment" (step-by-step) effect is lost — but `mw-collapsible` preserves the
  "interactivity" (it appears on click).
- Collect slide-level footnotes at the end of the article, not per slide.
- If the slideshow contains a "thank you" or "next topic" slide, convert it to a
  "Related articles" section.

---

## 3. PDF → MediaWiki

### PDF is the most difficult source, because the layout is binary.

#### How to extract the source (before writing wikitext)

> **NOTE:** We do NOT write scripts here, only the principles. The actual conversion is
> performed by the user with their tools (Pandoc, Adobe Acrobat, online PDF→HTML converter);
> they pass the result to you, and you produce wikitext from it.

1. **Text extraction:** the user can use `pdftotext`, Adobe Acrobat's "Export PDF",
   or an online PDF→TXT converter. The output is a `.txt` file.
2. **Image extraction:** the user uses the "Extract images" function (Adobe Acrobat) or
   the `pdfimages` command (poppler-utils) to export images.
3. **Tables:** tables in PDFs often "fall apart". In such cases, the user manually
   checks that rows/columns are correct.
4. **The extracted text + images** are passed by the user to you for wikitext generation.

#### Generating wikitext from a PDF

1. **Headings:** PDFs often indicate headings with a larger font size. This is lost
   after text extraction — the user needs to mark where the headings are.
2. **Paragraphs:** in text extracted from a PDF, paragraphs often run together. The
   user can indicate where a new paragraph starts.
3. **Tables:** if the extracted text contains recognizable tables (tabs, alignment),
   try to cast them into wikitable format. If not, ask the user.
4. **Images:** the user uploads the extracted images to the wiki and provides the file names.
5. **Footnotes:** collect the PDF's footnotes at the end of the article in `<ref>` format.
6. **Page numbers** should be omitted — MediaWiki has no concept of a "page".

#### Example: text extracted from a PDF → Wiki

**Source (plain text extracted from a PDF):**
```
Q4 2025 Analysis
This report summarizes the Q4 2025 results.

Metric     Value
Revenue    €4.2M
Growth     +18%

Conclusion: Q4 was a strong quarter.
```

**Result (MediaWiki):**
```wiki
== Q4 2025 Analysis ==

This report summarizes the Q4 2025 results.

{| class="wikitable sortable"
! Metric !! Value
|-
| Revenue || €4.2M
|-
| Growth || +18%
|}

'''Conclusion:''' Q4 was a strong quarter.
```

#### Tips

- PDFs often contain "lines" (—) in the text that are part of the PDF layout. Omit these
  from the wikitext.
- In text extracted from a PDF, "smart quotes" (curly quotes) often get mixed up
  with straight quotes. Normalize to the MediaWiki convention (straight `"`).
- If the PDF contains a table of contents, do NOT copy it in — MediaWiki generates an automatic TOC.
- If the PDF contains many images, ask the user how they would like to name them
  when uploading.

---

## 4. Word (DOCX) → MediaWiki

### Word is the most common source. The process is two-step.

#### How to extract the source

> **NOTE:** The user performs the actual conversion. You receive the finished `.txt` file
> and produce wikitext from it.

1. **Pandoc**: `pandoc -f docx -t mediawiki -o output.wiki input.docx`
2. **Manual**: Word "Save As" → "Plain Text", but formatting is lost.

#### Generating wikitext from Word

Typical issues with Word/Pandoc output:
- Headings appear as `== Heading 1 ==` (containing the Word style name).
- Lists sometimes start with `-` instead of `*`.
- Tables are in "word-table" format, which is not MediaWiki syntax.
- Footnotes use a different syntax.
- Images may be base64-encoded (Pandoc sometimes returns them this way).

**Steps:**
1. Review the Pandoc output and fix the above issues.
2. Verify the `==` level of headings (level 2 is the maximum).
3. Normalize lists to `*` (unordered) and `#` (ordered) format.
4. Convert tables to `class="wikitable"` format.
5. Replace base64 images with `[[File:...]]` references (the user uploads the images).
6. Convert footnotes to `<ref>` format, and add `<references />` at the end.
7. Remove Word-specific formatting (justification, indentation).

#### Example: Pandoc output → Wiki

**Pandoc output (raw):**
```
Heading 1
=========

This is a paragraph.

Heading 2
---------

-   First item
-   Second item

| Col1 | Col2 |
|------|------|
| A    | B    |
| C    | D    |
```

**Corrected wikitext:**
```wiki
== Heading 1 ==

This is a paragraph.

=== Heading 2 ===

* First item
* Second item

{| class="wikitable"
! Col1 !! Col2
|-
| A || B
|-
| C || D
|}
```

#### Tips

- Pandoc sometimes also produces `[[wiki]]` links if the DOCX contained hyperlinks. Keep these.
- Word "comments" → MediaWiki "talk page" (on a Wiki, discussion happens on the talk page, not in the page itself).
- Word "Track changes" → MediaWiki has no such feature, ignore it.
- Word "Table of contents" → generated automatically with the `__TOC__` magic word.

---

## 5. Excel (XLSX) → MediaWiki

### Excel provides only tables and data, nothing else.

#### How to extract the source

> **NOTE:** The user's tools: Excel "Save as CSV" or the `xlsx2csv` command,
> or Pandoc: `pandoc -f xlsx -t mediawiki`.

#### Generating wikitext from Excel

1. **Each worksheet → a separate table** in the wiki, or they can be merged into one "big" table.
2. **The first row** is usually the header (`!`).
3. **Cell types** (number, date, currency) are lost — they end up as text.
4. **The result of formulas** is included, not the formula itself.
5. **Charts** are NOT included — this must be reported to the user.
6. **Conditional formatting** is NOT included.
7. **Cell colors** are NOT included (wikitable does not support cell colors natively).
8. **Merged cells** are sometimes lost — verify the output.

#### Recommended format

```wiki
{| class="wikitable sortable"
! Name !! Age !! City !! Annual income
|-
| Anna || 28 || Budapest || 1 200 000 Ft
|-
| Béla || 34 || Debrecen || 1 800 000 Ft
|-
| Cecil || 25 || Pécs || 950 000 Ft
|}
```

#### Tips

- Format numbers according to the local convention (space as thousands separator, decimal comma).
- If the Excel table is too wide (>6 columns), consider splitting it into several smaller tables.
- Charts can be substituted with the `{{Graph:Chart}}` template (where available), or described in the table.

---

## 6. PowerPoint (PPTX) → MediaWiki

### PowerPoint is similar to a slideshow, linear.

#### How to extract the source

> **NOTE:** The user's tools: PowerPoint "Export as Outline" (text only),
> or Pandoc: `pandoc -f pptx -t mediawiki`, or manual copying.

#### Generating wikitext from PPTX

1. **Each slide → a section (`==` heading).** The slide's title becomes the section's title.
2. **Text boxes within a slide** become the section's content (lists or paragraphs).
3. **Images** go into `[[File:...|thumb|...]]` format.
4. **Charts** are NOT included — they must be described or omitted.
5. **Animations, transitions** are lost.
6. **Speaker notes** can optionally go under the section in `{{Note|...}}` format.

#### Example: PPTX slide → Wiki section

**Source (extracted from PPTX):**
```
Slide 1: "Project overview"
- 3-person team
- 6-month duration
- 50 000 € budget

Slide 2: "Results"
- Phase 1: complete (100%)
- Phase 2: in progress (75%)
- Phase 3: under planning
```

**Result (MediaWiki):**
```wiki
== Project overview ==

* 3-person team
* 6-month duration
* 50 000 € budget

== Results ==

* '''Phase 1:''' complete (100%)
* '''Phase 2:''' in progress (75%)
* '''Phase 3:''' under planning
```

#### Tips

- The "title slide" in PowerPoint → lead at the top of the article.
- The "agenda" / "outline" slide → the table of contents (automatic, do not copy it in).
- The "end" / "Q&A" / "questions" slide → "Related articles" section.
- Presentations often contain company logos → if the same logo appears on every slide, do NOT
  place it in every section, only at the top of the article.

---

## 7. Common steps for any source format

### Before the conversion

- [ ] Read the source through and understand it.
- [ ] Identify the source type (which playbook to use).
- [ ] Identify the source's images, tables, and special elements.
- [ ] If anything is unclear, ask the user.

### During the conversion

- [ ] The text is kept VERBATIM, only the format changes.
- [ ] Use MediaWiki's own templates (`{{Note}}`, `{{Tip}}`, etc.), not custom HTML.
- [ ] Tables use `class="wikitable"`.
- [ ] Code uses `<syntaxhighlight lang="...">`.
- [ ] Images are in `[[File:...|thumb|alt=...]]` format.
- [ ] Categories go at the bottom of the page.

### After the conversion (MANDATORY)

- [ ] Launch an independent agent based on `references/04-verification-checklist.md`.
- [ ] Process the agent's report.
- [ ] Fix the issues found.
- [ ] Show the final wikitext to the user.

---

## 8. When NOT to convert automatically

There are situations where you should consult the user before converting:

- **If the source language is not English** — ask whether to translate or only change the format.
- **If the source is very old or of poor quality** — warn the user that the output
  will also be limited.
- **If the source contains many images** — the user must upload the images before
  the wikitext can appear on the wiki.
- **If the source contains many special formatting elements** (custom CSS, JS, scripts) — these
  will be lost during conversion.
- **If the source is copyrighted** — warn the user that MediaWiki content is generally
  under a free license (CC BY-SA).

---

Sources:
- <https://www.mediawiki.org/wiki/Extension:PandocUltimateConverter>
- <https://www.mediawiki.org/wiki/Extension:ConvertPDF2Wiki>
- <https://www.mediawiki.org/wiki/Extension:Html2Wiki>
- <https://stackoverflow.com/questions/2833263/convert-from-microsoft-word-to-media-wiki-markup-style>
- <https://openwetware.org/wiki/Converting_documents_to_mediawiki_markup>
- <https://pandoc.org/MANUAL.html>
