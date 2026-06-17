# 00 — Native MediaWiki mapping (no extensions, no custom templates)

> **Goal of this file:** provide a single, copy-paste-ready reference for every
> "fancy" MediaWiki element (callout boxes, infoboxes, breadcrumbs, code with
> filename, etc.) so that the produced wikitext works on **any** MediaWiki
> install — without `TemplateStyles`, `Scribunto`, `ParserFunctions`, `Cite`,
> `Highlight`, `Mermaid`, `Graph`, or any other extension, and without any
> custom template (`{{Note}}`, `{{Tip}}`, `{{Warning}}`, `{{Caution}}`,
> `{{Infobox}}`, `{{Sidebar}}`, `{{Codesample}}`, `{{cite}}`, `{{Graph:Chart}}`,
> etc.).

> **Rule of thumb:** if it cannot be written with one of the items in the
> "Native MediaWiki toolbox" below, it does not belong in a portable article.
> It can still be added on wikis that DO have the extension, but always as a
> fallback with a `<noinclude>` warning or a plain alternative alongside.

---

## 1. Native MediaWiki toolbox (always available)

These work on every MediaWiki install (any version ≥ 1.20, with default
configuration). Nothing to install, nothing to enable, no permission required.

### Text formatting

| Effect | Syntax |
|--------|--------|
| Italic | `''text''` |
| Bold | `'''text'''` |
| Bold + italic | `'''''text'''''` |
| Strike | `<s>text</s>` |
| Underline | `<u>text</u>` |
| Sup / sub | `<sup>text</sup>` / `<sub>text</sub>` |
| Smaller / larger | `<small>text</small>` / `<big>text</big>` |
| Inline code | `<code>code</code>` |
| Keyboard key | `<kbd>Ctrl</kbd>` |
| Variable | `<var>name</var>` |
| Abbreviation | `<abbr title="...">JS</abbr>` |
| Line break | `<br />` |
| Horizontal rule | `----` (own line) |
| Paragraph break | blank line |
| Disable wikitext | `<nowiki>...</nowiki>` |

### Headings

```
== Level 2 ==     ← most common
=== Level 3 ===
==== Level 4 ====
===== Level 5 =====
====== Level 6 ======
```

> NEVER use level 1 (`= ... =`) — that is reserved for the page title.

### Lists

```
* unordered
** nested unordered
# ordered
## nested ordered
; term
: definition
```

### Links

| Target | Syntax |
|--------|--------|
| Internal link | `[[Page name]]` or `[[Page name\|display text]]` |
| External link | `[https://example.com label]` |
| Email | `[mailto:user@example.com user]` |

### Images

```
[[File:Name.jpg|thumb|alt=description|caption text]]
[[File:Name.jpg|180px|center|alt=description|caption text]]
```

Every image MUST carry an `alt=` attribute.

### Tables (native, with inline CSS)

```
{| class="wikitable"
|+ Caption (optional)
! Header !! Header
|-
| cell || cell
|-
| cell || cell
|}
```

Modifiers:

- `class="wikitable sortable"` — clickable column sort
- `class="wikitable striped"` — zebra rows (MediaWiki ≥ 1.39)
- `class="wikitable mw-collapsible"` — collapsible
- `class="wikitable mw-collapsed"` — collapsed by default
- `style="float:right;width:300px;margin:0 0 1em 1em;"` — sidebar layout

### Categories

```
[[Category:Topic]]
[[Category:Topic|sort key]]
```

### Magic words (behavior switches)

| Word | Effect |
|------|--------|
| `__TOC__` | force table of contents |
| `__NOTOC__` | hide TOC |
| `__FORCETOC__` | TOC even on short pages |
| `__HIDDENCAT__` | hide category from listing |
| `__NOEDITSECTION__` | disable section-edit links |

### Magic words (variables, native)

```
{{PAGENAME}}        — current page name
{{NAMESPACE}}       — current namespace
{{FULLPAGENAME}}    — namespace + page
{{CURRENTYEAR}}     — e.g. 2026
{{CURRENTMONTH}}    — 01..12
{{CURRENTDAY}}      — 01..31
{{CURRENTTIME}}     — HH:MM
{{SITENAME}}        — wiki name
{{SERVER}}          — base URL
{{lc: TeXT}}        → "text"
{{uc: text}}        → "TEXT"
{{lcfirst: text}}   → "text"
{{ucfirst: text}}   → "Text"
{{anchorencode: x}} → URL-safe anchor
{{urlencode: x y}}  → "x+y"
```

