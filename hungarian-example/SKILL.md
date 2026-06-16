---
name: mediawiki-article-creator
description: |
  Modern, letisztult designú MediaWiki cikkek készítése és más formátumok (HTML riport,
  slideshow, PDF, Word, Excel, PowerPoint) MediaWiki wikitextté konvertálása.

  HASZNÁLD EZT A SKILLT, amikor a user:
  - MediaWiki cikket, sablont, leírást, táblázatot vagy galériát akar írni
  - "wiki szintaxist", "wikitextet" vagy "mediawiki markupot" kér
  - más formátumot (PDF, Word, Excel, PPT, HTML, markdown, rich text) akar MediaWiki-be vinni
  - a MediaWiki designról kérdez (kártyák, dobozok, kódblokkok, tabok, infobox, sidebar)
  - MediaWiki design tippeket, layout recepteket vagy skin beállítást kér
  - a Wikipedia-stílusú cikkek struktúrájáról vagy konvencióiról kérdez

  Ne használd sima Markdown vagy HTML kimenethez (nem MediaWiki).

  Két workflow:
  1. Párhuzamos, iteratív készítés: az LLM a kódot mutatja, a user egyeztet.
  2. Formátum-konverzió: megőrzi a designt a MediaWiki eszközkészletével; minden konverziót
     külön ügynökkel KELL ellenőriztetni, hogy a szöveget nem írta-e át.
license: MIT
compatibility: Designed for Claude Code (or similar products).
metadata:
  author: eggp
  version: 1.0.0
---

# MediaWiki Article Creator

Ez a skill modern, letisztult MediaWiki cikkek készítésére és más formátumok MediaWiki-be
történő konvertálására szolgál.

A munkát **mindig** két fázisra bontjuk:

1. **Első fázis: wikitext generálása.** A forrás (szabad beszélgetés, PDF, Word, HTML stb.)
   alapján elkészül a MediaWiki wikitext, a design és a stílus a MediaWiki eszközkészletéhez
   igazítva. Lásd: `references/01-syntax-cheatsheet.md`, `references/02-design-patterns.md`,
   `references/05-advanced-features.md`, `references/06-design-recipes.md`.
2. **Második fázis: dupla ellenőrzés.** Egy **külön ügynök** (a `Agent` tool-lal, isolation:
   `worktree` opcióval, ha a fájlok írva vannak) lefordítja a wikitextet renderelt HTML-be
   (vagy szintaxis-ellenőrzéssel megnézi) és összeveti az eredetivel. CSAK formátumot
   konvertálunk, a szöveget nem írjuk át. Lásd: `references/04-verification-checklist.md`.

> A konverziós workflow-ban SOHA ne módosítsd a szöveget, csak a formátumot és a vizuális
> szerkezetet. Ha egy mondatot tömörebbnek, szebben hangzóvnak találsz, ne nyúlj bele — ez
> adatvesztés. A duplázó ügynök feladata ezt kiszúrni.

---

## Workflow 1 — Párhuzamos, iteratív cikk-készítés szabad beszélgetésből

### Lépések

1. **Cél és közönség tisztázása.** Mielőtt bármit írnál, kérdezd meg (vagy erősítsd meg) a
   következőket:
   - Mi a cikk célja? (dokumentáció, leírás, referencia, tutorial, projekt-jelentés, stb.)
   - Kik fogják olvasni? (fejlesztők, belsősök, nagyközönség, szakértők)
   - Milyen hosszú legyen? (egy-két szekció, vagy hosszú, strukturált oldal)
   - Kell-e infobox, táblázat, kód, kép, galéria?
2. **Vázlat.** Rövid, számozott vázlatot javasolj a cikk szekcióiról, a MediaWiki stílusnak
   megfelelően: áttekintés (lead), tartalomjegyzék (automatikus), szekciók, források, kategóriák.
3. **Párhuzamos kódírás.** A user beleegyezése után a teljes cikket egyszerre, egy blokkban
   készítsd el — ne told-foldd részenként. Így a felhasználó az egész cikket egyszerre látja.
4. **Design-tippek alkalmazása.** Olvasd el a `references/02-design-patterns.md` fájlt, és
   alkalmazd a modern letisztult design elveit:
   - Egyszerű, tiszta címsor-hierarchia (==, ===, ====)
   - `class="wikitable"` a táblázatokon
   - `class="wikitable sortable"` ha rendezhető kell
   - `<gallery mode="packed-hover">` ha képeket akarsz mutatni
   - `<syntaxhighlight lang="...">` a kódblokkokhoz
   - `{{Note}}`, `{{Tip}}`, `{{Warning}}` a kiemelésekhez (a MediaWiki saját sablonjai)
   - `__TOC__` a tartalomjegyzékhez, ha automatikusan nem jelenik meg
   - Kategóriák a lap alján: `[[Category:...]]`
5. **Iteráció.** A user visszajelzései alapján módosíts. Minden iterációban mutasd a **teljes
   frissített wikitextet**, ne csak a diffet — a felhasználó a teljes képet akarja látni.
