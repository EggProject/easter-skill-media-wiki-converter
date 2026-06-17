# 06 — Design recipes (ready-to-copy, native MediaWiki only)

> **HARD RULE — native MediaWiki only.** Every recipe in this file uses
> ONLY elements that ship with MediaWiki core. NO `{{Note}}`, NO
> `<syntaxhighlight>`, NO `<templatestyles>`, NO `<mermaid>`, NO
> `<tabber>`, NO `{{cite ...}}`, NO `{{Infobox}}`, NO `{{Sidebar}}`.
> The full mapping is in `references/00-native-only-mapping.md`.

This file contains ready-to-copy recipes for the most common MediaWiki
components. Each recipe includes:
- the goal (what we want to achieve),
- the code (copyable wikitext),
- the notes (palette tokens, customization tips).

---

## 1. Modern, clean table (default)

```wiki
{| class="wikitable sortable mw-collapsible"
! Név !! Kor !! Város
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

**Note:** The `sortable` class makes the table sortable, and `mw-collapsible`
makes it collapsible. Both are core MediaWiki classes — no extension required.
If the table is small (< 5 rows), omit `mw-collapsible`.

---

## 2. Striped (zebra) table

```wiki
{| class="wikitable sortable striped"
! Oszlop 1 !! Oszlop 2
|-
| adat 1 || adat 2
|-
| adat 3 || adat 4
|}
```

**Note:** The `striped` class has been available since MediaWiki 1.39 — it is
a core CSS class. For older MediaWiki, use a wikitable with explicit
`style="background:..."` per row.

---

## 3. Highlight boxes (native inline-CSS — replaces `{{Note|Tip|Warning|Caution}}`)

> **Hard rule:** `{{Note|...}}`, `{{Tip|...}}`, `{{Warning|...}}`,
> `{{Caution|...}}` are custom templates and may not exist on the target wiki.
> Use these native inline-CSS `<div>` boxes instead. They render on every
> MediaWiki install.

### Note (információs)

```wiki
<div style="background:#eaf3ff; border:1px solid #36c; border-left:4px solid #36c; border-radius:4px; padding:12px 16px; margin:12px 0;">
'''Note:''' Információs szöveg.
</div>
```

### Tip

```wiki
<div style="background:#d5fdf4; border:1px solid #14866d; border-left:4px solid #14866d; border-radius:4px; padding:12px 16px; margin:12px 0;">
'''Tip:''' Próbáld ki ezt a trükköt.
</div>
```

### Warning

```wiki
<div style="background:#fef6e7; border:1px solid #fc3; border-left:4px solid #fc3; border-radius:4px; padding:12px 16px; margin:12px 0;">
'''Warning:''' Legyél óvatos, ez adatvesztést okozhat.
</div>
```

### Caution (kritikus)

```wiki
<div style="background:#fee7e6; border:1px solid #d33; border-left:4px solid #d33; border-radius:4px; padding:12px 16px; margin:12px 0;">
'''Caution:''' Kritikus figyelmeztetés!
</div>
```

### Important

```wiki
<div style="background:#f3e8ff; border:1px solid #7c3aed; border-left:4px solid #7c3aed; border-radius:4px; padding:12px 16px; margin:12px 0;">
'''Important:''' Ezt mindenképpen olvasd el.
</div>
```

---

## 4. Page-level banners (replaces `{{ambox}}`, `{{Notice}}`, `{{Outdated}}`, etc.)

### Page under maintenance

```wiki
<div style="background:#fef6e7; border:1px solid #fc3; border-radius:4px; padding:8px 12px; margin:8px 0; text-align:center;">
'''Ez az oldal karbantartás alatt áll.''' A tartalom ideiglenesen pontatlan lehet.
</div>
```

### Outdated page

```wiki
<div style="background:#fef6e7; border:1px solid #fc3; border-radius:4px; padding:8px 12px; margin:8px 0; text-align:center;">
'''Elavult tartalom.''' Ez az oldal 2024-es információkat tartalmaz.
</div>
```

### Historical page

```wiki
<div style="background:#eaecf0; border:1px solid #c8ccd1; border-radius:4px; padding:8px 12px; margin:8px 0; text-align:center; color:#54595d;">
'''Történeti tartalom.''' Ez az oldal archivált, nem karbantartott.
</div>
```

### Needs fixing

```wiki
<div style="background:#fee7e6; border:1px solid #d33; border-radius:4px; padding:8px 12px; margin:8px 0; text-align:center;">
'''Javításra szorul.''' Ha hibát találsz, jelezd a vitalapon.
</div>
```

---

## 5. Generic message box (native — no `{{ambox}}`)

```wiki
<div style="background:#eaf3ff; border:1px solid #36c; border-left:4px solid #36c; border-radius:4px; padding:12px 16px; margin:12px 0;">
'''Egyszerű információs doboz.''' Bármilyen tartalommal feltölthető.
</div>
```

Variants (just change the colors):

| Style | Background | Border | Border-left |
|-------|------------|--------|-------------|
| info / notice | `#eaf3ff` | `#36c` | `#36c` |
| tip / success | `#d5fdf4` | `#14866d` | `#14866d` |
| warning | `#fef6e7` | `#fc3` | `#fc3` |
| caution / serious | `#fee7e6` | `#d33` | `#d33` |
| important | `#f3e8ff` | `#7c3aed` | `#7c3aed` |
| quote / aside | `#f8f9fa` | `#c8ccd1` | `#c8ccd1` |

