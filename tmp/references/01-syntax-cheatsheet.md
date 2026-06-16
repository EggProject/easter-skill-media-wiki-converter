# 01 — MediaWiki syntax cheatsheet (complete, with examples)

> Sources: <https://www.mediawiki.org/wiki/Cheatsheet>,
> <https://en.wikipedia.org/wiki/Help:Cheatsheet>,
> <https://www.mediawiki.org/wiki/Help:Formatting>,
> <https://www.mediawiki.org/wiki/Help:Tables>,
> <https://en.wikipedia.org/wiki/Help:Gallery_tag>,
> <https://www.mediawiki.org/wiki/Help:Extension:ParserFunctions>

This file is the complete MediaWiki syntax reference. If the user asks any formatting
question, or if any MediaWiki element must be used in the output, **work from here**.
Do not try to remember it from your own head — always be consistent with MediaWiki's
official syntax.

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

### Code block with syntax highlighting (recommended)

```wiki
<syntaxhighlight lang="python">
def hello(name: str) -> str:
    return f"Hello, {name}!"

print(hello("World"))
</syntaxhighlight>
```

Supported languages: `python`, `javascript`, `java`, `c`, `cpp`, `csharp`, `go`, `rust`,
`ruby`, `php`, `html`, `css`, `scss`, `xml`, `json`, `yaml`, `sql`, `bash`, `sh`, `powershell`,
`markdown`, `diff`, `dockerfile`, `nginx`, `apache`, `ini`, `toml`, `lua`, `perl`, `r`,
`matlab`, `scala`, `kotlin`, `swift`, `typescript`, `jsx`, `tsx`, `vue`, `graphql` and
hundreds more (see: Pygments lexers).

### Inline syntax highlighting

```wiki
Use the <syntaxhighlight lang="css" inline>color: red</syntaxhighlight> rule.
```

### Simple code block (no highlighting)

```wiki
<pre>
this
  preserves
    the indentation
</pre>
```

### Keyboard shortcut

```wiki
To save, press {{Key press|Ctrl|S}}.
```

### Button

```wiki
Click the {{Button|Save}} button.
```

### Abbreviation

```wiki
<abbr title="JavaScript">JS</abbr> is a popular language.
```

### Code sample window (with external filename)

```wiki
{{Codesample
|lang=python
|code=print("Hello, World!")
|title=hello.py
}}
```

---

## 9. Quotes

### Inline quote

```wiki
He said: "I'll be back soon".
```

### Block-level quote (its own block)

```wiki
<blockquote>
This is a longer quote that can span multiple lines.
In MediaWiki the <blockquote> tag can be used for this.
</blockquote>
```

### Templated quote

```wiki
{{blockquote
|text=Life is what happens when you're busy making other plans.
|sign=John Lennon
|source=
|width=
}}
```

### Other quote templates

```wiki
{{pull quote|text}}
{{Quotation|text|source}}
```

---

## 10. Highlights, boxes, messages

### MediaWiki's built-in highlight templates

```wiki
{{Note|text}}
{{Tip|text}}
{{Warning|text}}
{{Caution|text}}
{{Important|text}}
{{NoteTip|text}}
```

### Generic message-box (MediaWiki core)

```wiki
{{ambox
|type=notice
|text=Informational message
|image=[[File:Icon-info.svg|40px]]
}}
```

Types: `notice`, `style`, `content`, `warning`, `serious`. The appearance depends on the `type` value.

### Page-level messages (top of the page)

```wiki
{{Notice|this page is under maintenance}}
{{Outdated|2024-01-01}}
{{Historical}}
{{Fixme}}
{{Todo|finish the table}}
{{Update}}
```

### Hatnote (link to another page within the page)

```wiki
{{Main|Main article}}
{{See also|See also|Related article}}
{{For|other uses|See|Other meaning}}
```

### Highlight with TemplateStyles (custom box)

```wiki
<div class="my-highlight-box">
This is a custom-styled box whose appearance is defined by TemplateStyles.
</div>
```

To use TemplateStyles, the page (or the containing template) must reference it:
```wiki
<templatestyles src="Module:Highlight box/styles.css" />
```

---

## 11. Templates

### Calling a template

```wiki
{{template name}}
{{template name|parameter=value}}
{{template name|first|2|third}}
{{template name|param1=value1|param2=value2}}
```

### Conditional include

```wiki
{{template name|{{{parameter|default}}}}}
```

### Recursion / circular reference

```wiki
{{#invoke:Module:Foo|function_name|arg1|arg2}}
```

### Most common useful templates

- `{{PAGENAME}}` — the current page name
- `{{NAMESPACE}}` — the current namespace
- `{{FULLPAGENAME}}` — namespace + page name
- `{{TALKPAGENAME}}` — the talk page name
- `{{REVISIONID}}` — the current revision ID
- `{{REVISIONUSER}}` — the editor's username
- `{{SITENAME}}` — the wiki name
- `{{SERVER}}` — the wiki's server URL
- `{{localurl:page name}}` — the page's relative URL
- `{{fullurl:page name}}` — the page's full URL
- `{{CURRENTYEAR}}`, `{{CURRENTMONTH}}`, `{{CURRENTDAY}}`, `{{CURRENTTIME}}` — date
- `{{NUMBEROFARTICLES}}`, `{{NUMBEROFPAGES}}`, `{{NUMBEROFUSERS}}` — statistics
- `{{ns:1}}` — namespace ID → namespace name
- `{{#time: Y-m-d }}` — formatted date (see below)

---

## 12. Parser functions (ParserFunctions extension)