> `{{#if:}}`, `{{#switch:}}`, `{{#expr:}}`, `{{#time:}}` need the
> **ParserFunctions** extension. They are installed on almost every public
> wiki, but if portability matters, prefer plain templates and conditional
> wikitext via categories / page structure.

### Footnotes (native `<ref>` / `<references />`)

```
This is a statement.<ref>Source: ...</ref>
Same source used again.<ref name="x" />
<references />
```

`<references responsive="0" columns="2" />` arranges footnotes in columns
(layout feature of core, no extension needed).

### Gallery (native `<gallery>`)

```
<gallery caption="Caption" widths="200" heights="150">
File:Image1.jpg|Description 1
File:Image2.jpg|Description 2
</gallery>
```

Modes: `traditional`, `packed`, `packed-hover`, `packed-overlay`, `slideshow`.
`packed-hover` is the most modern look — pure core, no extension needed.

### Block-level quote

```
<blockquote>
Long quoted passage, multiple paragraphs allowed.
</blockquote>
```

### Native code block (no syntax highlighting, no extension)

```
<pre>
any text
  preserves
    indentation
</pre>
```

### Collapsible content

| Method | Syntax | Notes |
|--------|--------|-------|
| Native HTML5 details | `<details><summary>Title</summary>body</details>` | works on MediaWiki ≥ 1.40 |
| Core class | `<div class="mw-collapsible">body</div>` | client-side JS, ships with core |
| Table variant | `{\| class="wikitable mw-collapsible mw-collapsed" ! Title \|- body \|}` | very common, no extension |

---

## 2. The forbidden list (extensions / custom templates)

If you find yourself writing any of these in a portable article, replace them
with the native equivalent from section 3.

| DO NOT WRITE (without confirming the extension exists) | Why |
|---|---|
| `{{Note\|...}}`, `{{Tip\|...}}`, `{{Warning\|...}}`, `{{Caution\|...}}`, `{{Important\|...}}` | Custom templates, only on Wikipedia/mediawiki.org |
| `{{ambox\|...}}`, `{{Notice\|...}}`, `{{Outdated\|...}}`, `{{Historical}}`, `{{Fixme}}`, `{{Todo\|...}}`, `{{Update}}` | Wikipedia message-box family, not core |
| `{{Infobox\|...}}`, `{{Sidebar\|...}}` | Domain-specific, every wiki has its own |
| `{{Codesample\|...}}`, `{{Key press\|...}}`, `{{Button\|...}}` | Wikipedia gadgets |
| `{{blockquote\|...}}`, `{{pull quote\|...}}`, `{{Quotation\|...}}` | Wikipedia formatting gadgets |
| `{{cite web\|...}}`, `{{cite book\|...}}`, `{{cite journal\|...}}`, `{{cite news\|...}}` | Needs `Cite` extension |
| `{{Graph:Chart\|...}}` | Needs `Graph` extension |
| `{{#breadcrumb:...}}`, `{{Breadcrumb\|...}}` | Extension / custom template |
| `{{Forrás\|...}}`, `{{Source\|...}}` | Custom Hungarian/Wikipedia template |
| `<templatestyles src="..." />` | Needs `TemplateStyles` extension |
| `<syntaxhighlight lang="...">` / `<source lang="...">` | Needs `SyntaxHighlight` (`Highlight`/`Geshi`) extension |
| `<mermaid>...</mermaid>` | Needs `Mermaid` extension |
| `<graphviz>...</graphviz>` | Needs `Graphviz` extension |
| `<tabber>...</tabber>` | Needs `TabberNeue` extension |
| `{{#invoke:Module:...}}` | Needs `Scribunto` extension |

> **Rule:** the article that ships to the user must use the native mapping
> below. The "what if the extension IS available?" question is answered in
> section 5 — only after the native version is already on the page.

---

## 3. Native replacement cookbook (copy-paste-ready)

### 3.1 Callout boxes (replacing `{{Note}}`, `{{Tip}}`, `{{Warning}}`, `{{Caution}}`, `{{Important}}`)

