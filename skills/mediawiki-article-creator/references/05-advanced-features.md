# 05 — Advanced MediaWiki features (native core only)

> Sources: <https://www.mediawiki.org/wiki/Help:Magic_words>,
> <https://www.mediawiki.org/wiki/Help:Templates>,
> <https://www.mediawiki.org/wiki/Help:Links>,
> <https://www.mediawiki.org/wiki/Help:Categories>,
> <https://www.mediawiki.org/wiki/Manual:Interface/Sidebar>,
> <https://www.mediawiki.org/wiki/Help:VisualEditor/User_guide>

> **HARD RULE — native MediaWiki only.** This file describes ONLY the advanced
> features that ship with MediaWiki core. NO TemplateStyles, NO Scribunto, NO
> ParserFunctions, NO Variables, NO Page Forms, NO Semantic MediaWiki, NO
> Cargo, NO InputBox, NO Header Tabs, NO Maps. The full mapping is in
> `references/00-native-only-mapping.md`.

This file collects the native advanced features the skill uses. Most of what
the old version called "advanced" was in fact extension-based — those
sections are now either removed or replaced by the native equivalent.

---

## 1. Inline CSS on `<div>` and `<table>` (the only permitted styling)

MediaWiki core allows minimal inline CSS on `<div>` and `<table>` via the
`style="..."` attribute. This is the ONLY styling surface the skill uses.

### Allowed CSS properties (core, inline only)

| Property | Use case |
|----------|----------|
| `background`, `background-color` | box background |
| `color` | text color |
| `border`, `border-radius` | box border |
| `padding` | inner spacing |
| `margin` | outer spacing |
| `width`, `max-width`, `min-width` | sizing |
| `float` | sidebar / infobox positioning |
| `text-align` | cell alignment |
| `font-size`, `font-weight`, `font-family` | typography |
| `line-height` | vertical rhythm |
| `overflow-x` | horizontal scroll for long `<pre>` |

### Forbidden CSS features

- `position: fixed` / `position: sticky` — breaks on mobile
- `@keyframes`, `animation`, `transition` — no JS hookup
- `display: none` on important content — accessibility violation
- `transform`, `filter`, `box-shadow` (large) — keep it simple
- Any `@media` query or external CSS file — requires TemplateStyles
- CSS custom properties (`--var`) — inconsistent browser support

### Reference color palette (from `00-native-only-mapping.md` section 3.1)

| Token | Hex | Use |
|-------|-----|-----|
| Primary blue | `#36c` | links, primary callouts |
| Primary blue light bg | `#eaf3ff` | info box background |
| Primary blue border | `#36c` | info box border |
| Success green | `#14866d` | success box border |
| Success green bg | `#d5fdf4` | success box background |
| Warning yellow | `#fc3` | warning box background |
| Error red | `#d33` | error / caution box border |
| Error red bg | `#fee7e6` | error / caution box background |
| Gray (secondary) | `#54595d` | muted text |
| Gray bg | `#eaecf0` | quote / aside background |
| Gray border | `#c8ccd1` | quote / aside border |
| Code bg | `#f8f9fa` | code / terminal background |
| Code border | `#eaecf0` | code / terminal border |
| Purple | `#7c3aed` | info accent |

### Example

```wiki
<div style="background:#eaf3ff; border:1px solid #36c; border-left:4px solid #36c; border-radius:4px; padding:12px 16px; margin:12px 0;">
This is an info callout, styled with native inline CSS only.
</div>
```

See `references/00-native-only-mapping.md` section 3 for the full set of
copy-paste-ready recipes.

---

## 2. Core magic words (the only kind the skill uses)

> **Hard rule:** only the CORE magic words listed at
> <https://www.mediawiki.org/wiki/Help:Magic_words> are used. The
> ParserFunctions extension (`{{#if:}}`, `{{#switch:}}`, `{{#expr:}}`,
> `{{#time:}}` from ParserFunctions, etc.) is NEVER used. If you find
> yourself reaching for `{{#if:...}}`, rewrite the article in plain
> wikitext instead.

### Page metadata (core)