---

## 6. Card grid (3 columns, native wikitable — no TemplateStyles)

```wiki
{| class="wikitable" style="width:100%;"
|+ style="text-align:left; font-weight:600;" | Három szempont
|-
| style="width:33%; vertical-align:top; padding:16px; background:#fff;" |
'''Ingyenes'''
Mindenki számára elérhető alapfunkciók.
| style="width:33%; vertical-align:top; padding:16px; background:#fff;" |
'''Gyors'''
5 perc alatt beüzemelhető.
| style="width:33%; vertical-align:top; padding:16px; background:#fff;" |
'''Biztonságos'''
SOC 2 tanúsítvánnyal.
|}
```

### Card with colored top border

```wiki
{| class="wikitable" style="width:100%;"
|-
| style="width:33%; vertical-align:top; padding:0;" |
<div style="background:#36c; color:#fff; padding:8px 16px; font-weight:600;">Ingyenes</div>
<div style="padding:12px 16px;">Mindenki számára elérhető alapfunkciók.</div>
| style="width:33%; vertical-align:top; padding:0;" |
<div style="background:#14866d; color:#fff; padding:8px 16px; font-weight:600;">Gyors</div>
<div style="padding:12px 16px;">5 perc alatt beüzemelhető.</div>
| style="width:33%; vertical-align:top; padding:0;" |
<div style="background:#7c3aed; color:#fff; padding:8px 16px; font-weight:600;">Biztonságos</div>
<div style="padding:12px 16px;">SOC 2 tanúsítvánnyal.</div>
|}
```

---

## 7. Accordion / collapsible sections

### Simple table-based (mw-collapsible, core)

```wiki
{| class="wikitable mw-collapsible mw-collapsed"
! Bővebben...
|-
Részletes tartalom, ami alapértelmezetten rejtve van.
|}
```

### Native HTML5 `<details>` / `<summary>` (MediaWiki ≥ 1.40)

```wiki
<details>
<summary>1. lépés: Előkészületek (kattints a kibontáshoz)</summary>
Első lépés részletei...
</details>

<details>
<summary>2. lépés: Konfiguráció</summary>
Második lépés részletei...
</details>

<details>
<summary>3. lépés: Indítás</summary>
Harmadik lépés részletei...
</details>
```

### With open styling (use a wikitable for a "card" look)

```wiki
{| class="wikitable" style="width:100%;"
|-
! style="cursor:pointer; background:#eaecf0;" | 1. lépés: Előkészületek
|-
| Első lépés részletei...
|}
```

**Note:** for true expand/collapse behavior, use `class="mw-collapsible"`
or native `<details>` — the wikitable alone won't collapse without one of
these.

---

