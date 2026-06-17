# 01 — MediaWiki syntax cheatsheet (complete, with examples)

> Sources: <https://www.mediawiki.org/wiki/Cheatsheet>,
> <https://en.wikipedia.org/wiki/Help:Cheatsheet>,
> <https://www.mediawiki.org/wiki/Help:Formatting>,
> <https://www.mediawiki.org/wiki/Help:Tables>,
> <https://en.wikipedia.org/wiki/Help:Gallery_tag>,
> <https://www.mediawiki.org/wiki/Help:Magic_words>

This file is the complete **native-only** MediaWiki syntax reference. It uses
**only** the elements that ship with MediaWiki core: wikitext, `class="wikitable"`,
`<div>`, `<blockquote>`, `<pre>`, `<ref>`, `<gallery>`, `<details>`, and core magic
words. It does NOT show `<templatestyles>`, `<syntaxhighlight>`,
`<mermaid>`, `<tabber>`, ParserFunctions, Scribunto, Cite, Highlight, Mermaid,
Graph, TabberNeue, or any custom template call (`{{Note|...}}`, `{{Infobox|...}}`,
`{{cite ...}}`, etc.). For the explicit native replacement table see
`references/00-native-only-mapping.md`.

If the user asks any formatting question, or if any MediaWiki element must be
used in the output, **work from here**. Do not try to remember it from your own
head — always be consistent with MediaWiki's official native syntax.

---

## 1. Text formatting (usable anywhere)

| Effect | What you type | Result |
|------|-------------|----------|
| Italic | `''italic''` | *italic* |
| Bold | `'''bold'''` | **bold** |
| Bold + italic | `'''''bold and italic'''''` | ***bold and italic*** |
| Strikethrough | `<s>text</s>` | ~~text~~ |
| Underline | `<u>text</u>` | <u>text</u> |
| Superscript | `<sup>text</sup>` | text<sup>text</sup> |
| Subscript | `<sub>text</sub>` | text<sub>text</sub> |
| Smaller | `<small>text</small>` | <small>text</small> |
| Larger | `<big>text</big>` | <big>text</big> |
| Disable wikitext | `<nowiki>not ''formatted''</nowiki>` | not ''formatted'' |
| Line break within a cell | `<br />` or `<br>` | (line break) |
| Horizontal rule | `----` (on its own line) | — — — |
| Paragraph break | empty line | new paragraph |

**Important:** in MediaWiki, the number of apostrophes determines the formatting:
- `''x''` = italic
- `'''x'''` = bold
- `''''x''''` = 4 apostrophes = italic (because they pair up)
- `'''''x'''''` = 5 apostrophes = bold + italic

---

## 2. Headings (only at the start of a line)

```
== Level 2 heading ==     (most common)
=== Level 3 heading ===
==== Level 4 heading ====
===== Level 5 heading =====
====== Level 6 heading ======
```

- The level 1 heading (`= ... =`) is **reserved** for the page title; NEVER use it.
- Always put an empty line between the heading and the content (makes translation easier).
- Headings are automatically added to the table of contents (TOC).
- If the table of contents does not appear, place this at the top of the page: `__TOC__`
- To hide it: `__NOTOC__`

---

## 3. Lists (only at the start of a line)

### Unordered (bulleted)

```
* Apple
* Pear
** Pear - Williams
** Pear - Bosc
* Apricot
```

Result:
- Apple
- Pear
  - Pear - Williams
  - Pear - Bosc
- Apricot

### Ordered

```
# First
# Second
## Second - A subitem
# Third
```

### Definition list

```
; Server
: The machine that serves the clients.
; Client
: The machine that connects to the server.
```

Result:

Server
: The machine that serves the clients.

Client
: The machine that connects to the server.

### Mixed list

```
* Fruits
*# Apple
*# Pear
* Vegetables
*# Carrot
```

### Indentation for talk pages

```
: single indent
:: double indent
::: triple indent
```

---

## 4. Links

### Internal links (wikilink)

```
[[MediaWiki]]                  → MediaWiki (link to the MediaWiki page)
[[MediaWiki|MediaWiki software]] → MediaWiki software (different label)
[[Help:Contents]]              → Help:Contents (namespace + page)
[[#Syntax]]                    → #Syntax (anchor on the same page)
[[Help#Links]]                 → Help#Links (anchor on another page)
[[Hungary|hu]]                 → hu (shortened label)
```

### Category link (shows the category's contents)

```
[[:Category:Formatting]]       → link, does not put the page in the category
[[Category:Formatting]]        → puts the page in the category
[[Category:Formatting|sorted]] → sorts under the "sorted" key
```

