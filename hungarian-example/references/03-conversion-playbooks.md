# 03 — Konverziós playbook-ok (HTML / PDF / Word / Excel / PPT → MediaWiki)

> Források: <https://www.mediawiki.org/wiki/Extension:PandocUltimateConverter>,
> <https://www.mediawiki.org/wiki/Extension:ConvertPDF2Wiki>,
> <https://stackoverflow.com/questions/2833263/convert-from-microsoft-word-to-media-wiki-markup-style>,
> <https://openwetware.org/wiki/Converting_documents_to_mediawiki_markup>

> **FONTOS:** Ez a fájl a konverzió elveit írja le, NEM készít scripteket. A tényleges
> konverziót mindig kézzel vagy LLM-mel végezzük, hogy a design és a stílus megmaradjon.

---

## Általános szabályok (minden formátumra)

1. **A szöveget SOHA ne módosítsd.** Csak a formátumot és a vizuális szerkezetet alakítjuk.
2. **A design elemeit próbáld megőrizni** — ha a forrásban van szín, doboz, táblázat, ikon,
   keresd meg a MediaWiki-s megfelelőjét.
3. **Dupla ellenőrzés KÖTELEZŐ.** Minden konverzió után indíts független ügynököt
   (lásd: `references/04-verification-checklist.md`).
4. **Forrásmegjelölés.** A kész cikk tetejére opcionálisan tegyél `{{Forrás|...}}` sablont.
5. **Teljes cikket mutass, ne részleteket.** A user az egész konverziót akarja látni.

---

## 1. HTML riport → MediaWiki

### A legkönnyebb konverzió, mert a HTML-elemek 1:1-re leképezhetők.

#### Lépésről lépésre

1. **Címsorok**: `<h1>` → `=` (de SOHA ne használd), `<h2>` → `==`, `<h3>` → `===`, stb.
2. **Bekezdések**: `<p>` → szöveg, dupla sortöréssel elválasztva.
3. **Listák**: `<ul><li>` → `*`, `<ol><li>` → `#`, nested listáknál növeld a `*`/`#` számát.
4. **Táblázatok**: `<table>` → `{|`, `<tr>` → `|-`, `<th>` → `!`, `<td>` → `|`.
   Adj `class="wikitable"`-t.
5. **Linkek**: `<a href="...">` belső → `[[...]]`, külső → `[...]`.
6. **Képek**: `<img src="...">` → `[[File:...|thumb|...]]`.
7. **Kód**: `<pre>` → `<syntaxhighlight>` (ha van nyelv), vagy `<pre>` ha nincs.
8. **Idézetek**: `<blockquote>` → `{{blockquote|...}}` vagy `<blockquote>`.
9. **Dobozok, kiemelések**: `<div class="note">` → `{{Note|...}}`.
10. **HTML entitások**: `&amp;` `&lt;` `&gt;` — ezek maradjanak (a MediaWiki is támogatja).

#### HTML attribútum → MediaWiki attribútum

| HTML | MediaWiki | Megjegyzés |
|------|-----------|------------|
| `class="wikitable"` | `class="wikitable"` | változatlan |
| `class="wikitable sortable"` | `class="wikitable sortable"` | változatlan |
| `style="text-align: center"` | `style="text-align: center"` | változatlan |
| `style="color: red"` | `style="color: red"` | változatlan (ritkán) |
| `colspan="2"` | `colspan="2"` | változatlan |
| `rowspan="2"` | `rowspan="2"` | változatlan |
| `align="center"` | `style="text-align: center"` | HTML5-ben elavult, cseréld |
| `bgcolor="..."` | `style="background: ..."` | HTML5-ben elavult, cseréld |

#### Példa: HTML → Wiki

**Forrás (HTML):**
```html
<h2>Q4 2025 Elemzés</h2>
<p>Ez a jelentés a <strong>Q4 2025-ös</strong> eredményeket foglalja össze.</p>
<table class="data">
  <thead>
    <tr><th>Metrika</th><th>Érték</th></tr>
  </thead>
  <tbody>
    <tr><td>Bevétel</td><td>€4.2M</td></tr>
    <tr><td>Növekedés</td><td>+18%</td></tr>
  </tbody>
</table>
<p><strong>Konklúzió:</strong> A Q4 erős negyedév volt.</p>
<ul>
  <li>Első pont</li>
  <li>Második pont</li>
</ul>
```