## 8. Tabs (native multi-column wikitable — no `<tabber>`)

> **Hard rule:** do NOT use `<tabber>` (TabberNeue extension). Use a
> multi-column `class="wikitable"` instead. On wide screens the columns
> sit side-by-side; on narrow screens they flow vertically.

```wiki
{| class="wikitable"
! style="width:33%;" | Első tab
! style="width:33%;" | Második tab
! style="width:33%;" | Harmadik tab
|-
| valign="top" |
Ez az első tab tartalma.

* Lista elem 1
* Lista elem 2
| valign="top" |
{| class="wikitable"
! Fejléc !! Adat
|-
| A || 1
|}
| valign="top" |
Harmadik tab '''félkövér''' szöveggel.
|}
```

---

## 9. Breadcrumbs (native wikilink chain)

> **Hard rule:** do NOT use `{{#breadcrumb:Főoldal|Projekt|Aloldal}}` or
> `{{Breadcrumb|...}}` — those require extensions/custom templates.
> Use a plain wikilink chain in a small inline-CSS `<div>`.

### Plain (simplest)

```wiki
[[Főoldal]] > [[Projekt]] > Aloldal
```

### Styled (subtle gray, smaller font)

```wiki
<div style="font-size:0.9em; color:#54595d; margin-bottom:12px;">
[[Főoldal]] &rsaquo; [[Projekt]] &rsaquo; Aloldal
</div>
```

### Deep breadcrumb (4+ levels)

```wiki
<div style="font-size:0.9em; color:#54595d; margin-bottom:12px;">
[[Főoldal]] &rsaquo; [[Projekt]] &rsaquo; [[Alprojekt]] &rsaquo; [[Modul]] &rsaquo; Jelenlegi oldal
</div>
```

---

