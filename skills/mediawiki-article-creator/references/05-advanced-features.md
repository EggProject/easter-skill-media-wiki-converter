# 05 — Advanced MediaWiki features

> Sources: <https://www.mediawiki.org/wiki/Help:Magic_words>,
> <https://www.mediawiki.org/wiki/Help:Extension:ParserFunctions>,
> <https://www.mediawiki.org/wiki/Extension:TemplateStyles>,
> <https://www.mediawiki.org/wiki/Extension:Scribunto>,
> <https://www.mediawiki.org/wiki/Manual:Interface/Sidebar>

This file collects advanced MediaWiki features. Use it only when the target wiki
contains the given extension.

---

## 1. TemplateStyles (custom CSS on pages)

TemplateStyles lets users store CSS on wiki pages (typically in the `Module:`
namespace) and reference it embedded in the content.

### Usage

```wiki
<templatestyles src="Modul:Highlight/styles.css" />

<div class="hl-primary">
Ez egyedi stílusú doboz.
</div>
```

### The CSS page (Modul:Highlight/styles.css)

```css
.hl-primary {
  background: #eaf3ff;
  border-left: 4px solid #36c;
  padding: 12px 16px;
  border-radius: 4px;
  margin: 12px 0;
}
```

### Security

TemplateStyles allows **restricted CSS** — no `position: fixed`, no `@import`,
no JavaScript. The full security list is in the extension's documentation.

### When to use it

- When the wiki's design elements need to be consistent (e.g. every highlight
  block looks the same).
- When the user requests a custom design that built-in templates cannot
  provide.

---

## 2. Scribunto (Lua modules)

Scribunto lets templates run Lua code. This is the most powerful customization
tool in MediaWiki.

### Basic usage

```wiki
{{#invoke:Module:Hello|sayHi|name=Anna}}
```

The contents of `Module:Hello`:

```lua
local p = {}

function p.sayHi(frame)
    local name = frame.args.name or "World"
    return "Hello, " .. name .. "!"
end

return p
```

### When to use it

- When the wiki contains complex logic (string handling, math, date
  formatting).
- When the user asks for a "Lua module".

### Note

Scribunto must be installed by the MediaWiki admin. It is available on most
wikis, but not on some private ones.

---

## 3. ParserFunctions (extension)

The ParserFunctions extension is the most common extension and is installed on
nearly every wiki.

### Most common functions

```wiki
{{#if: string | yes | no}}
{{#ifeq: 01 | 1 | equal | not equal}}
{{#ifexists: Lap neve | exists | no }}
{{#ifexpr: 1 > 0 | yes | no }}
{{#expr: 1 + 1 }}
{{#time: Y-m-d }}
{{#switch: kulcs | eset1 = Eredmény1 | eset2 = Eredmény2 | alapértelmezett }}
{{#rel2abs: ../path | Alap útvonal }}
{{#titleparts: Talk:Foo/bar/baz | 2 }}
```

### String functions (optional)

```wiki
{{#len: string }}
{{#pos: string | keresett }}
{{#sub: string | start | length }}
{{#replace: string | keresett | csere }}
{{#explode: string | delim | index }}
{{#urldecode: encoded }}
{{#urlencode: text }}
```

### Core functions (available without the extension)

```wiki
{{lc: AbC}}                                 → "abc"
{{uc: AbC}}                                 → "ABC"
{{lcfirst: AbC}}                            → "abC"
{{ucfirst: abc}}                            → "Abc"
{{anchorencode: AbC dEf}}                   → "AbC_dEf"
{{urlencode: hello world}}                  → "hello+world"
```

### Example: conditional content

```wiki
{{#ifeq: {{NAMESPACE}} | Súgó
| Ez a szöveg csak a Súgó névtérben jelenik meg.
| Ez a szöveg mindenhol máshol jelenik meg.
}}
```

---

## 4. Variables (local variables)

The Variables extension (optional) lets you create local variables within a
page.

```wiki
{{#vardefine: szin | kék}}
{{#var: szin}}                                → "kék"
{{#ifexpr: {{#var: szin}} = kék | ... | ... }}
```

### Note

The Variables extension must be installed by the MediaWiki admin. If it is not
available, use Scribunto or the page's parameters.

