# 02 — Design patterns for modern, clean MediaWiki articles

> Sources: <https://www.mediawiki.org/wiki/Documentation/Style_guide>,
> <https://www.mediawiki.org/wiki/Skin:Citizen>,
> <https://www.mediawiki.org/wiki/Skin:Timeless>,
> <https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style>

> **HARD RULE — native MediaWiki only.** Every pattern in this file uses
> elements that ship with MediaWiki core (wikitext, `class="wikitable"`,
> `<div>`, `<blockquote>`, `<pre>`, `<ref>`, `<gallery>`, `<details>`, magic
> words, native inline CSS). NO `{{Note}}`/`{{Tip}}`/`{{Warning}}`/
> `{{Caution}}`/`{{ambox}}`, NO `<templatestyles>`, NO `<syntaxhighlight>`,
> NO `{{Infobox}}`, NO `{{Sidebar}}`, NO `{{cite}}`, NO `<mermaid>`, NO
> `<graphviz>`, NO `<tabber>`, NO `{{#breadcrumb}}`, NO ParserFunctions /
> Scribunto / Cite / Highlight / Mermaid / Graph / TemplateStyles / TabberNeue.
> The exact mapping is in `references/00-native-only-mapping.md`.

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

## 4. Highlights and boxes (your best friend) — native `<div>` only

> **Hard rule:** do NOT use `{{Note|...}}`, `{{Tip|...}}`, `{{Warning|...}}`,
> `{{Caution|...}}`, `{{Important|...}}`, `{{NoteTip|...}}`, `{{ambox|...}}`,
> `{{Notice|...}}`, `{{Outdated|...}}`, `{{Historical}}`, `{{Fixme}}`,
> `{{Todo|...}}`, `{{Update}}`, or any `<templatestyles>`-loaded class. They
> render as raw text on wikis where the templates / extension are not defined.
> Use only the inline-CSS `<div>` patterns below.

### Note (informational, blue)

```wiki
<div style="background:#eaf3ff;border-left:4px solid #36c;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Megjegyzés:''' This is an informational note.
</div>
```

### Tip (success, green)

```wiki
<div style="background:#e6f4ea;border-left:4px solid #14866d;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Tipp:''' Try this trick.
</div>
```

### Warning (yellow)

```wiki
<div style="background:#fef6e7;border-left:4px solid #fc3;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Figyelem:''' Be careful, this can cause data loss.
</div>
```

### Caution (red, critical)

```wiki
<div style="background:#fee7e6;border-left:4px solid #d33;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Vigyázat:''' Critical warning!
</div>
```

### Important (purple)

```wiki
<div style="background:#f0e7ff;border-left:4px solid #7c3aed;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Fontos:''' Be sure to read this.
</div>
```

### Page-level banner (top of the article)

```wiki
<div style="background:#fef6e7;border:1px solid #fc3;padding:12px 16px;margin:0 0 16px 0;border-radius:4px;color:#202122;">
'''Az oldal karbantartás alatt áll.'''
</div>
```

### Outdated / historical / FIXME / TODO / UPDATE banners

The same recipe, with a different background colour and a one-line message.
See `references/00-native-only-mapping.md` section 3.2 for the full list of
colour recipes.

### Why no TemplateStyles here?

TemplateStyles is an optional extension. On wikis that do not have it, the
`<templatestyles src="...">` tag is shown as raw text and any `class="hl-..."`
divs end up unstyled. Inline `<div style="...">` works on every MediaWiki
install with no extension.

---

## 5. Card layout (native wikitable only)

> **Hard rule:** do NOT use TemplateStyles (`<templatestyles src="...">` +
> `<div class="card-grid">`). The portable equivalent is a multi-column
> wikitable with inline CSS on each cell — works on every MediaWiki install.

### Three-column cards using wikitable + inline cell colour

```wiki
{| class="wikitable" style="width:100%; text-align:center;"
|+ Our three services
|-
| style="width:33%; background:#f0f7ff;" |
'''Free'''
<br />
Available to everyone.
| style="width:33%; background:#e6f4ea;" |
'''Fast'''
<br />
Set up in 5 minutes.
| style="width:33%; background:#fef6e7;" |
'''Secure'''
<br />
SOC 2 certified.
|}
```

### "Card" with icon, title, body, and CTA link

```wiki
{| class="wikitable" style="width:300px; margin:8px 0;"
|-
| style="text-align:center; background:#f0f7ff; font-size:2em;" | 🎉
|-
! style="text-align:center;" | Free
|-
| style="text-align:center;" |
Available to everyone.
<br /><br />
[https://example.com '''Sign up →''']
|}
```

### Why no `<div class="card-grid">`?

`card-grid` requires a TemplateStyles stylesheet. The equivalent wikitable
above renders the same look on every MediaWiki install with no extension and
no extra page.

