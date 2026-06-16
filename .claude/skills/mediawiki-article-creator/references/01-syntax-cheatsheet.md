# 01 — MediaWiki szintaxis cheatsheet (teljes, példákkal)

> Források: <https://www.mediawiki.org/wiki/Cheatsheet>,
> <https://en.wikipedia.org/wiki/Help:Cheatsheet>,
> <https://www.mediawiki.org/wiki/Help:Formatting>,
> <https://www.mediawiki.org/wiki/Help:Tables>,
> <https://en.wikipedia.org/wiki/Help:Gallery_tag>,
> <https://www.mediawiki.org/wiki/Help:Extension:ParserFunctions>

Ez a fájl a teljes MediaWiki szintaxis-referencia. Ha a felhasználó bármilyen formázási
kérdést tesz fel, vagy a kimenetben bármilyen MediaWiki elemet kell használni, **innen
dolgozz**. Ne a saját fejedből próbáld megjegyezni — mindig légy konzisztens a MediaWiki
hivatalos szintaxisával.

---

## 1. Szövegformázás (bárhol használható)

| Hatás | Amit beírsz | Eredmény |
|------|-------------|----------|
| Dőlt | `''dőlt''` | *dőlt* |
| Félkövér | `'''félkövér'''` | **félkövér** |
| Félkövér + dőlt | `'''''félkövér és dőlt'''''` | ***félkövér és dőlt*** |
| Áthúzott | `<s>szöveg</s>` | ~~szöveg~~ |
| Aláhúzott | `<u>szöveg</u>` | <u>szöveg</u> |
| Felülíró | `<sup>szöveg</sup>` | szöveg<sup>szöveg</sup> |
| Alulíró | `<sub>szöveg</sub>` | szöveg<sub>szöveg</sub> |
| Kisméretű | `<small>szöveg</small>` | <small>szöveg</small> |
| Nagyméretű | `<big>szöveg</big>` | <big>szöveg</big> |
| Markdown kikapcsolása | `<nowiki>nem ''formázott''</nowiki>` | nem ''formázott'' |
| Sortörés cellán belül | `<br />` vagy `<br>` | (sortörés) |
| Vízszintes elválasztó | `----` (saját sorban) | — — — |
| Soremelés (új bekezdés) | üres sor | új bekezdés |

**Fontos:** a MediaWiki-ben az aposztrófok száma adja a formázást:
- `''x''` = dőlt
- `'''x'''` = félkövér
- `''''x''''` = 4 aposztróf = dőlt (mert párosával működik)
- `'''''x'''''` = 5 aposztróf = félkövér + dőlt

---

## 2. Címsorok (csak sor elején)

```
== 2. szintű címsor ==     (ez a leggyakoribb)
=== 3. szintú címsor ===
==== 4. szintű címsor ====
===== 5. szintű címsor =====
====== 6. szintű címsor ======
```

- Az 1. szintű címsor (`= ... =`) **fenntartott** a lap címének, SOHA ne használd.
- Mindig tegyél üres sort a címsor és a tartalom közé (könnyíti a fordítást).
- A címsorok automatikusan bekerülnek a tartalomjegyzékbe (TOC).
- Ha a tartalomjegyzék nem jelenik meg, írd a lap tetejére: `__TOC__`
- Ha el akarod rejteni: `__NOTOC__`

---

## 3. Listák (csak sor elején)

### Számozatlan (felsorolás)

```
* Alma
* Körte
** Körte - Vilmos
** Körte - Bosc
* Barack
```

Eredmény:
- Alma
- Körte
  - Körte - Vilmos
  - Körte - Bosc
- Barack

### Számozott

```
# Első
# Második
## Második - A alpont
# Harmadik
```

### Definíciós lista

```
; Szerver
: Az a gép, ami kiszolgálja a klienseket.
; Kliens
: Az a gép, ami csatlakozik a szerverhez.
```

Eredmény:

Szerver
: Az a gép, ami kiszolgálja a klienseket.

Kliens
: Az a gép, ami csatlakozik a szerverhez.

### Kevert lista

```
* Gyümölcsök
*# Alma
*# Körte
* Zöldségek
*# Répa
```

### Behúzás beszélgetőoldalakhoz

```
: egyszeres behúzás
:: kétszeres behúzás
::: háromszoros behúzás
```

