# 02 — Design minták modern, letisztult MediaWiki cikkekhez

> Források: <https://www.mediawiki.org/wiki/Documentation/Style_guide>,
> <https://www.mediawiki.org/wiki/Skin:Citizen>,
> <https://www.mediawiki.org/wiki/Skin:Timeless>,
> <https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style>

A "modern, letisztult" MediaWiki cikk három pilléren nyugszik:

1. **Tipográfia és hierarchia** — tiszta címsor-szerkezet, következetes listák, helyes escape.
2. **Vizuális ritmus** — egységes táblázat-stílus, kiemelések, dobozok, kártyák.
3. **Reszponzivitás és akadálymentesség** — mobilon is jól olvasható, scope attribútumok,
   alt szövegek, kontrasztos színek.

---

## 1. Az "öt alapelv" — amit MINDIG tarts be

1. **A 2. szintű címsor a maximum** (`==`). Ritkán van szükség 4. szintnél mélyebbre —
   ha igen, gondold át, nem kellene-e két külön cikkre bontani.
2. **Egy fogalom = egy címsor.** Ne duplikálj szekciókat, ne tegyél két dolgot egy
   címsor alá.
3. **Az első bekezdés (lead) összefoglaló.** A felhasználó 5 másodperc alatt eldönti,
   hogy kell-e neki a cikk. Az első 1-2 mondat ezt döntse el.
4. **A táblázat ne legyen layout.** A táblázat adatot mutasson, ne oszlopokra bontson
   egyébként szöveges tartalmat. Ha csak elrendezés kell, használj CSS-t.
5. **Minden képnek legyen alt szövege.** A `[[File:...|alt=...]]` attribútum kötelező.

---

## 2. Tipográfia és hierarchia

### Címsor-szerkezet egy hosszú cikkben

```wiki
== Áttekintés ==

(rövid, 1-2 mondatos összefoglaló)

== Történet ==

### Korai időszak

### Modern korszak

== Jellemzők ==

== Használati esetek ==

== Kapcsolódó cikkek ==

== Források ==

<references />
```

### Címsor-stílus

- A MediaWiki alapértelmezetten nagy és félkövér címsorokat használ — ezt ne próbáld
  meg változtatni TemplateStyles nélkül.
- A címsorok tartalmazzanak utalást a tartalomra: "Használati esetek", ne "Hol használjuk".
- A címsor NE legyen link. A mobil-élmény sérül, ha a címsor link is.
- NE tegyél kétszer ugyanazzal a szöveggel címsort (zavaró a navigációban).

### Bekezdés-szerkezet

- 1-3 mondat / bekezdés. Hosszabb bekezdés = nehezebb olvasni.
- Az első bekezdés tartalmazza a kulcsszavakat (SEO és áttekinthetőség miatt).
- Ha 4+ mondat van egy témáról, bontsd két bekezdésre vagy használj alcímet.

### Sortörés és üres sorok

- Bekezdés = 1 üres sor.
- Sortörés cellán belül: `<br />` (NE `<br>` — bár működik, a XHTML-kompatibilis `<br />`
  a hivatalos).
- Ne tegyél 2+ üres sort egymás után — egyik böngészőben sem okoz extra helyet, csak
  zavaró a forrásban.

---

## 3. Színhasználat és vizuális ritmus

A MediaWiki alapértelmezetten semleges palettát használ:
- szürke háttér a tartalomnak,
- világosabb kék a linkeknek,
- piros a nemlétező linkeknek,
- zöld a meglátogatott linkeknek.

**NE próbáld meg ezt felülírni TemplateStyles nélkül.** Ha mégis, legyél nagyon
konzervatív: 1-2 kiegészítő szín, a többi maradjon a default.

### Ajánlott színpaletta (saját kiemelésekhez)

| Szín | Hex | Hol használd |
|------|-----|--------------|
| Elsődleges kék | `#36c` | linkek, gombok |
| Siker zöld | `#14866d` | "OK" állapot, pipa |
| Figyelmeztető sárga | `#fc3` | "Figyelem" doboz |
| Hiba piros | `#d33` | "Hiba" doboz, kritikus |
| Semleges szürke | `#54595d` | alcímek, kiegészítő szöveg |
| Háttér szürke | `#f8f9fa` | kiemelő dobozok |
| Sötét háttér | `#eaecf0` | táblázat fejléc |

### Színkontraszt szabály

- Világos háttér: `#fff` → szöveg: `#202122` (alapértelmezett)
- Sötét mód: a MediaWiki Vector 2022 skin támogatja, a színek automatikusan váltanak

---

## 4. Kiemelések és dobozok (a legjobb barátod)

