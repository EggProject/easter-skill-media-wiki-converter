# 02 — Design patterns for modern, clean MediaWiki articles

> Sources: <https://www.mediawiki.org/wiki/Documentation/Style_guide>,
> <https://www.mediawiki.org/wiki/Skin:Citizen>,
> <https://www.mediawiki.org/wiki/Skin:Timeless>,
> <https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style>

A "modern, clean" MediaWiki article rests on three pillars:

1. **Typography and hierarchy** — clean heading structure, consistent lists, proper escaping.
2. **Visual rhythm** — unified table style, highlights, boxes, cards.
3. **Responsiveness and accessibility** — readable on mobile, scope attributes,
   alt text, contrasting colors.

---

## 1. The "five basic principles" — ALWAYS follow them

1. **Level 2 headings are the maximum** (`==`). You rarely need to go deeper than
   level 4 — if you do, consider whether the content should be split into two separate articles.
2. **One concept = one heading.** Do not duplicate sections, do not put two things
   under one heading.
3. **The first paragraph (lead) is a summary.** The user decides in 5 seconds
   whether they need this article. The first 1-2 sentences must make that decision.
4. **A table is not a layout tool.** A table should display data, not split
   otherwise textual content into columns. If you only need layout, use CSS.
5. **Every image must have alt text.** The `[[File:...|alt=...]]` attribute is mandatory.

---

## 2. Typography and hierarchy

### Heading structure in a long article

```wiki
== Overview ==

(short, 1-2 sentence summary)

== History ==

### Early period

### Modern era

== Features ==

== Use cases ==

== Related articles ==

== References ==

<references />
```

### Heading style

- MediaWiki uses large, bold headings by default — do not try to change this
  without TemplateStyles.
- Headings should reference the content: "Use cases", not "Where we use it".
- A heading should NOT be a link. The mobile experience suffers if a heading is also a link.
- Do NOT put two headings with the same text (confusing for navigation).

### Paragraph structure

- 1-3 sentences per paragraph. Longer paragraphs = harder to read.
- The first paragraph should contain the keywords (for SEO and clarity).
- If you have 4+ sentences on one topic, split into two paragraphs or use a subheading.

### Line breaks and empty lines

- Paragraph = 1 empty line.
- Line break within a cell: `<br />` (NOT `<br>` — although it works, the
  XHTML-compatible `<br />` is the official one).
- Do not put 2+ empty lines in a row — no browser gives extra space for it, it
  just clutters the source.

---

## 3. Color usage and visual rhythm

MediaWiki uses a neutral palette by default:
- gray background for the content,
- lighter blue for links,
- red for non-existent links,
- green for visited links.

**Do NOT try to override this without TemplateStyles.** If you do, be very
conservative: 1-2 additional colors, the rest stays default.

### Recommended color palette (for your own highlights)

| Color | Hex | Where to use |
|------|-----|--------------|
| Primary blue | `#36c` | links, buttons |
| Success green | `#14866d` | "OK" status, checkmark |
| Warning yellow | `#fc3` | "Caution" box |
| Error red | `#d33` | "Error" box, critical |
| Neutral gray | `#54595d` | subheadings, supplementary text |
| Background gray | `#f8f9fa` | highlight boxes |
| Dark background | `#eaecf0` | table header |

### Color contrast rule

- Light background: `#fff` → text: `#202122` (default)
- Dark mode: the MediaWiki Vector 2022 skin supports it, colors switch automatically

---

## 4. Highlights and boxes (your best friend)

In MediaWiki, the `{{Note}}`, `{{Tip}}`, `{{Warning}}`, `{{Caution}}` templates are
the official highlight tools. Always use these, NOT your own `<div>`s.

### Built-in message boxes (Wikipedia/MediaWiki)

```wiki
{{Note|This is an informational note.}}
{{Tip|Try this trick.}}
{{Warning|Be careful, this can cause data loss.}}
{{Caution|Critical warning!}}
{{Important|Be sure to read this.}}
```

