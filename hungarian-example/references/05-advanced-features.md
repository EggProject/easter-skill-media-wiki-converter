# 05 — Haladó MediaWiki funkciók

> Források: <https://www.mediawiki.org/wiki/Help:Magic_words>,
> <https://www.mediawiki.org/wiki/Help:Extension:ParserFunctions>,
> <https://www.mediawiki.org/wiki/Extension:TemplateStyles>,
> <https://www.mediawiki.org/wiki/Extension:Scribunto>,
> <https://www.mediawiki.org/wiki/Manual:Interface/Sidebar>

Ez a fájl a haladó MediaWiki funkciókat gyűjti össze. Csak akkor használd, ha a
cél wiki tartalmazza az adott kiterjesztést.

---

## 1. TemplateStyles (saját CSS a lapokon)

A TemplateStyles lehetővé teszi, hogy a felhasználók CSS-t tároljanak wiki lapokon
(általában a `Modul:` névtérben), és a tartalomba ágyazottan hivatkozzanak rá.

### Használata

```wiki
<templatestyles src="Modul:Highlight/styles.css" />

<div class="hl-primary">
Ez egyedi stílusú doboz.
</div>
```

### A CSS lap (Modul:Highlight/styles.css)

```css
.hl-primary {
  background: #eaf3ff;
  border-left: 4px solid #36c;
  padding: 12px 16px;
  border-radius: 4px;
  margin: 12px 0;
}
```

### Biztonság

A TemplateStyles **korlátozott CSS**-t engedélyez — nincs `position: fixed`, nincs
`@import`, nincs JavaScript. A teljes biztonsági lista a kiterjesztés dokumentációjában.

### Mikor használd

- Ha a wiki design elemei konzisztensek kell, hogy legyenek (pl. minden kiemelés ugyanúgy
  nézzen ki).
- Ha a felhasználó kér egyedi design-t, amit a beépített sablonokkal nem lehet elérni.

---

## 2. Scribunto (Lua modulok)

A Scribunto lehetővé teszi, hogy a sablonok Lua kódot futtassanak. Ez a legerősebb
testreszabási eszköz MediaWiki-ben.

### Alap használat

```wiki
{{#invoke:Module:Hello|sayHi|name=Anna}}
```

A `Module:Hello` tartalma:

```lua
local p = {}

function p.sayHi(frame)
    local name = frame.args.name or "World"
    return "Hello, " .. name .. "!"
end

return p
```

### Mikor használd

- Ha a wiki tartalmaz összetett logikát (string kezelés, matematika, dátumformázás).
- Ha a felhasználó kéri, hogy "Lua modul" legyen.

### Megjegyzés

A Scribunto-t a MediaWiki adminnak kell telepítenie. A legtöbb wikin elérhető,
de néhány privát wikin nem.

---

## 3. ParserFunctions (kiterjesztés)

A ParserFunctions kiterjesztés a leggyakoribb kiterjesztés, szinte mindig telepítve van.

### Leggyakoribb függvények

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

### String függvények (opcionális)

```wiki
{{#len: string }}
{{#pos: string | keresett }}
{{#sub: string | start | length }}
{{#replace: string | keresett | csere }}
{{#explode: string | delim | index }}
{{#urldecode: encoded }}
{{#urlencode: text }}
```

### Core függvények (kiterjesztés nélkül is)

```wiki
{{lc: AbC}}                                 → "abc"
{{uc: AbC}}                                 → "ABC"
{{lcfirst: AbC}}                            → "abC"
{{ucfirst: abc}}                            → "Abc"
{{anchorencode: AbC dEf}}                   → "AbC_dEf"
{{urlencode: hello world}}                  → "hello+world"
```

### Példa: feltételes tartalom

```wiki
{{#ifeq: {{NAMESPACE}} | Súgó
| Ez a szöveg csak a Súgó névtérben jelenik meg.
| Ez a szöveg mindenhol máshol jelenik meg.
}}
```

---

## 4. Variables (lokális változók)

A Variables kiterjesztés (opcionális) lehetővé teszi, hogy a lapon belül lokális
változókat hozz létre.

```wiki
{{#vardefine: szin | kék}}
{{#var: szin}}                                → "kék"
{{#ifexpr: {{#var: szin}} = kék | ... | ... }}
```

### Megjegyzés

A Variables kiterjesztést a MediaWiki adminnak kell telepítenie. Ha nincs, használj
Scribunto-t vagy a lap paramétereit.

---

## 5. Page Forms (űrlap alapú szerkesztés)

A Page Forms (korábban Semantic Forms) kiterjesztés lehetővé teszi, hogy a lapokat
űrlapokon keresztül szerkeszd.

### Példa

```wiki
{{#forminput:form=Személy űrlap}}
{{Special:FormEdit/Személy űrlap/Anna}}
```