A MediaWiki-ben a `{{Note}}`, `{{Tip}}`, `{{Warning}}`, `{{Caution}}` sablonok a
hivatalos kiemelő eszközök. Mindig ezeket használd, NEM saját `<div>`-eket.

### Beépített üzenet-dobozok (Wikipedia/MediaWiki)

```wiki
{{Note|Ez egy információs megjegyzés.}}
{{Tip|Próbáld ki ezt a trükköt.}}
{{Warning|Legyél óvatos, ez adatvesztést okozhat.}}
{{Caution|Kritikus figyelmeztetés!}}
{{Important|Ezt mindenképp olvasd el.}}
```

Ezek a sablonok a Wikipedia-n és a mediawiki.org-on is elérhetők. Ha a cél wikin
nem elérhetők, használd az `{{ambox}}` magot:

```wiki
{{ambox
|type=notice
|text=Egyszerű információs doboz.
}}
```

Típusok: `notice` (kék), `style` (sárga), `content` (sárga-narancs), `warning` (narancs),
`serious` (piros).

### Oldal-szintű üzenetek

```wiki
{{Notice|ez a lap karbantartás alatt áll}}
{{Outdated|2024-01-15}}
{{Historical}}
{{Fixme}}
{{Todo|befejezni a táblázatot}}
{{Update}}
```

### Kiemelés TemplateStyles-szel (saját design)

Ha a wiki tartalmazza a TemplateStyles kiterjesztést, készíthetsz saját dobozt:

```wiki
<templatestyles src="Modul:Highlight/styles.css" />

<div class="hl-primary">
Elsődleges kiemelés
</div>

<div class="hl-warning">
Figyelmeztetés
</div>

<div class="hl-success">
Siker
</div>
```

A `Modul:Highlight/styles.css` tartalma (példa):
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

**Megjegyzés:** TemplateStyles-t a MediaWiki adminnak kell telepítenie. Ha nem elérhető,
a cikk tetején megjegyzésben jelezd.

---

## 5. Kártyás megjelenés (Card layout)

A "modern" dizájn egyik legfontosabb eleme. MediaWiki-ben a kártyákat táblázattal
vagy TemplateStyles-szel készíthetjük.

### Egyszerű kártyák wikitable segítségével

```wiki
{| class="wikitable" style="width: 100%; text-align: center;"
|+ Három szolgáltatásunk
|-
| style="width: 33%; background: #f0f7ff;" |
'''Ingyenes'''
<br />
Mindenki számára elérhető.
| style="width: 33%; background: #e6f4ea;" |
'''Gyors'''
<br />
5 perc alatt beüzemelhető.
| style="width: 33%; background: #fef6e7;" |
'''Biztonságos'''
<br />
SOC 2 tanúsítvánnyal.
|}
```

### Kártyák TemplateStyles-szel (sokkal szebb)

```wiki
<templatestyles src="Modul:Cards/styles.css" />

<div class="card-grid">
  <div class="card">
    <div class="card-title">Ingyenes</div>
    <div class="card-body">Mindenki számára elérhető.</div>
  </div>
  <div class="card">
    <div class="card-title">Gyors</div>
    <div class="card-body">5 perc alatt beüzemelhető.</div>
  </div>
  <div class="card">
    <div class="card-title">Biztonságos</div>
    <div class="card-body">SOC 2 tanúsítvánnyal.</div>
  </div>
</div>
```

A CSS:
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

**Megjegyzés:** a `prefers-color-scheme` támogatás csak a Vector 2022+ és Citizen skin-eken
működik.

---

## 6. Accordion / összecsukható szekciók

### Egyszerű táblázat-alapú accordion

```wiki
{| class="wikitable mw-collapsible mw-collapsed"
! Bővebben...
|-
Részletes tartalom, ami alapértelmezetten rejtve van.
|}
```

### Részletes (mw-collapsible bármely elemre)

```wiki
<div class="mw-collapsible mw-collapsed">
Ez a tartalom alapértelmezetten össze van csukva.
</div>
```

### Gomb-barát accordion (details/summary)

A MediaWiki 1.40+ támogatja a natív `<details>` tag-ot:

```wiki
<details>
<summary>Részletek (kattints a kibontáshoz)</summary>
Rejtett tartalom, ami csak kattintásra jelenik meg.
</details>
```

### Haladó accordion TemplateStyles-szel

```wiki
<templatestyles src="Modul:Accordion/styles.css" />

<details class="accordion">
  <summary>1. lépés: Előkészületek</summary>
  <div class="accordion-body">Első lépés részletei...</div>
</details>
<details class="accordion">
  <summary>2. lépés: Konfiguráció</summary>
  <div class="accordion-body">Második lépés részletei...</div>
</details>
```