```wiki
{{#if: string | yes | no}}
{{#ifeq: 01 | 1 | equal | not equal}}      → "equal" (numeric comparison)
{{#ifexist: Help:Foo | exists | no}}       → "exists" if the page exists
{{#ifexpr: 1 > 0 | yes | no}}               → "yes"
{{#expr: 1 + 1 }}                           → 2
{{#time: Y-m-d }}                           → 2026-06-16
{{#time: c }}                               → ISO 8601
{{#switch: baz | foo = Foo | baz = Baz }}   → "Baz"
{{#rel2abs: ../quok | Help:Foo/bar/baz }}   → Help:Foo/bar/quok
{{#titleparts: Talk:Foo/bar/baz | 2 }}      → Talk:Foo/bar
```

String functions (optional, only if `$wgPFEnableStringFunctions = true`):
```wiki
{{#len: string }}
{{#pos: string | search }}
{{#rpos: string | search }}
{{#sub: string | start | length }}
{{#count: string | search }}
{{#replace: string | search | replacement }}
{{#explode: string | delim | index }}
{{#urldecode: encoded }}
{{#urlencode: text }}
```

Core functions (available without an extension):
```wiki
{{lc: AbC}}                                 → "abc"
{{uc: AbC}}                                 → "ABC"
{{lcfirst: AbC}}                            → "abC"
{{ucfirst: abc}}                            → "Abc"
{{anchorencode: AbC dEf}}                   → "AbC_dEf"
{{urlencode: hello world}}                  → "hello+world"
```

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

```wiki
{{CategoryTree|Category name|depth=3}}     → category tree
{{Pages with prefix|Page prefix}}            → list of pages by prefix
{{PAGESINCAT:Category name}}                 → number of pages in the category
{{PAGESINCATEGORY:Category name}}            → same as above
{{FORMATNUM:1234567}}                          → formatted number
```

### File listing in a category

```wiki
<gallery>
File:Image1.jpg
File:Image2.jpg
</gallery>
```

or all files in a category (with certain extensions):
```wiki
{{#categorytree:Files|showcount|depth=2}}
```

---

## 19. Recursive templates and content inclusion

```wiki
{{:Another page}}              → transclude another page's content
{{Another page}}               → include a template
{{subst:Another page}}         → substitute the template's text on save
{{safesubst:Another page}}     → same, but safer
{{msgnw:Another page}}         → include without template, as raw wiki text
```

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

### Multiple columns (with extension or CSS)

```wiki
<div style="column-count: 2; column-gap: 20px;">
First column content.
Second column content.
</div>
```

or with the `{{div col}}` / `{{div col end}}` template (where available):
```wiki
{{div col|2}}
First column
{{div col end}}
```

---

## 21. Extensions worth knowing (only if installed)

| Extension | Function |
|--------------|---------|
| SyntaxHighlight | Code highlighting (Pygments) |
| TemplateStyles | Custom CSS per page |
| ParserFunctions | `{{#if}}`, `{{#switch}}`, `{{#expr}}`, etc. |
| Scribunto | Lua modules (Module: namespace) |
| Variables | `{{#var:}}` local variables |
| Cargo | Structured data storage |
| VisualEditor | WYSIWYG editor |
| TabberNeue | Tabbed interface |
| MultimediaViewer | Image zoom |
| PageImages | Page's lead image |
| InputBox | Quick page-creation box |
| Replace Text | Replace text in a category |
| CategoryTree | Category tree |
| CollapsibleVector | Collapsible sections |
| Header Tabs | Header tabs |
| Page Forms | Form-based editing |
| Semantic MediaWiki | Semantic data |
| Math | LaTeX formulas (`<math>`) |
| Poem | Poetry formatting |
| Arrays | Array handling |
| Maps | Map embed |

**Note:** Always verify whether the given extension is installed on the target wiki.
If not, use a native MediaWiki alternative, or notify the user.

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

### Deactivating HTML5

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

---

## 23. Example: complete, modern article

```wiki
__NOTOC__{{short description|Modern, clean MediaWiki article example}}
{{#time: Y | now }}.

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

<syntaxhighlight lang="python">
def hello(name: str) -> str:
    return f"Hello, {name}!"

print(hello("World"))
</syntaxhighlight>

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

1. **Always use `<syntaxhighlight>` for code blocks**, not `<pre>`. Syntax highlighting
   greatly improves readability.
2. **Always use `class="wikitable"` for tables**, unless there is a specific design reason.
3. **Use sortable tables when the data is sortable** (`class="wikitable sortable"`).
4. **`mode="packed-hover"` is the most attractive gallery** in most cases.
5. **Escape the pipe character with `{{!}}`** in tables.
6. **Never use a level `=` heading** (level 2 is the minimum: `==`).
7. **Place categories at the bottom of the page**, one per line or on a single line.
8. **When linking, prefer the `[[Target|Label]]` format** if the target page's name is long.
9. **The `__TOC__` magic word** is rarely needed (by default it appears after the
   level-4 heading), but if you need it, place it at the top of the page.
10. **You rarely need to type the `<templatestyles>` tag directly** — templates
    usually include it, and the editor loads it automatically.

---

Sources:
- <https://www.mediawiki.org/wiki/Cheatsheet>
- <https://www.mediawiki.org/wiki/Documentation/Style_guide>
- <https://www.mediawiki.org/wiki/Help:Formatting>
- <https://www.mediawiki.org/wiki/Help:Tables>
- <https://en.wikipedia.org/wiki/Help:Cheatsheet>
- <https://en.wikipedia.org/wiki/Help:Advanced_table_features>
- <https://en.wikipedia.org/wiki/Help:Gallery_tag>
- <https://www.mediawiki.org/wiki/Help:Extension:ParserFunctions>
- <https://www.mediawiki.org/wiki/Help:Magic_words>