---

## 6. Accordion / collapsible sections (core-only)

### Simple table-based accordion (`mw-collapsible`)

`class="mw-collapsible"` is a core MediaWiki class — no extension required.
It is rendered client-side by the standard MediaWiki JavaScript bundle.

```wiki
{| class="wikitable mw-collapsible mw-collapsed"
! More details...
|-
Detailed content, hidden by default.
|}
```

### `<div class="mw-collapsible">` (core)

```wiki
<div class="mw-collapsible mw-collapsed">
This content is collapsed by default.
</div>
```

### Native HTML5 `<details>` / `<summary>` (MediaWiki ≥ 1.40)

The native HTML5 tag renders identically without any extension or JS
hookup — pure core.

```wiki
<details>
<summary>Step 1: Preparations</summary>
First step details...
</details>

<details>
<summary>Step 2: Configuration</summary>
Second step details...
</details>
```

### Why no TemplateStyles accordion?

TemplateStyles + `<details class="accordion">` requires the TemplateStyles
extension. The `<details>` / `<summary>` native tag is identical in look and
works on every MediaWiki install with no extension. Use it instead.

---

## 7. Tabs (native multi-column wikitable — no TabberNeue)

> **Hard rule:** do NOT use `<tabber>` (TabberNeue extension) and do NOT use
> TemplateStyles + JS for tabs. The portable equivalent is a multi-column
> wikitable with one cell per "tab". On wide screens the columns sit
> side-by-side; on narrow screens the wikitable flows vertically.

```wiki
{| class="wikitable"
! style="width:33%;" | Overview
! style="width:33%;" | Examples
! style="width:33%;" | Limits
|-
| valign="top" |
Short summary text goes here.
* Bullet 1
* Bullet 2
| valign="top" |
Example 1.

Example 2.
| valign="top" |
* Only on POSIX systems
* Maximum 1 GB of data
|}
```

For a stacked, one-above-the-other layout (more like a real accordion),
use `class="mw-collapsible"` on each header row of separate wikitables.

---

## 8. Breadcrumbs (native wikilink chain — no `{{#breadcrumb}}`)

> **Hard rule:** do NOT use `{{#breadcrumb:Home|Project|Subpage}}` (extension)
> or `{{Breadcrumb|link1=...|link2=...}}` (custom template). Use a plain
> wikilink chain inside a small `<div>`.

```wiki
<div style="font-size:0.9em;color:#54595d;margin-bottom:12px;">
[[Home]] &rsaquo; [[Project]] &rsaquo; Subpage
</div>
```

The `›` (single right-pointing angle quotation mark) is visually cleaner than
`>` on most skins. Use `[[Page|Display text]]` to control the link label.

---

## 9. Navigation box / Sidebar (native wikitable)

> **Hard rule:** do NOT use `{{Sidebar|title=...|content=...}}` (custom
> template). Use a `class="wikitable"` floated to the right (or to the left)
> with the same content.

```wiki
{| class="wikitable" style="width:240px; float:right; margin:0 0 1em 1em;"
|+ '''Navigation'''
|-
|
'''Main topics'''
* [[First page]]
* [[Second page]]
* [[Third page]]
|-
|
'''Sub-pages'''
* [[Sub-page 1]]
* [[Sub-page 2]]
|-
|
'''External links'''
* [https://example.com External source]
|}
```

---

## 10. Infobox (native wikitable — no `{{Infobox}}`)

> **Hard rule:** do NOT use `{{Infobox|title=...|image=...|label1=...}}`
> (custom template). The portable equivalent is a `class="wikitable"` floated
> to the right, with one row per data field.

```wiki
{| class="wikitable" style="float:right; width:300px; margin:0 0 1em 1em;"
|+ '''Project name'''
|-
| style="text-align:center;" | [[File:Logo.png|180px|center|alt=Logo]]
|-
! Basic data
|-
| '''Version:''' || 2.0
|-
| '''Release date:''' || 2026-01-15
|-
| '''Author:''' || [[Anna Kovacs]]
|-
| '''License:''' || [[MIT]]
|-
! Links
|-
| [[Documentation]]
|-
| [https://github.com/project/project GitHub]
|-
| [https://github.com/project/project/issues Issue tracker]
|}
```

The exact same recipe handles the inverse layout (full-width "card" header at
the top of the article) — use a single-row, single-cell table with inline
`style="background:..."`:

```wiki
{| style="width:100%; background:linear-gradient(135deg,#36c 0%,#447ff5 100%); color:#fff; border-radius:8px; padding:24px; margin-bottom:24px;"
|-
| style="text-align:center;" |
<div style="font-size:1.5em;font-weight:600;">Article title</div>
<div style="margin-top:8px;opacity:0.9;">Short tagline</div>
|}
```