**Eredmény (MediaWiki):**
```wiki
== Q4 2025 Elemzés ==

Ez a jelentés a '''Q4 2025-ös''' eredményeket foglalja össze.

{| class="wikitable sortable"
! Metrika !! Érték
|-
| Bevétel || €4.2M
|-
| Növekedés || +18%
|}

'''Konklúzió:''' A Q4 erős negyedév volt.

* Első pont
* Második pont

[[Category:Jelentések]][[Category:2025]]
```

#### Tippek

- A HTML class-ok elveszhetnek, ha a MediaWiki skin másképp stílusozza azokat. Mindig
  `class="wikitable"`-ra cseréld.
- A `<div>` alapú layout-ot próbáld meg táblázatokkal vagy TemplateStyles-szel helyettesíteni.
- Ha a HTML-ben sok nested `<div>` van, a lap komplex lesz. Fontold meg, hogy a forrás
  mely elemeit érdemes megtartani.

---

## 2. HTML slideshow (reveal.js, Swiper, stb.) → MediaWiki

### Kihívás: a slideshow strukturális, de a MediaWiki lineáris.

#### Lépésről lépésre

1. **Minden dia → szekció (`==` címsor).** A dia címe a szekció címe.
2. **A dián belüli tartalom** a szekció tartalma.
3. **A progress bar** kikerül, helyette `__TOC__` a lap tetején.
4. **A navigációs nyilak** helyett szekció-linkek a szövegben, vagy `[[#Következő szekció]]`.
5. **A képek** `[[File:...|thumb|...]]` formátumba kerülnek.
6. **A kód** `<syntaxhighlight>` formátumba.
7. **A fragmensek** (reveal.js-ben lépésről lépésre megjelenő tartalom) → "Összecsukható
   szekciók" (`mw-collapsible`) vagy "Részletek" (`<details>`).

#### Példa: reveal.js dia → Wiki szekció

**Forrás (HTML):**
```html
<section>
  <h2>1. Bevezetés</h2>
  <p>Ez a prezentáció a MediaWiki-ről szól.</p>
  <ul>
    <li class="fragment">A wiki szintaxisáról</li>
    <li class="fragment">A design lehetőségeiről</li>
    <li class="fragment">A konverzió módszereiről</li>
  </ul>
</section>
<section>
  <h2>2. A wiki szintaxis</h2>
  <pre><code class="language-python">def hello(): return "Hello"</code></pre>
</section>
```

**Eredmény (MediaWiki):**
```wiki
__TOC__

== 1. Bevezetés ==

Ez a prezentáció a MediaWiki-ről szól.

{| class="mw-collapsible mw-collapsed"
! Tematika (kattints a kibontáshoz)
|-
* A wiki szintaxisáról
* A design lehetőségeiről
* A konverzió módszereiről
|}

== 2. A wiki szintaxis ==

<syntaxhighlight lang="python">
def hello():
    return "Hello"
</syntaxhighlight>
```

#### Tippek

- A "fragment" (lépésről lépésre) hatás elveszik — de a `mw-collapsible` megőrzi az
  "interaktivitást" (kattintásra jelenik meg).
- A dia-szintű lábjegyzeteket gyűjtsd össze a cikk végén, ne diánként.
- Ha a slideshow tartalmaz "köszönet" vagy "következő téma" diát, azt alakítsd "Kapcsolódó cikkek"
  szekcióvá.

---

## 3. PDF → MediaWiki

### A PDF a legnehezebb forrás, mert a layout bináris.

#### Hogyan nyerd ki a forrást (mielőtt wikitextet írnál)

> **MEGJEGYZÉS:** Itt NEM scripteket írunk, csak az elveket. A tényleges konverziót
> a felhasználó eszközeivel (Pandoc, Adobe Acrobat, online PDF→HTML konverter) végzi,
> az eredményt átadja neked, te pedig wikitextet készítesz belőle.