---

## 7. Tabok (TabberNeue kiterjesztéssel)

Ha a TabberNeue telepítve van:

```wiki
<tabber>
|-| Első tab =
Ez az első tab tartalma.
|-| Második tab =
Ez a második tab tartalma.
|-| Harmadik tab =
Ez a harmadik tab tartalma.
</tabber>
```

### Tabok TemplateStyles-szel (natív megoldás)

Ha nincs TabberNeue, használhatsz CSS-alapú megoldást is, de a tabok kattinthatósága
JavaScript-et igényel. **Egyszerűbb a TabberNeue, ha van.**

---

## 8. Breadcrumbs (morzsa)

```wiki
{{#breadcrumb:Főoldal|Projekt|Aloldal}}
```

vagy kézzel (ha nincs breadcrumb sablon):

```wiki
[[Főoldal]] > [[Projekt]] > Aloldal
```

A `>` szóközök nélkül használható, mint breadcrumb elválasztó. Egyes skin-eken
(VisualEditor) jobban mutat, ha `›` karaktert használsz.

---

## 9. Navigációs doboz (sidebar template)

```wiki
{{Sidebar
|title=Név
|content=
[[Első lap]]
[[Második lap]]
*[[Al-oldal 1]]
*[[Al-oldal 2]]
*[[Al-oldal 3]]
|content2=
[[Harmadik lap]]
[[Negyedik lap]]
}}
```

A Sidebar template a `Template:Sidebar` néven érhető el a legtöbb wikin.

---

## 10. Infobox

Az Infobox a legjellemzőbb "modern" MediaWiki elem. A legegyszerűbb:

```wiki
{{Infobox
|title=Projekt neve
|image=[[File:Logo.png|180px|center]]
|header1=Alapadatok
|label2=Verzió
|data2=2.0
|label3=Kiadás dátuma
|data3=2025-01-15
|label4=Szerző
|data4=[[Anna Kovács]]
|label5=Licenc
|data5=[[MIT]]
|header6=Linkek
|data6=
*[[Dokumentáció]]
*[[Forráskód]]
*[[Issue tracker]]
}}
```

A legtöbb wikin saját Infobox template van (pl. `Template:Infobox software`,
`Template:Infobox person`). **Mindig a cél wiki saját Infobox-át használd**, ne
generikusat.

---

## 11. Kódblokk design

### Alap szintaxiskiemelés

```wiki
<syntaxhighlight lang="python">
def fibonacci(n: int) -> int:
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)
</syntaxhighlight>
```

### Sorszámozott kód (CSS-sel)

A `<syntaxhighlight>` támogatja a `line` és `highlight` paramétereket:

```wiki
<syntaxhighlight lang="python" line start="1" highlight="2,4">
def hello(name):
    print(f"Hello, {name}!")  # 2. sor kiemelve
    return name
    return None  # 4. sor kiemelve
</syntaxhighlight>
```

### Kód címkével

```wiki
{{Codesample
|lang=python
|code=
def hello():
    return "Hello"
|title=hello.py
}}
```

### Kód + magyarázat (két oszlop)

A legegyszerűbb: táblázat két oszloppal.

```wiki
{| class="wikitable"
! Kód
! Magyarázat
|-
|
<syntaxhighlight lang="python">
def hello():
    return "Hello"
</syntaxhighlight>
|
A <code>hello</code> függvény egy egyszerű üdvözlést ad vissza.
|}
```

### Gomb (CTA) stílusú kódrészlet

```wiki
<div style="background: #f8f9fa; border: 1px solid #eaecf0; border-radius: 6px; padding: 12px 16px; font-family: monospace;">
$ npm install mediawiki-cli
</div>
```

---

## 12. Hivatkozások és lábjegyzetek

### Egyszerű lábjegyzet

```wiki
Ez egy állítás.<ref>{{cite web|url=https://example.com|title=Cím|author=Szerző|date=2025-01-15}}</ref>
```

### Lábjegyzetek listája

```wiki
<references />
```

vagy oszlopokba rendezve:

```wiki
<references responsive="0" columns="2" />
```

### Számozatlan lábjegyzet (egyszerű)

```wiki
Ezt a megközelítést alkalmazzuk.<ref name="forras1">Forrás: Belső dokumentáció, 2025.</ref>

<references />
```

### Többször használt lábjegyzet

```wiki
Első említés.<ref name="smith2020" />
Későbbi említés.<ref name="smith2020" />

<references />
<ref name="smith2020">Smith, John (2020). "Cím". Journal, 12(3), 45-67.</ref>
```