6. **Végső csomag.** Ha kész, ajánld fel:
   - `{{Infobox}}` vagy infobox-sablon kiemeléséhez
   - A cikk felépítéséhez illő kategóriák hozzáadását
   - A belső linkek (`[[Másik lap]]`) ellenőrzését

### Példa nyitó kérdésre

> "Kezdjük azzal, hogy megbeszéljük: mi a cikk célja, kik fogják olvasni, és milyen hosszú
> legyen? Ha van kedvenc referenciád (pl. egy meglévő MediaWiki oldal, ami tetszik), másold
> be a linkjét, és abból dolgozunk."

---

## Workflow 2 — Formátum konverzió MediaWiki-be

### Általános szabályok

- **SOHA ne módosítsd a szöveget.** Csak a formátumot és a vizuális szerkezetet alakítod át.
- **Dizájn megőrzése.** Ha a forrásban van adott design (színek, dobozok, táblázatok, ikonok),
  azt próbáld meg a MediaWiki eszközkészletével (TemplateStyles, wikitable, gallery, syntax
  highlight) visszaadni.
- **Két fázis, mindig.** Az első fázis a wikitext generálása, a második fázis a független
  ügynökkel való ellenőrzés.
- **Forrásmegjelölés.** A kész cikk tetejére tegyél egy `{{Forrás|...}}` vagy hasonló
  sablont, jelezve, honnan származik (opcionális, a user kérésére).

### Támogatott forrásformátumok

Az egyes formátumok részletes konverziós útmutatója: `references/03-conversion-playbooks.md`.

1. **HTML riport** (kézzel írt vagy eszközzel generált): a leggazdagabb forrás. A
   `class` attribútumok, `<table>`, `<pre>`, `<code>`, `<blockquote>`, `<h1>–h6>` elemek
   szinte 1:1-re leképezhetők. Lásd a `03-conversion-playbooks.md` "HTML riport" szekcióját.
2. **HTML slideshow** (reveal.js, Swiper, stb.): a diák MediaWiki szekciókká alakíthatók,
   a progress-bar kikerül, ahol lehet, `__TOC__`-t használunk helyette.
3. **PDF**: kicsit korlátosabb, mert a layout-ot elveszítjük. Szöveg + táblázat + kép
   általában kinyerhető. A pontos teendők: lásd a `03-conversion-playbooks.md` "PDF" szekcióját.
4. **Word (DOCX)**: az Open Office "Export as MediaWiki" funkciója vagy pandoc használható
   alapnak, de a kimeneten mindig manuálisan javítani kell a MediaWiki-specifikus dolgokat
   (TemplateStyles, gallery, syntaxhighlight, kategóriák).
5. **Excel (XLSX)**: csak a táblázatot és a cellaszövegeket nyerjük ki. A diagramok, feltételes
   formázás, cella-színek elvesznek. A kinyert táblázatot `class="wikitable sortable"` formában
   tesszük be.
6. **PowerPoint (PPTX)**: a diák címsorokká (== Slide title ==), a tartalmi blokkok
   listákká, a képek `[[File:...|thumb]]` formátumba kerülnek. A diánkénti animációk és
   átmenetek elvesznek — ezt jelezni kell a usernek.

### A konverzió 7 lépése

1. **Forrás átolvasása.** Olvasd el a forrást VÉGIG, mielőtt bármit írnál. Ha a forrás
   hiányos, kérdezz rá.
2. **Stratégia felállítása.** Döntsd el, hogy a forrás mely elemeit hogyan képezed le:
   - Címek → `==`/`===`
   - Bekezdések → szöveg
   - Listák → `*` / `#`
   - Táblázatok → `class="wikitable"`
   - Képek → `[[File:...|thumb|...]]`
   - Kód → `<syntaxhighlight lang="...">`
   - Idézetek → `{{blockquote|...}}` vagy `>`
   - Dobozok, kiemelések → `{{Note}}`, `{{Tip}}`, `{{Warning}}`, `{{Caution}}`
3. **Wikitext generálása.** Egyben, teljes cikk formájában. NE aprózd darabokra — a user az
   egész cikket akarja látni.
4. **Független ügynök indítása.** Add át a forrást + az elkészült wikitextet egy
   `Agent`-nek (lehetőleg `isolation: "worktree"`-vel, ha írhatóak a fájlok). A
   `references/04-verification-checklist.md` alapján ellenőriztesse vele:
   - A szöveg tartalma betűről betűre megegyezik-e
   - Csak a formátum konvertálódott-e
   - A design elemei megmaradtak-e, vagy ahol nem lehetett, ott a MediaWiki-s megfelelőjük
     van-e használva
5. **Visszajelzés feldolgozása.** Ha az ügynök eltérést talál:
   - **Szövegeltérés** (akár egyetlen szó is!) → azonnal javítsd, a user beleegyezése nélkül
   - **Formátum/design eltérés** → ha van jobb MediaWiki-s megoldás, alkalmazd; ha nincs,
     jelezd a usernek, hogy mi maradt ki és miért
6. **Végleges wikitext.** A javítások után mutasd a teljes végleges wikitextet a usernek.
7. **Forrásmegjelölés.** Ha a user kéri, tegyél `{{Forrás|...}}` vagy hasonló sablont a
   cikk tetejére.