### Interwiki links (Wikimedia projects)

```
[[w:Hungary]]                  → Wikipedia
[[commons:Category:Photos]]    → Wikimedia Commons
[[b:Hungarian]]                → Wikibooks
[[n:News]]                     → Wikinews
[[q:Hungary]]                  → Wikiquote
[[s:Source]]                   → Wikisource
[[v:Hungary]]                  → Wikivoyage
[[species:Homo sapiens]]       → Wikispecies
[[m:Main Page]]                → Meta-Wiki
[[mw:Help:Contents]]           → MediaWiki.org
```

### External links

```
http://example.com                 → automatic link
[http://example.com]               → numbered link [1]
[http://example.com Example.com]   → link labeled "Example.com"
[mailto:user@example.com E-mail]   → e-mail link
```

### Section link (slink)

```
{{slink|Help#Links}}              → links to the Help#Links section, using the page title as the label
{{slink|Help#Links|some text}}    → the "some text" label is shown
```

### Redirect

```
#REDIRECT [[Target page]]
#REDIRECT [[Target page#Section]]
```

### Pipe link trick

```
[[Target page|]]              → empty label, the page name itself is shown
[[Target page| ]]             → space label (less clean, but works)
```

---

## 5. Images and files

### Basic syntax

```wiki
[[File:Image.jpg]]
[[File:Image.jpg|thumb]]
[[File:Image.jpg|thumb|Caption text]]
[[File:Image.jpg|right|thumb|220px|Caption]]
[[File:Image.jpg|left|thumb|Caption]]
[[File:Image.jpg|center|thumb|Caption]]
[[File:Image.jpg|border|200px|Caption]]
```

### Size and layout

| Parameter | Effect |
|-----------|-------|
| `|thumb|` | thumbnail with frame |
| `|frame|` | frame around the image, with caption |
| `|frameless|` | thumbnail without frame |
| `|border|` | thin border |
| `|left` | float to the left |
| `|right` | float to the right |
| `|center` | center |
| `|none` | no formatting |
| `|200px` | width of 200 pixels |
| `|x200px` | height of 200 pixels (proportional width) |
| `|upright` | proportional scale based on the user's thumbnail setting |
| `|upright=0.5` | proportional scale to 0.5x |
| `|alt=Description` | alternative text |
| `|link=Target page` | make the image link to an internal page |

### File link (link to the file, not the displayed image)

```wiki
[[:File:Image.jpg]]         → link to the file's description page
[[Media:Image.jpg]]         → direct link to the file
[[File:Image.jpg|See link]] → the label links to the file
```

### Example: widely used image reference format

```wiki
[[File:Logo.png|thumb|right|220px|The project logo|alt=Red circle with a white "P" inside]]
```

---

## 6. Galleries

### Simple gallery

```wiki
<gallery>
File:Image1.jpg|First image
File:Image2.jpg|Second image
File:Image3.jpg|Third image
</gallery>
```

### Gallery modes

| Mode | Description |
|-----|--------|
| `mode=traditional` (default) | rows and columns, frame, caption below |
| `mode=nolines` | no frame, caption centered |
| `mode=packed` | uniform height, aligned, caption centered |
| `mode=packed-overlay` | caption in a translucent box at the bottom of the image |
| `mode=packed-hover` | caption appears on hover (packed-overlay on touch) |
| `mode=slideshow` | slideshow |

### Gallery attributes

```wiki
<gallery caption="Example gallery" widths="180px" heights="120px" perrow="3">
File:Image1.jpg|Image 1
File:Image2.jpg|alt=alternative text|Image 2
File:Image3.jpg|[[Internal link]] in the caption
File:Image4.jpg|This<br />is<br />a<br />multi-line<br />caption
</gallery>
```

| Attribute | Effect |
|-----------|-------|
| `caption="..."` | caption at the top of the gallery |
| `widths="180px"` | max width (no effect in packed/slideshow mode) |
| `heights="120px"` | max height (no effect in slideshow mode) |
| `perrow="3"` | number of images per row (no effect in packed/slideshow mode) |
| `showfilename=yes` | show filename below the caption |
| `class="center"` | center the gallery (does not work with perrow) |
| `class="skin-invert-image"` | invert images in dark mode |

### Recommended settings for a modern, clean look

```wiki
<gallery mode="packed-hover" heights="200px" caption="Modern gallery" class="center">
File:Image1.jpg|Caption
File:Image2.jpg|Caption
File:Image3.jpg|Caption
</gallery>
```