Every box is a plain `<div>` with inline CSS. No classes, no external
stylesheet, no template. The colours match the convention used in
`02-design-patterns.md`.

#### Note (informational, blue)

```wiki
<div style="background:#eaf3ff;border-left:4px solid #36c;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Megjegyzés:''' Ez egy információs doboz.
</div>
```

#### Tip (success, green)

```wiki
<div style="background:#e6f4ea;border-left:4px solid #14866d;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Tipp:''' Próbáld ki ezt a trükköt.
</div>
```

#### Warning (yellow)

```wiki
<div style="background:#fef6e7;border-left:4px solid #fc3;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Figyelem:''' Legyél óvatos, ez adatvesztést okozhat.
</div>
```

#### Caution (red, critical)

```wiki
<div style="background:#fee7e6;border-left:4px solid #d33;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Vigyázat:''' Kritikus figyelmeztetés!
</div>
```

#### Important (purple)

```wiki
<div style="background:#f0e7ff;border-left:4px solid #7c3aed;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Fontos:''' Mindenképpen olvasd el.
</div>
```

#### Compact one-liner (no padding, for inline use)

```wiki
<div style="background:#eaf3ff;border:1px solid #36c;padding:4px 10px;border-radius:4px;display:inline-block;">tömör szöveg</div>
```

### 3.2 Page-level banners (replacing `{{Notice}}`, `{{Outdated}}`, `{{Historical}}`, `{{Fixme}}`, `{{Todo}}`, `{{Update}}`)

Place at the very top of the page, before any heading:

```wiki
<div style="background:#fef6e7;border:1px solid #fc3;padding:12px 16px;margin:0 0 16px 0;border-radius:4px;color:#202122;">
'''Az oldal karbantartás alatt áll.''' (Utoljára frissítve: 2026-01-15)
</div>
```

```wiki
<div style="background:#fee7e6;border:1px solid #d33;padding:12px 16px;margin:0 0 16px 0;border-radius:4px;color:#202122;">
'''Elavult tartalom.''' A cikk 2024-01-15 előtti információkat tartalmaz, kérlek frissítsd.
</div>
```

```wiki
<div style="background:#eaecf0;border:1px solid #54595d;padding:12px 16px;margin:0 0 16px 0;border-radius:4px;color:#202122;">
'''Historikus dokumentum.''' Csak archív célokat szolgál.
</div>
```

### 3.3 Infobox (replacing `{{Infobox|...}}`)

A wikitable floated to the right. This is the closest portable equivalent —
every MediaWiki install renders `class="wikitable"` out of the box.

```wiki
{| class="wikitable" style="float:right; width:300px; margin:0 0 1em 1em;"
|+ '''Projekt neve'''
|-
| style="text-align:center;" | [[File:Logo.png|180px|center|alt=Logo]]
|-
! Alapadatok
|-
| '''Verzió:''' || 2.0
|-
| '''Kiadás:''' || 2026-01-15
|-
| '''Szerző:''' || [[Anna Kovács]]
|-
| '''Licenc:''' || [[MIT]]
|-
! Linkek
|-
| [[Dokumentáció]]
|-
| [https://github.com/projekt/projekt GitHub]
|-
| [https://github.com/projekt/projekt/issues Issue tracker]
|}
```

For the inverse layout (full-width "card" header at the top of the article),
use a single-row, single-cell table with inline CSS:

```wiki
{| style="width:100%; background:linear-gradient(135deg,#36c 0%,#447ff5 100%); color:#fff; border-radius:8px; padding:24px; margin-bottom:24px;"
|-
| style="text-align:center;" |
<div style="font-size:1.5em;font-weight:600;">A cikk címe</div>
<div style="margin-top:8px;opacity:0.9;">Rövid leírás a cikkről</div>
|}
```

### 3.4 Sidebar / navigation box (replacing `{{Sidebar|...}}`)

```wiki
{| class="wikitable" style="width:240px; float:right; margin:0 0 1em 1em;"
|+ '''Navigáció'''
|-
|
'''Fő témák'''
* [[Első lap]]
* [[Második lap]]
* [[Harmadik lap]]
|-
|
'''Al-oldalak'''
* [[Al-oldal 1]]
* [[Al-oldal 2]]
|-
|
'''Külső linkek'''
* [https://example.com Külső forrás]
|}
```