1. **Szöveg kinyerése:** a felhasználó használhat `pdftotext`, Adobe Acrobat "Export PDF",
   vagy online PDF→TXT konvertert. A kimenet egy `.txt` fájl.
2. **Képek kinyerése:** a felhasználó "Extract images" funkcióval (Adobe Acrobat) vagy
   `pdfimages` paranccsal (poppler-utils) kimenti a képeket.
3. **Táblázatok:** a PDF-ben a táblázat gyakran "szétszakad". Ilyenkor a felhasználó
   kézzel ellenőrzi, hogy a sorok/oszlopok helyesek-e.
4. **A kinyert szöveget + képeket** a felhasználó átadja neked wikitext generáláshoz.

#### Wikitext generálás PDF-ből

1. **Címsorok:** a PDF gyakran nagyobb betűmérettel jelöli a címsorokat. A szöveges
   kinyerés után ezt elveszítjük — a felhasználónak kell jeleznie, hol vannak címsorok.
2. **Bekezdések:** a PDF-ből kinyert szövegben a bekezdések sokszor összefolynak. A
   felhasználó jelezheti, hol kezdődik új bekezdés.
3. **Táblázatok:** ha a kinyert szövegben felismerhetők a táblázatok (tabulátorok, igazítás),
   próbáld meg wikitable formába önteni. Ha nem, kérdezd meg a felhasználót.
4. **Képek:** a kinyert képeket a felhasználó feltölti a wiki-re, és megadja a fájlnevet.
5. **Lábjegyzetek:** a PDF lábjegyzeteit a cikk végére gyűjtsd `<ref>` formátumban.
6. **Az oldalszámokat** hagyd ki — a MediaWiki-ben nincs "oldal" fogalom.

#### Példa: PDF-ből kinyert szöveg → Wiki

**Forrás (PDF-ből kinyert plain text):**
```
Q4 2025 Elemzés
Ez a jelentés a Q4 2025-ös eredményeket foglalja össze.

Metrika    Érték
Bevétel    €4.2M
Növekedés  +18%

Konkluszió: A Q4 erős negyedév volt.
```

**Eredmény (MediaWiki):**
```wiki
== Q4 2025 Elemzés ==

Ez a jelentés a Q4 2025-ös eredményeket foglalja össze.

{| class="wikitable sortable"
! Metrika !! Érték
|-
| Bevétel || €4.2M
|-
| Növekedés || +18%
|}

'''Konklúzió:''' A Q4 erős negyedév volt.
```

#### Tippek

- A PDF gyakran tartalmaz "vonalakat" (—) a szövegben, amik a PDF-layout részei. Ezeket
  hagyd ki a wikitextből.
- A PDF-ből kinyert szövegben sokszor a "smart quotes" (gömbölyű idézőjelek) összekeverednek
  a straight quotes-okkal. Egységesítsd a MediaWiki konvencióra (egyenes `"`).
- Ha a PDF-ben van tartalomjegyzék, NE másold be — a MediaWiki automatikus TOC-ot generál.
- Ha a PDF sok képet tartalmaz, kérdezd meg a felhasználót, hogy a képeket hogyan
  szeretné elnevezni a feltöltésnél.

---

## 4. Word (DOCX) → MediaWiki

### A Word a leggyakoribb forrás. Kétlépcsős a folyamat.

#### Hogyan nyerd ki a forrást

> **MEGJEGYZÉS:** A tényleges konverziót a felhasználó végzi. Te a kész `.txt` fájlt
> kapod meg, és abból készítesz wikitextet.

1. **Pandoc**: `pandoc -f docx -t mediawiki -o output.wiki input.docx`
2. **Manuális**: a Word "Save As" → "Plain Text", de elveszik a formázás.

#### Wikitext generálás Word-ből

A Word/Pandoc kimenet tipikus hibái:
- A címsorok `== Heading 1 ==` formában vannak (a Word stílus nevét tartalmazzák).
- A listák néha `*` helyett `-` jellel kezdődnek.
- A táblázatok "word-table" formátumban vannak, ami nem MediaWiki-szintaxis.
- A lábjegyzetek eltérő szintaxissal vannak.
- A képek base64-kódolásúak lehetnek (a Pandoc néha így adja vissza).

