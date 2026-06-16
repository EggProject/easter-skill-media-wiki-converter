# 06 — Design receptek (kész, másolható kódrészletek)

Ez a fájl a leggyakoribb MediaWiki komponensek kész, másolható receptjeit tartalmazza.
Minden recept tartalmazza:
- a célt (mit akarunk elérni),
- a kódot (másolható wikitext),
- a megjegyzéseket (meddig érhető el, testreszabási tippek).

---

## 1. Modern, letisztult táblázat (alapértelmezett)

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

**Megjegyzés:** A `sortable` rendezhetővé teszi, a `mw-collapsible` összecsukhatóvá.
Ha a táblázat kicsi (< 5 sor), hagyd el a `mw-collapsible`-t.

---

## 2. Sávos (zebra) táblázat

```wiki
{| class="wikitable sortable striped"
! Oszlop 1 !! Oszlop 2
|-
| adat 1 || adat 2
|-
| adat 3 || adat 4
|}
```

**Megjegyzés:** A `striped` class a MediaWiki 1.39+ óta elérhető. Régebbi verziókban
használj TemplateStyles-t a sávos megjelenítéshez.

---

## 3. Kiemelő doboz (Note, Tip, Warning, Caution)

```wiki
{{Note|Információs szöveg.}}
{{Tip|Próbáld ki ezt a trükköt.}}
{{Warning|Legyél óvatos, ez adatvesztést okozhat.}}
{{Caution|Kritikus figyelmeztetés!}}
```

**Megjegyzés:** Ezek a sablonok a mediawiki.org-on és a Wikipédián elérhetők. Ha a
cél wikin nem elérhetők, használd az `{{ambox}}` magot (lásd lent).

---

## 4. Általános üzenetdoboz (minden wikin működik)

```wiki
{{ambox
|type=notice
|text=Egyszerű információs doboz.
|image=[[File:Icon-info.svg|40px]]
}}
```

Típusok: `notice` (kék), `style` (sárga), `content` (sárga-narancs), `warning` (narancs),
`serious` (piros).

---

## 5. Saját színű doboz TemplateStyles-szel

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