These templates are available on Wikipedia and on mediawiki.org. If they are not
available on the target wiki, use the `{{ambox}}` core:

```wiki
{{ambox
|type=notice
|text=Simple informational box.
}}
```

Types: `notice` (blue), `style` (yellow), `content` (yellow-orange), `warning` (orange),
`serious` (red).

### Page-level messages

```wiki
{{Notice|this page is under maintenance}}
{{Outdated|2024-01-15}}
{{Historical}}
{{Fixme}}
{{Todo|finish the table}}
{{Update}}
```

### Highlights with TemplateStyles (custom design)

If the wiki includes the TemplateStyles extension, you can create your own box:

```wiki
<templatestyles src="Modul:Highlight/styles.css" />

<div class="hl-primary">
Primary highlight
</div>

<div class="hl-warning">
Warning
</div>

<div class="hl-success">
Success
</div>
```

The content of `Modul:Highlight/styles.css` (example):
```css
.hl-primary {
  background: #eaf3ff;
  border-left: 4px solid #36c;
  padding: 12px 16px;
  border-radius: 4px;
  margin: 12px 0;
}
.hl-warning {
  background: #fef6e7;
  border-left: 4px solid #fc3;
  padding: 12px 16px;
  border-radius: 4px;
  margin: 12px 0;
}
.hl-success {
  background: #e6f4ea;
  border-left: 4px solid #14866d;
  padding: 12px 16px;
  border-radius: 4px;
  margin: 12px 0;
}
```

**Note:** TemplateStyles must be installed by the MediaWiki admin. If not available,
note it in a comment at the top of the article.

---

## 5. Card layout

One of the most important elements of "modern" design. In MediaWiki, cards can
be created with a table or with TemplateStyles.

### Simple cards using wikitable

```wiki
{| class="wikitable" style="width: 100%; text-align: center;"
|+ Our three services
|-
| style="width: 33%; background: #f0f7ff;" |
'''Free'''
<br />
Available to everyone.
| style="width: 33%; background: #e6f4ea;" |
'''Fast'''
<br />
Set up in 5 minutes.
| style="width: 33%; background: #fef6e7;" |
'''Secure'''
<br />
SOC 2 certified.
|}
```

### Cards with TemplateStyles (much nicer)

```wiki
<templatestyles src="Modul:Cards/styles.css" />

<div class="card-grid">
  <div class="card">
    <div class="card-title">Free</div>
    <div class="card-body">Available to everyone.</div>
  </div>
  <div class="card">
    <div class="card-title">Fast</div>
    <div class="card-body">Set up in 5 minutes.</div>
  </div>
  <div class="card">
    <div class="card-title">Secure</div>
    <div class="card-body">SOC 2 certified.</div>
  </div>
</div>
```

The CSS:
```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 16px;
  margin: 16px 0;
}
.card {
  background: #fff;
  border: 1px solid #eaecf0;
  border-radius: 8px;
  padding: 16px 20px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.04);
  transition: box-shadow 0.2s;
}
.card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}
.card-title {
  font-size: 1.1em;
  font-weight: 600;
  margin-bottom: 8px;
  color: #36c;
}
.card-body {
  color: #54595d;
  line-height: 1.5;
}
@media (prefers-color-scheme: dark) {
  .card { background: #101418; border-color: #2c2c2c; }
  .card-body { color: #c8ccd1; }
  .card-title { color: #88aaee; }
}
```

**Note:** `prefers-color-scheme` support only works on Vector 2022+ and Citizen skins.

---

## 6. Accordion / collapsible sections

### Simple table-based accordion

```wiki
{| class="wikitable mw-collapsible mw-collapsed"
! More details...
|-
Detailed content, hidden by default.
|}
```

### Detailed (mw-collapsible on any element)

```wiki
<div class="mw-collapsible mw-collapsed">
This content is collapsed by default.
</div>
```

### Button-friendly accordion (details/summary)

MediaWiki 1.40+ supports the native `<details>` tag:

```wiki
<details>
<summary>Details (click to expand)</summary>
Hidden content that only appears on click.
</details>
```

