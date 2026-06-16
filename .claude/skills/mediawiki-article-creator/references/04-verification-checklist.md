# 04 — Verifikációs checklist (a független ügynök feladatleírása)

> Ez a fájl leírja, hogyan kell a független ügynöknek (amit az `Agent` tool indít)
> ellenőriznie a MediaWiki wikitextet a forrással szemben.

---

## Az ügynök indítása

A fő ügynök (aki a wikitextet generálta) a következőképpen indítja a független
ellenőrző ügynököt:

```
Agent tool hívás:
  subagent_type: "general-purpose" (vagy "claude")
  isolation: "worktree" (opcionális, csak ha a wikitext fájlba van írva)
  prompt: |
    Az alábbi feladatod a MediaWiki wikitext konverziójának ellenőrzése.

    ## Forrás (az eredeti formátum)
    <IDE ILLESZD BE A FORRÁST>

    ## Elkészült MediaWiki wikitext
    <IDE ILLESZD BE A WIKITEXTET>

    ## A wikitext forrásfájlja
    <ÚTVONAL A WIKITEXT FÁJLOKHOZ, ha vannak>

    ## Feladatod
    Olvasd el a `references/04-verification-checklist.md` fájlt (a teljes
    checklist-et), és annak alapján készíts egy RÉSZLETES RIPORTOT arról, hogy:

    1. A wikitextBEN a szöveg betűről betűre megegyezik-e a forrás szövegével.
    2. CSAK a formátum konvertálódott-e (nincs tartalmi változtatás).
    3. A design elemei megmaradtak-e, vagy ahol nem lehetett, ott a MediaWiki-s
       megfelelőjük használva van-e.
    4. A MediaWiki szintaxis helyes-e (idézd meg a references/01-syntax-cheatsheet.md-t).

    ## Kritikus szabály
    A szövegeltéréseket (akár EGYETLEN SZÓT IS!) SOHA ne fogadd el.
    Minden eltérést jelents, és idézd be mindkét verziót.

    Készíts egy strukturált riportot:
    - Összefoglaló (pass/fail)
    - Szövegeltérések listája (forrás vs. wikitext, sor-szintű)
    - Formátum/design eltérések listája
    - Szintaxis hibák listája
    - Javaslatok a javításra

    A riport legyen magyar nyelvű.
```

---

## A checklist (amit az ügynök használ)

### 1. Szöveg-egyezés (LEGKRITIKUSABB)

**Cél:** A wikitextBEN a szöveg BETŰRŐL BETŰRE megegyezzen a forrás szövegével.

#### 1.1. Szószintű ellenőrzés

- [ ] A wikitext összes szava megtalálható a forrásban?
- [ ] A forrás összes szava megtalálható a wikitextBEN?
- [ ] Nincs kihagyott szó, mondat, bekezdés?
- [ ] Nincs hozzáadott szó, mondat, bekezdés?
- [ ] Nincs átfogalmazott szövegrész?

#### 1.2. Specifikus szövegelemek

- [ ] Számok (árak, dátumok, százalékok) változatlanok?
- [ ] Tulajdonnevek, cégnevek, terméknevek változatlanok?
- [ ] Linkek szövege változatlan?
- [ ] Képfeliratok (caption) változatlanok?
- [ ] Lábjegyzetek szövege változatlan?
- [ ] Kód (programkód) változatlan (a szintaxiskiemelés NEM számít módosításnak)?

#### 1.3. Specifikus escape-ek