## 10. Infobox (native wikitable — no `{{Infobox}}`)

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
| '''Kiadás dátuma:''' || 2026-01-15
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
| [https://github.com/projekt/projekt/issues GitHub Issues]
|}
```

The exact same recipe works for any "key-value" infobox; just adjust the
labels and values.

---

## 11. Navigation box / Sidebar (native wikitable)

```wiki
{| class="wikitable" style="float:right; width:240px; margin:0 0 1em 1em;"
|+ '''Navigáció'''
|-
| '''Fő témák'''
* [[Első lap]]
* [[Második lap]]
* [[Harmadik lap]]
|-
| '''Al-oldalak'''
* [[Al-oldal 1]]
* [[Al-oldal 2]]
* [[Al-oldal 3]]
|-
| '''Külső linkek'''
* [https://example.com Külső forrás]
* [https://example.com Másik forrás]
|}
```

---

## 12. Code blocks (native `<pre>` only)

### Plain code block

```wiki
<pre>
from dataclasses import dataclass

@dataclass
class User:
    name: str
    age: int

    def greet(self) -> str:
        return f"Hello, {self.name}!"

if __name__ == "__main__":
    u = User("Anna", 28)
    print(u.greet())
</pre>
```

### Code with a filename label (replaces `{{Codesample|...}}`)

```wiki
'''hello.py'''
<pre>
def hello():
    return "Hello"
</pre>
```

### Code with a "file header" (visual equivalent of `{{Codesample}}`)

```wiki
{| class="wikitable" style="background:#f8f9fa; border:1px solid #eaecf0; border-radius:4px; padding:0;"
|-
| style="background:#eaecf0; border-bottom:1px solid #c8ccd1; padding:4px 8px; font-family:monospace; font-size:0.9em;" | hello.py
|-
| style="padding:12px;" |
<pre>
def hello():
    return "Hello"
</pre>
|}
```

### Inline code (`<code>` — core)

```wiki
A <code>hello()</code> függvény meghívása a <kbd>Ctrl</kbd>+<kbd>C</kbd> billentyűkombinációval történik.
```

### Keyboard input (`<kbd>` — core)

```wiki
A mentéshez nyomd meg: <kbd>Ctrl</kbd> + <kbd>S</kbd>
```

### Button-style text (`<kbd>` — core, replaces `{{Button|...}}`)

```wiki
Kattints a <kbd>Mentés</kbd> gombra.
```

### Variable or placeholder (`<var>` — core)

```wiki
A felhasználónév: <var>{{{username}}}</var>
```

> **Note:** `<syntaxhighlight lang="...">` requires the SyntaxHighlight
> extension. Plain `<pre>` is portable.

---

## 13. Code + explanation (two-column wikitable)

```wiki
{| class="wikitable"
! Kód
! Magyarázat
|-
|
<pre>
def hello(name: str) -> str:
    return f"Hello, {name}!"
</pre>
|
A <code>hello</code> függvény egy üdvözlést ad vissza a <var>name</var> paraméter alapján.
A függvény típus annotációval van ellátva, ami segíti a kód olvasását.
|-
|
<pre>
print(hello("World"))
</pre>
|
A függvény hívása a <code>print</code> függvénnyel együtt a konzolra írja az üdvözlést.
|}
```

---

## 14. Button-style code snippet (terminal look)

```wiki
<div style="background:#1e1e1e; color:#d4d4d4; padding:12px 16px; border-radius:6px; font-family:'Courier New', monospace; margin:12px 0;">
$ <span style="color:#4ec9b0;">npm</span> install mediawiki-cli<br />
$ <span style="color:#4ec9b0;">mediawiki-cli</span> --version<br />
<span style="color:#608b4e;">mediawiki-cli v2.0.1</span>
</div>
```

---

## 15. Galleries (native `<gallery>` tag)

### Basic gallery

```wiki
<gallery caption="Példa galéria" widths="200" heights="150">
File:Kép1.jpg|1. kép
File:Kép2.jpg|2. kép
File:Kép3.jpg|3. kép
</gallery>
```

### Modern caption appearing on hover (packed-hover)

```wiki
<gallery mode="packed-hover" heights="200px" caption="Modern galéria" class="center">
File:Kép1.jpg|Címke 1
File:Kép2.jpg|Címke 2
File:Kép3.jpg|Címke 3
File:Kép4.jpg|Címke 4
</gallery>
```

### Slideshow mode

```wiki
<gallery mode="slideshow" caption="Slideshow">
File:Kép1.jpg|1. dia
File:Kép2.jpg|2. dia
File:Kép3.jpg|3. dia
</gallery>
```

> **Note:** `<gallery>` is a core MediaWiki tag — no extension required.

---

## 16. Mathematical formulas (native HTML only)

```wiki
A Pitagorasz-tétel: <span class="texhtml"><i>a</i><sup>2</sup> + <i>b</i><sup>2</sup> = <i>c</i><sup>2</sup></span>

Egy egyenlet: <span class="texhtml">&sum;<sub><i>i</i>=1</sub><sup><i>n</i></sup> <i>i</i> = <i>n</i>(<i>n</i>+1)/2</span>

Integrál: <span class="texhtml">&int;<sub>0</sub><sup>1</sup> <i>x</i><sup>2</sup> d<i>x</i> = 1/3</span>
```

For a centered, display-style formula:

```wiki
<div class="texhtml" style="text-align:center; margin:16px 0; font-size:1.1em;">
<i>A</i> = <sup>1</sup>&frasl;<sub>2</sub> |<span style="text-decoration:overline;">v</span> &times; <span style="text-decoration:overline;">w</span>|
</div>
```

> **Note:** `<math>...</math>` requires the Math extension. The native
> `<span class="texhtml">` + HTML `<sup>` / `<sub>` renders on every
> MediaWiki install.

---

## 17. Diagrams (no `<mermaid>` — native `<pre>` or `<table>`)

### ASCII flowchart in `<pre>`

```wiki
<pre>
[Kezdés]
   |
   v
{Döntés: folytatjuk?}
   |
   +--- nem ---> [Stop]
   |
  igen
   |
   v
[1. lépés] --> [2. lépés] --> [Vége]
</pre>
```

### Box-and-line diagram with native `<table>`

```wiki
{| style="border:none; margin: 12px auto;"
|+ '''Munkafolyamat'''
|-
| style="text-align:center; padding:8px 16px; background:#eaecf0; border:1px solid #c8ccd1; border-radius:6px;" | Kezdés
| style="text-align:center; font-size:1.4em;" | &darr;
| style="text-align:center; padding:8px 16px; background:#eaf3ff; border:1px solid #36c; border-radius:6px;" | 1. lépés
| style="text-align:center; font-size:1.4em;" | &darr;
| style="text-align:center; padding:8px 16px; background:#eaf3ff; border:1px solid #36c; border-radius:6px;" | 2. lépés
| style="text-align:center; font-size:1.4em;" | &darr;
| style="text-align:center; padding:8px 16px; background:#d5fdf4; border:1px solid #14866d; border-radius:6px;" | Vége
|}
```

### Sequence / class diagram (use a wikitable)

```wiki
{| class="wikitable"
! Actor !! Action !! Result
|-
| Kliens || Kérés küldése || Kérés a szervernek
|-
| Szerver || Feldolgozás || Válasz előkészítése
|-
| Szerver || Válasz küldése || Válasz a kliensnek
|}
```

> **Note:** `<mermaid>...</mermaid>` requires the Mermaid extension. The
> native `<pre>` + `<table>` alternatives render on every MediaWiki install.

---

## 18. Footnotes (native `<ref>` — no `{{cite ...}}`)

### Simple footnote

```wiki
Ez egy állítás.<ref>[https://example.com Cím], Szerző, 2026-01-15.</ref>
```

### Reference list at the bottom of the page

```wiki
<references />
```

or arranged in columns:

```wiki
<references responsive="0" columns="2" />
```

### Reused footnote

```wiki
Első említés.<ref name="smith2020" />
Későbbi említés.<ref name="smith2020" />
Még egy említés.<ref name="smith2020" />

<references />
<ref name="smith2020">Smith, John (2020). "A cikk címe". Journal, 12(3), 45-67.</ref>
```

### Citation string formats (manual)

```wiki
<ref>[https://example.com Web oldal címe], Szerző Neve, 2026-01-15.</ref>
<ref>Kovács Anna (2024). ''A könyv címe''. Akadémiai Kiadó. ISBN 978-963-XXX-XXX-X.</ref>
<ref>Nagy Béla (2024). "A cikk". ''Folyóirat neve'', 12(3), 45-67.</ref>
<ref>[https://example.com/news Hírek]. ''Újság neve'', 2026-01-15.</ref>
<ref>Source: Internal documentation, 2026-01-15.</ref>
```

> **Note:** `{{cite web|...}}`, `{{cite book|...}}`, etc. require the Cite
> extension. Manual citation strings render on every MediaWiki install.

---

## 19. Modern, clean article header (hero)

```wiki
{| style="width:100%; background:linear-gradient(135deg,#36c 0%,#447ff5 100%); color:#fff; border-radius:8px; padding:24px; margin-bottom:24px;"
|-
| style="text-align:center;" |
<div style="font-size:1.5em; font-weight:600;">A cikk címe</div>
<div style="margin-top:8px; opacity:0.9;">Rövid leírás a cikkről</div>
|}
```

> **Note:** this is the native alternative to the TemplateStyles-based hero.
> It looks identical and works on every MediaWiki install.

---

## 20. Hatnote (replaces `{{Main|...}}`, `{{See also|...}}`, `{{For|...}}`)

### Main / kapcsolódó cikk

```wiki
<div style="font-style:italic; margin-bottom:12px; color:#54595d;">
Bővebben lásd: [[Másik cikk]].
</div>
```

### See also / lásd még

```wiki
<div style="font-style:italic; margin-bottom:12px; color:#54595d;">
Lásd még: [[Kapcsolódó 1]], [[Kapcsolódó 2]].
</div>
```

### For / más néven

```wiki
<div style="font-style:italic; margin-bottom:12px; color:#54595d;">
Más néven: [[Alternatív név]].
</div>
```

---

## 21. Source attribution (replaces `{{Forrás|...}}`, `{{Source|...}}`)

```wiki
<div style="background:#f8f9fa; border:1px solid #eaecf0; border-radius:4px; padding:8px 12px; margin:12px 0; font-size:0.9em; color:#54595d;">
'''Forrás:''' Eredeti dokumentum, 2026-01-15. Fordította: [[Anna Kovács]].
</div>
```

Place at the top of the article (after the lead paragraph).

---

## 22. Complete, modern article template (native only)

```wiki
{| style="width:100%; background:linear-gradient(135deg,#36c 0%,#447ff5 100%); color:#fff; border-radius:8px; padding:24px; margin-bottom:24px;"
|-
| style="text-align:center;" |
<div style="font-size:1.5em; font-weight:600;">A cikk címe</div>
<div style="margin-top:8px; opacity:0.9;">Rövid leírás a cikkről</div>
|}

<div style="background:#f8f9fa; border:1px solid #eaecf0; border-radius:4px; padding:8px 12px; margin:12px 0; font-size:0.9em; color:#54595d;">
'''Forrás:''' Eredeti dokumentum, 2026-01-15.
</div>

== Áttekintés ==

Ez a cikk egy rövid áttekintést ad a témáról. Az első bekezdés összefoglalja a
legfontosabb tudnivalókat.

<div style="background:#eaf3ff; border:1px solid #36c; border-left:4px solid #36c; border-radius:4px; padding:12px 16px; margin:12px 0;">
'''Note:''' Hasznos információ a cikkről.
</div>

== Három fő szempont ==

{| class="wikitable" style="width:100%;"
|+ style="text-align:left; font-weight:600;" | Összehasonlítás
|-
| style="width:33%; vertical-align:top; padding:16px;" |
'''1. szempont'''
Első szempont részletes leírása.
| style="width:33%; vertical-align:top; padding:16px;" |
'''2. szempont'''
Második szempont részletes leírása.
| style="width:33%; vertical-align:top; padding:16px;" |
'''3. szempont'''
Harmadik szempont részletes leírása.
|}

== Részletes leírás ==

=== Első alfejezet ===

<pre>
def hello(name: str) -> str:
    return f"Hello, {name}!"
</pre>

=== Második alfejezet ===

{| class="wikitable sortable"
! Oszlop 1 !! Oszlop 2
|-
| adat 1 || adat 2
|-
| adat 3 || adat 4
|}

== Példa galéria ==

<gallery mode="packed-hover" heights="200px" caption="Példa galéria">
File:Example.jpg|1. kép
File:Example.jpg|2. kép
File:Example.jpg|3. kép
</gallery>

== GYIK ==

<details>
<summary>1. kérdés: Hogyan működik?</summary>
A rendszer úgy működik, hogy...
</details>

<details>
<summary>2. kérdés: Mibe kerül?</summary>
A rendszer ingyenes, de...
</details>

== Kapcsolódó cikkek ==

* [[Kapcsolódó cikk 1]]
* [[Kapcsolódó cikk 2]]
* [[Kapcsolódó cikk 3]]

== Források ==

<references />

[[Category:Példák]][[Category:Dokumentáció]]
```

---

## 23. When to use which recipe

| If the user asks for... | Use this recipe... |
|------------------------------|----------------------------|
| "create a table" | 1, 2 |
| "create a Note / Tip / Warning box" | 3 |
| "create a page banner" | 4 |
| "create a generic message box" | 5 |
| "create cards" | 6 |
| "create a collapsible section" | 7 |
| "create tabs" | 8 |
| "create breadcrumbs" | 9 |
| "create an infobox" | 10 |
| "create a navigation box" | 11 |
| "create code" | 12, 13, 14 |
| "create a gallery" | 15 |
| "create a mathematical formula" | 16 |
| "create a diagram" | 17 |
| "create a footnote" | 18 |
| "create a hero header" | 19 |
| "create a hatnote" | 20 |
| "create source attribution" | 21 |
| "create a complete article" | 22 |

---

Sources:
- MediaWiki syntax: `references/01-syntax-cheatsheet.md`
- Design patterns: `references/02-design-patterns.md`
- Advanced features: `references/05-advanced-features.md`
- Native-only mapping: `references/00-native-only-mapping.md`