### Advanced accordion with TemplateStyles

```wiki
<templatestyles src="Modul:Accordion/styles.css" />

<details class="accordion">
  <summary>Step 1: Preparations</summary>
  <div class="accordion-body">First step details...</div>
</details>
<details class="accordion">
  <summary>Step 2: Configuration</summary>
  <div class="accordion-body">Second step details...</div>
</details>
```

---

## 7. Tabs (with the TabberNeue extension)

If TabberNeue is installed:

```wiki
<tabber>
|-| First tab =
This is the first tab's content.
|-| Second tab =
This is the second tab's content.
|-| Third tab =
This is the third tab's content.
</tabber>
```

### Tabs with TemplateStyles (native solution)

If TabberNeue is not available, you can use a CSS-based solution, but the
clickable tabs require JavaScript. **TabberNeue is simpler, if available.**

---

## 8. Breadcrumbs

```wiki
{{#breadcrumb:Home|Project|Subpage}}
```

or manually (if there is no breadcrumb template):

```wiki
[[Home]] > [[Project]] > Subpage
```

The `>` character can be used as a breadcrumb separator without spaces. On some
skins (VisualEditor) it looks better if you use the `›` character.

---

## 9. Navigation box (sidebar template)

```wiki
{{Sidebar
|title=Name
|content=
[[First page]]
[[Second page]]
*[[Sub-page 1]]
*[[Sub-page 2]]
*[[Sub-page 3]]
|content2=
[[Third page]]
[[Fourth page]]
}}
```

The Sidebar template is available as `Template:Sidebar` on most wikis.

---

## 10. Infobox

The Infobox is the most characteristic "modern" MediaWiki element. The simplest version:

```wiki
{{Infobox
|title=Project name
|image=[[File:Logo.png|180px|center]]
|header1=Basic data
|label2=Version
|data2=2.0
|label3=Release date
|data3=2025-01-15
|label4=Author
|data4=[[Anna Kovacs]]
|label5=License
|data5=[[MIT]]
|header6=Links
|data6=
*[[Documentation]]
*[[Source code]]
*[[Issue tracker]]
}}
```

Most wikis have their own Infobox template (e.g. `Template:Infobox software`,
`Template:Infobox person`). **Always use the target wiki's own Infobox**, not
a generic one.

---

## 11. Code block design

### Basic syntax highlighting

```wiki
<syntaxhighlight lang="python">
def fibonacci(n: int) -> int:
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)
</syntaxhighlight>
```

### Numbered code (with CSS)

`<syntaxhighlight>` supports the `line` and `highlight` parameters:

```wiki
<syntaxhighlight lang="python" line start="1" highlight="2,4">
def hello(name):
    print(f"Hello, {name}!")  # line 2 highlighted
    return name
    return None  # line 4 highlighted
</syntaxhighlight>
```

### Code with a title

```wiki
{{Codesample
|lang=python
|code=
def hello():
    return "Hello"
|title=hello.py
}}
```

### Code + explanation (two columns)

The simplest: a table with two columns.

```wiki
{| class="wikitable"
! Code
! Explanation
|-
|
<syntaxhighlight lang="python">
def hello():
    return "Hello"
</syntaxhighlight>
|
The <code>hello</code> function returns a simple greeting.
|}
```

### Button (CTA) style code snippet

```wiki
<div style="background: #f8f9fa; border: 1px solid #eaecf0; border-radius: 6px; padding: 12px 16px; font-family: monospace;">
$ npm install mediawiki-cli
</div>
```

---

## 12. References and footnotes

### Simple footnote

```wiki
This is a statement.<ref>{{cite web|url=https://example.com|title=Title|author=Author|date=2025-01-15}}</ref>
```

### List of footnotes

```wiki
<references />
```

or arranged in columns:

```wiki
<references responsive="0" columns="2" />
```

### Unnumbered footnote (simple)

```wiki
This is the approach we use.<ref name="source1">Source: Internal documentation, 2025.</ref>

<references />
```

### Reused footnote