| Magic word | Output | Use case |
|------------|--------|----------|
| `{{PAGENAME}}` | current page title (no namespace) | breadcrumbs, page-title in templates |
| `{{FULLPAGENAME}}` | full title with namespace | canonical link |
| `{{BASEPAGENAME}}` | title without subpage | subpage navigation |
| `{{SUBPAGENAME}}` | subpage name | breadcrumb tail |
| `{{NAMESPACE}}` | current namespace | conditional help text |
| `{{NAMESPACENUMBER}}` | numeric namespace id | sorting |
| `{{ROOTPAGENAME}}` | root page title | subpage breadcrumbs |
| `{{TALKSPACE}}` | the talk namespace of the current page | cross-namespace link |
| `{{SUBJECTSPACE}}` / `{{ARTICLESPACE}}` | subject (article) namespace | cross-namespace link |
| `{{REVISIONID}}` | current revision id | citation |
| `{{REVISIONUSER}}` | user who made the current revision | article footer |
| `{{REVISIONTIMESTAMP}}` | ISO 8601 timestamp | article footer |
| `{{CURRENTYEAR}}` | current year (4 digits) | copyright notice |
| `{{CURRENTMONTH}}` | current month (01–12) | copyright notice |
| `{{CURRENTDAY}}` | current day (01–31) | copyright notice |
| `{{CURRENTTIME}}` | current time HH:MM | copyright notice |
| `{{CURRENTTIMESTAMP}}` | ISO 8601 timestamp | copyright notice |
| `{{CURRENTWEEK}}` / `{{CURRENTDOW}}` | current week / day-of-week | copyright notice |
| `{{SITENAME}}` | wiki's site name | sidebar |
| `{{SERVER}}` | wiki server URL | canonical link |
| `{{SCRIPTPATH}}` | script path | link to scripts |
| `{{DIRECTIONMARK}}` | bidirectional isolation mark | RTL languages |
| `{{CONTENTLANGUAGE}}` | content language | multilingual wikis |
| `{{NUMBEROFFILES}}`, `{{NUMBEROFEDITS}}`, etc. | site statistics | rarely used in articles |

### TOC control (core)

| Magic word | Effect |
|------------|--------|
| `__TOC__` | force the table of contents at this position |
| `__NOTOC__` | hide the table of contents |
| `__FORCETOC__` | force the table of contents (older alternative) |
| `__NOEDITSECTION__` | hide section edit links |
| `__NEWSECTIONLINK__` | add "+" button next to each heading |

### Category control (core)

| Magic word | Effect |
|------------|--------|
| `__HIDDENCAT__` | hide the category from the page footer (still functional) |
| `__NOGALLERY__` | switch the default gallery mode to traditional |

### Display control (core)

| Magic word | Effect |
|------------|--------|
| `__NOCC__` | suppress the table of contents |
| `__NOCONTENTCONVERT__` | disable language variant conversion |
| `__NOINDEX__` | tell search engines not to index this page |
| `__STATICREDIRECT__` | mark a redirect as not following a double-redirect |

### Switch parser functions (CORE — no extension)

MediaWiki core supports a small set of parser functions that look like
`{{...}}` but are actually built-in. They do NOT require the ParserFunctions
extension:

| Function | Effect |
|----------|--------|
| `{{int:...}}` | localized interface message |
| `{{msg:...}}` | alias of `{{int:...}}` |
| `{{ns:...}}` | namespace name by id or localized name |
| `{{nse:...}}` | localized namespace name with fallback |
| `{{urlencode:...}}` | URL-encode a string |
| `{{lc:...}}` / `{{lcfirst:...}}` | lowercase / lowercase first letter |
| `{{uc:...}}` / `{{ucfirst:...}}` | uppercase / uppercase first letter |
| `{{padleft:...}}` / `{{padright:...}}` | pad string to length |
| `{{plural:...}}` | plural form by count |
| `{{grammar:...}}` | inflected form |
| `{{gender:...}}` | gendered form by user preference |
| `{{#time:...}}` | date/time formatting (CORE — but rarely used; if you need it, see `Help:Magic_words`) |

> Note: `{{#time:...}}` is part of MediaWiki core (Help:Magic_words).
> It is NOT the same as ParserFunctions' `{{#time:}}`. Use only the core
> one. If the wiki has ParserFunctions installed, the two may coexist —
> prefer the core one.

### What is BANNED (extension-only)

These appear in `Help:Extension:ParserFunctions` and require the extension.
The skill NEVER produces them:

- `{{#if:condition|then|else}}` — rewrite in plain wikitext
- `{{#ifeq:...}}`, `{{#iferror:...}}`, `{{#ifexist:...}}` — same
- `{{#switch:value|case1=output1|case2=output2|default}}` — same
- `{{#expr:1+2}}` — calculate inline (just write the result)
- `{{#time:Y-m-d}}` (ParserFunctions flavor) — use the core `{{#time:}}` if
  a date is truly needed, or write the date statically in the wikitext.
- `{{#invoke:Module:...}}` (Scribunto) — Lua modules are banned. Rewrite in plain wikitext.
- `{{#tag:tabber|...}}` (Tag extension) — `tabber` is banned.
- `{{#ask:...}}` / `{{#show:...}}` (Semantic MediaWiki) — banned.

---

## 3. Categories and category systems (core)

### Basic category placement

```wiki
[[Category:Documentation]]
[[Category:Reference]]
[[Category:2026 releases]]
```

### Sorted category index

```wiki
[[Category:Software|A]]   <-- sorts under "A"
[[Category:Software]]     <-- default sort by page title
```