### 3.5 Blockquote (replacing `{{blockquote|...}}`, `{{pull quote|...}}`, `{{Quotation|...}}`)

```wiki
<blockquote style="border-left:4px solid #c8ccd1;padding:8px 16px;margin:12px 0;color:#54595d;">
Life is what happens when you're busy making other plans.
<div style="text-align:right;margin-top:8px;font-size:0.9em;">— John Lennon</div>
</blockquote>
```

### 3.6 Keyboard key / button (replacing `{{Key press|...}}`, `{{Button|...}}`)

```wiki
Press <kbd>Ctrl</kbd>+<kbd>C</kbd> to copy.

Click the <kbd>Save</kbd> button to apply the changes.
```

### 3.7 Code with filename (replacing `{{Codesample|lang=...|code=...|title=...}}`)

Use `<pre>` plus a bold label above:

```wiki
'''<code>hello.py</code>'''
<pre style="background:#f8f9fa;border:1px solid #eaecf0;border-radius:4px;padding:12px;margin:8px 0;">
def hello():
    return "Hello"
</pre>
```

Or as a one-row table that visually mimics a "file header":

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

### 3.8 Code block (replacing `<syntaxhighlight lang="...">`)

Plain `<pre>` — no syntax highlighting, but renders on every wiki:

```wiki
<pre>
def fibonacci(n: int) -> int:
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)
</pre>
```

For a "monospace button" look (terminal / shell snippet):

```wiki
<div style="background:#1e1e1e; color:#d4d4d4; padding:12px 16px; border-radius:6px; font-family:'Courier New', monospace; margin:12px 0;">
$ <span style="color:#4ec9b0;">npm</span> install mediawiki-cli
</div>
```

### 3.9 References / footnotes (replacing `{{cite web|...}}` etc.)

Use native `<ref>` with an inline link — no Cite extension needed:

```wiki
This is a statement.<ref>[https://example.com "Cím"] — Szerző, 2026-01-15.</ref>
A second citation.<ref name="smith2020" />
The same source used again.<ref name="smith2020" />

== References ==

<references />
<ref name="smith2020">Smith, John (2020). "A cikk címe". Journal, 12(3), 45-67.</ref>
```

### 3.10 Breadcrumb (replacing `{{#breadcrumb:...}}`, `{{Breadcrumb|...}}`)

Plain wikilink chain:

```wiki
<div style="font-size:0.9em;color:#54595d;margin-bottom:12px;">
[[Főoldal]] &rsaquo; [[Projekt]] &rsaquo; Aloldal
</div>
```

### 3.11 Chart / graph (replacing `{{Graph:Chart|...}}`)

A wikitable that holds the same data points, with a caption:

```wiki
{| class="wikitable sortable"
|+ '''Negyedéves árbevétel (millió Ft)'''
|-
! Negyedév !! 2024 !! 2025
|-
| Q1 || 320 || 380
|-
| Q2 || 410 || 460
|-
| Q3 || 380 || 430
|-
| Q4 || 520 || 610
|}
```

Add a textual description immediately after the table for screen readers
and search engines:

```wiki
A táblázat a 2024-es és 2025-ös negyedéves árbevételt mutatja millió forintban.
Az éves növekedés 2025-ben közel 18% volt 2024-hez képest.
```

### 3.12 Diagram (replacing `<mermaid>...</mermaid>`, `<graphviz>...</graphviz>`)

A wikitable "node" + "edge" representation is the portable equivalent:

```wiki
{| class="wikitable"
|+ '''Folyamatábra: bejelentkezés'''
|-
! Lépés !! Művelet
|-
| 1 || Felhasználó megnyitja a bejelentkezési űrlapot
|-
| 2 || Elküldi az email + jelszó párost
|-
| 3 || Szerver ellenőrzi a hitelesítő adatokat
|-
| 4 || Sikeres esetben: átirányítás a főoldalra
|-
| 4 || Sikertelen esetben: hibaüzenet megjelenítése
|}
```

For a visually richer diagram (when ASCII art works), use `<pre>` with
monospace:

```wiki
<pre>
[ Start ] --> [ Login form ]
                |
                v
        [ Submit credentials ]
                |
        +-------+-------+
        |               |
   [ Valid ]      [ Invalid ]
        |               |
        v               v
[ Redirect ]    [ Show error ]
</pre>
```

### 3.13 Tabs (replacing `<tabber>...</tabber>`)

Use collapsible sections (one per "tab") in a wikitable — they appear
side-by-side on wide screens and stack on mobile:

```wiki
{| class="wikitable"
|-
! style="width:33%;" | Áttekintés
! style="width:33%;" | Példák
! style="width:33%;" | Korlátok
|-
|
Az eszköz célja, hogy ...

|
* Első példa
* Második példa

|
* Csak POSIX rendszereken működik
* Maximum 1 GB adat
|}
```

### 3.14 Source attribution (replacing `{{Source|...}}`, `{{Forrás|...}}`)

```wiki
<div style="background:#f8f9fa;border:1px solid #eaecf0;border-radius:4px;padding:8px 12px;margin:0 0 16px 0;font-size:0.9em;color:#54595d;">
Forrás: belső dokumentáció, 2026-01-15.
</div>
```

### 3.15 Hatnote (replacing `{{Main|...}}`, `{{See also|...}}`, `{{For|...}}`)

```wiki
<div style="font-size:0.9em;color:#54595d;margin-bottom:12px;">
Fő cikk: [[A téma részletesen]]. Lásd még: [[Kapcsolódó cikk]].
</div>
```

---

## 4. Quick conversion table

| Original | Native replacement |
|----------|--------------------|
| `{{Note\|text}}` | `<div style="background:#eaf3ff;border-left:4px solid #36c;...">'''Megjegyzés:''' text</div>` |
| `{{Tip\|text}}` | `<div style="background:#e6f4ea;border-left:4px solid #14866d;...">'''Tipp:''' text</div>` |
| `{{Warning\|text}}` | `<div style="background:#fef6e7;border-left:4px solid #fc3;...">'''Figyelem:''' text</div>` |
| `{{Caution\|text}}` | `<div style="background:#fee7e6;border-left:4px solid #d33;...">'''Vigyázat:''' text</div>` |
| `{{Important\|text}}` | `<div style="background:#f0e7ff;border-left:4px solid #7c3aed;...">'''Fontos:''' text</div>` |
| `{{Notice\|text}}` | top banner div (yellow) |
| `{{Outdated\|date}}` | top banner div (red, "Elavult") |
| `{{Historical}}` | top banner div (gray, "Historikus") |
| `{{Fixme}}` | top banner div (red, "Javítandó") |
| `{{Todo\|text}}` | top banner div (yellow, "Teendő") |
| `{{Update}}` | top banner div (blue, "Frissítendő") |
| `{{Infobox\|...}}` | `{\| class="wikitable" style="float:right;width:300px;..." \|}` |
| `{{Sidebar\|...}}` | `{\| class="wikitable" style="width:240px;float:right;..." \|}` |
| `{{blockquote\|...}}` | `<blockquote style="border-left:4px solid #c8ccd1;...">...</blockquote>` |
| `{{pull quote\|text}}` | `<blockquote>...</blockquote>` |
| `{{Quotation\|t\|s}}` | `<blockquote>...<div>— s</div></blockquote>` |
| `{{Codesample\|...}}` | bold label + `<pre>` |
| `{{Key press\|Ctrl\|S}}` | `<kbd>Ctrl</kbd>+<kbd>S</kbd>` |
| `{{Button\|Save}}` | `<kbd>Save</kbd>` |
| `{{cite web\|...}}` | `<ref>[url "title"] — author, date.</ref>` |
| `{{cite book\|...}}` | `<ref>Author (year). Title. Publisher. ISBN.</ref>` |
| `{{cite journal\|...}}` | `<ref>Author (year). "Title". Journal, vol(issue), pages.</ref>` |
| `{{cite news\|...}}` | `<ref>"Title". Publisher, date. [url]</ref>` |
| `{{Graph:Chart\|...}}` | `{\| class="wikitable sortable" \|}` + description |
| `<mermaid>...</mermaid>` | wikitable step list or ASCII art in `<pre>` |
| `<graphviz>...</graphviz>` | wikitable step list or ASCII art in `<pre>` |
| `<syntaxhighlight lang="x">` | `<pre>` (no highlighting) |
| `<source lang="x">` | `<pre>` (no highlighting) |
| `<tabber>...</tabber>` | multi-column wikitable or collapsible sections |
| `{{#breadcrumb:A\|B\|C}}` | `<div>[[A]] &rsaquo; [[B]] &rsaquo; C</div>` |
| `{{Breadcrumb\|...}}` | `<div>[[A]] &rsaquo; [[B]] &rsaquo; C</div>` |
| `{{Source\|...}}`, `{{Forrás\|...}}` | `<div style="background:#f8f9fa;...">Forrás: ...</div>` |
| `{{Main\|...}}`, `{{See also\|...}}` | `<div style="font-size:0.9em;color:#54595d;">Fő cikk: [[...]]. Lásd még: [[...]].</div>` |
| `<templatestyles src="...">` | inline `style="..."` on the element |
| `{{#invoke:Module:Foo\|bar}}` | describe the result in plain wikitext (table / list) |