---

## 4. Linkek

### Belső linkek (wikilink)

```
[[MediaWiki]]                  → MediaWiki (link a MediaWiki lapra)
[[MediaWiki|MediaWiki szoftver]] → MediaWiki szoftver (más felirat)
[[Súgó:Tartalom]]              → Súgó:Tartalom (névtér + lap)
[[#Szintaxis]]                  → #Szintaxis (anchor ugyanazon a lapon)
[[Súgó#Linkek]]                → Súgó#Linkek (anchor másik lapon)
[[Magyarország|hu]]            → hu (rövidített felirat)
```

### Kategórialink (a kategória tartalmát mutatja)

```
[[:Kategória:Formázás]]        → link, nem téve a lapot a kategóriába
[[Category:Formázás]]          → a lapot a kategóriába teszi
[[Category:Formázás|sorted]]   → a "sorted" kulccsal rendezi
```

### Interwiki linkek (Wikimédia projektek)

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

### Külső linkek

```
http://example.com                 → automatikus link
[http://example.com]               → számozott link [1]
[http://example.com Example.com]   → "Example.com" feliratú link
[mailto:user@example.com E-mail]   → e-mail link
```

### Szakasz link (slink)

```
{{slink|Súgó#Linkek}}              → a Súgó#Linkek szakaszra mutat, a lap címével
{{slink|Súgó#Linkek|szöveges}}     → a "szöveges" felirat jelenik meg
```

### Átirányítás

```
#REDIRECT [[Cél lap]]
#REDIRECT [[Cél lap#Szakasz]]
```

### Pípás link trükk

```
[[Cél lap|]]              → a felirat üres, maga a lap neve jelenik meg
[[Cél lap| ]]             → szóköz felirat (kevésbé tiszta, de működik)
```

---

## 5. Képek és fájlok

### Alap szintaxis

```wiki
[[File:Kép.jpg]]
[[File:Kép.jpg|thumb]]
[[File:Kép.jpg|thumb|Címke szöveg]]
[[File:Kép.jpg|right|thumb|220px|Címke]]
[[File:Kép.jpg|left|thumb|Címke]]
[[File:Kép.jpg|center|thumb|Címke]]
[[File:Kép.jpg|border|200px|Címke]]
```

### Méret és elrendezés

| Paraméter | Hatás |
|-----------|-------|
| `|thumb|` | bélyegkép kerettel |
| `|frame|` | keret a kép köré, címkével együtt |
| `|frameless|` | keret nélküli bélyegkép |
| `|border|` | vékony keret |
| `|left` | balra úsztatás |
| `|right` | jobbra úsztatás |
| `|center` | középre |
| `|none` | nincs formázás |
| `|200px` | szélesség 200 pixel |
| `|x200px` | magasság 200 pixel (arányos szélességgel) |
| `|upright` | arányos kicsinyítés a felhasználó mini-beállítása alapján |
| `|upright=0.5` | arányos kicsinyítés 0.5-szeresére |
| `|alt=Leírás` | alternatív szöveg |
| `|link=Cél lap` | a kép egy belső lapra linkeljen |

### Fájl link (link a fájlra, nem a kép megjelenítése)

```wiki
[[:File:Kép.jpg]]         → link a fájl leíró lapjára
[[Media:Kép.jpg]]         → közvetlen link a fájlra
[[File:Kép.jpg|See link]] → a felirat linkeli a fájlt
```

### Példa: képhivatkozás széles körben használt formátuma

```wiki
[[File:Logo.png|thumb|right|220px|A projekt logója|alt=Piros kör, benne fehér "P" betű]]
```

---

## 6. Galériák

### Egyszerű galéria

```wiki
<gallery>
File:Kép1.jpg|Első kép
File:Kép2.jpg|Második kép
File:Kép3.jpg|Harmadik kép
</gallery>
```

### Galéria módok

| Mód | Leírás |
|-----|--------|
| `mode=traditional` (alap) | sorok és oszlopok, keret, címke alul |
| `mode=nolines` | keret nélkül, címke középen |
| `mode=packed` | egyforma magasság, igazított, címke középen |
| `mode=packed-overlay` | címke áttetsző dobozban a kép alján |
| `mode=packed-hover` | címke hover-re jelenik meg (touch-en packed-overlay) |
| `mode=slideshow` | diavetítés |