---

## 7. Tables

### Minimal table

```wiki
{|
| Apple
| Pear
|-
| Bread
| Pie
|}
```

Result:

| Apple | Pear |
|------|-------|
| Bread | Pie |

### Basic syntax

| Symbol | Function |
|-----------|---------|
| `{|` | start of table (required) |
| `|+` | caption |
| `|-` | new row (can be omitted for the first row) |
| `!` | header cell |
| `\|` | data cell |
| `\|}` | end of table (required) |

### Headers

```wiki
{| class="wikitable"
! Item !! Quantity !! Price
|-
| Apple || 10 || 700 Ft
|}
```

### Caption

```wiki
{|
|+ Food list
|-
| Apple || 10
|}
```

### Modern, sortable table (recommended)

```wiki
{| class="wikitable sortable"
! Name !! Age !! City
|-
| Anna || 28 || Budapest
|-
| Béla || 34 || Debrecen
|-
| Cecil || 25 || Pécs
|-
| Dani || 41 || Szeged
|}
```

### Style attributes

```wiki
{| class="wikitable" style="text-align: center; color: green;"
```

Cell-level style:
```wiki
| style="text-align:right;" | 12 333.00
```

Row style:
```wiki
|- style="font-style: italic; color: green;"
```

### Cells spanning rows and columns

```wiki
{| class="wikitable"
!colspan="6"| Shopping list
|-
|rowspan="2"| Bread and butter
| Pie
| colspan="2"| Croissant
|}
```

### Accessibility

```wiki
! scope="col" | Name
! scope="col" | Age
! scope="col" | City
|-
! scope="row" | Anna
| 28 || Budapest
```

### Useful classes

| Class | Effect |
|-------|-------|
| `wikitable` | light background, border, padding (default) |
| `wikitable sortable` | + sortable columns |
| `mw-collapsible` | collapsible table |
| `mw-collapsed` | collapsed by default |
| `plainrows` | minimal styling |
| `wikitable striped` | striped (zebra) rows |
| `wikitable hover-highlight` | row highlight on hover |

### Negative numbers at the start of a cell

❌ WRONG: `| -6` (interpreted as a row break)
✅ CORRECT: `| |-6` or `| &minus;6` or `| style="text-align:right;" | -6`

### Recommended modern table recipe

```wiki
{| class="wikitable sortable mw-collapsible"
! Name !! Role !! Status
|-
| Anna || Developer || Active
|-
| Béla || Tester || Active
|-
| Cecil || PM || On leave
|}
```

---

## 8. Code, code blocks, typography

### Inline code

```wiki
The <code>git status</code> command shows the changes.
Press <kbd>Ctrl</kbd>+<kbd>C</kbd> to copy.
Replace the <var>username</var> variable.
```

Result:
The <code>git status</code> command shows the changes.
Press <kbd>Ctrl</kbd>+<kbd>C</kbd> to copy.
Replace the <var>username</var> variable.

### Code block (native `<pre>` — no syntax highlighting, no extension)

```wiki
<pre>
def hello(name: str) -> str:
    return f"Hello, {name}!"

print(hello("World"))
</pre>
```

> **Why no `<syntaxhighlight>`?** The `<syntaxhighlight>` tag is provided by
> the `SyntaxHighlight` (formerly `Geshi`) extension. It is widespread but
> NOT a core MediaWiki element. To stay portable, use plain `<pre>`. If the
> target wiki definitely has the `SyntaxHighlight` extension, you may add it
> on top of the native version — but never replace the native version.

### Code block with a filename label (native, replaces `{{Codesample|...}}`)

```wiki
'''<code>hello.py</code>'''
<pre>
def hello():
    return "Hello"
</pre>
```

Or with a styled "file header":

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

### Keyboard shortcut (native, replaces `{{Key press|...}}`)

```wiki
To save, press <kbd>Ctrl</kbd>+<kbd>S</kbd>.
```

### Button (native, replaces `{{Button|...}}`)

```wiki
Click the <kbd>Save</kbd> button.
```

### Abbreviation

```wiki
<abbr title="JavaScript">JS</abbr> is a popular language.
```

### Inline code

```wiki
Use the <code>color: red</code> rule.
```

### Variable

```wiki
Replace the <var>username</var> variable.
```

---

## 9. Quotes

### Inline quote

```wiki
He said: "I'll be back soon".
```

### Block-level quote (native `<blockquote>`)

```wiki
<blockquote>
This is a longer quote that can span multiple lines.
In MediaWiki the <blockquote> tag can be used for this.
</blockquote>
```