**Lépések:**
1. A Pandoc kimenetét nézd át, és javítsd a fentieket.
2. A címsorok `==` szintjét ellenőrizd (2. szint a maximum).
3. A listákat egységesítsd `*` (számozatlan) és `#` (számozott) formátumra.
4. A táblázatokat alakítsd `class="wikitable"` formátumra.
5. A base64 képeket cseréld `[[File:...]]` hivatkozásokra (a felhasználó feltölti a képeket).
6. A lábjegyzeteket alakítsd `<ref>` formátumra, és a végére `<references />`.
7. A Word-specifikus formázásokat (sorkizárt, behúzás) töröld.

#### Példa: Pandoc kimenet → Wiki

**Pandoc kimenet (nyers):**
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

**Javított wikitext:**
```wiki
== Címsor 1 ==

Ez egy bekezdés.

=== Címsor 2 ===

* Első pont
* Második pont

{| class="wikitable"
! Col1 !! Col2
|-
| A || B
|-
| C || D
|}
```

#### Tippek

- A Pandoc néha `[[wiki]]` linkeket is készít, ha a DOCX-ben hyperlinkek voltak. Ezeket tartsd meg.
- A Word "comments" → MediaWiki "vitalap" (a Wikin a vitalapon beszélgetnek, nem a lapon belül).
- A Word "Track changes" → MediaWiki-ben nincs ilyen, hagyd figyelmen kívül.
- A Word "Table of contents" → automatikusan generálódik a `__TOC__` magic word-del.

---

## 5. Excel (XLSX) → MediaWiki

### Az Excel csak táblázatot és adatot ad, semmi mást.

#### Hogyan nyerd ki a forrást

> **MEGJEGYZÉS:** A felhasználó eszköze: Excel "Save as CSV" vagy `xlsx2csv` parancs,
> vagy Pandoc: `pandoc -f xlsx -t mediawiki`.

#### Wikitext generálás Excel-ből

1. **Minden munkalap → külön táblázat** a wikiben, vagy egyesíthetők egy "nagy" táblázatba.
2. **Az első sor** általában fejléc (`!`).
3. **A cellatípusok** (szám, dátum, pénznem) elvesznek — szövegként kerülnek be.
4. **A képletek eredménye** kerül be, nem a képlet.
5. **A diagramok** NEM kerülnek be — ezt jelezni kell a felhasználónak.
6. **A feltételes formázás** NEM kerül be.
7. **A cellaszínek** NEM kerülnek be (a wikitable nem támogatja cellaszínt natívan).
8. **Az egyesített cellák** néha elvesznek — ellenőrizd a kimenetet.

#### Ajánlott formátum

```wiki
{| class="wikitable sortable"
! Név !! Kor !! Város !! Éves jövedelem
|-
| Anna || 28 || Budapest || 1 200 000 Ft
|-
| Béla || 34 || Debrecen || 1 800 000 Ft
|-
| Cecil || 25 || Pécs || 950 000 Ft
|}
```

#### Tippek

- A számokat a magyar konvenciónak megfelelően formázd (szóköz ezres-elválasztónak, tizedesvessző).
- Ha az Excel-táblázat túl széles (>6 oszlop), fontold meg, hogy több kisebb táblázatra bontod.
- A diagramok pótolhatók `{{Graph:Chart}}` sablonnal (ahol elérhető), vagy leírhatod a táblázatban.

---

## 6. PowerPoint (PPTX) → MediaWiki

### A PowerPoint a slideshow-hoz hasonló, lineáris.

#### Hogyan nyerd ki a forrást

> **MEGJEGYZÉS:** A felhasználó eszköze: PowerPoint "Export as Outline" (csak szöveget ad),
> vagy Pandoc: `pandoc -f pptx -t mediawiki`, vagy manuális másolás.

#### Wikitext generálás PPTX-ből

1. **Minden dia → szekció (`==` címsor).** A dia címe a szekció címe.
2. **A dián belüli szöveges dobozok** a szekció tartalmává válnak (listák vagy bekezdések).
3. **A képek** `[[File:...|thumb|...]]` formátumba kerülnek.
4. **A diagramok** NEM kerülnek be — le kell írni, vagy kihagyni.
5. **Az animációk, átmenetek** elvesznek.
6. **A "speaker notes"** opcionálisan a szekció alá kerülhetnek `{{Note|...}}` formában.