---

## 5. Page Forms (form-based editing)

The Page Forms extension (formerly Semantic Forms) lets you edit pages through
forms.

### Example

```wiki
{{#forminput:form=Személy űrlap}}
{{Special:FormEdit/Személy űrlap/Anna}}
```

### When to use it

- When the wiki contains structured data (people, projects, etc.).
- When the user asks for editing "via a form".

---

## 6. Semantic MediaWiki (semantic data)

The Semantic MediaWiki (SMW) extension lets you store semantic data on pages
and run queries against it.

### Basic usage

```wiki
[[Főváros::Budapest]]
[[Népesség::1750000]]
[[Alapítás éve::1873]]
```

### Query

```wiki
{{#ask:
[[Főváros::+]]
[[Népesség::>1000000]]
| ?Népesség
| ?Főváros
| format=table
}}
```

### When to use it

- When the wiki contains structured data and the user wants database-like
  queries.

---

## 7. Cargo (structured data storage)

The Cargo extension is an alternative to SMW with a simpler syntax.

### Storing data in a template

```wiki
{{#cargo_store:
 _table=személyek
 |név=Anna Kovács
 |kor=28
 |város=Budapest
}}
```

### Query

```wiki
{{#cargo_query:
 tables=személyek
 |fields=név, kor, város
 |where=kor > 25
 |format=table
}}
```

---

## 8. Categories and category systems

### Basic category

```wiki
[[Category:Projektek]]
[[Category:2025-ös projektek]]
```

### Multi-level category tree

```wiki
[[Category:Projektek]]
[[Category:Aktív projektek]]
[[Category:Befejezett projektek]]
```

### Hidden category

```wiki
__HIDDENCAT__
[[Category:Admin]]
```

or

```wiki
[[Category:Admin|__HIDDENCAT__]]
```

### Displaying the category tree

```wiki
{{CategoryTree|Projektek|depth=3|hideroot=on}}
```

### Listing pages in a category

```wiki
{{PAGESINCAT:Projektek}}                     → only number
{{Special:CategoryViewer|Projektek}}         → list
```

or with the CategoryPager extension:

```wiki
{{#categorypager:Projektek}}
```

### Smart category sorting

Use the `sort key` parameter to customize the ordering within a category:

```wiki
[[Category:Személyek|Kovács, Anna]]
```

---

## 9. Interwiki links

Interwiki links point to other wikis (typically the Wikimedia projects).

```wiki
[[w:Hungary]]                  → Wikipedia (Hungarian or English)
[[w:en:Hungary]]               → English Wikipedia
[[w:de:Ungarn]]                → Deutsche Wikipedia
[[commons:Category:Hungary]]   → Wikimedia Commons
[[b:Hungarian]]                → Wikibooks
[[n:Hungary]]                  → Wikinews
[[q:Hungary]]                  → Wikiquote
[[s:Hungary]]                  → Wikisource
[[v:Hungary]]                  → Wikivoyage
[[species:Homo sapiens]]       → Wikispecies
[[m:Main Page]]                → Meta-Wiki
[[mw:Help:Contents]]           → MediaWiki.org
[[phab:T2001]]                 → Phabricator
[[gerrit:604435]]              → Gerrit (code review)
```

### Setting up a custom interwiki prefix

The wiki administrator can add new interwiki prefixes on the
`MediaWiki:Interwiki map` page.

---

## 10. VisualEditor (WYSIWYG editor)

VisualEditor is MediaWiki's WYSIWYG editor, available since 2013.

### Advantages

- No need to learn wikitext syntax
- Real-time preview
- Special tools: table editor, template inserter, etc.

### Disadvantages

- Can be slower than wikitext editing
- Complex templates sometimes render incorrectly
- `<syntaxhighlight>` and other HTML extensions have limited editability

### Recommendation

If the user uses VisualEditor, the wikitext should be "VE-friendly":
- Simple tables, not complex ones with many classes
- Templates should not be deeply nested
- `<syntaxhighlight>` is supported, but special parameters (`line`, `highlight`)
  are not editable from VE

---

## 11. Extensions that are often requested