To force a default sort key for the entire category tree:

```wiki
{{DISPLAYTITLE:Article Title}}
{{DEFAULTSORT:Article Title}}
```

### Category hierarchy (core)

A category can be a subcategory of another category simply by including
it in the parent:

```wiki
<!-- On page "Category:Python libraries" -->
[[Category:Libraries]]
[[Category:Python]]
```

This makes `Category:Python libraries` appear under both
`Category:Libraries` and `Category:Python` in the category tree.

### Hidden category

```wiki
[[Category:Maintenance templates| ]]
__HIDDENCAT__
```

### Category tree visualization (native — no extension)

MediaWiki core renders the category tree automatically on `Category:Foo`
pages. The page lists subcategories, pages, and parent categories.

If you want a visual "tree" of categories on a non-category page:

```wiki
== See also ==

* [[:Category:Python]]
* [[:Category:Libraries]]
* [[:Category:Documentation]]

<gallery>
File:Cat.jpg|alt=Category icon|Categories
</gallery>
```

NEVER use `{{#categorytree:Foo}}` or `<categorytree>` (CategoryTree extension).

### Banned category-related extensions

- `CategoryTree` extension (`{{#categorytree:...}}`) — banned.
- `Page Forms` extension — banned.
- `Semantic MediaWiki` — banned.
- `Cargo` — banned.

---

## 4. Interwiki links (core)

```wiki
[[en:Wikipedia]]            <-- English Wikipedia
[[de:MediaWiki]]            <-- German Wikipedia
[[commons:File:Foo.png]]    <-- Wikimedia Commons
[[wikipedia:Wikitext]]      <-- alias of en:
[[w:Wikitext]]              <-- shortcut of en:
[[wikt:hello]]              <-- Wiktionary shortcut
[[n:News article]]          <-- Wikinews
[[b:Cookbook:Recipe]]       <-- Wikibooks
[[q:Quote]]                 <-- Wikiquote
[[s:Source text]]           <-- Wikisource
[[v:Video]]                 <-- Wikiversity
```

### Wikitext for cross-wiki link rendering

```wiki
[[wikitext]]     <-- local link (searches interwiki prefix table first)
[[:en:Wikipedia]] <-- always links to English Wikipedia
```

---

## 5. The `__TOC__` magic word and section formatting (core)

### Force the TOC at a specific location

```wiki
__TOC__

== Section 1 ==
content

== Section 2 ==
content
```

### Hide the TOC on pages with very few sections

```wiki
__NOTOC__
```

### Force TOC even when few sections

```wiki
__FORCETOC__
```

### Auto-number headings

```wiki
== Numbered 1 ==
content

== Numbered 2 ==
content
```

By default, MediaWiki auto-numbers the TOC. Disable per-section with:

```wiki
== Unnumbered heading ==
content
```