### Quote with attribution (native, replaces `{{blockquote|...}}`, `{{pull quote|...}}`, `{{Quotation|...}}`)

```wiki
<blockquote style="border-left:4px solid #c8ccd1;padding:8px 16px;margin:12px 0;color:#54595d;">
Life is what happens when you're busy making other plans.
<div style="text-align:right;margin-top:8px;font-size:0.9em;">— John Lennon</div>
</blockquote>
```

> **Note:** do NOT use the Wikipedia/mediawiki.org-only quote templates
> (`{{blockquote|...}}`, `{{pull quote|...}}`, `{{Quotation|...}}`). They render
> as raw text on wikis where they are not defined. The native `<blockquote>`
> works on every MediaWiki install.

---

## 10. Highlights, boxes, messages (native `<div>` only)

> **Hard rule:** do NOT use `{{Note|...}}`, `{{Tip|...}}`, `{{Warning|...}}`,
> `{{Caution|...}}`, `{{Important|...}}`, `{{ambox|...}}`, `{{Notice|...}}`,
> `{{Outdated|...}}`, `{{Historical}}`, `{{Fixme}}`, `{{Todo|...}}`,
> `{{Update}}`, or any TemplateStyles class. They render as raw text on wikis
> where the templates are not defined. Use only the native `<div style="...">`
> patterns below.

### Callout box: Note (informational, blue)

```wiki
<div style="background:#eaf3ff;border-left:4px solid #36c;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Megjegyzés:''' Ez egy információs doboz.
</div>
```

### Callout box: Tip (success, green)

```wiki
<div style="background:#e6f4ea;border-left:4px solid #14866d;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Tipp:''' Próbáld ki ezt a trükköt.
</div>
```

### Callout box: Warning (yellow)

```wiki
<div style="background:#fef6e7;border-left:4px solid #fc3;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Figyelem:''' Legyél óvatos, ez adatvesztést okozhat.
</div>
```

### Callout box: Caution (red, critical)

```wiki
<div style="background:#fee7e6;border-left:4px solid #d33;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Vigyázat:''' Kritikus figyelmeztetés!
</div>
```

### Callout box: Important (purple)

```wiki
<div style="background:#f0e7ff;border-left:4px solid #7c3aed;padding:12px 16px;margin:12px 0;border-radius:4px;color:#202122;">
'''Fontos:''' Mindenképpen olvasd el.
</div>
```

### Page-level banner: under maintenance (yellow)

Place at the very top of the article, before the first heading:

```wiki
<div style="background:#fef6e7;border:1px solid #fc3;padding:12px 16px;margin:0 0 16px 0;border-radius:4px;color:#202122;">
'''Az oldal karbantartás alatt áll.'''
</div>
```

### Page-level banner: outdated (red)

```wiki
<div style="background:#fee7e6;border:1px solid #d33;padding:12px 16px;margin:0 0 16px 0;border-radius:4px;color:#202122;">
'''Elavult tartalom.''' A cikk 2024-01-01 előtti információkat tartalmaz.
</div>
```

### Page-level banner: historical (gray)

```wiki
<div style="background:#eaecf0;border:1px solid #54595d;padding:12px 16px;margin:0 0 16px 0;border-radius:4px;color:#202122;">
'''Historikus dokumentum.''' Csak archív célokat szolgál.
</div>
```

### Page-level banner: needs fixing (red)

```wiki
<div style="background:#fee7e6;border:1px solid #d33;padding:12px 16px;margin:0 0 16px 0;border-radius:4px;color:#202122;">
'''Javítandó:''' a cikk hiányos.
</div>
```

### Page-level banner: TODO (yellow)

```wiki
<div style="background:#fef6e7;border:1px solid #fc3;padding:12px 16px;margin:0 0 16px 0;border-radius:4px;color:#202122;">
'''Teendő:''' fejezd be a táblázatot.
</div>
```

### Page-level banner: needs update (blue)

```wiki
<div style="background:#eaf3ff;border:1px solid #36c;padding:12px 16px;margin:0 0 16px 0;border-radius:4px;color:#202122;">
'''Frissítésre szorul.'''
</div>
```

### Hatnote (native, replaces `{{Main|...}}`, `{{See also|...}}`, `{{For|...}}`)

```wiki
<div style="font-size:0.9em;color:#54595d;margin-bottom:12px;">
Fő cikk: [[A téma részletesen]]. Lásd még: [[Kapcsolódó cikk]].
</div>
```

### Why no TemplateStyles here?