### Mikor használd

- Ha a wiki tartalmaz strukturált adatokat (személyek, projektek, stb.).
- Ha a felhasználó kéri, hogy "űrlapon lehessen szerkeszteni".

---

## 6. Semantic MediaWiki (szemantikus adatok)

A Semantic MediaWiki (SMW) kiterjesztés lehetővé teszi, hogy a lapokon szemantikus
adatokat tárolj, és lekérdezéseket futtass.

### Alap használat

```wiki
[[Főváros::Budapest]]
[[Népesség::1750000]]
[[Alapítás éve::1873]]
```

### Lekérdezés

```wiki
{{#ask:
[[Főváros::+]]
[[Népesség::>1000000]]
| ?Népesség
| ?Főváros
| format=table
}}
```

### Mikor használd

- Ha a wiki tartalmaz strukturált adatokat, és a felhasználó adatbázis-szerű lekérdezéseket akar.

---

## 7. Cargo (strukturált adat tárolás)

A Cargo kiterjesztés a SMW alternatívája, egyszerűbb szintaxissal.

### Adattárolás sablonban

```wiki
{{#cargo_store:
 _table=személyek
 |név=Anna Kovács
 |kor=28
 |város=Budapest
}}
```

### Lekérdezés

```wiki
{{#cargo_query:
 tables=személyek
 |fields=név, kor, város
 |where=kor > 25
 |format=table
}}
```

---

## 8. Kategóriák és kategória-rendszerek

### Alap kategória

```wiki
[[Category:Projektek]]
[[Category:2025-ös projektek]]
```

### Többszintű kategória-fa

```wiki
[[Category:Projektek]]
[[Category:Aktív projektek]]
[[Category:Befejezett projektek]]
```

### Rejtett kategória

```wiki
__HIDDENCAT__
[[Category:Admin]]
```

vagy

```wiki
[[Category:Admin|__HIDDENCAT__]]
```

### Kategória fa megjelenítése

```wiki
{{CategoryTree|Projektek|depth=3|hideroot=on}}
```

### Kategóriában lévő lapok listázása

```wiki
{{PAGESINCAT:Projektek}}                     → csak szám
{{Special:CategoryViewer|Projektek}}         → lista
```

vagy a CategoryPager kiterjesztéssel:

```wiki
{{#categorypager:Projektek}}
```

### Smarty kategória rendezés

A `sort key` paraméterrel a kategórián belüli rendezést testreszabhatod:

```wiki
[[Category:Személyek|Kovács, Anna]]
```

---

## 9. Interwiki linkek

Az interwiki linkek más wikikre mutatnak (általában a Wikimédia projektekre).

```wiki
[[w:Hungary]]                  → Wikipedia (magyar vagy angol)
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
[[gerrit:604435]]              → Gerrit (kód review)
```

### Saját interwiki előtag beállítása

A wiki adminisztrátor a `MediaWiki:Interwiki map` lapon adhat hozzá új interwiki
előtagokat.

---

## 10. VisualEditor (WYSIWYG szerkesztő)

A VisualEditor a MediaWiki WYSIWYG szerkesztője, ami 2013 óta elérhető.

### Előnyei

- Nem kell megtanulni a wikitext szintaxist
- Valós idejű előnézet
- Speciális eszközök: táblázat szerkesztő, sablon beszúrás, stb.

### Hátrányai

- Lassabb lehet, mint a wikitext szerkesztés
- A komplex sablonok néha rosszul jelennek meg
- A `<syntaxhighlight>` és egyéb HTML kiterjesztések korlátozottan szerkeszthetők

### Ajánlás

Ha a felhasználó VisualEditor-t használ, a wikitext legyen "VE-barát":
- Egyszerű táblázatok, ne összetett class-okkal
- Sablonok ne legyenek túl mélyen egymásba ágyazva
- A `<syntaxhighlight>` támogatott, de a spéci paraméterek (`line`, `highlight`)
  nem szerkeszthetők VE-ből

---

## 11. Kiterjesztések, amiket gyakran keresnek