---

## 11. Code block design (native `<pre>` only)

### Basic code block (`<pre>` — no syntax highlighting)

```wiki
<pre>
def fibonacci(n: int) -> int:
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)
</pre>
```

### Code with a filename label (native, replaces `{{Codesample|...}}`)

```wiki
'''<code>hello.py</code>'''
<pre>
def hello():
    return "Hello"
</pre>
```

### Code with a "file header" (visual `{{Codesample|...}}` equivalent)

```wiki
{| class="wikitable" style="background:#f8f9fa;border:1px solid #eaecf0;border-radius:4px;padding:0;"
|-
| style="background:#eaecf0;border-bottom:1px solid #c8ccd1;padding:4px 8px;font-family:monospace;font-size:0.9em;" | hello.py
|-
| style="padding:12px;" |
<pre>
def hello():
    return "Hello"
</pre>
|}
```

### Code + explanation (two columns)

The simplest: a wikitable with two columns. Each cell contains a `<pre>` for
the code and a `<p>` for the prose.

```wiki
{| class="wikitable"
! Code
! Explanation
|-
|
<pre>
def hello():
    return "Hello"
</pre>
|
The <code>hello</code> function returns a simple greeting.
|}
```

### Button (CTA) style code snippet

```wiki
<div style="background:#f8f9fa; border:1px solid #eaecf0; border-radius:6px; padding:12px 16px; font-family:monospace;">
$ npm install mediawiki-cli
</div>
```

### Terminal look (dark background)

```wiki
<div style="background:#1e1e1e; color:#d4d4d4; padding:12px 16px; border-radius:6px; font-family:'Courier New', monospace; margin:12px 0;">
$ <span style="color:#4ec9b0;">npm</span> install mediawiki-cli
</div>
```

### Why no `<syntaxhighlight>` here?

`<syntaxhighlight lang="...">` requires the `SyntaxHighlight` extension.
Plain `<pre>` renders on every MediaWiki install. If the extension is
definitely available, you may add `<syntaxhighlight>` on top — but never
replace the native `<pre>`.

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

### Recommended citation strings (manual — replaces `{{cite ...}}`)

| Source type | Recommended format |
|-------------|--------------------|
| Web page | `[https://example.com Article title], Author, YYYY-MM-DD. Accessed YYYY-MM-DD.` |
| Book | `Smith, John (2020). ''Book title''. Publisher. ISBN 978-...` |
| Journal | `Smith, John (2020). "Article title". ''Journal name'', 12(3), 45-67.` |
| News | `[https://example.com/news News title]. ''Newspaper'', YYYY-MM-DD.` |
| Internal | `Source: Internal documentation, YYYY-MM-DD.` |

> The Cite extension's `{{cite web|...}}`, `{{cite book|...}}`, etc. require an
> extension that may not be installed. Use the manual format above so the
> citation always renders correctly on stock MediaWiki.

---

## 13. Mathematical formulas (native HTML only)

> **Hard rule:** do NOT use `<math>...</math>` — that requires the Math
> extension. Use the native HTML `<sup>` / `<sub>` tags and the
> `class="texhtml"` wrapper if you want the same vertical alignment as
> LaTeX. Everything renders on a stock MediaWiki install.

### Inline formula

```wiki
Pythagorean theorem: <span class="texhtml"><i>a</i><sup>2</sup> + <i>b</i><sup>2</sup> = <i>c</i><sup>2</sup></span>
```

### Display formula (centered, larger)

```wiki
<div class="texhtml" style="text-align:center; margin:16px 0; font-size:1.1em;">
<i>f</i>(<i>x</i>) = <i>a</i><sub>0</sub> + &sum;<sub><i>i</i>=1</sub><sup><i>n</i></sup> <i>a<sub>i</sub></i><i>x<sup>i</sup></i>
</div>
```

### Common mathematical entities (HTML entities)

| Glyph | HTML entity |
|-------|-------------|
| × | `&times;` |
| ÷ | `&divide;` |
| ± | `&plusmn;` |
| √ | `&radic;` |
| ∑ | `&sum;` |
| ∏ | `&prod;` |
| ∫ | `&int;` |
| ∞ | `&infin;` |
| ≤ | `&le;` |
| ≥ | `&ge;` |
| ≠ | `&ne;` |
| ≈ | `&asymp;` |
| ° | `&deg;` |
| π | `&pi;` |

If your target wiki has the Math extension installed, you may add `<math>`
on top for prettier formulas. Native HTML markup is the portable default.

---

## 14. Diagrams, flowcharts (no Mermaid, no Graphviz)

> **Hard rule:** do NOT use `<mermaid>...</mermaid>` or
> `<graphviz>...</graphviz>` — both require extensions. Use ASCII inside
> `<pre>` (always available) or a native `<table>` box-and-line diagram
> (full visual control with inline CSS).