TemplateStyles is an optional extension. On wikis that do not have it, the
`<templatestyles src="...">` tag is shown as raw text and any `class="hl-..."`
divs end up unstyled. Inline `<div style="...">` works on every MediaWiki
install with no extension.

---

## 11. Templates (native magic words only)

> **Hard rule:** do NOT use custom template calls like `{{Note|...}}`,
> `{{Infobox|...}}`, `{{cite web|...}}`, `{{Forrás|...}}`, `{{Source|...}}`,
> `{{#breadcrumb:...}}`, `{{#invoke:Module:...}}`, `{{#if:...}}`,
> `{{#switch:...}}`, etc. They need either a custom template installed on
> the target wiki, or the ParserFunctions / Scribunto extension. Stick to
> the **native core magic words** below.

### Calling a native magic word

```wiki
{{PAGENAME}}       → the current page name
{{NAMESPACE}}      → the current namespace
{{FULLPAGENAME}}   → namespace + page name
{{TALKPAGENAME}}   → the talk page name
{{REVISIONID}}     → the current revision ID
{{REVISIONUSER}}   → the editor's username
{{SITENAME}}       → the wiki name
{{SERVER}}         → the wiki's server URL
{{localurl:page}}  → the page's relative URL
{{fullurl:page}}   → the page's full URL
{{CURRENTYEAR}}    → 2026
{{CURRENTMONTH}}   → 06
{{CURRENTDAY}}     → 18
{{CURRENTTIME}}    → 14:25
{{NUMBEROFARTICLES}} — statistics
{{NUMBEROFPAGES}}   — statistics
{{NUMBEROFUSERS}}   — statistics
{{ns:1}}            — namespace ID → namespace name
```

### Conditional include (native parameter default)

```wiki
{{template name|{{{parameter|default}}}}}
```

This works ONLY if a template exists; for portable articles prefer hard-coded
text and let the editor restructure later.

### Why no `{{#invoke:Module:Foo|...}}`?

Scribunto (the Lua module engine) is an extension. On wikis without it, the
call renders as raw text. Use plain wikitext + native tables + native lists
instead.

---

## 12. Core string and encoding functions (no extension needed)

```wiki
{{lc: AbC}}                   → "abc"       (lowercase)
{{uc: AbC}}                   → "ABC"       (uppercase)
{{lcfirst: AbC}}              → "abC"       (lowercase first char)
{{ucfirst: abc}}              → "Abc"       (uppercase first char)
{{anchorencode: AbC dEf}}     → "AbC_dEf"   (URL anchor)
{{urlencode: hello world}}    → "hello+world" (URL encoding)
```

> **`{{#if:}}`, `{{#ifeq:}}`, `{{#ifexist:}}`, `{{#ifexpr:}}`, `{{#expr:}}`,
> `{{#time:}}`, `{{#switch:}}`, `{{#len:}}`, `{{#pos:}}`, `{{#sub:}}`,
> `{{#replace:}}` etc.** are provided by the **ParserFunctions** extension.
> They are present on most public wikis, but NOT in stock MediaWiki core. If
> you need conditional rendering, prefer plain wikitext with categories, or
> pre-compute the value outside the article.

---

## 13. Variables and behavior switches

### Date/time variables

```wiki
{{CURRENTYEAR}}                             → 2026
{{CURRENTMONTH}}                            → 06
{{CURRENTMONTHNAME}}                        → June
{{CURRENTMONTHABBREV}}                      → Jun
{{CURRENTDAY}}                              → 16
{{CURRENTDAY2}}                             → 16
{{CURRENTDOW}}                              → 2 (Tuesday)
{{CURRENTDAYNAME}}                          → Tuesday
{{CURRENTTIME}}                             → 13:24
{{CURRENTHOUR}}                             → 13
{{CURRENTWEEK}}                             → 24
{{CURRENTTIMESTAMP}}                        → 20260616132456
```

### Statistics

```wiki
{{NUMBEROFARTICLES}}                        → 1,234,567
{{NUMBEROFPAGES}}                           → 5,678,901
{{NUMBEROFUSERS}}                           → 12,345
{{NUMBEROFEDITS}}                           → 99,999,999
{{NUMBEROFVIEWS}}                           → 999,999,999
{{NUMBEROFFILES}}                           → 10,000
{{SITENAME}}                                → The Wiki Name
{{SERVER}}                                  → //hu.wikipedia.org
{{SERVERNAME}}                              → hu.wikipedia.org
{{CURRENTVERSION}}                          → MediaWiki version
```

### Page and namespace variables