### Ajánlott sablonok

```wiki
{{cite web|url=...|title=...|author=...|date=...}}
{{cite book|last=...|first=...|title=...|year=...|publisher=...|isbn=...}}
{{cite journal|last=...|first=...|title=...|journal=...|volume=...|issue=...|year=...|pages=...}}
{{cite news|title=...|url=...|publisher=...|date=...}}
```

---

## 13. Matematikai formulák

A MediaWiki a TeX szintaxist támogatja (Math kiterjesztés):

```wiki
A Pitagorasz-tétel: <math>a^2 + b^2 = c^2</math>

Egy egyenlet: <math>\sum_{i=1}^{n} i = \frac{n(n+1)}{2}</math>
```

---

## 14. Diagramok, folyamatábrák (Mermaid / Graphviz)

Ha a Mermaid kiterjesztés telepítve van:

```wiki
<mermaid>
graph TD
  A[Kezdés] --> B{Döntés}
  B -->|Igen| C[1. út]
  B -->|Nem| D[2. út]
  C --> E[Vége]
  D --> E
</mermaid>
```

Ha a Graphviz telepítve van:

```wiki
<graphviz>
digraph G {
  A -> B;
  B -> C;
}
</graphviz>
```

**Megjegyzés:** Ellenőrizd, hogy ezek a kiterjesztések elérhetők-e a cél wikin.

---

## 15. Design checklist (mielőtt befejezed a cikket)

Mielőtt a cikket "kész"-nek nyilvánítod, ellenőrizd:

- [ ] Van-e áttekinthető lead (1-2 mondat) a cikk tetején?
- [ ] A címsor-hierarchia logikus (max. 3 szint, ritkán 4)?
- [ ] Minden táblázatnak van `class="wikitable"` attribútuma?
- [ ] A rendezhető táblázatokon van `sortable`?
- [ ] Minden képnek van `alt=` attribútuma?
- [ ] A kód `<syntaxhighlight lang="...">`-tal van, nem `<pre>`-vel?
- [ ] A kiemelések (Note, Tip, Warning) a MediaWiki saját sablonjaival vannak?
- [ ] A kategóriák a lap alján vannak?
- [ ] A lábjegyzetek `<references />` taggel vannak listázva?
- [ ] Nincsenek "== Címsor ==" alatti üres szekciók?
- [ ] Nincsenek 1. szintű (=) címsorok?
- [ ] A táblázatok `{|` után van `|+` caption, ha van felirat?
- [ ] A pipe karakterek escape-elve vannak (`{{!}}` ahol kell)?
- [ ] A HTML entitások helyesek (`&amp;` `&lt;` stb.)?
- [ ] A mobilos nézet is olvasható? (kevés táblázat, sok bekezdés)

---

## 16. Modern MediaWiki skin-ek (összehasonlítás)

| Skin | Erősség | Gyengeség |
|------|---------|-----------|
| Vector 2022 (alap) | modern, letisztult, sötét mód | kevés testreszabható |
| Citizen | gyönyörű, command palette, testreszabható | telepíteni kell |
| Timeless | reszponzív, jól strukturált | elavult érzés |
| Chameleon | Bootstrap alapú, testreszabható | nehézkes beállítás |
| Medik | modern, letisztult | kevéssé elterjedt |

Ha a user a "modern, letisztult" megjelenést kéri, de a wiki alapértelmezetten Vector 2010-et
használ, jelezd, hogy a Citizen vagy a Vector 2022 skin jobb élményt adna.

---

## 17. Mikor NE használj dizájnt

- **Ha a cél wiki engedélyezi a sablonokat, DE nincs TemplateStyles** — ne írj
  bonyolult CSS-t, használj inkább beépített sablonokat (`{{Note}}`, `{{Tip}}`).
- **Ha a cél wiki nagyon régi MediaWiki verziót futtat** — ellenőrizd, hogy a
  `class="wikitable sortable"`, `<syntaxhighlight>`, `<gallery>` támogatott-e.
- **Ha a cél wiki privát/intranet** — valószínűleg nincsenek beépített sablonok,
  mindent saját sablonnal kell megoldani.
- **Ha a felhasználó kimondottan a letisztult, minimalista stílust kéri** — ne
  erőltesd a sok színt és dobozt.

---

Források:
- <https://www.mediawiki.org/wiki/Documentation/Style_guide>
- <https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style>
- <https://www.mediawiki.org/wiki/Skin:Citizen>
- <https://www.mediawiki.org/wiki/Skin:Timeless>
- <https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style/Layout>
- <https://www.mediawiki.org/wiki/Extension:TemplateStyles>