#### Példa: PPTX dia → Wiki szekció

**Forrás (PPTX-ből kinyert):**
```
Dia 1: "A projekt áttekintése"
- 3 fős csapat
- 6 hónapos időtartam
- 50 000 € költségvetés

Dia 2: "Eredmények"
- 1. fázis: kész (100%)
- 2. fázis: folyamatban (75%)
- 3. fázis: tervezés alatt
```

**Eredmény (MediaWiki):**
```wiki
== A projekt áttekintése ==

* 3 fős csapat
* 6 hónapos időtartam
* 50 000 € költségvetés

== Eredmények ==

* '''1. fázis:''' kész (100%)
* '''2. fázis:''' folyamatban (75%)
* '''3. fázis:''' tervezés alatt
```

#### Tippek

- A PowerPointban a "title slide" → a cikk tetejére lead-nek.
- Az "agenda" / "outline" dia → a tartalomjegyzék (automatikus, ne másold be).
- Az "end" / "Q&A" / "kérdések" dia → "Kapcsolódó cikkek" szekció.
- A prezentációk gyakran tartalmaznak céges logókat → ha minden dián ugyanaz a logó, NE
  rakd be minden szekcióba, csak a cikk tetejére.

---

## 7. Közös lépések bármely forrásformátumhoz

### A konverzió előtt

- [ ] A forrás végigolvasása, megértése.
- [ ] A forrás típusának azonosítása (melyik playbook-ot használjuk).
- [ ] A forrás képeinek, táblázatainak, speciális elemeinek azonosítása.
- [ ] Ha bármi nem egyértelmű, kérdezz rá a felhasználónak.

### A konverzió közben

- [ ] A szöveget BETŰRŐL BETŰRE megtartjuk, csak a formátumot változtatjuk.
- [ ] A MediaWiki saját sablonjait használjuk (`{{Note}}`, `{{Tip}}`, stb.), nem saját HTML-t.
- [ ] A táblázatok `class="wikitable"`-tal vannak.
- [ ] A kód `<syntaxhighlight lang="...">`-tal van.
- [ ] A képek `[[File:...|thumb|alt=...]]` formátumban vannak.
- [ ] A kategóriák a lap alján vannak.

### A konverzió után (KÖTELEZŐ)

- [ ] Független ügynök indítása a `references/04-verification-checklist.md` alapján.
- [ ] Az ügynök riportjának feldolgozása.
- [ ] A talált hibák javítása.
- [ ] A végleges wikitext megmutatása a felhasználónak.

---

## 8. Mikor NE konvertálj automatikusan

Vannak helyzetek, amikor a felhasználóval egyeztetni kell, mielőtt konvertálnál:

- **Ha a forrás nyelve nem magyar** — kérdezd meg, hogy fordítsd-e vagy csak a formátumot.
- **Ha a forrás nagyon régi vagy rossz minőségű** — jelezd a felhasználónak, hogy a kimenet
  is korlátos lesz.
- **Ha a forrás sok képet tartalmaz** — a képeket a felhasználónak kell feltöltenie, mielőtt
  a wikitext megjelenhet a wikin.
- **Ha a forrás sok speciális formázást tartalmaz** (saját CSS, JS, scriptek) — ezek
  elvesznek a konverzió során.
- **Ha a forrás jogvédett** — jelezd a felhasználónak, hogy a MediaWiki tartalma általában
  szabad licenc alatt áll (CC BY-SA).

---

Források:
- <https://www.mediawiki.org/wiki/Extension:PandocUltimateConverter>
- <https://www.mediawiki.org/wiki/Extension:ConvertPDF2Wiki>
- <https://www.mediawiki.org/wiki/Extension:Html2Wiki>
- <https://stackoverflow.com/questions/2833263/convert-from-microsoft-word-to-media-wiki-markup-style>
- <https://openwetware.org/wiki/Converting_documents_to_mediawiki_markup>
- <https://pandoc.org/MANUAL.html>