```wiki
{{PAGENAME}}                                → Article name (without namespace)
{{PAGENAMEE}}                               → URL-encoded PAGENAME
{{FULLPAGENAME}}                            → Namespace:Page name
{{NAMESPACE}}                               → Namespace name
{{SUBJECTPAGENAME}}                         → Subject-namespace page name
{{TALKPAGENAME}}                            → Talk page name
{{ROOTPAGENAME}}                            → Root page name (for /Subpage)
{{BASEPAGENAME}}                            → Base page name
{{SUBPAGENAME}}                             → Subpage name
{{REVISIONID}}                              → 12345678
{{REVISIONUSER}}                            → Editor
{{REVISIONTIMESTAMP}}                       → 2025-01-15T10:30:00Z
{{PROTECTIONLEVEL}}                         → e.g. "autoconfirmed"
{{EDITSECTION}}                             → for "edit" links
```

### Behavior switches (at the start of a line or anywhere)

```wiki
__NOTOC__           → hide the table of contents
__TOC__             → force the table of contents (at the top of the page)
__FORCETOC__        → force the table of contents (can appear anywhere)
__NOEDITSECTION__   → disable section editing
__NEWSECTIONLINK__  → "+" button to add a section
__NONEWSECTIONLINK__→ hide the "+" button
__NOGALLERY__       → disable automatic image gallery (for categories)
__HIDDENCAT__       → hide a category from category pages
```

### Special characters

```wiki
&amp;    → &     (ampersand)
&lt;     → <     (less than)
&gt;     → >     (greater than)
&nbsp;   → (non-breaking space)
&ndash;  → –     (en dash)
&mdash;  → —     (em dash)
&hellip; → …     (horizontal ellipsis)
&laquo;  → «     (left French quotation mark)
&raquo;  → »     (right French quotation mark)
&copy;   → ©
&reg;    → ®
&trade;  → ™
```

HTML entity: `&#NNN;` (decimal) or `&#xHH;` (hexadecimal).

---

## 14. Signatures and pings

```wiki
~~~      → username
~~~~     → username + timestamp
~~~~~    → timestamp only
{{u|User|text}}       → ping (link to the user)
{{ping|User1|User2}}  → ping multiple users
```

---

## 15. Categories and hidden comments

### Categories (at the bottom of the page)

```wiki
[[Category:Formatting]]
[[Category:2025 events]]
[[Category:Software development|sorted]]
[[:Category:Hidden]]                  → link, does not categorize the page
[[Category:Formatting| ]]             → space = the category name
```

### Hidden category

```wiki
[[Category:Hidden category|__HIDDENCAT__]]
```

or

```wiki
__HIDDENCAT__
[[Category:Hidden category]]
```

### Comments (do not appear in the output)

```wiki
<!-- This is a comment, it does not appear on the rendered page -->
```

Multi-line comments are also possible:
```wiki
<!--
Multiple
lines
work
-->
```

---

## 16. Redirects, renames

```wiki
#REDIRECT [[Target page]]
#REDIRECT [[Target page#Section]]
#REDIRECT [[Category:Category name]]    → category redirect
#REDIRECT [[File:File.jpg]]             → file redirect
```

### Page-rename magic word (delete + redirect)

```wiki
{{Db-redirect|comment=...}}
```

---

## 17. Uploading and embedding files

```wiki
[[File:Example.png|thumb|180px|right|Caption]]
[[File:Example.pdf|thumb|The PDF document]]
[[File:Example.svg|thumb|The SVG figure]]
[[File:Example.ogv|thumb|The OGG video]]
[[Media:Example.zip|Downloadable ZIP]]     → download link
```

### Media player

```wiki
[[File:Example.ogv|thumb|The video]]
[[File:Example.ogg|The audio]]
[[File:Example.webm|A WebM video]]
```

The media player is automatic if the extension is supported (ogg, ogv, webm, mp3, etc.).

---

## 18. Automatic listing of categories, pages, and images

> **Hard rule:** do NOT use `{{CategoryTree|...}}`, `{{#categorytree:...}}`,
> `{{Pages with prefix|...}}` — these need the **CategoryTree** extension.
> The core-only equivalents are listed below.

### Native core: gallery of files

```wiki
<gallery caption="File gallery" widths="200" heights="150">
File:Image1.jpg|Description 1
File:Image2.jpg|Description 2
</gallery>
```

### Native core: list files in a category via Special page link

The core native option is a **plain link** to the category's file listing:

```wiki
A kategória képeit lásd: [[:Category:Files]].
```

