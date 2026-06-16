# MediaWiki Article Creator — Claude Code Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code Skill](https://img.shields.io/badge/Claude_Code-Skill-blueviolet)](https://docs.claude.com/en/docs/claude-code/skills)
[![Skill Format: SKILL.md](https://img.shields.io/badge/Format-SKILL.md-success)](https://agentskills.io/specification)
[![Wikitext](https://img.shields.io/badge/Output-MediaWiki_wikitext-36c)](https://www.mediawiki.org/wiki/Wikitext)
[![English](https://img.shields.io/badge/Docs-English-blue)](README.md)
[![Magyar](https://img.shields.io/badge/Docs-Magyar-green)](README.hu.md)

> Egy Claude Code skill, amellyel modern, letisztult designú MediaWiki cikkeket
> készíthetsz, valamint más formátumokat (HTML riport, HTML slideshow, PDF, Word,
> Excel, PowerPoint) MediaWiki wikitextté konvertálhatsz — független ügynök által
> végzett dupla ellenőrzéssel, amely garantálja, hogy a szöveget soha nem írjuk
> át, csak a formátumot konvertáljuk.

[🇬🇧 **English version here** →](README.md)

---

## Tartalomjegyzék

- [Mi ez?](#-mi-ez)
- [Miért jó ez a skill?](#-miért-jó-ez-a-skill)
- [Funkciók](#-funkciók)
- [Gyors indulás](#-gyors-indulás)
- [Telepítés](#-telepítés)
- [A két workflow](#-a-két-workflow)
- [Használati példák](#-használati-példák)
- [A skill struktúrája](#-a-skill-struktúrája)
- [Mit tartalmaznak a referenciák?](#-mit-tartalmaznak-a-referenciák)
- [Követelmények és kompatibilitás](#-követelmények-és-kompatibilitás)
- [skills.sh megfelelőség és publikálás](#-skillssh-megfelelőség-és-publikálás)
- [Verziókezelés](#-verziókezelés)
- [Közreműködés](#-közreműködés)
- [Licenc](#-licenc)
- [Köszönetnyilvánítás](#-köszönetnyilvánítás)
- [English version (angol nyelv)](#-english-version-angol-nyelv)

---

## 💡 Mi ez?

A `mediawiki-article-creator` egy [Claude Code](https://docs.claude.com/en/docs/claude-code)
[skill](https://docs.claude.com/en/docs/claude-code/skills), amely segít modern,
letisztult designú MediaWiki cikkeket írni — a nulláról vagy más formátumú
tartalom konvertálásával. Ez a legátfogóbb, production-ready MediaWiki skill,
amely elérhető: teljes referencia-könyvtárral, tesztelt dupla-ellenőrző
workflow-val, és ~4000 gondosan összeállított dokumentációs sorral.

A skill a **bemenetnél formátum-agnosztikus**, de a **kimenetnél kizárólag
MediaWiki**. Két fő workflow-t támogat: párhuzamos iteratív szerkesztést, valamint
hat formátum-konverziós playbookot (HTML, PDF, Word, Excel, PowerPoint, HTML
slideshow). Minden konverziót független ügynök ellenőriz, így a szöveges
tartalom soha nem változik — csak a formátum.

---

## 🎯 Miért jó ez a skill?

| Probléma | Hogyan oldja meg a skill |
|----------|--------------------------|
| "Nem tudom fejből a MediaWiki szintaxist." | 1100 soros cheatsheet példákkal, mindig betöltve a kontextusba. |
| "A konvertált dokumentumok elveszítik a designt." | Hat formátum-specifikus playbook, amely megőrzi a layoutot a MediaWiki eszközkészletével. |
| "Konvertáláskor mindig átírom a szöveget." | Kötelező független-ügynök ellenőrzés, amely kiszúrja a szövegváltozásokat. |
| "A MediaWiki oldalaim úgy néznek ki, mint 2005-ös Wikipedia." | 22 másolható design recept modern elrendezésekhez (kártyák, accordion, galéria, infobox). |
| "Más-más megjelenítés kell különböző helyeken." | Pattern könyvtár: wikitable, TemplateStyles, syntax highlight, gallery, accordion, tabok, stb. |
| "Egyetlen forrást akarok a MediaWiki-hoz." | Hat referencia fájl: szintaxis, design, konverzió, ellenőrzés, haladó funkciók, receptek. |

---

## ✨ Funkciók

- **Két tiszta workflow**
  - **Párhuzamos iteratív szerkesztés** — cél egyeztetés → vázlat → teljes wikitext egy blokkban → visszajelzés ciklus
  - **Formátum-konverzió** — forrás olvasása → stratégia → wikitext → független-ügynök ellenőrzés → véglegesítés
- **Hat bemeneti formátum**
  - HTML riportok
  - HTML slideshow-k (reveal.js, Swiper, stb.)
  - PDF
  - Microsoft Word (DOCX)
  - Microsoft Excel (XLSX)
  - Microsoft PowerPoint (PPTX)
- **Modern MediaWiki design**
  - `class="wikitable sortable"` táblázatok
  - `<gallery mode="packed-hover">` képgalériák
  - `<syntaxhighlight lang="...">` szintaxis-kiemelt kód
  - `{{Note}}` / `{{Tip}}` / `{{Warning}}` / `{{Caution}}` kiemelések
  - `<details>` accordion szekciók
  - TabberNeue tabok
  - Mermaid / Graphviz diagramok
  - TemplateStyles-alapú kártyák és hero fejlécek
  - Breadcrumbs, infobox-ok, sidebar-ok
- **Dupla ellenőrzés független ügynökkel**
  - Szigorúan ellenőrzi, hogy a szöveges tartalom betűről betűre megegyezik-e a forrással
  - Még az egyetlen szavas eltérést is kiszúrja
  - Kiszűri a csak whitespace-re vonatkozó különbségeket
  - Külön jelenti a formátum- és design-eltéréseket
- **Nyelv-semleges kommunikáció**
  - A felhasználó nyelvén beszél; nincs ráerőltetett nyelv
  - A kód, a technikai kifejezések és a MediaWiki tagek mindig az eredeti formájukban maradnak
- **Production-ready referencia könyvtár (~4000 sor)**
  - `01-syntax-cheatsheet.md` — teljes MediaWiki szintaxis példákkal
  - `02-design-patterns.md` — modern, letisztult design minták
  - `03-conversion-playbooks.md` — formátum-specifikus konverziós útmutatók
  - `04-verification-checklist.md` — mit ellenőriz a független ügynök
  - `05-advanced-features.md` — TemplateStyles, Scribunto, ParserFunctions, kiterjesztések
  - `06-design-recipes.md` — 22 másolható kódrészlet

---

## 🚀 Gyors indulás

1. **Telepítsd a skillt** (lásd lentebb: [Telepítés](#-telepítés)).
2. **Hívd elő** egy Claude Code beszélgetésben a [Használati példák](#-használati-példák)
   egyik promptjával.
3. **Iterálj** — a skill a teljes wikitextet egy blokkban mutatja, adsz visszajelzést,
   a skill frissíti.

Példa beszélgetés:

> **Te:** "Készíts egy modern MediaWiki cikket az OpenAI o1 modellről. Legyen benne
> egy összehasonlító táblázat az o1-preview-val, egy Python kód példa, és egy GYIK
> szekció összecsukható válaszokkal."
>
> **Claude (a skillel):** *generál egy 100 soros wikitextet egy ` ```wiki ` blokkban,
> `class="wikitable sortable"`, `<syntaxhighlight lang="python">`, és `<details>`
> accordionokkal*

Ennyi. A wikitextet bemásolod a MediaWiki szerkesztőbe.

---

## 📦 Telepítés

### 1. opció — A `.skill` fájl bemásolása a projekt `skills/` mappájába

```bash
# A projekt gyökeréből
mkdir -p skills
cp /eleresi/ut/mediawiki-article-creator.skill skills/
cd skills
unzip mediawiki-article-creator.skill
```

A Claude Code automatikusan felismeri a `skills/` mappában lévő skill-eket.

### 2. opció — A kicsomagolt mappa bemásolása a `.claude/skills/` mappába

```bash
mkdir -p .claude/skills
cp -r /eleresi/ut/mediawiki-article-creator .claude/skills/
```

### 3. opció — Telepítés a skills.sh-ról

A skill publikálva van a skills.sh-n. Telepítés:

```bash
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator
```

Globálisan is telepítheted (az összes projektedben elérhető) a `-g` kapcsolóval,
vagy egy adott ügynökhöz a `-a` kapcsolóval:

```bash
# Globális telepítés
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator -g

# Egy adott ügynökhöz (claude-code, codex, opencode, cursor, stb.)
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator -a claude-code
```

A CLI átmásolja vagy szimbolikusan linkeli a skillt az ügynök lokális skills
mappájába (`./<agent>/skills/` alapértelmezetten, `~/<agent>/skills/` a `-g`
kapcsolóval).

Telepítés után indítsd újra a Claude Code-ot, hogy felismerje az új skillt.

---

## 🛠 A két workflow

### Workflow 1 — Párhuzamos, iteratív cikk-készítés

Akkor használd, amikor a nulláról indulsz, és az LLM-mel szeretnél brainstormolni.

```
Cél és közönség egyeztetés
        ↓
Vázlat (számozott szekció-lista)
        ↓
Teljes wikitext egy blokkban ← az LLM az egész cikket egyszerre mutatja
        ↓
A te visszajelzésed
        ↓
Frissített teljes wikitext
        ↓
Végleges lezárás (kategóriák, linkek)
```

A skill négy dolgot kérdez meg előre: **cél, közönség, hossz, szükséges elemek**.
Aztán egyben, nem darabokban állítja elő a teljes cikket, hogy lásd a teljes képet.

### Workflow 2 — Formátum-konverzió

Akkor használd, amikor más formátumban (HTML, PDF, Word, stb.) van tartalmad, és
MediaWiki-be akarod vinni.

```
Forrás (bármely támogatott formátum)
        ↓
Stratégia (hogyan képezzük le az egyes elemeket wikitextre)
        ↓
Wikitext egy blokkban
        ↓
Független-ügynök ellenőrzés ← KÖTELEZŐ konverzióknál
        ↓
Eltérések javítása
        ↓
Végleges wikitext
```

A független ügynököt az `Agent` tool hívja, `subagent_type: "general-purpose"`
típussal, a `references/04-verification-checklist.md` checklist alapján. Az ügynök
jelenti a szövegeltéréseket (akár egyetlen szót is!), a formátum- és design-eltéréseket,
valamint a szintaxis hibákat.

---

## 💼 Használati példák

### 1. példa — Cikk készítése a nulláról (Workflow 1)

> "Írj egy modern MediaWiki cikket a vektor-adatbázisokról. Legyen benne egy
> összehasonlító táblázat (Pinecone vs. Weaviate vs. Qdrant vs. Milvus), egy Python
> kód példa Qdrant-tal, és egy GYIK szekció."

A skill egyetlen wikitext blokkot készít, `__TOC__`, `class="wikitable sortable"`,
`<syntaxhighlight lang="python">`, `<details>` accordionok, és `[[Category:Databases]]`
elemekkel.

### 2. példa — HTML negyedéves riport konvertálása (Workflow 2)

> "Konvertáld át ezt a HTML riportot MediaWiki-be. Ne változtasd meg a szöveget, csak
> a formátumot. Ellenőrizd egy független ügynökkel."

A skill:
1. Végigolvassa a HTML-t.
2. Leképezi: `<h2>` → `==`, `<h3>` → `===`, `<table>` → `{| class="wikitable"`,
   `<strong>` → `'''...'''`, `<div class="warning">` → `{{Warning|...}}`.
3. Elindít egy független ügynököt a konverzió ellenőrzésére.
4. Jelenti az eltéréseket és javítja őket.

### 3. példa — Word dokumentum (DOCX) konvertálása

> "Van egy Word dokumentumom itt: `/docs/architecture.docx`. Konvertáld MediaWiki-be,
> és a wikitextet tedd az `output.wiki` fájlba."

A skill elmagyarázza, hogy először pandoc-ot vagy OpenOffice-ot kell futtatnod, hogy
nyers MediaWiki-formátumú szöveget kapj, majd visszaadod neki. A skill kitisztítja
a pandoc kimenetét, és modern designt alkalmaz.

### 4. példa — PowerPoint prezentáció konvertálása

> "Konvertáld ezt a PPTX-et MediaWiki cikkké. Minden dia legyen egy szekció."

A skill minden dia címét `==` címsorrá, a tartalmat listákká, a képeket
`[[File:...|thumb]]` hivatkozásokká, a jegyzeteket `{{Note|...}}` formába alakítja.

### 5. példa — Modern design kártyákkal

> "Csinálj egy landing-page stílusú cikket három egymás melletti feature kártyával,
> egy GYIK accordionnal, és egy hero fejléccel."

A skill TemplateStyles-alapú hero-t, `card-grid` három `.card` divvel, és
`<details>`-alapú GYIK-ot készít.

### 6. példa — MediaWiki saját sablonjainak használata

> "Tedd a megfelelő helyekre a kiemelő dobozokat (`{{Note}}`, `{{Warning}}`, `{{Tip}}`)."

A skill azonosítja, hogy milyen típusú kiemelés illik az adott helyre, és hozzáadja
őket a megfelelő súlyossággal (Note = info, Tip = javaslat, Warning = figyelem,
Caution = kritikus).

### 7. példa — Migráció egyedi wikiből

> "Van egy egyedi wikink 200 oldallal. Segíts migrációs tervet készíteni."

A skill végigvezet a migráción: azonosítja a kiterjesztés-specifikus elemeket,
sablononkénti konverziós szabályokat épít, pandoc-ot futtat oldalanként, és a
független ügynökkel ellenőriztet.

### 8. példa — Best-practice review

> "Itt van a MediaWiki cikkem piszkozata. Mondd el, mit javítanál design és
> akadálymentesség szempontjából."

A skill a `references/02-design-patterns.md` design checklistjét és a
`references/04-verification-checklist.md` ellenőrző listáját alkalmazza.

---

## 📁 A skill struktúrája

```
mediawiki-article-creator/
├── SKILL.md                                  # Fő belépési pont (workflow-k, szabályok)
├── references/
│   ├── 01-syntax-cheatsheet.md               # 1100 sor: teljes MediaWiki szintaxis
│   ├── 02-design-patterns.md                 # 640 sor: modern design minták
│   ├── 03-conversion-playbooks.md            # 480 sor: formátum-konverzió
│   ├── 04-verification-checklist.md          # 350 sor: független ügynök checklist
│   ├── 05-advanced-features.md               # 460 sor: TemplateStyles, Lua, stb.
│   └── 06-design-recipes.md                  # 810 sor: 22 másolható recept
└── workspace/                                # Opcionális munkatér
```

A `SKILL.md` mindig betöltődik az LLM kontextusába, amikor a skill triggerelődik.
A `references/` fájlok igény szerint töltődnek be, amikor az LLM-nek részletes
információra van szüksége egy adott témában. Ez karcsún tartja a kontextusablakot.

---

## 📚 Mit tartalmaznak a referenciák?

| Fájl | Cél | Sorok |
|------|-----|-------|
| `01-syntax-cheatsheet.md` | Teljes MediaWiki szintaxis referencia példákkal. **Innen indulj, ha nem ismered a szintaxist.** | ~1100 |
| `02-design-patterns.md` | Tipográfia, színhasználat, kiemelések, accordionok, tabok, infobox-ok, sidebar-ok, hero fejlécek. | ~640 |
| `03-conversion-playbooks.md` | Lépésről-lépésre konverziós útmutatók HTML riport, HTML slideshow, PDF, Word, Excel, PowerPoint formátumokhoz. | ~480 |
| `04-verification-checklist.md` | A független ügynök által használt pontos checklist. Tartalmazza a prompt sablont és a riport formátumot. | ~350 |
| `05-advanced-features.md` | TemplateStyles, Scribunto (Lua), ParserFunctions, Variables, Page Forms, Semantic MediaWiki, Cargo, biztonsági modell. | ~460 |
| `06-design-recipes.md` | 22 másolható design recept: kártyák, accordionok, tabok, breadcrumbs, infobox, sidebar, galéria, diagramok, stb. | ~810 |

---

## 📋 Követelmények és kompatibilitás

### MediaWiki verzió

A skill a **MediaWiki 1.39+** verziót célozza a teljes funkcionalitáshoz:

- `class="wikitable striped"` — 1.39+ szükséges
- `<details>` / `<summary>` — 1.40+ szükséges
- `class="skin-invert-image"` — Vector 2022 skin szükséges
- `{{short description}}` magic word — 1.38+ szükséges

Régebbi MediaWiki verzióknál a skill visszafelé kompatibilis natív szintaxist
használ, és kerüli az újabb funkciókat.

### Kiterjesztések

A skill ezeket a MediaWiki kiterjesztéseket használja, ha elérhetők (a
SyntaxHighlight kivételével mind opcionális):

- **SyntaxHighlight** (szinte mindig elérhető) — `<syntaxhighlight>` elemekhez
- **ParserFunctions** (szinte mindig elérhető) — `#if`, `#switch`, stb.
- **TemplateStyles** (opcionális) — egyedi CSS
- **Scribunto** (opcionális) — Lua modulok
- **TabberNeue** (opcionális) — tabok
- **Mermaid** (opcionális) — diagramok
- **Math** (általában elérhető) — LaTeX
- **Variables** (opcionális) — `#vardefine`
- **Page Forms** (opcionális) — űrlap-alapú szerkesztés
- **Semantic MediaWiki** vagy **Cargo** (opcionális) — strukturált adatok

A skill figyelmezteti a felhasználót, ha olyan kiterjesztést használ, amely
nem biztos, hogy elérhető.

### Skill specifikáció

A skill az [Agent Skills specifikáció](https://agentskills.io/specification) szerint
készült. A `SKILL.md` frontmatter érvényes:

```yaml
---
name: mediawiki-article-creator
description: |
  ... (max 1024 karakter, nincs < vagy >)
---
```

---

## 📡 skills.sh megfelelőség és publikálás

A skill úgy van kialakítva, hogy telepíthető legyen a [skills.sh](https://skills.sh)-
n, a nyílt agent-skills ökoszisztémán keresztül. Az alábbiakban a felfedezési
szabályok és a frontmatter követelmények találhatók.

### Kötelező frontmatter

A `SKILL.md` frontmatter érvényes az [Agent Skills specifikáció](https://agentskills.io/specification)
szerint:

| Mező | Kötelező | Érték |
|------|----------|-------|
| `name` | igen | `mediawiki-article-creator` — meg kell egyezzen a szülő mappa nevével |
| `description` | igen | ≤ 1024 karakter, nincs `<` vagy `>`, leírja **mit csinál és mikor használd** |
| `license` | nem | `MIT` (ebben a projektben) |
| `compatibility` | nem | `Designed for Claude Code (or similar products).` |
| `metadata` | nem | `author: eggp`, `version: 1.0.0` |
| `allowed-tools` | nem | nincs beállítva (a skill az alapértelmezett toolkészletet használja) |

### A `name` mező szabályai

- Csak kisbetűk, számok és kötőjelek
- 1-64 karakter
- Nem kezdődhet és nem végződhet kötőjellel
- Nem tartalmazhat dupla kötőjelet
- **Meg kell egyezzen a szülő mappa nevével** (a skill a `skills/mediawiki-article-creator/`
  útvonalon van, tehát a neve `mediawiki-article-creator`)

### Felfedezési útvonalak ebben a repóban

A skill a kanonikus `skills/<név>/` flat layout alatt van, amelyet a skills.sh
automatikusan felfedez (egy szint mélyen):

```
skills/
└── mediawiki-article-creator/      ← a név megegyezik a mappa nevével
    ├── SKILL.md
    ├── references/
    ├── workspace/
    └── (itt nincs SKILL.md, tehát a beágyazott skillt veszi, nem ezt a fájlt)
```

A skillt a `.claude/skills/`, `skills/.curated/` útvonalakon, vagy plugin
manifestekből (`.claude-plugin/marketplace.json`) is fel tudná fedezni.

### Telepítési parancs

```bash
# Ebből a GitHub repóból
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator
```

A `--list` kapcsolóval először listázhatod az összes skillt, a `-g` kapcsolóval
globálisan telepítheted, a `-a` kapcsolóval pedig adott ügynökökhöz.

### Publikálás — nincs szükség külön regisztrációra

A [Vercel agent skills útmutató](https://vercel.com/kb/guide/agent-skills-creating-installing-and-sharing-reusable-agent-context)
szerint a skills.sh-hoz **nincs formális publikálási folyamat**. Ahhoz, hogy
egy skill felfedezhető legyen:

1. Tedd egy publikus GitHub repóba
2. Győződj meg róla, hogy a `SKILL.md` érvényes (a fenti specifikáció szerint)
3. Adj hozzá egy `README.md` fájlt (ez a fájl pontosan az)
4. Opcionálisan csatolj egy `LICENSE` fájlt
5. Oszd meg a telepítési parancsot — a telepítések organikusan megjelennek
   a skills.sh leaderboardján az install telemetria alapján

A skill **telepíthető** amint a CLI eléri a repót, még mielőtt megjelenne a
nyilvános leaderboardon.

### Validáció

A frontmatter lokálisan validálható a [skills-ref validátorral](https://github.com/agentskills/agentskills/tree/main/skills-ref):

```bash
npx skills-ref validate skills/mediawiki-article-creator
```

Vagy használd a kézi checklistet:

- [x] `name` mező jelen van, kebab-case, ≤ 64 karakter, megegyezik a mappa nevével
- [x] `description` mező jelen van, ≤ 1024 karakter, nincs `<` vagy `>`, leírja a mit és a mikor
- [x] Opcionális mezők érvényesek (`license`, `compatibility`, `metadata`)
- [x] Nincsenek váratlan top-level kulcsok a frontmatterben
- [x] A fájl neve `SKILL.md` (case-sensitive)

---

## 🔢 Verziókezelés

A projekt [Szemantikus Verziózást](https://semver.org/) használ — `MAJOR.MINOR.PATCH`.

- **MAJOR** — a skill struktúrájában vagy workflow-jában bekövetkező törést okozó változások
- **MINOR** — új funkciók (új referencia, új workflow lépés, új példa)
- **PATCH** — bugfixek, elgépelések, kisebb pontosítások

Jelenlegi verzió: **1.0.0** (első kiadás)

---

## 🤝 Közreműködés

A közreműködés szívesen fogadott! Nyiss egy issue-t vagy pull requestet.

Új referencia fájl hozzáadásakor:
- Kövesd a meglévő fájlok Markdown stílusát
- Ha a fájl 300 sornál hosszabb, tegyél tartalomjegyzéket az elejére
- Linkeld az új fájlt a `SKILL.md`-ben a "Referenced files" szekcióban
- Futtasd le a skill-creator validációt commitolás előtt

Bug javításakor:
- Írd le a PR leírásában a hibát és a javítást
- Ha konverziós edge case-ről van szó, adj példát a
  `references/03-conversion-playbooks.md` fájlhoz

---

## 📄 Licenc

Ez a projekt az [MIT Licenc](LICENSE) alatt áll.

Szabadon használhatod, másolhatod, módosíthatod, egyesítheted, publikálhatod,
terjesztheted, allicencbe adhatod és/vagy eladhatod a szoftver másolatait, a
következő feltételekkel:

- A fenti copyright értesítést és ezt az engedélyt minden másolatba vagy a
  szoftver jelentős részébe be kell illeszteni.
- A SZOFTVERT "AHOGY VAN" BIZTOSÍTJUK, BÁRMILYEN GARANCIA NÉLKÜL.

---

## 🙏 Köszönetnyilvánítás

- A [MediaWiki közösség](https://www.mediawiki.org/wiki/Community) — a kiváló
  dokumentációért, amelyet ez a skill tömörít
- A [Claude Code csapat](https://docs.claude.com/en/docs/claude-code) — a
  skill rendszerért, amely ezt lehetővé teszi
- A [skill-creator skill](https://github.com/anthropics/skills) — a meta-skillért,
  amely ezt a skillt építette
- A [Pandoc](https://pandoc.org/) és az [OpenOffice](https://www.openoffice.org/)
  — a konverziós eszközökért, amelyeket a skill ajánl

---

## 🇬🇧 English version (angol nyelv)

The English documentation is here: [README.md](README.md).

A magyar verziójú skill a `hungarian-example/` mappában található, példaként
szolgál a kétnyelvűségre.