| Kiterjesztés | Funkció | Elérhetőség |
|--------------|---------|-------------|
| SyntaxHighlight | Kódkiemelés | Szinte mindig |
| TemplateStyles | Saját CSS | Általában |
| ParserFunctions | Parser function-ök | Szinte mindig |
| Scribunto | Lua modulok | Általában |
| VisualEditor | WYSIWYG | Alapértelmezett (1.35+) |
| TabberNeue | Tab fülek | Telepíteni kell |
| MultimediaViewer | Kép nagyítás | Alapértelmezett |
| PageImages | Lap kiemelt képe | Alapértelmezett |
| InputBox | Gyors lap-létrehozás | Telepíteni kell |
| CategoryTree | Kategória fa | Telepíteni kell |
| CollapsibleVector | Összecsukható | Alapértelmezett |
| Header Tabs | Fejléc tabok | Telepíteni kell |
| Page Forms | Űrlap szerkesztés | Telepíteni kell |
| Semantic MediaWiki | Szemantikus adatok | Telepíteni kell |
| Math | LaTeX képletek | Általában |
| Mermaid | Diagramok | Telepíteni kell |
| Graphviz | Diagramok | Telepíteni kell |
| Poem | Versformázás | Alapértelmezett |
| Arrays | Tömb kezelés | Telepíteni kell |
| Maps | Térkép embed | Telepíteni kell |
| Cargo | Strukturált adat | Telepíteni kell |
| CodeMirror | Kiemelt szerkesztő | Általában |
| WikiEditor | Haladó wikitext szerkesztő | Alapértelmezett |
| LiquidThreads | Új generációs vitalap | Telepíteni kell |
| Flow | Új generációs vitalap | Helyette LiquidThreads |
| Thanks | Köszönet adás | Telepíteni kell |
| Echo | Értesítések | Alapértelmezett (1.22+) |
| Flow (régi) | Vitalap átalakítás | Már nem fejlesztett |

---

## 12. Saját kiterjesztések írása

Ha a felhasználó kéri, hogy "saját kiterjesztést" írj, **NE** írj PHP kódot a skill
használatával. Ehelyett:

1. Jelezd, hogy a kiterjesztés fejlesztés a MediaWiki adminisztrátor feladata.
2. Javasolj alternatívát:
   - TemplateStyles (CSS)
   - Scribunto (Lua modul)
   - ParserFunctions (sablon szintű logika)
3. Ha mindenképp PHP kiterjesztés kell, ajánld fel, hogy a felhasználó konzultáljon
   egy MediaWiki fejlesztővel.

---

## 13. A MediaWiki biztonsági modellje

Ha a felhasználó biztonsági kérdést tesz fel:

### Mit véd a MediaWiki?

- **XSS (cross-site scripting)**: a felhasználói tartalom escape-elve van, a HTML csak
  bizonyos tageket engedélyez.
- **CSRF (cross-site request forgery)**: a szerkesztésekhez token kell.
- **Clickjacking**: a MediaWiki frame-ellenes headert küld.
- **SQL injection**: a MediaWiki ORM-et használ, escape-eli a bemenetet.

### Mit NE tegyél a wikitextben

- **NE használj `<script>` tag-et** — a MediaWiki eltávolítja.
- **NE használj `onclick`, `onerror` stb. attribútumokat** — a MediaWiki eltávolítja.
- **NE használj `javascript:` URL-t** — a MediaWiki blokkolja.
- **NE próbálj meg iframe-et embedelni** ismeretlen oldalról.
- **NE használj `{{#css:...}}` vagy hasonló escape-eletlen szintaxist** — XSS-hez vezethet.

### Ami engedélyezett

- A `<templatestyles>` tag-en belüli CSS.
- A Scribunto Lua modulok.
- A ParserFunctions függvények.
- A `<syntaxhighlight>` (csak a kódot jeleníti meg, nem futtatja).

---

## 14. Telepítési útmutató (röviden)

Ha a felhasználó kéri, hogy "telepítsd a TemplateStyles-t" vagy hasonlót:

> A kiterjesztések telepítése a MediaWiki szerver adminisztrátorának feladata.
> Az alábbi lépések szükségesek:
>
> 1. A kiterjesztés letöltése (Git vagy tarball).
> 2. A `extensions/` könyvtárba másolás.
> 3. A `LocalSettings.php` fájlban a `wfLoadExtension('Kiterjesztés neve');` sor hozzáadása.
> 4. A `php maintenance/update.php` futtatása.
> 5. A `MediaWiki:Kiterjesztés-neve` lapok szerkesztése (ha van).
>
> A felhasználó ezt nem a skill segítségével csinálja, hanem a MediaWiki admin.

A skill célja a **tartalom** (wikitext) előállítása, nem a szerver konfiguráció.

---

Források:
- <https://www.mediawiki.org/wiki/Help:Magic_words>
- <https://www.mediawiki.org/wiki/Help:Extension:ParserFunctions>
- <https://www.mediawiki.org/wiki/Extension:TemplateStyles>
- <https://www.mediawiki.org/wiki/Extension:Scribunto>
- <https://www.mediawiki.org/wiki/Extension:Semantic_MediaWiki>
- <https://www.mediawiki.org/wiki/Extension:Cargo>
- <https://www.mediawiki.org/wiki/Extension:Page_Forms>
- <https://www.mediawiki.org/wiki/Manual:Interface/Sidebar>
- <https://www.mediawiki.org/wiki/Security>