If the **CategoryTree** extension is available, you may also write
`{{CategoryTree|Files|depth=2}}` — but always provide the native link as the
fallback.

### Counting pages in a category (native, simple)

Use the core magic word:

```wiki
{{PAGESINCAT:Files}}
```

(`{{PAGESINCATEGORY:Files}}` is an alias.)

---

## 19. Recursive templates and content inclusion

```wiki
{{:Another page}}              → transclude another page's content (native core)
{{Another page}}               → include a template (NEEDS the template to exist)
{{subst:Another page}}         → substitute the template's text on save
{{safesubst:Another page}}     → same, but safer
{{msgnw:Another page}}         → include without template, as raw wiki text
```

> **Caveat:** `{{Another page}}` only renders if `Template:Another page`
> exists on the target wiki. For portable articles, prefer plain transclusion
> (`{{:Another page}}`) of real pages, or copy the relevant content into the
> article body.

### Transclusion (embed a page's content)

```wiki
{{:Help:Contents}}             → copies the full content of Help:Contents here
{{:Help:Contents|Section}}      → copies only a section
```

---

## 20. Vertical alignment, horizontal lines, layout

### Horizontal line

```wiki
----
```

(Can be placed on any line, but at the start of a line is safest.)

### Floating box

```wiki
<div style="float:right; width:300px; margin-left:10px; padding:10px; background:#f9f9f9; border:1px solid #ddd;">
This box floats to the right.
</div>
```

### Multiple columns (native, with a wikitable)

The portable equivalent of `column-count: 2` is a simple wikitable — every
MediaWiki install renders `class="wikitable"`, and the columns reflow
naturally on narrow screens.

```wiki
{| class="wikitable"
! style="width:50%;" | Bal oszlop
! style="width:50%;" | Jobb oszlop
|-
|
Első oszlop tartalma.
Több bekezdés is lehet.

* Listaelem 1
* Listaelem 2
|
Második oszlop tartalma.

* Listaelem A
* Listaelem B
|}
```

### Three or more columns (native, wikitable)

```wiki
{| class="wikitable"
! style="width:33%;" | Első
! style="width:33%;" | Második
! style="width:33%;" | Harmadik
|-
| A | B | C
|}
```

### "Column-count CSS" is not portable