### ASCII diagram in `<pre>`

```wiki
<pre>
[Start]
   |
   v
{Decision: continue?}
   |
   +--- no --> [Stop]
   |
  yes
   |
   v
[Step 1] --> [Step 2] --> [Done]
</pre>
```

### Box-and-line diagram with native `<table>`

```wiki
{| style="border:none; margin: 12px auto;"
|+ '''Workflow'''
|-
| style="text-align:center; padding:8px 16px; background:#eaecf0; border:1px solid #c8ccd1; border-radius:6px;" | Start
| style="text-align:center; font-size:1.4em;" | &darr;
| style="text-align:center; padding:8px 16px; background:#eaf3ff; border:1px solid #36c; border-radius:6px;" | Step 1
| style="text-align:center; font-size:1.4em;" | &darr;
| style="text-align:center; padding:8px 16px; background:#eaf3ff; border:1px solid #36c; border-radius:6px;" | Step 2
| style="text-align:center; font-size:1.4em;" | &darr;
| style="text-align:center; padding:8px 16px; background:#d5fdf4; border:1px solid #14866d; border-radius:6px;" | Done
|}
```

### Decision-tree wikitable

```wiki
{| class="wikitable"
! Step !! Action !! Outcome
|-
| 1 || Start process || Process running
|-
| 2 || Check condition || Yes / No
|-
| 3a || If Yes || Continue to step 4
|-
| 3b || If No || Abort process
|-
| 4 || Finalize || Done
|}
```

### State-machine wikitable

```wiki
{| class="wikitable"
! State !! Trigger !! Next state
|-
| Idle || user logs in || Authenticating
|-
| Authenticating || credentials valid || Logged in
|-
| Authenticating || credentials invalid || Error
|-
| Logged in || user logs out || Idle
|}
```

These three recipes cover the vast majority of diagram needs without extensions.

---

## 15. Design checklist (before you finish the article)

Before you declare the article "done", check:

- [ ] Is there a clear lead (1-2 sentences) at the top of the article?
- [ ] Is the heading hierarchy logical (max 3 levels, rarely 4)?
- [ ] Does every table have a `class="wikitable"` attribute?
- [ ] Do sortable tables have `sortable`?
- [ ] Does every image have an `alt=` attribute?
- [ ] Is code in `<pre>` (NOT `<syntaxhighlight>` — requires extension)?
- [ ] Are callouts inline-CSS `<div>` (NOT `{{Note}}`, `{{Tip}}`, `{{Warning}}`)?
- [ ] Are categories at the bottom of the page?
- [ ] Are footnotes listed with the `<references />` tag (NOT `{{cite ...}}`)?
- [ ] Is the infobox a wikitable floated right (NOT `{{Infobox|...}}`)?
- [ ] Are breadcrumbs a wikilink chain (NOT `{{#breadcrumb}}`)?
- [ ] Are diagrams ASCII-in-`<pre>` or `<table>` (NOT `<mermaid>`/`<graphviz>`)?
- [ ] Are math expressions HTML `<sup>`/`<sub>` (NOT `<math>`)?
- [ ] Are there no empty sections under `== Heading ==`?
- [ ] Are there no level 1 (`=`) headings?
- [ ] Do tables have a `|+` caption after `{|` when there is a title?
- [ ] Are pipe characters escaped (`{{!}}` where needed)?
- [ ] Are HTML entities correct (`&amp;`, `&lt;`, etc.)?
- [ ] Is the mobile view readable too? (few tables, many paragraphs)
- [ ] **No `<templatestyles>` tag anywhere in the wikitext**

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

- **If the target wiki is stock MediaWiki with no extensions** — use only the
  native toolbox (wikitext, `class="wikitable"`, `<div>`, `<blockquote>`,
  `<pre>`, `<ref>`, `<gallery>`, `<details>`, inline CSS). NEVER fall back to
  `{{Note}}`, `{{Tip}}`, `{{Warning}}` — those templates probably don't
  exist on the target wiki either.
- **If the target wiki runs an old MediaWiki version (< 1.39)** — avoid
  `<details>` / `<summary>` (added in 1.40); use `class="mw-collapsible"`
  on `<div>` or `<table>` instead. `<gallery>` is supported from 1.28+.
- **If the target wiki is private/intranet** — assume only core; never rely
  on third-party templates that may or may not be installed.
- **If the user explicitly asks for a clean, minimalist style** — don't force
  lots of colors and boxes.

---

Sources:
- <https://www.mediawiki.org/wiki/Documentation/Style_guide>
- <https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style>
- <https://www.mediawiki.org/wiki/Skin:Citizen>
- <https://www.mediawiki.org/wiki/Skin:Timeless>
- <https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style/Layout>