<div class="hl-error">
Hiba
</div>
```

A `Modul:Highlight/styles.css` tartalma:

```css
.hl-primary {
  background: #eaf3ff;
  border-left: 4px solid #36c;
  padding: 12px 16px;
  border-radius: 4px;
  margin: 12px 0;
  color: #202122;
}
.hl-warning {
  background: #fef6e7;
  border-left: 4px solid #fc3;
  padding: 12px 16px;
  border-radius: 4px;
  margin: 12px 0;
  color: #202122;
}
.hl-success {
  background: #e6f4ea;
  border-left: 4px solid #14866d;
  padding: 12px 16px;
  border-radius: 4px;
  margin: 12px 0;
  color: #202122;
}
.hl-error {
  background: #fee7e6;
  border-left: 4px solid #d33;
  padding: 12px 16px;
  border-radius: 4px;
  margin: 12px 0;
  color: #202122;
}
@media (prefers-color-scheme: dark) {
  .hl-primary   { background: #1a2f4f; border-left-color: #88aaee; }
  .hl-warning   { background: #4a3a00; border-left-color: #ffcb6e; }
  .hl-success   { background: #1d3a2a; border-left-color: #5dd39e; }
  .hl-error     { background: #4a1a1a; border-left-color: #f8a4a4; }
  .hl-primary, .hl-warning, .hl-success, .hl-error { color: #eaecf0; }
}
```

---

## 6. Kártyás grid (3 oszlop, reszponzív)

```wiki
<templatestyles src="Modul:Cards/styles.css" />

<div class="card-grid">
  <div class="card">
    <div class="card-title">Ingyenes</div>
    <div class="card-body">Mindenki számára elérhető alapfunkciók.</div>
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

A `Modul:Cards/styles.css` tartalma:

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

---

## 7. Accordion (összecsukható szekciók)

### Egyszerű (mw-collapsible)

```wiki
{| class="wikitable mw-collapsible mw-collapsed"
! Bővebben...
|-
Részletes tartalom, ami alapértelmezetten rejtve van.
|}
```

### Natív (HTML5 details)

```wiki
<details>
<summary>1. lépés: Előkészületek (kattints a kibontáshoz)</summary>
Első lépés részletei...
</details>

<details>
<summary>2. lépés: Konfiguráció</summary>
Második lépés részletei...
</details>
```

### Sötét dizájnnal (TemplateStyles)

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

A `Modul:Accordion/styles.css`:

```css
.accordion {
  background: #fff;
  border: 1px solid #eaecf0;
  border-radius: 6px;
  margin: 8px 0;
  overflow: hidden;
}
.accordion > summary {
  padding: 12px 16px;
  cursor: pointer;
  font-weight: 600;
  list-style: none;
  user-select: none;
}
.accordion > summary::-webkit-details-marker {
  display: none;
}
.accordion > summary::before {
  content: "▶";
  display: inline-block;
  margin-right: 8px;
  transition: transform 0.2s;
}
.accordion[open] > summary::before {
  transform: rotate(90deg);
}
.accordion-body {
  padding: 12px 16px;
  border-top: 1px solid #eaecf0;
  color: #54595d;
  line-height: 1.5;
}
@media (prefers-color-scheme: dark) {
  .accordion { background: #101418; border-color: #2c2c2c; }
  .accordion-body { color: #c8ccd1; border-top-color: #2c2c2c; }
}
```

---

## 8. Tabok (TabberNeue kiterjesztéssel)

```wiki
<tabber>
|-| Első tab =
Ez az első tab tartalma.

* Lista elem 1
* Lista elem 2
|-| Második tab =
Ez a második tab tartalma.

{| class="wikitable"
! Fejléc !! Adat
|-
| A || 1
|}
|-| Harmadik tab =
Harmadik tab '''félkövér''' szöveggel.
</tabber>
```

**Megjegyzés:** A TabberNeue kiterjesztést telepíteni kell. Ha nincs, használj
TemplateStyles + JavaScript megoldást (lásd: MediaWiki snippets).

---

## 9. Breadcrumbs (morzsa)

### Egyszerű (kézzel)

```wiki
[[Főoldal]] > [[Projekt]] > Aloldal
```

### Sablonnal (ha elérhető)

```wiki
{{#breadcrumb:Főoldal|Projekt|Aloldal}}
```

vagy saját sablon (Template:Breadcrumb):

```wiki
{{Breadcrumb
|link1=Főoldal
|link2=Projekt
|link3=Aktuális oldal
}}
```

---

## 10. Infobox (saját)

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
|label7=Dokumentáció
|data7=[[Dokumentáció|Olvasd el]]
|label8=Forráskód
|data8=[https://github.com/projekt/projekt GitHub]
|label9=Issue tracker
|data9=[https://github.com/projekt/projekt/issues GitHub Issues]
}}
```

**Megjegyzés:** A legtöbb wiken saját Infobox sablon van (pl. `Template:Infobox software`).
Mindig a cél wiki saját Infobox-át használd.

---

## 11. Navigációs doboz (Sidebar)

```wiki
{{Sidebar
|title=Navigáció
|content=
[[Első lap]]
[[Második lap]]
*[[Al-oldal 1]]
*[[Al-oldal 2]]
*[[Al-oldal 3]]
|content2=
[[Harmadik lap]]
[[Negyedik lap]]
|content3=
* [https://example.com Külső link]
}}
```

---

## 12. Modern kódblokk szintaxiskiemeléssel

```wiki
<syntaxhighlight lang="python">
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
</syntaxhighlight>
```

### Sorszámozott kód (kiemelt sorokkal)

```wiki
<syntaxhighlight lang="python" line start="1" highlight="2,4">
def hello(name: str) -> str:
    return f"Hello, {name}!"  # 2. sor kiemelve

def main() -> None:
    print(hello("World"))  # 4. sor kiemelve

main()
</syntaxhighlight>
```

### Kód címkével (külső fájlnév)

```wiki
{{Codesample
|lang=python
|code=
def hello():
    return "Hello"
|title=hello.py
}}
```

---

## 13. Kód + magyarázat (két oszlop)

```wiki
{| class="wikitable"
! Kód
! Magyarázat
|-
|
<syntaxhighlight lang="python">
def hello(name: str) -> str:
    return f"Hello, {name}!"
</syntaxhighlight>
|
A <code>hello</code> függvény egy üdvözlést ad vissza a <var>name</var> paraméter alapján.
A függvény típus annotációval van ellátva, ami segíti a kód olvasását.
|-
|
<syntaxhighlight lang="python">
print(hello("World"))
</syntaxhighlight>
|
A függvény hívása a <code>print</code> függvénnyel együtt a konzolra írja az üdvözlést.
|}
```

---

## 14. Gomb stílusú kódrészlet (terminál kinézet)

```wiki
<div style="background: #1e1e1e; color: #d4d4d4; padding: 12px 16px; border-radius: 6px; font-family: 'Courier New', monospace; margin: 12px 0;">
$ <span style="color: #4ec9b0;">npm</span> install mediawiki-cli<br />
$ <span style="color: #4ec9b0;">mediawiki-cli</span> --version<br />
<span style="color: #608b4e;">mediawiki-cli v2.0.1</span>
</div>
```

---

## 15. Galériák

### Alap galéria (képek rácsban)

```wiki
<gallery caption="Példa galéria" widths="200" heights="150">
File:Kép1.jpg|1. kép
File:Kép2.jpg|2. kép
File:Kép3.jpg|3. kép
</gallery>
```

### Modern, hover-re megjelenő címke

```wiki
<gallery mode="packed-hover" heights="200px" caption="Modern galéria" class="center">
File:Kép1.jpg|Címke 1
File:Kép2.jpg|Címke 2
File:Kép3.jpg|Címke 3
File:Kép4.jpg|Címke 4
</gallery>
```

### Slideshow mód

```wiki
<gallery mode="slideshow" caption="Slideshow">
File:Kép1.jpg|1. dia
File:Kép2.jpg|2. dia
File:Kép3.jpg|3. dia
</gallery>
```

---

## 16. Matematikai képletek

```wiki
A Pitagorasz-tétel: <math>a^2 + b^2 = c^2</math>

Egy egyenlet: <math>\sum_{i=1}^{n} i = \frac{n(n+1)}{2}</math>

Integrál: <math>\int_{0}^{1} x^2 \, dx = \frac{1}{3}</math>

Mátrix: <math>\begin{pmatrix} a & b \\ c & d \end{pmatrix}</math>
```

**Megjegyzés:** A Math kiterjesztést telepíteni kell (általában alapértelmezetten elérhető).

---

## 17. Diagramok (Mermaid kiterjesztéssel)

### Folyamatábra

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

### Szekvencia-diagram

```wiki
<mermaid>
sequenceDiagram
  participant A as Kliens
  participant B as Szerver
  A->>B: Kérés
  B-->>A: Válasz
  A->>B: Új kérés
  B-->>A: Új válasz
</mermaid>
```

### Osztálydiagram

```wiki
<mermaid>
classDiagram
  class Animal {
    +String name
    +int age
    +makeSound()
  }
  class Dog {
    +fetch()
  }
  Animal <|-- Dog
</mermaid>
```

**Megjegyzés:** A Mermaid kiterjesztést telepíteni kell.

---

## 18. Lábjegyzetek

### Egyszerű lábjegyzet

```wiki
Ez egy állítás.<ref>{{cite web|url=https://example.com|title=Cím|author=Szerző|date=2025-01-15}}</ref>
```

### Lista a lap alján

```wiki
<references />
```

vagy oszlopokba rendezve:

```wiki
<references responsive="0" columns="2" />
```

### Többször használt lábjegyzet

```wiki
Első említés.<ref name="smith2020" />
Későbbi említés.<ref name="smith2020" />
Még egy említés.<ref name="smith2020" />

<references />
<ref name="smith2020">Smith, John (2020). "A cikk címe". Journal, 12(3), 45-67.</ref>
```

### Magyar sablonok

```wiki
{{cite web|url=https://example.com|title=Cím|access-date=2025-01-15}}
{{cite book|last=Kovács|first=Anna|title=A könyv címe|year=2024|publisher=Akadémiai|location=Budapest|isbn=978-963-XXX-XXX-X}}
{{cite journal|last=Nagy|first=Béla|title=A cikk|journal=Folyóirat|volume=12|issue=3|year=2024|pages=45-67}}
```

---

## 19. Modern, letisztult cikk fejléc

```wiki
__NOTOC__{{short description|Modern, letisztult MediaWiki cikk példa}}

<div style="text-align: center; padding: 24px; background: linear-gradient(135deg, #36c 0%, #447ff5 100%); color: white; border-radius: 8px; margin-bottom: 24px;">
<div style="font-size: 1.5em; font-weight: 600;">A cikk címe</div>
<div style="margin-top: 8px; opacity: 0.9;">Rövid leírás a cikkről</div>
</div>

== Áttekintés ==

...
```

**Megjegyzés:** Ez a "hero" elem TemplateStyles-szel szebb és reszponzívabb lenne.

---

## 20. Saját TemplateStyles: teljes példa

### A modul: `Modul:Hero/styles.css`

```css
.hero {
  text-align: center;
  padding: 32px 24px;
  background: linear-gradient(135deg, #36c 0%, #447ff5 100%);
  color: white;
  border-radius: 8px;
  margin-bottom: 24px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
.hero-title {
  font-size: 1.75em;
  font-weight: 700;
  margin-bottom: 8px;
  letter-spacing: -0.01em;
}
.hero-subtitle {
  font-size: 1em;
  opacity: 0.9;
  font-weight: 400;
}
@media (prefers-color-scheme: dark) {
  .hero {
    background: linear-gradient(135deg, #1a2f4f 0%, #2c4a7f 100%);
  }
}
@media (max-width: 600px) {
  .hero {
    padding: 24px 16px;
  }
  .hero-title {
    font-size: 1.4em;
  }
}
```

### A használat a lapon

```wiki
<templatestyles src="Modul:Hero/styles.css" />

<div class="hero">
  <div class="hero-title">A cikk címe</div>
  <div class="hero-subtitle">Rövid leírás a cikkről</div>
</div>
```

---

## 21. Teljes, modern cikk sablon (minden fentit egyben)

```wiki
__NOTOC__{{short description|Modern, letisztult MediaWiki cikk sablon}}
<templatestyles src="Modul:Hero/styles.css" />
<templatestyles src="Modul:Cards/styles.css" />

<div class="hero">
  <div class="hero-title">A cikk címe</div>
  <div class="hero-subtitle">Rövid leírás a cikkről</div>
</div>

== Áttekintés ==

Ez a cikk egy rövid áttekintést ad a témáról. Az első bekezdés összefoglalja a
legfontosabb tudnivalókat.

{{Note|Hasznos információ a cikkről.}}

== Három fő szempont ==

<div class="card-grid">
  <div class="card">
    <div class="card-title">1. szempont</div>
    <div class="card-body">Első szempont részletes leírása.</div>
  </div>
  <div class="card">
    <div class="card-title">2. szempont</div>
    <div class="card-body">Második szempont részletes leírása.</div>
  </div>
  <div class="card">
    <div class="card-title">3. szempont</div>
    <div class="card-body">Harmadik szempont részletes leírása.</div>
  </div>
</div>

== Részletes leírás ==

=== Első alfejezet ===

<syntaxhighlight lang="python">
def hello(name: str) -> str:
    return f"Hello, {name}!"
</syntaxhighlight>

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

## 22. Mikor melyik receptet használd

| Ha a felhasználó ezt kéri... | Használd ezt a receptet... |
|------------------------------|----------------------------|
| "készíts egy táblázatot" | 1, 2 |
| "készíts egy dobozt" | 3, 4, 5 |
| "készíts kártyákat" | 6 |
| "készíts összecsukható szekciót" | 7 |
| "készíts tabokat" | 8 |
| "készíts morzsát" | 9 |
| "készíts infoboxot" | 10 |
| "készíts navigációs dobozt" | 11 |
| "készíts kódot" | 12, 13, 14 |
| "készíts galériát" | 15 |
| "készíts matematikai képletet" | 16 |
| "készíts diagramot" | 17 |
| "készíts lábjegyzetet" | 18 |
| "készíts hero fejlécet" | 19, 20 |
| "készíts egy teljes cikket" | 21 |

---

Források:
- A MediaWiki szintaxis: `references/01-syntax-cheatsheet.md`
- A design minták: `references/02-design-patterns.md`
- A haladó funkciók: `references/05-advanced-features.md`