```wiki
First mention.<ref name="smith2020" />
Later mention.<ref name="smith2020" />

<references />
<ref name="smith2020">Smith, John (2020). "Title". Journal, 12(3), 45-67.</ref>
```

### Recommended templates

```wiki
{{cite web|url=...|title=...|author=...|date=...}}
{{cite book|last=...|first=...|title=...|year=...|publisher=...|isbn=...}}
{{cite journal|last=...|first=...|title=...|journal=...|volume=...|issue=...|year=...|pages=...}}
{{cite news|title=...|url=...|publisher=...|date=...}}
```

---

## 13. Mathematical formulas

MediaWiki supports TeX syntax (Math extension):

```wiki
Pythagorean theorem: <math>a^2 + b^2 = c^2</math>

An equation: <math>\sum_{i=1}^{n} i = \frac{n(n+1)}{2}</math>
```

---

## 14. Diagrams, flowcharts (Mermaid / Graphviz)

If the Mermaid extension is installed:

```wiki
<mermaid>
graph TD
  A[Start] --> B{Decision}
  B -->|Yes| C[Path 1]
  B -->|No| D[Path 2]
  C --> E[End]
  D --> E
</mermaid>
```

If Graphviz is installed:

```wiki
<graphviz>
digraph G {
  A -> B;
  B -> C;
}
</graphviz>
```

**Note:** Check whether these extensions are available on the target wiki.

---

## 15. Design checklist (before you finish the article)

Before you declare the article "done", check:

- [ ] Is there a clear lead (1-2 sentences) at the top of the article?
- [ ] Is the heading hierarchy logical (max 3 levels, rarely 4)?
- [ ] Does every table have a `class="wikitable"` attribute?
- [ ] Do sortable tables have `sortable`?
- [ ] Does every image have an `alt=` attribute?
- [ ] Is code in `<syntaxhighlight lang="...">`, not `<pre>`?
- [ ] Are highlights (Note, Tip, Warning) using MediaWiki's own templates?
- [ ] Are categories at the bottom of the page?
- [ ] Are footnotes listed with the `<references />` tag?
- [ ] Are there no empty sections under "== Heading =="?
- [ ] Are there no level 1 (=) headings?
- [ ] Do tables have a `|+` caption after `{|` when there is a title?
- [ ] Are pipe characters escaped (`{{!}}` where needed)?
- [ ] Are HTML entities correct (`&amp;` `&lt;` etc.)?
- [ ] Is the mobile view readable too? (few tables, many paragraphs)

---

## 16. Modern MediaWiki skins (comparison)

| Skin | Strength | Weakness |
|------|---------|-----------|
| Vector 2022 (default) | modern, clean, dark mode | limited customization |
| Citizen | beautiful, command palette, customizable | requires installation |
| Timeless | responsive, well-structured | feels dated |
| Chameleon | Bootstrap-based, customizable | cumbersome to configure |
| Medik | modern, clean | not widely used |

If the user asks for a "modern, clean" look, but the wiki uses Vector 2010 by
default, point out that the Citizen or Vector 2022 skin would provide a better experience.

---

## 17. When NOT to use design

- **If the target wiki allows templates, BUT has no TemplateStyles** — don't write
  complex CSS, use built-in templates instead (`{{Note}}`, `{{Tip}}`).
- **If the target wiki runs a very old MediaWiki version** — check that
  `class="wikitable sortable"`, `<syntaxhighlight>`, `<gallery>` are supported.
- **If the target wiki is private/intranet** — there are probably no built-in
  templates, everything must be solved with custom templates.
- **If the user explicitly asks for a clean, minimalist style** — don't force
  lots of colors and boxes.

---

Sources:
- <https://www.mediawiki.org/wiki/Documentation/Style_guide>
- <https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style>
- <https://www.mediawiki.org/wiki/Skin:Citizen>
- <https://www.mediawiki.org/wiki/Skin:Timeless>
- <https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style/Layout>
- <https://www.mediawiki.org/wiki/Extension:TemplateStyles>