`column-count: 2` and similar multi-column CSS work on most skins, but the
`column-count` property is not guaranteed to be applied to `<div>` content in
every MediaWiki configuration. Use the wikitable above instead.
First column content.
Second column content.
</div>
```

or with the `{{div col}}` / `{{div col end}}` template (extension; prefer
the wikitable above for portability):
```wiki
{{div col|2}}
First column
{{div col end}}
```

---

## 21. Extensions to AVOID in portable articles

> This skill is **native-only**. The extensions below are useful in their
> own right, but you must NOT add them to the article unless the user
> explicitly confirms they are installed on the target wiki (and even then,
> keep a native fallback in the same article).

| Extension | Why we avoid it by default | Native replacement |
|-----------|---------------------------|--------------------|
| `SyntaxHighlight` (`<syntaxhighlight>`) | not core | `<pre>` |
| `TemplateStyles` (`<templatestyles>`) | not core | inline `<div style="...">` |
| `ParserFunctions` (`{{#if}}`, `{{#switch}}`, `{{#expr}}`, `{{#time}}`) | not core | native magic words + plain wikitext |
| `Scribunto` (`{{#invoke:Module:...}}`) | not core | wikitable / list / table |
| `Cite` (`{{cite web}}`, `{{cite book}}`, etc.) | not core | `<ref>[url "title"]</ref>` |
| `Highlight` (`<source>`) | not core | `<pre>` |
| `Mermaid` (`<mermaid>`) | not core | wikitable step list / ASCII art in `<pre>` |
| `Graphviz` (`<graphviz>`) | not core | wikitable step list / ASCII art in `<pre>` |
| `Graph` (`{{Graph:Chart}}`) | not core | `class="wikitable sortable"` |
| `TabberNeue` (`<tabber>`) | not core | multi-column wikitable |
| `Variables` (`{{#var:}}`) | not core | plain wikitext |
| `CategoryTree` (`{{CategoryTree}}`) | not core | `[[:Category:Name]]` link |
| `Page Forms` (`{{#forminput:...}}`) | not core | regular edit form |
| `Semantic MediaWiki` (`[[Prop::Value]]`, `{{#ask:...}}`) | not core | plain wikitext |
| `Cargo` (`{{#cargo_store:...}}`, `{{#cargo_query:...}}`) | not core | wikitable |
| `InputBox` (`<inputbox>`) | not core | regular form link |
| `CollapsibleVector` / `Header Tabs` | not core | `class="mw-collapsible"` / `<details>` |
| `Math` (`<math>`) | widely available but still an extension | plain wikitext describing the formula |
| `Poem` (`<poem>`) | not core | plain `<br />` separation |
| `Arrays`, `Maps` | not core | wikitable / external link |

---

## 22. Two important, often-forgotten things

### Proper escaping

```wiki
{{!}}       → |       (pipe escape, in tables)
{{=}}       → =       (equals sign escape, in headers)
{{!!}}      → ||      (double pipe)
{{!}}-      → |-      (row break)
```

Example: in a table, if a cell's text contains a `|` character:
```wiki
{| class="wikitable"
| A or B {{!}} C
|}
```

### `<templatedata>` (core, but rarely needed for portable articles)

```wiki
<templatedata>
{
  "description": "Template description",
  "params": {
    "name": { "type": "string", "label": "Name" }
  }
}
</templatedata>
```

`<templatedata>` is a core feature for documenting template parameters and
shows up in VisualEditor. It is NOT a styling tool — it has no rendering
effect on the page itself.

---

## 23. Example: complete, native-only article

```wiki
__NOTOC__

<div style="font-size:0.9em;color:#54595d;margin-bottom:12px;">
This article provides a brief overview of the topic.
</div>

== Overview ==

This article provides a brief overview of the [[Main topic|Main topic]]. The topic is important
and is discussed in detail in the following sections.

== Core concepts ==

; First concept
: An explanation of the first concept that helps understand the topic.

; Second concept
: An explanation of the second concept.

== Example table ==

{| class="wikitable sortable mw-collapsible"
! Column 1 !! Column 2 !! Column 3
|-
| data A1 || data B1 || data C1
|-
| data A2 || data B2 || data C2
|}

== Example code ==

<pre>
def hello(name: str) -> str:
    return f"Hello, {name}!"

print(hello("World"))
</pre>

== Example gallery ==

<gallery mode="packed-hover" heights="180px" caption="Example gallery" class="center">
File:Example.jpg|Image 1
File:Example.jpg|Image 2
File:Example.jpg|Image 3
</gallery>

== References ==

<references />

[[Category:Examples]][[Category:Documentation]]
```

---

## 24. Best practices (TL;DR)

1. **Use `<pre>` for code blocks** (native, no extension). Only switch to
   `<syntaxhighlight>` if the user confirms the `SyntaxHighlight` extension
   is available — and even then, keep `<pre>` as a fallback.
2. **Always use `class="wikitable"` for tables**, unless there is a specific design reason.
3. **Use sortable tables when the data is sortable** (`class="wikitable sortable"`).
4. **`mode="packed-hover"` is the most attractive gallery** in most cases.
5. **Escape the pipe character with `{{!}}`** in tables.
6. **Never use a level `=` heading** (level 2 is the minimum: `==`).
7. **Place categories at the bottom of the page**, one per line or on a single line.
8. **When linking, prefer the `[[Target|Label]]` format** if the target page's name is long.
9. **The `__TOC__` magic word** is rarely needed (by default it appears after the
   level-4 heading), but if you need it, place it at the top of the page.
10. **Do NOT use `<templatestyles>` at all** — use inline `<div style="...">`
    instead. Inline CSS works on every MediaWiki install with no extension.
11. **Use `<div style="...">` for callout boxes** (Note, Tip, Warning, Caution,
    Important). The exact color recipes are in
    `references/00-native-only-mapping.md` section 3.1.
12. **Use a wikitable floated right for infoboxes**; a plain wikitable for sidebars.
13. **Use `<blockquote>` for quotes**; do NOT use `{{blockquote|...}}` or
    `{{pull quote|...}}`.
14. **Use `<ref>...</ref>` + `<references />` for citations**; do NOT use
    `{{cite web|...}}` and family.

---

Sources:
- <https://www.mediawiki.org/wiki/Cheatsheet>
- <https://www.mediawiki.org/wiki/Documentation/Style_guide>
- <https://www.mediawiki.org/wiki/Help:Formatting>
- <https://www.mediawiki.org/wiki/Help:Tables>
- <https://en.wikipedia.org/wiki/Help:Cheatsheet>
- <https://en.wikipedia.org/wiki/Help:Advanced_table_features>
- <https://en.wikipedia.org/wiki/Help:Gallery_tag>
- <https://www.mediawiki.org/wiki/Help:Magic_words>
- `references/00-native-only-mapping.md` (in this skill)