- [ ] Az aposztrófok (') megmaradtak a wikiben is? (`''dőlt''`, `'''félkövér'''`)
- [ ] Az idézőjelek (", ') egységesek?
- [ ] A speciális karakterek (&, <, >) megfelelően vannak escape-elve?
- [ ] A pipe karakterek (|) táblázatokban `{{!}}-vel` vannak escape-elve?

#### 1.4. Kiemelések

- [ ] A forrásban lévő félkövér szövegek a wikiben is `'''...'''` formában vannak?
- [ ] A forrásban lévő dőlt szövegek a wikiben is `''...''` formában vannak?
- [ ] A forrásban lévő aláhúzott szövegek a wikiben is `<u>...</u>` formában vannak?
- [ ] A forrásban lévő áthúzott szövegek a wikiben is `<s>...</s>` formában vannak?

#### 1.5. Szövegkörnyezet

- [ ] A címsorok szövege változatlan?
- [ ] A táblázatok celláinak szövege változatlan?
- [ ] A listák elemeinek szövege változatlan?

**Ha bármelyik pontban eltérést találsz → azonnal PASS-FAIL a teljes riportra.**

---

### 2. Formátum-konverzió (FONTOS, de a szövegnél kisebb prioritás)

**Cél:** A forrás vizuális szerkezete a MediaWiki-ben is megjelenjen, a MediaWiki
saját eszközkészletével.

#### 2.1. Címsorok

- [ ] A forrásban lévő címsorok a wikiben `==`, `===`, `====` formában vannak?
- [ ] A hierarchia megmaradt (1-es szintű = soha, 2-es az alap)?
- [ ] Nincs 1. szintű (`=`) címsor?
- [ ] A címsorok szövege VÁLTOZATLAN (csak a formátum változott)?

#### 2.2. Bekezdések

- [ ] A bekezdések üres sorokkal vannak elválasztva?
- [ ] Nincs összefésült bekezdés?
- [ ] A sortörések ahol kell, `<br />` formában vannak?

#### 2.3. Listák

- [ ] A számozatlan listák `*`-gal kezdődnek?
- [ ] A számozott listák `#`-tel kezdődnek?
- [ ] A nested listáknál nő a `*`/`#` száma?
- [ ] A definíciós listák `;` és `:` formában vannak?

#### 2.4. Táblázatok

- [ ] Minden táblázat `class="wikitable"`-tal van?
- [ ] A fejlécek `!` formában vannak?
- [ ] Az adatcellák `|` formában vannak?
- [ ] A `|+` caption ott van, ahol kell?
- [ ] A `|-` sorok jól vannak elhelyezve?
- [ ] A `colspan`/`rowspan` attribútumok megmaradtak?
- [ ] A cella-igazítás (`style="text-align:..."`) megvan, ahol kell?
- [ ] A táblázat szövege VÁLTOZATLAN?

#### 2.5. Képek

- [ ] A képek `[[File:...|thumb|...]]` formátumban vannak?
- [ ] Az `alt` attribútum megvan, ahol a forrásban volt?
- [ ] A képfelirat (caption) VÁLTOZATLAN?
- [ ] A kép pozíciója (left/right/center) megmaradt, ahol volt?

#### 2.6. Kód

- [ ] A kód `<syntaxhighlight lang="...">` formátumban van?
- [ ] A nyelv (`lang`) attribútum megfelelő (python, javascript, stb.)?
- [ ] A kód szövege VÁLTOZATLAN (a kiemelés nem módosítás)?

#### 2.7. Idézetek

- [ ] A `>`-sorok vagy `<blockquote>` formátumban vannak?
- [ ] Az idézet szövege VÁLTOZATLAN?
- [ ] A forrásmegjelölés megvan, ahol volt?

#### 2.8. Kiemelések, dobozok

- [ ] A forrásban lévő "Note" típusú doboz `{{Note|...}}` formátumban van?
- [ ] A forrásban lévő "Warning" típusú doboz `{{Warning|...}}` formátumban van?
- [ ] A doboz szövege VÁLTOZATLAN?

#### 2.9. Linkek

- [ ] A belső linkek `[[...]]` formában vannak?
- [ ] A külső linkek `[...]` formában vannak?
- [ ] A piped linkek `[[cél|felirat]]` formában vannak?
- [ ] A linkek szövege VÁLTOZATLAN?

#### 2.10. Lábjegyzetek

- [ ] A lábjegyzetek `<ref>...</ref>` formában vannak?
- [ ] A `<references />` tag a lap alján van?
- [ ] A lábjegyzet szövege VÁLTOZATLAN?

---

### 3. Design megőrzés (a MediaWiki képességeihez igazítva)

**Cél:** Ahol a forrás design elemei megmaradhatnak, ott maradjanak meg. Ahol nem,
ott a MediaWiki-s megfelelőjük legyen használva.

#### 3.1. Színek

- [ ] Ha a forrásban konkrét színek voltak, a wikiben is megjelennek (TemplateStyles-szel)?
- [ ] Ha a forrás színpalettája konzisztens volt, a wikiben is az?
- [ ] Ha TemplateStyles NEM elérhető, a színek `style="..."` attribútummal vannak?

#### 3.2. Dobozok, kártyák

- [ ] A forrásban lévő "card" típusú dobozok a wikiben is megvannak (TemplateStyles-szel)?
- [ ] Ha TemplateStyles NEM elérhető, a dobozok `<div style="...">` formában vannak?

#### 3.3. Accordion

- [ ] A forrásban lévő "expand/collapse" elemek a wikiben `mw-collapsible` vagy
      `<details>` formában vannak?

#### 3.4. Galériák

- [ ] Ha a forrásban több kép volt, a wikiben `<gallery>` van?
- [ ] A galéria módja (`mode`) megfelelő (packed-hover, slideshow, stb.)?

---

### 4. MediaWiki szintaxis helyessége

**Cél:** A wikitext szintaktikailag helyes legyen, és a MediaWiki-motor helyesen
renderelje.

#### 4.1. Alap szintaxis

- [ ] Nincs `=` szintű címsor?
- [ ] A pipe karakterek táblázatokban `{{!}}-vel` vannak escape-elve, ahol kell?
- [ ] Az egyenlőségjel címsorokban `{{=}}` formában van escape-elve, ahol kell?
- [ ] A HTML entitások helyesek (`&amp;`, `&lt;`, `&gt;`, stb.)?

#### 4.2. Sablonok

- [ ] A sablonhívások helyes formátumban vannak (`{{név|param=érték}}`)?
- [ ] A sablon paraméterei helyesek?

#### 4.3. Linkek

- [ ] A külső linkek `[URL felirat]` formátumban vannak (szóköz a felirat előtt)?
- [ ] Az e-mail linkek `[mailto:... cím]` formátumban vannak?

#### 4.4. Kategóriák

- [ ] A kategóriák a lap alján vannak?
- [ ] A kategórianevek ékezethelyesek és konzisztensek?

#### 4.5. Magic words

- [ ] A `__TOC__`, `__NOTOC__` magic words helyesen vannak használva?
- [ ] A `{{PAGENAME}}`, `{{CURRENTYEAR}}` stb. magic words helyesek?

---

### 5. Akadálymentesség és reszponzivitás

- [ ] A táblázatok fejléceinek van `scope="col"` attribútuma?
- [ ] A képeknek van `alt` attribútuma?
- [ ] A linkek szövege beszédes (nem "kattints ide")?
- [ ] A kódblokkok mobilon is olvashatók (hosszú sorok esetén `overflow-x: auto`)?
- [ ] A táblázatok mobilon nem törnek szét?

---

## A riport formátuma

A független ügynök által készített riport legyen strukturált, így:

```markdown
# Verifikációs riport

## Összefoglaló
- **Státusz:** PASS / FAIL
- **Szövegeltérések száma:** X
- **Formátum eltérések száma:** Y
- **Szintaxis hibák száma:** Z
- **Összbenyomás:** ...

## 1. Szövegeltérések (KRITIKUS)

### 1.1. Bekezdés X
- **Forrás:** "Ez a pontos szöveg."
- **Wikitext:** "Ez a megváltoztatott szöveg."
- **Sor:** wiki fájl X. sora
- **Javítás:** Cseréld ki a wikitextet a forrás pontos szövegére.

### 1.2. ...

## 2. Formátum eltérések

### 2.1. Táblázat Y
- **Probléma:** A táblázat nem `class="wikitable"`-tal van.
- **Javítás:** Adj `class="wikitable"`-t a táblázathoz.

### 2.2. ...

## 3. Szintaxis hibák

### 3.1. Pipe karakter táblázatban
- **Sor:** wiki X. sora
- **Probléma:** A `|` karakter escape-eletlen.
- **Javítás:** Cseréld `|`-t `{{!}}-re`.

## 4. Design / megjelenés

- A forrásban volt "card" típusú doboz, a wikiben ez...
- ...

## 5. Javaslatok

1. ...
2. ...
```

---

## A fő ügynök teendői a riport után

1. **Ha a riport PASS** → a wikitext végleges, mutasd meg a felhasználónak.
2. **Ha a riport FAIL** (vannak szövegeltérések):
   - Javítsd a szövegeltéréseket a forrás alapján (NE a saját fejedből!).
   - Javítsd a szintaxis hibákat.
   - Javítsd a design eltéréseket, ahol a MediaWiki eszközkészlete engedi.
   - **Indíts ÚJABB független ügynököt** a javítások ellenőrzésére.
   - Ismételd, amíg PASS nem lesz.
3. **Ha a design elemet NEM lehet MediaWiki-ben visszaadni** → jelezd a felhasználónak,
   hogy mi maradt ki, és miért.

---

## Gyakori hibák, amiket az ügynöknek keresnie kell

1. **Stopwords hozzáadása** (pl. "Fontos megjegyezni, hogy..." — ez NEM volt a forrásban).
2. **Címsorok eltávolítása vagy összevonása** (a felhasználó szétszedte, az LLM összevonta).
3. **Számok kerekítése** (12.5% → ~13%).
4. **Pénznemek átváltása** (€4.2M → 4 200 000 Ft).
5. **Nevek sorrendje** (Anna Kovács → Kovács Anna — EZ ELTÉRÉS!).
6. **Idézetek elvesztése** (a forrásban volt "..." idézet, a wikiben nincs).
7. **Linkek szövegének változtatása** ("itt" → "ezen a linken").
8. **Sortörések elvesztése** (a forrásban sortörés volt, a wikiben nincs).
9. **Whitespace változtatás** (a forrásban 2 szóköz volt, a wikiben 1 — ez technikai, NEM
   tartalmi eltérés, de jelezni kell).
10. **A forrás "TODO" megjegyzéseinek eltávolítása** (ezek a wikiben is kellenek).

---

## Mikor KELL a felhasználót megkérdezni

A független ügynök NE döntsön önállóan az alábbi esetekben — jelezze a felhasználónak:

1. **Ha a forrásban lévő design elemet nem lehet MediaWiki-ben visszaadni** (pl. JS-alapú
   animáció, egyedi CSS, amihez TemplateStyles kell).
2. **Ha a forrásban lévő képet a felhasználó nem töltötte fel** a wikire.
3. **Ha a forrásban lévő kategóriák nem egyértelműek** (melyik kategóriába tegye a cikket?).
4. **Ha a forrásban lévő lábjegyzet formátuma nem egyértelmű** (hivatkozás, idézet, stb.).
5. **Ha a forrás nyelve nem magyar**, és a felhasználó nem mondta meg, hogy fordítson-e.

---

Források:
- A MediaWiki szintaxis részletes referencia: `references/01-syntax-cheatsheet.md`
- A MediaWiki design minták: `references/02-design-patterns.md`
- A konverziós playbook-ok: `references/03-conversion-playbooks.md`