---

## 5. Optional enhancements (only when the extension is confirmed present)

When the target wiki has been **explicitly** confirmed to ship a specific
extension, the original syntax MAY be added **in addition** to the native
fallback. Never replace the native version — always keep it as a `<noinclude>`
alternative or comment, so that the article remains portable.

Examples (one per extension):

- `Cite` extension available → keep `{{cite web|...}}` BUT also wrap it in
  `<ref>...</ref>` and provide a `<ref name="fallback">...</ref>` form for
  wikis without the extension.
- `SyntaxHighlight` extension available → use `<syntaxhighlight lang="x">`
  instead of `<pre>`; if you do, mark the page with `[[Category:SyntaxHighlight required]]`.
- `Mermaid` extension available → wrap the diagram in `<mermaid>...</mermaid>`
  AND keep a wikitable step-list fallback for non-Mermaid wikis.
- `TemplateStyles` available → add `<templatestyles src="...">` AFTER the
  page already works with inline CSS.

The default article, however, is the **native-only** version.

---

## 6. Verification of "native-only" compliance

Before declaring an article "done", check the following in the produced
wikitext:

- [ ] No `<templatestyles src="...">` tags.
- [ ] No `<syntaxhighlight lang="...">` or `<source lang="...">` tags.
- [ ] No `<mermaid>`, `<graphviz>`, `<tabber>`, `<math>` tags (or `<math>` is
      acceptable only if the Math extension is confirmed).
- [ ] No `{{#invoke:Module:...}}`, `{{#if:...}}`, `{{#switch:...}}`,
      `{{#expr:...}}`, `{{#time:...}}` (ParserFunctions / Scribunto calls).
- [ ] No `{{Note|...}}`, `{{Tip|...}}`, `{{Warning|...}}`, `{{Caution|...}}`,
      `{{Important|...}}`, `{{Notice|...}}`, `{{Outdated|...}}`,
      `{{Historical}}`, `{{Fixme}}`, `{{Todo|...}}`, `{{Update}}`.
- [ ] No `{{Infobox|...}}`, `{{Sidebar|...}}`, `{{Codesample|...}}`,
      `{{blockquote|...}}`, `{{pull quote|...}}`, `{{Quotation|...}}`,
      `{{Key press|...}}`, `{{Button|...}}`.
- [ ] No `{{cite web|...}}`, `{{cite book|...}}`, `{{cite journal|...}}`,
      `{{cite news|...}}`.
- [ ] No `{{Graph:Chart|...}}`, `{{#breadcrumb:...}}`, `{{Breadcrumb|...}}`,
      `{{Forrás|...}}`, `{{Source|...}}`.
- [ ] Every callout box uses inline `style="..."` on a `<div>`.
- [ ] Every infobox is a `class="wikitable"` table.
- [ ] Every sidebar is a `class="wikitable"` table.
- [ ] Every breadcrumb is a `<div>` with chained wikilinks.
- [ ] Every code block is `<pre>` (or a styled `<div>` for terminal look).
- [ ] Every citation is a `<ref>...</ref>` with a plain link / text inside.
- [ ] Every chart is a `class="wikitable sortable"` table.
- [ ] Every diagram is either a wikitable step list or ASCII art in `<pre>`.