### Példa: HTML riport konverzió

Forrás (HTML):
```html
<h1>Q4 2025 Elemzés</h1>
<p>Ez a jelentés a Q4 2025-ös eredményeket foglalja össze.</p>
<table class="data">
  <thead><tr><th>Metrika</th><th>Érték</th></tr></thead>
  <tbody>
    <tr><td>Bevétel</td><td>€4.2M</td></tr>
    <tr><td>Növekedés</td><td>+18%</td></tr>
  </tbody>
</table>
<p><strong>Konklúzió:</strong> A Q4 erős negyedév volt.</p>
```

Eredmény (wikitext):
```wiki
== Q4 2025 Elemzés ==

Ez a jelentés a Q4 2025-ös eredményeket foglalja össze.

{| class="wikitable sortable"
! Metrika !! Érték
|-
| Bevétel || €4.2M
|-
| Növekedés || +18%
|-
| colspan="2" | <small>Forrás: belső pénzügyi riport, 2026-01-15</small>
|}

'''Konklúzió:''' A Q4 erős negyedév volt.

[[Category:Jelentések]][[Category:2025]]
```

---

## Dupla ellenőrzés: mikor KÖTELEZŐ és hogyan

**MINDIG** indíts független ügynököt (az `Agent` tool segítségével), ha:

- A user más formátumból kér konverziót (Workflow 2)
- A wikitext hosszabb, mint ~30 sor
- A user kéri a "duplaellenőrzést"
- Kétség merül fel, hogy a szöveget nem írtuk-e át

A független ügynök felé ezt az üzenetet küldd:

> "Ellenőrizd az alábbi MediaWiki wikitextet a forrással szemben. CSAK a formátumot
> szabad konvertálni — a szövegnek betűről betűre meg kell egyeznie. Olvasd el a
> `references/04-verification-checklist.md` fájlt, és annak alapján készíts egy
> részletes riportot. A szövegeltéréseket (akár egyetlen szó is!) SOHA ne fogadd el:
> minden eltérést jelents."

A független ügynököt mindig a `general-purpose` (vagy `claude`) subagent-típussal indítsd,
és add át neki a `references/04-verification-checklist.md` teljes tartalmát a feladatkiadásnál.

---

## Hivatkozott referencia fájlok (mindig olvasd el, ha a téma releváns)

- `references/01-syntax-cheatsheet.md` — Teljes MediaWiki szintaxis referencia, példákkal
  (formázás, címsorok, listák, linkek, képek, táblázatok, kód, kategóriák, magic words).
- `references/02-design-patterns.md` — Modern, letisztult design minták MediaWiki-hez
  (tipográfia, színek, dobozok, kártyák, szekció-elválasztók, kiemelések).
- `references/03-conversion-playbooks.md` — Lépésről-lépésre útmutatók HTML riport,
  HTML slideshow, PDF, Word, Excel, PowerPoint → MediaWiki konverzióhoz.
- `references/04-verification-checklist.md` — Az a checklist, amit a független
  ügynök használ a wikitext ellenőrzéséhez.
- `references/05-advanced-features.md` — Haladó MediaWiki funkciók: TemplateStyles,
  ParserFunctions, magic words, Scribunto/Lua, kiterjesztések (Infobox, Sidebar, Tabber).
- `references/06-design-recipes.md` — Kész, másolható kódrészletek: infobox, navigációs
  doboz, breadcrumbs, accordion, tabber, kódszintaxis-kiemelés, slideshow, accordion,
  kategória-lista, stb.

---

## Stílus- és viselkedési szabályok

- **Nyelv.** A kommunikáció a felhasználó nyelvén történik. Ne erőltess a felhasználóra
  más nyelvet, mint amit ő használ. A kód és a technikai kifejezések maradjanak az
  eredeti formájukban.
- **Kódblokk használata.** Minden wikitext output ` ```wiki ` blokkban legyen (NE sima
  markdown ` ``` `), hogy a user egyből be tudja másolni a MediaWiki szerkesztőbe.
- **Teljes cikk, ne részletek.** Ha a user nem kér konkrét részt, az egész cikket mutasd.
- **Iteratív visszajelzés.** Mindig kérdezz rá a végén, hogy mi a következő lépés:
  módosítás, más design, más szekció, kész?
- **Lépcsős megközelítés.** Workflow 1 esetén: cél → vázlat → wikitext → visszajelzés.
  Workflow 2 esetén: forrás → stratégia → wikitext → független ellenőrzés → végleges.
- **NE használj médiawiki-specifikus bővítményeket, amik nem biztos, hogy elérhetők.**
  Ha TemplateStyles-t, Scribunto-t, vagy más kiterjesztést használsz, a cikk tetején
  egy megjegyzésben jelezd, hogy ezeket a MediaWiki adminnak telepítenie kell.
- **A user nevében soha ne commitolj, ne pusholj, ne töröld a meglévő fájlokat.** A skill
  célja a wikitext előállítása, a feltöltés a user dolga.