(If the wiki uses a skin that doesn't auto-number, this is a non-issue.)

### Hide "edit" links per section

```wiki
__NOEDITSECTION__
```

---

## 6. The `<gallery>` tag (core)

The `<gallery>` tag is built into MediaWiki core and renders a thumbnail
gallery of images.

### Basic gallery

```wiki
<gallery>
File:Photo1.jpg|alt=Photo 1 description
File:Photo2.jpg|alt=Photo 2 description
File:Photo3.jpg|alt=Photo 3 description
</gallery>
```

### Modes

| Mode | Description |
|------|-------------|
| `traditional` | thumbnails in a horizontal row |
| `nolines` | no border between thumbnails |
| `packed` | packed horizontal layout (Vector 2022+) |
| `packed-hover` | packed, with captions appearing on hover |
| `slideshow` | image slideshow |

```wiki
<gallery mode="packed-hover" widths="180" heights="120">
File:Photo1.jpg|alt=Photo 1
File:Photo2.jpg|alt=Photo 2
</gallery>
```

### Captions

```wiki
<gallery>
File:Photo1.jpg|This is photo 1.
File:Photo2.jpg|This is photo 2.
</gallery>
```

NEVER use `{{Gallery|...}}`, `{{Image gallery|...}}`, or `{{Slider|...}}`
— they are custom templates. Native `<gallery>` is the answer.

---

## 7. `<ref>` and `<references />` (core footnotes)

### Inline footnote

```wiki
This is a fact.<ref>[https://example.com Source], Author, 2026-01-15.</ref>
```

### Reused footnote

```wiki
According to Smith.<ref name="smith2020">Smith, John (2020). ''Trends''. Press.</ref>
And again.<ref name="smith2020" />
```

### Reference list

```wiki
<references />
```

### Multiple columns for the reference list

```wiki
<references responsive="0" columns="2" />
```

NEVER use `{{cite web|...}}`, `{{cite book|...}}`, `{{cite journal|...}}`,
`{{cite news|...}}` — Cite extension is banned.

---

## 8. `<details>` and `<summary>` (core, MediaWiki ≥ 1.40)

Native HTML5 tag, supported as-is by MediaWiki core from version 1.40.

```wiki
<details>
<summary>Click to expand</summary>
Hidden content here.
</details>
```

For MediaWiki < 1.40, use `class="mw-collapsible"` instead:

```wiki
<div class="mw-collapsible mw-collapsed">
Hidden content here.
</div>
```

---

## 9. `<templatedata>` (core, documentation only)

`<templatedata>` is a core MediaWiki tag that describes template parameters
in a structured way for the VisualEditor template wizard. It is **not** an
extension, **not** for styling — it is purely documentation.

```wiki
<templatedata>
{
  "description": "Infobox for software projects",
  "params": {
    "title": { "label": "Title", "type": "string", "required": true },
    "version": { "label": "Version", "type": "string" },
    "image": { "label": "Image", "type": "wiki-file-name" }
  }
}
</templatedata>
```

The skill produces `<templatedata>` ONLY when authoring templates (the
template's own page), never inside article content.

---

## 10. VisualEditor (core, WYSIWYG)

The VisualEditor is bundled with MediaWiki by default in most modern
installations. The skill produces wikitext, which is what VisualEditor
reads and writes. The two are fully compatible.

### Switching between wikitext and VisualEditor

The user can switch modes via the "Edit" tab dropdown in the wiki UI. The
skill always produces wikitext so that the user can:

1. Paste the wikitext into the wikitext editor
2. Or paste it into VisualEditor (which converts it back to VE's internal format)

### When the user prefers VisualEditor

The skill produces wikitext regardless. The user can paste it into either
editor — both render the same article.

---

## 11. Extensions the skill NEVER uses (the "do not request" list)

If the user asks for one of these, the answer is: "this is not supported
by this skill; native MediaWiki is enough." Each is replaced by a native
equivalent.

| Extension | Native replacement |
|-----------|--------------------|
| TemplateStyles | inline CSS on `<div>` / `<table>` |
| Scribunto (Lua) | plain wikitext |
| ParserFunctions | plain wikitext (or core `{{#time:}}` for dates) |
| Variables | plain wikitext (or transclusion via `{{PAGENAME}}` etc.) |
| SyntaxHighlight | native `<pre>` |
| Cite | native `<ref>` + manual citation strings |
| Mermaid | ASCII in `<pre>` or `<table>` box diagram |
| Graph / Graphviz | ASCII in `<pre>` or `<table>` box diagram |
| TabberNeue | multi-column `class="wikitable"` |
| InputBox | native search form wikitext |
| Header Tabs | native heading + `__TOC__` |
| Math | `<span class="texhtml">` + `<sup>` / `<sub>` |
| Poem | simple `<br />` lines |
| Arrays | plain wikitext + wikitable |
| Maps | native `<gallery>` + `[[:Category:Geography]]` links |
| Highlight | inline CSS for highlighting |
| CollapsibleVector | native `class="mw-collapsible"` |
| CategoryTree | native `<gallery>` + `[[:Category:...]]` |
| Page Forms | not supported |
| Semantic MediaWiki (SMW) | not supported |
| Cargo | not supported |

---

## 12. Writing custom extensions — OUT OF SCOPE

This skill does not produce MediaWiki extensions, custom skins, or
JavaScript gadgets. If the user wants such things, point them to the
MediaWiki developer documentation:

- <https://www.mediawiki.org/wiki/Manual:Developing_extensions>
- <https://www.mediawiki.org/wiki/Manual:Skinning>
- <https://doc.wikimedia.org/>

---

## 13. MediaWiki's security model — OUT OF SCOPE

The skill does not produce user-input forms, JavaScript gadgets, or
anything that would interact with the wiki's security model. Wikitext
is rendered server-side; JavaScript is disabled for anonymous users by
default. The native CSS palette we use is fully static and safe.

---

## 14. Installation guide — OUT OF SCOPE

The skill assumes the wiki is already installed and running. If the user
needs installation guidance, point them to:

- <https://www.mediawiki.org/wiki/Manual:Installation_guide>
- <https://www.mediawiki.org/wiki/Manual:System_administration>

---

Sources:
- <https://www.mediawiki.org/wiki/Help:Magic_words>
- <https://www.mediawiki.org/wiki/Help:Templates>
- <https://www.mediawiki.org/wiki/Help:Links>
- <https://www.mediawiki.org/wiki/Help:Categories>
- <https://www.mediawiki.org/wiki/Manual:Interface/Sidebar>
- <https://www.mediawiki.org/wiki/Help:VisualEditor/User_guide>
- <https://www.mediawiki.org/wiki/Help:Gallery>
- <https://www.mediawiki.org/wiki/Help:Footnotes>