### Galéria attribútumok

```wiki
<gallery caption="Példa galéria" widths="180px" heights="120px" perrow="3">
File:Kép1.jpg|1. kép
File:Kép2.jpg|alt=alternatív szöveg|2. kép
File:Kép3.jpg|[[Belső link]] a címkében
File:Kép4.jpg|Ez<br />egy<br />többsoros<br />címke
</gallery>
```

| Attribútum | Hatás |
|-----------|-------|
| `caption="..."` | felirat a galéria tetején |
| `widths="180px"` | max szélesség (nincs hatása packed/slideshow módban) |
| `heights="120px"` | max magasság (nincs hatása slideshow módban) |
| `perrow="3"` | képek száma soronként (nincs hatása packed/slideshow módban) |
| `showfilename=yes` | fájlnév megjelenítése a címke alatt |
| `class="center"` | galéria középre (perrow-val nem működik) |
| `class="skin-invert-image"` | sötét módban invertálja a képeket |

### Ajánlott beállítás modern, letisztult megjelenéshez

```wiki
<gallery mode="packed-hover" heights="200px" caption="Modern galéria" class="center">
File:Kép1.jpg|Címke
File:Kép2.jpg|Címke
File:Kép3.jpg|Címke
</gallery>
```

---

## 7. Táblázatok

### Minimális táblázat

```wiki
{|
| Alma
| Körte
|-
| Kenyér
| Pite
|}
```

Eredmény:

| Alma | Körte |
|------|-------|
| Kenyér | Pite |

### Alap szintaxis

| Szimbólum | Funkció |
|-----------|---------|
| `{|` | táblázat kezdete (kötelező) |
| `|+` | felirat (caption) |
| `|-` | új sor (az első sornál elhagyható) |
| `!` | fejléc cella |
| `\|` | adatcella |
| `\|}` | táblázat vége (kötelező) |

### Fejlécek

```wiki
{| class="wikitable"
! Tétel !! Mennyiség !! Ár
|-
| Alma || 10 || 700 Ft
|}
```

### Felirat (caption)

```wiki
{|
|+ Élelmiszer lista
|-
| Alma || 10
|}
```

### Modern, rendezhető táblázat (ajánlott)