| Extension | Function | Availability |
|--------------|---------|-------------|
| SyntaxHighlight | Code highlighting | Almost always |
| TemplateStyles | Custom CSS | Usually |
| ParserFunctions | Parser functions | Almost always |
| Scribunto | Lua modules | Usually |
| VisualEditor | WYSIWYG | Default (1.35+) |
| TabberNeue | Tab tabs | Must be installed |
| MultimediaViewer | Image zoom | Default |
| PageImages | Page's lead image | Default |
| InputBox | Quick page creation | Must be installed |
| CategoryTree | Category tree | Must be installed |
| CollapsibleVector | Collapsible | Default |
| Header Tabs | Header tabs | Must be installed |
| Page Forms | Form editing | Must be installed |
| Semantic MediaWiki | Semantic data | Must be installed |
| Math | LaTeX formulas | Usually |
| Mermaid | Diagrams | Must be installed |
| Graphviz | Diagrams | Must be installed |
| Poem | Poetry formatting | Default |
| Arrays | Array handling | Must be installed |
| Maps | Map embed | Must be installed |
| Cargo | Structured data | Must be installed |
| CodeMirror | Highlighted editor | Usually |
| WikiEditor | Advanced wikitext editor | Default |
| LiquidThreads | Next-gen talk page | Must be installed |
| Flow | Next-gen talk page | Replaced by LiquidThreads |
| Thanks | Give thanks | Must be installed |
| Echo | Notifications | Default (1.22+) |
| Flow (old) | Talk page refactor | No longer developed |

---

## 12. Writing custom extensions

If the user asks you to "write a custom extension", **DO NOT** write PHP code
using this skill. Instead:

1. Indicate that extension development is the responsibility of the MediaWiki
   administrator.
2. Suggest an alternative:
   - TemplateStyles (CSS)
   - Scribunto (Lua module)
   - ParserFunctions (template-level logic)
3. If a PHP extension is absolutely required, suggest that the user consult a
   MediaWiki developer.

---

## 13. MediaWiki's security model

If the user asks a security question:

### What does MediaWiki protect against?

- **XSS (cross-site scripting)**: user content is escaped; HTML only allows
  certain tags.
- **CSRF (cross-site request forgery)**: edits require a token.
- **Clickjacking**: MediaWiki sends a frame-busting header.
- **SQL injection**: MediaWiki uses an ORM and escapes input.

### What NOT to do in wikitext

- **Do NOT use `<script>` tags** — MediaWiki removes them.
- **Do NOT use `onclick`, `onerror` etc. attributes** — MediaWiki removes them.
- **Do NOT use `javascript:` URLs** — MediaWiki blocks them.
- **Do NOT try to embed iframes** from unknown origins.
- **Do NOT use `{{#css:...}}` or similar unescaped syntax** — it can lead to
  XSS.

### What is allowed

- CSS inside the `<templatestyles>` tag.
- Scribunto Lua modules.
- ParserFunctions.
- `<syntaxhighlight>` (only displays code, does not execute it).

---

## 14. Installation guide (briefly)

If the user asks to "install TemplateStyles" or similar:

> Installing extensions is the responsibility of the MediaWiki server
> administrator. The following steps are required:
>
> 1. Download the extension (Git or tarball).
> 2. Copy it into the `extensions/` directory.
> 3. Add the line `wfLoadExtension('Extension name');` to `LocalSettings.php`.
> 4. Run `php maintenance/update.php`.
> 5. Edit the `MediaWiki:Extension-name` pages (if any).
>
> The user does not do this with the help of the skill, but the MediaWiki
> admin does.

The skill's purpose is to produce **content** (wikitext), not server
configuration.

---

Sources:
- <https://www.mediawiki.org/wiki/Help:Magic_words>
- <https://www.mediawiki.org/wiki/Help:Extension:ParserFunctions>
- <https://www.mediawiki.org/wiki/Extension:TemplateStyles>
- <https://www.mediawiki.org/wiki/Extension:Scribunto>
- <https://www.mediawiki.org/wiki/Extension:Semantic_MediaWiki>
- <https://www.mediawiki.org/wiki/Extension:Cargo>
- <https://www.mediawiki.org/wiki/Extension:Page_Forms>
- <https://www.mediawiki.org/wiki/Manual:Interface/Sidebar>
- <https://www.mediawiki.org/wiki/Security>