```wiki
{| class="wikitable sortable"
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

### Stílus attribútumok

```wiki
{| class="wikitable" style="text-align: center; color: green;"
```

Cellaszintű stílus:
```wiki
| style="text-align:right;" | 12 333,00
```

Sor stílus:
```wiki
|- style="font-style: italic; color: green;"
```

### Sort és oszlopot átívelő cellák

```wiki
{| class="wikitable"
!colspan="6"| Bevásárló lista
|-
|rowspan="2"| Kenyér és vaj
| Pite
| colspan="2"| Croissant
|}
```

### Akadálymentesség

```wiki
! scope="col" | Név
! scope="col" | Kor
! scope="col" | Város
|-
! scope="row" | Anna
| 28 || Budapest
```

### Hasznos class-ok

| Class | Hatás |
|-------|-------|
| `wikitable` | világos háttér, keret, padding (alapértelmezett) |
| `wikitable sortable` | + rendezhető oszlopok |
| `mw-collapsible` | összecsukható táblázat |
| `mw-collapsed` | alapból összecsukva |
| `plainrows` | minimális stílus |
| `wikitable striped` | sávos (zebra) sorok |
| `wikitable hover-highlight` | sor kiemelés hover-re |

### Negatív számok cellakezdésként

❌ HIBÁS: `| -6` (sortörést jelent)
✅ HELYES: `| |-6` vagy `| &minus;6` vagy `| style="text-align:right;" | -6`

### Ajánlott modern táblázat recept

```wiki
{| class="wikitable sortable mw-collapsible"
! Név !! Szerep !! Státusz
|-
| Anna || Fejlesztő || Aktív
|-
| Béla || Tesztelő || Aktív
|-
| Cecil || PM || Szabadságon
|}
```

---

## 8. Kód, kódblokk, gépelés

### Soron belüli kód

```wiki
A <code>git status</code> parancs megmutatja a változásokat.
A <kbd>Ctrl</kbd>+<kbd>C</kbd> billentyűkombináció másol.
A <var>felhasználónév</var> változót cseréld ki.
```

Eredmény:
A <code>git status</code> parancs megmutatja a változásokat.
A <kbd>Ctrl</kbd>+<kbd>C</kbd> billentyűkombináció másol.
A <var>felhasználónév</var> változót cseréld ki.

### Kódblokk szintaxiskiemeléssel (ajánlott)

```wiki
<syntaxhighlight lang="python">
def hello(name: str) -> str:
    return f"Hello, {name}!"

print(hello("World"))
</syntaxhighlight>
```

Támogatott nyelvek: `python`, `javascript`, `java`, `c`, `cpp`, `csharp`, `go`, `rust`,
`ruby`, `php`, `html`, `css`, `scss`, `xml`, `json`, `yaml`, `sql`, `bash`, `sh`, `powershell`,
`markdown`, `diff`, `dockerfile`, `nginx`, `apache`, `ini`, `toml`, `lua`, `perl`, `r`,
`matlab`, `scala`, `kotlin`, `swift`, `typescript`, `jsx`, `tsx`, `vue`, `graphql` és még
több száz (lásd: Pygments lexerek).

### Soron belüli szintaxiskiemelés

```wiki
Használd a <syntaxhighlight lang="css" inline>color: red</syntaxhighlight> szabályt.
```

### Egyszerű kódblokk (kiemelés nélkül)

```wiki
<pre>
ez
  megőrzi
    a behúzást
</pre>
```

### Billentyűkombináció

```wiki
A mentéshez nyomd meg a {{Key press|Ctrl|S}} billentyűkombinációt.
```

### Gomb

```wiki
Kattints a {{Button|Mentés}} gombra.
```

### Rövidítés

```wiki
Az <abbr title="JavaScript">JS</abbr> egy népszerű nyelv.
```

### Kódrészlet ablak (külső fájlnévvel)

```wiki
{{Codesample
|lang=python
|code=print("Hello, World!")
|title=hello.py
}}
```

---

## 9. Idézetek

### Soron belüli idézet

```wiki
Azt mondta: "Hamarosan jövök".
```

### Bekezdés szintű idézet (saját blokk)

```wiki
<blockquote>
Ez egy hosszabb idézet, ami több sort is elfoglalhat.
A MediaWiki-ben a <blockquote> tag használható erre.
</blockquote>
```

### Sablonos idézet

```wiki
{{blockquote
|text=Az élet az, ami akkor történik, míg te más terveket szövögetsz.
|sign=John Lennon
|source=
|width=
}}
```

### Egyéb idéző sablonok

```wiki
{{pull quote|szöveg}}
{{Quotation|szöveg|forrás}}
```

---

## 10. Kiemelések, dobozok, üzenetek

### MediaWiki saját kiemelő sablonjai

```wiki
{{Note|szöveg}}
{{Tip|szöveg}}
{{Warning|szöveg}}
{{Caution|szöveg}}
{{Important|szöveg}}
{{NoteTip|szöveg}}
```

### Általános message-box (MediaWiki core)

```wiki
{{ambox
|type=notice
|text=Információs üzenet
|image=[[File:Icon-info.svg|40px]]
}}
```

Típusok: `notice`, `style`, `content`, `warning`, `serious`. Kinézet a `type` értéktől függ.

### Oldal-szintű üzenetek (lap tetejére)

```wiki
{{Notice|ez a lap karbantartás alatt áll}}
{{Outdated|2024-01-01}}
{{Historical}}
{{Fixme}}
{{Todo|befejezni a táblázatot}}
{{Update}}
```

### Hatnote (lapon belüli link más lapra)

```wiki
{{Main|Fő cikk}}
{{See also|Lásd még|Kapcsolódó cikk}}
{{For|other uses|Lásd|Egyéb jelentés}}
```

### Kiemelés TemplateStyles-szel (saját doboz)

```wiki
<div class="my-highlight-box">
Ez egyedi stílusú doboz, aminek a kinézetét a TemplateStyles határozza meg.
</div>
```

A TemplateStyles használatához a lapon (vagy a tartalmazó sablonban) hivatkozni kell:
```wiki
<templatestyles src="Modul:Highlight box/styles.css" />
```

---

## 11. Sablonok (templates)

### Sablon meghívása

```wiki
{{sablon neve}}
{{sablon neve|paraméter=érték}}
{{sablon neve|első|2|harmadik}}
{{sablon neve|param1=érték1|param2=érték2}}
```

### Feltételes include

```wiki
{{sablon neve|{{{paraméter|alapérték}}}}}
```

### Rekurzió / körkörös hivatkozás

```wiki
{{#invoke:Module:Foo|function_name|arg1|arg2}}
```

### Leggyakoribb hasznos sablonok

- `{{PAGENAME}}` — az aktuális lap neve
- `{{NAMESPACE}}` — az aktuális névtér
- `{{FULLPAGENAME}}` — névtér + lap név
- `{{TALKPAGENAME}}` — a vitalap neve
- `{{REVISIONID}}` — az aktuális revízió azonosítója
- `{{REVISIONUSER}}` — a szerkesztő felhasználóneve
- `{{SITENAME}}` — a wiki neve
- `{{SERVER}}` — a wiki szerver URL-je
- `{{localurl:lapnév}}` — a lap relatív URL-je
- `{{fullurl:lapnév}}` — a lap teljes URL-je
- `{{CURRENTYEAR}}`, `{{CURRENTMONTH}}`, `{{CURRENTDAY}}`, `{{CURRENTTIME}}` — dátum
- `{{NUMBEROFARTICLES}}`, `{{NUMBEROFPAGES}}`, `{{NUMBEROFUSERS}}` — statisztikák
- `{{ns:1}}` — névtér ID → névtér név
- `{{#time: Y-m-d }}` — formázott dátum (lásd lentebb)

---

## 12. Parser functions (ParserFunctions kiterjesztés)

```wiki
{{#if: string | yes | no}}
{{#ifeq: 01 | 1 | equal | not equal}}      → "equal" (numerikus összehasonlítás)
{{#ifexist: Help:Foo | exists | no}}       → "exists" ha a lap létezik
{{#ifexpr: 1 > 0 | yes | no}}               → "yes"
{{#expr: 1 + 1 }}                           → 2
{{#time: Y-m-d }}                           → 2026-06-16
{{#time: c }}                               → ISO 8601
{{#switch: baz | foo = Foo | baz = Baz }}   → "Baz"
{{#rel2abs: ../quok | Help:Foo/bar/baz }}   → Help:Foo/bar/quok
{{#titleparts: Talk:Foo/bar/baz | 2 }}      → Talk:Foo/bar
```

String függvények (opcionális, csak ha `$wgPFEnableStringFunctions = true`):
```wiki
{{#len: string }}
{{#pos: string | keresett }}
{{#rpos: string | keresett }}
{{#sub: string | start | length }}
{{#count: string | keresett }}
{{#replace: string | keresett | csere }}
{{#explode: string | delim | index }}
{{#urldecode: encoded }}
{{#urlencode: text }}
```

Core függvények (kiterjesztés nélkül is):
```wiki
{{lc: AbC}}                                 → "abc"
{{uc: AbC}}                                 → "ABC"
{{lcfirst: AbC}}                            → "abC"
{{ucfirst: abc}}                            → "Abc"
{{anchorencode: AbC dEf}}                   → "AbC_dEf"
{{urlencode: hello world}}                  → "hello+world"
```

---

## 13. Változók és behavior switch-ek

### Date/time változók

```wiki
{{CURRENTYEAR}}                             → 2026
{{CURRENTMONTH}}                            → 06
{{CURRENTMONTHNAME}}                        → június
{{CURRENTMONTHABBREV}}                      → jún.
{{CURRENTDAY}}                              → 16
{{CURRENTDAY2}}                             → 16
{{CURRENTDOW}}                              → 2 (kedd)
{{CURRENTDAYNAME}}                          → kedd
{{CURRENTTIME}}                             → 13:24
{{CURRENTHOUR}}                             → 13
{{CURRENTWEEK}}                             → 24
{{CURRENTTIMESTAMP}}                        → 20260616132456
```

### Statisztikák

```wiki
{{NUMBEROFARTICLES}}                        → 1 234 567
{{NUMBEROFPAGES}}                           → 5 678 901
{{NUMBEROFUSERS}}                           → 12 345
{{NUMBEROFEDITS}}                           → 99 999 999
{{NUMBEROFVIEWS}}                           → 999 999 999
{{NUMBEROFFILES}}                           → 10 000
{{SITENAME}}                                → A Wiki neve
{{SERVER}}                                  → //hu.wikipedia.org
{{SERVERNAME}}                              → hu.wikipedia.org
{{CURRENTVERSION}}                          → MediaWiki verzió
```

### Lap és névtér változók

```wiki
{{PAGENAME}}                                → Cikk neve (névtér nélkül)
{{PAGENAMEE}}                               → URL-encoded PAGENAME
{{FULLPAGENAME}}                            → Névtér:Lap neve
{{NAMESPACE}}                               → Névtér neve
{{SUBJECTPAGENAME}}                         → Alap névtér lap neve
{{TALKPAGENAME}}                            → Vitalap neve
{{ROOTPAGENAME}}                            → Gyökér lap neve (ha /Subpage)
{{BASEPAGENAME}}                            → Alap lap neve
{{SUBPAGENAME}}                             → Alp neve
{{REVISIONID}}                              → 12345678
{{REVISIONUSER}}                            → Szerkesztő
{{REVISIONTIMESTAMP}}                       → 2025-01-15T10:30:00Z
{{PROTECTIONLEVEL}}                         → pl. "autoconfirmed"
{{EDITSECTION}}                             → "edit" linkekhez
```

### Behavior switch-ek (sor elején vagy bárhol)

```wiki
__NOTOC__           → tartalomjegyzék elrejtése
__TOC__             → tartalomjegyzék kényszerítése (a lap tetejére)
__FORCETOC__        → tartalomjegyzék kényszerítése (bárhol megjelenhet)
__NOEDITSECTION__   → szekció-szerkesztés tiltása
__NEWSECTIONLINK__  → "+" gomb a szekció hozzáadásához
__NONEWSECTIONLINK__→ "+" gomb elrejtése
__NOGALLERY__       → automatikus képgaléria kikapcsolása (kategóriáknál)
__HIDDENCAT__       → kategória elrejtése a kategória-lapokról
```

### Speciális karakterek

```wiki
&amp;    → &     (ampersand)
&lt;     → <     (kisebb mint)
&gt;     → >     (nagyobb mint)
&nbsp;   → (nem törhető szóköz)
&ndash;  → –     (en dash)
&mdash;  → —     (em dash)
&hellip; → …     (három pont)
&laquo;  → «     (bal francia idézőjel)
&raquo;  → »     (jobb francia idézőjel)
&copy;   → ©
&reg;    → ®
&trade;  → ™
```

HTML entitás: `&#NNN;` (decimális) vagy `&#xHH;` (hexadecimális).

---

## 14. Aláírások, pingek

```wiki
~~~      → felhasználónév
~~~~     → felhasználónév + időbélyeg
~~~~~    → csak időbélyeg
{{u|User|szöveg}}    → ping (link a userre)
{{ping|User1|User2}} → több user pingelése
```

---

## 15. Kategóriák és rejtett megjegyzések

### Kategóriák (a lap aljára)

```wiki
[[Category:Formázás]]
[[Category:2025-es események]]
[[Category:Szoftverfejlesztés|sorted]]
[[:Category:Rejtett]]                  → link, nem téve a lapot
[[Category:Formázás| ]]                → space = a kategória neve
```

### Rejtett kategória

```wiki
[[Category:Rejtett kategória|__HIDDENCAT__]]
```

vagy

```wiki
__HIDDENCAT__
[[Category:Rejtett kategória]]
```

### Megjegyzések (a kimenetben nem jelennek meg)

```wiki
<!-- Ez egy megjegyzés, nem jelenik meg a renderelt oldalon -->
```

Többsoros megjegyzés is lehetséges:
```wiki
<!--
Több
sor
megy
-->
```

---

## 16. Átirányítások, átnevezések

```wiki
#REDIRECT [[Cél lap]]
#REDIRECT [[Cél lap#Szakasz]]
#REDIRECT [[Category:Kategória név]]    → kategória átirányítás
#REDIRECT [[File:Fájl.jpg]]             → fájl átirányítás
```

### Lapátnevezés varázsszó (törlés + átirányítás)

```wiki
{{Db-redirect|comment=...}}
```

---

## 17. Fájlok feltöltése, beágyazása

```wiki
[[File:Pelda.png|thumb|180px|jobbra|Képaláírás]]
[[File:Pelda.pdf|thumb|A PDF dokumentum]]
[[File:Pelda.svg|thumb|Az SVG ábra]]
[[File:Pelda.ogv|thumb|Az OGG videó]]
[[Media:Pelda.zip|Letölthető ZIP]]     → letöltési link
```

### Média lejátszó

```wiki
[[File:Pelda.ogv|thumb|A videó]]
[[File:Pelda.ogg|Az audió]]
[[File:Pelda.webm|A WebM videó]]
```

A média lejátszó automatikus, ha a kiterjesztés támogatott (ogg, ogv, webm, mp3, stb.).

---

## 18. Kategóriák, lapok, képek automatikus listázása

```wiki
{{CategoryTree|Kategória neve|depth=3}}     → kategória fa
{{Pages with prefix|Lap előtag}}              → lapok listája előtag szerint
{{PAGESINCAT:Kategória neve}}                 → kategóriában lévő lapok száma
{{PAGESINCATEGORY:Kategória neve}}            → ua.
{{FORMATNUM:1234567}}                          → formázott szám
```

### File lista kategóriában

```wiki
<gallery>
File:Kep1.jpg
File:Kep2.jpg
</gallery>
```

vagy a kategória összes fájlja (egyes kiterjesztésekkel):
```wiki
{{#categorytree:Fájlok|showcount|depth=2}}
```

---

## 19. Rekurzív sablonok és tartalom beillesztés

```wiki
{{:Másik lap}}                → egy másik lap tartalmának beillesztése
{{Másik lap}}                  → sablon beillesztés
{{subst:Másik lap}}            → a sablon szövegének behelyettesítése mentéskor
{{safesubst:Másik lap}}        → ua., de biztonságosabb
{{msgnw:Másik lap}}            → sablon nélkül, nyers wiki szövegként
```

### Transclusion (lap tartalmának beágyazása)

```wiki
{{:Súgó:Tartalom}}            → a Súgó:Tartalom lap teljes tartalmát ide másolja
{{:Súgó:Tartalom|Section}}     → csak egy szekciót másol
```

---

## 20. Függőleges igazítás, vízszintes vonalak, layout

### Vízszintes vonal

```wiki
----
```

(Bármely sorban elhelyezhető, de a sor elején a legbiztonságosabb.)

### Lebegő doboz (float)

```wiki
<div style="float:right; width:300px; margin-left:10px; padding:10px; background:#f9f9f9; border:1px solid #ddd;">
Ez a doboz jobbra lebeg.
</div>
```

### Több oszlop (kiterjesztéssel vagy CSS-sel)

```wiki
<div style="column-count: 2; column-gap: 20px;">
Első oszlop tartalma.
Második oszlop tartalma.
</div>
```

vagy a `{{div col}}` / `{{div col end}}` sablonnal (ahol elérhető):
```wiki
{{div col|2}}
Első oszlop
{{div col end}}
```

---

## 21. Bővítmények, amiket érdemes ismerni (csak ha telepítve vannak)

| Kiterjesztés | Funkció |
|--------------|---------|
| SyntaxHighlight | Kódkiemelés (Pygments) |
| TemplateStyles | Egyedi CSS laponként |
| ParserFunctions | `{{#if}}`, `{{#switch}}`, `{{#expr}}`, stb. |
| Scribunto | Lua modulok (Module: névtér) |
| Variables | `{{#var:}}` lokális változók |
| Cargo | Strukturált adat tárolás |
| VisualEditor | WYSIWYG szerkesztő |
| TabberNeue | Tab fülek |
| MultimediaViewer | Kép nagyítás |
| PageImages | Lap kiemelt képe |
| InputBox | Gyors lap-létrehozás doboz |
| Replace Text | Szöveg cseréje kategóriában |
| CategoryTree | Kategória fa |
| CollapsibleVector | Összecsukható szekciók |
| Header Tabs | Fejléc tabok |
| Page Forms | Űrlap alapú szerkesztés |
| Semantic MediaWiki | Szemantikus adatok |
| Math | LaTeX képletek (`<math>`) |
| Poem | Versformázás |
| Arrays | Tömb kezelés |
| Maps | Térkép embed |

**Megjegyzés:** Mindig ellenőrizd, hogy az adott bővítmény telepítve van-e a cél wikin.
Ha nem, használj natív MediaWiki alternatívát, vagy jelezd a usernek.

---

## 22. Két fontos, gyakran elfelejtett dolog

### Helyes escape

```wiki
{{!}}       → |       (pipe escape, táblázatban)
{{=}}       → =       (egyenlőségjel escape, fejlécben)
{{!!}}      → ||      (két pipe)
{{!}}-      → |-      (sorvége)
```

Példa: táblázatban, ha a cella szövege `|` karaktert tartalmaz:
```wiki
{| class="wikitable"
| A vagy B {{!}} C
|}
```

### HTML5 deaktiválás

```wiki
<templatedata>
{
  "description": "Sablon leírás",
  "params": {
    "nev": { "type": "string", "label": "Név" }
  }
}
</templatedata>
```

---

## 23. Példa: teljes, modern cikk

```wiki
__NOTOC__{{short description|Modern, letisztult MediaWiki cikk példa}}
{{#time: Y | now }}.

== Áttekintés ==

Ez a cikk egy rövid áttekintést ad a [[Fő téma|Fő témáról]]. A téma fontos, és a
következő szekciókban részletesen is tárgyaljuk.

== Alapfogalmak ==

; Első fogalom
: Az első fogalom magyarázata, ami segít a téma megértésében.

; Második fogalom
: A második fogalom magyarázata.

== Példa táblázat ==

{| class="wikitable sortable mw-collapsible"
! Oszlop 1 !! Oszlop 2 !! Oszlop 3
|-
| adat A1 || adat B1 || adat C1
|-
| adat A2 || adat B2 || adat C2
|}

== Példa kód ==

<syntaxhighlight lang="python">
def hello(name: str) -> str:
    return f"Hello, {name}!"

print(hello("World"))
</syntaxhighlight>

== Példa galéria ==

<gallery mode="packed-hover" heights="180px" caption="Példa galéria" class="center">
File:Example.jpg|1. kép
File:Example.jpg|2. kép
File:Example.jpg|3. kép
</gallery>

== Források ==

<references />

[[Category:Példák]][[Category:Dokumentáció]]
```

---

## 24. Legjobb gyakorlatok (TL;DR)

1. **Mindig `<syntaxhighlight>` a kódblokkokhoz**, ne `<pre>`. A szintaxiskiemelés
   sokat dob az olvashatóságon.
2. **Mindig `class="wikitable"` a táblázatokhoz**, ha nincs különösebb design ok.
3. **Sortable táblázatot használj, ha az adatok rendezhetők** (`class="wikitable sortable"`).
4. **A `mode="packed-hover"` a legszebb galéria** a legtöbb esetben.
5. **A pipe karaktert `{{!}}`-vel escape-eld** táblázatban.
6. **Sose használj `=` szintű címsort** (a 2. szint a minimum: `==`).
7. **Kategóriák a lap aljára**, egyesével vagy egy sorban.
8. **Linkeléskor a `[[Cél|Felirat]]` formát** preferáld, ha a cél lap neve hosszú.
9. **A `__TOC__` magic word-öt** ritkán kell használni (alapértelmezetten a 4. címsor
   után jelenik meg), de ha kell, a lap tetejére tedd.
10. **A `<templatestyles>` tag-et** ritkán kell beírnod közvetlenül — a sablonok
    általában tartalmazzák, és a szerkesztő automatikusan betölti.

---

Források:
- <https://www.mediawiki.org/wiki/Cheatsheet>
- <https://www.mediawiki.org/wiki/Documentation/Style_guide>
- <https://www.mediawiki.org/wiki/Help:Formatting>
- <https://www.mediawiki.org/wiki/Help:Tables>
- <https://en.wikipedia.org/wiki/Help:Cheatsheet>
- <https://en.wikipedia.org/wiki/Help:Advanced_table_features>
- <https://en.wikipedia.org/wiki/Help:Gallery_tag>
- <https://www.mediawiki.org/wiki/Help:Extension:ParserFunctions>
- <https://www.mediawiki.org/wiki/Help:Magic_words>
