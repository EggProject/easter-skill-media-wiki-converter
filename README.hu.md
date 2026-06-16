# MediaWiki Article Creator — Claude Code Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude_Code-Skill-blueviolet)](https://docs.claude.com/en/docs/claude-code/skills)
[![Skill Format: SKILL.md](https://img.shields.io/badge/Format-SKILL.md-success)](https://agentskills.io/specification)
[![Wikitext](https://img.shields.io/badge/Output-MediaWiki_wikitext-36c)](https://www.mediawiki.org/wiki/Wikitext)
[![skills.sh](https://img.shields.io/badge/skills.sh-installable-orange)](https://skills.sh)
[![skills.sh](https://skills.sh/b/EggProject/easter-skill-media-wiki-converter)](https://skills.sh/EggProject/easter-skill-media-wiki-converter)
[![English](https://img.shields.io/badge/Docs-English-blue)](README.md)
[![Magyar](https://img.shields.io/badge/Docs-Magyar-green)](README.hu.md)

> Egy Claude Code skill, amellyel modern, letisztult designú MediaWiki
> cikkeket készíthetsz, valamint más formátumokat (HTML riport, HTML
> slideshow, PDF, Word, Excel, PowerPoint) MediaWiki wikitextté
> konvertálhatsz — független ügynök által végzett dupla ellenőrzéssel,
> amely garantálja, hogy a szöveget soha nem írjuk át, csak a formátumot
> konvertáljuk.

[🇬🇧 **English version here** →](README.md)

---

## 📖 Dokumentáció

A teljes dokumentáció szét van bontva erre a hub-ra és egy `docs/hu/`
mappára. A hub fókuszált marad; a mélyebb részletekre kattints át.

| Oldal | Mi van benne |
|-------|--------------|
| **[📦 Telepítés](docs/hu/installation.md)** | Három telepítési útvonal: skills.sh, `.skill` zip, `.claude/skills/`. |
| **[🛠 Workflow-k](docs/hu/workflows.md)** | A két workflow — párhuzamos szerkesztés és formátum-konverzió — diagramokkal. |
| **[💼 Használati példák](docs/hu/usage-examples.md)** | Nyolc valósághű prompt, és amit a skill készít belőlük. |
| **[🌐 Más AI platformok](docs/hu/other-platforms.md)** | A skill használata ChatGPT-n, Claude weben, Claude Cowork-on és Gemini — **beleértve egy előfizetés nélküli, ingyenes receptet, amely minden platformon működik**. |
| **[📁 A skill struktúrája](docs/hu/skill-format.md)** | Lemezen lévő elrendezés, és mire való az egyes `references/*.md`. |
| **[📋 Kompatibilitás](docs/hu/compatibility.md)** | MediaWiki verziók, kiterjesztések, ügynök hosztok, skill spec. |
| **[📡 Publikálás](docs/hu/publishing.md)** | skills.sh frontmatter, felfedezés, validáció, csomagolás. |
| **[ℹ️ Projekt info](docs/hu/project-info.md)** | Verziókezelés, közreműködés, licenc, köszönetnyilvánítás. |

---

## 💡 Mi ez?

A `mediawiki-article-creator` egy
[Claude Code skill](https://docs.claude.com/en/docs/claude-code/skills),
amely segít modern, letisztult designú MediaWiki cikkeket írni — a
nulláról vagy más formátumú tartalom konvertálásával. Ez a
legátfogóbb, production-ready MediaWiki skill, amely elérhető: teljes
referencia-könyvtárral, tesztelt dupla-ellenőrző workflow-val, és
~4000 gondosan összeállított dokumentációs sorral.

A skill a **bemenetnél formátum-agnosztikus**, de a **kimenetnél
kizárólag MediaWiki**. Két fő workflow-t támogat: párhuzamos iteratív
szerkesztést, valamint hat formátum-konverziós playbookot (HTML, PDF,
Word, Excel, PowerPoint, HTML slideshow). Minden konverziót független
ügynök ellenőriz, így a szöveges tartalom soha nem változik — csak a
formátum.

## 🎯 Miért jó ez a skill?

| Probléma | Hogyan oldja meg a skill |
|----------|--------------------------|
| "Nem tudom fejből a MediaWiki szintaxist." | 1100 soros cheatsheet példákkal, mindig betöltve a kontextusba. |
| "A konvertált dokumentumok elveszítik a designt." | Hat formátum-specifikus playbook, amely megőrzi a layoutot a MediaWiki eszközkészletével. |
| "Konvertáláskor mindig átírom a szöveget." | Kötelező független-ügynök ellenőrzés, amely kiszúrja a szövegváltozásokat. |
| "A MediaWiki oldalaim úgy néznek ki, mint 2005-ös Wikipedia." | 22 másolható design recept modern elrendezésekhez (kártyák, accordion, galéria, infobox). |
| "Más-más megjelenítés kell különböző helyeken." | Pattern könyvtár: wikitable, TemplateStyles, syntax highlight, gallery, accordion, tabok, stb. |
| "Egyetlen forrást akarok a MediaWiki-hoz." | Hat referencia fájl: szintaxis, design, konverzió, ellenőrzés, haladó funkciók, receptek. |

## ✨ Funkciók

- **Két tiszta workflow** — párhuzamos iteratív szerkesztés, valamint
  formátum-konverzió kötelező független-ügynök ellenőrzéssel
- **Hat bemeneti formátum** — HTML riportok, HTML slideshow-k, PDF, Word,
  Excel, PowerPoint
- **Modern MediaWiki design** — `wikitable sortable`, `<gallery>`,
  szintaxis-kiemelés, `{{Note}}` / `{{Tip}}` / `{{Warning}}` / `{{Caution}}`
  kiemelések, `<details>` accordionok, TabberNeue tabok, Mermaid diagramok,
  TemplateStyles-alapú kártyák és hero fejlécek
- **Dupla ellenőrzés független ügynökkel** — betűről betűre szövegegyezés,
  egyetlen szavas eltérés kiszűrése, whitespace szűrő, formátum- és
  design-diff külön jelentve
- **Nyelv-semleges kommunikáció** — a felhasználó nyelvén beszél; a kód,
  a technikai kifejezések és a MediaWiki tagek mindig az eredeti formájukban
  maradnak
- **Production-ready referencia könyvtár** — ~4000 sor hat fájlban

## 🚀 Gyors indulás

1. **Telepítsd a skillt** — lásd [📦 Telepítés](docs/hu/installation.md).
2. **Hívd elő** a [💼 Használati példák](docs/hu/usage-examples.md) egyik
   promptjával.
3. **Iterálj** — a skill a teljes wikitextet egy blokkban mutatja, adsz
   visszajelzést, a skill frissíti.

Példa beszélgetés:

> **Te:** "Készíts egy modern MediaWiki cikket az OpenAI o1 modellről.
> Legyen benne egy összehasonlító táblázat az o1-preview-val, egy Python
> kód példa, és egy GYIK szekció összecsukható válaszokkal."
>
> **Claude (a skillel):** *generál egy 100 soros wikitextet egy ` ```wiki `
> blokkban, `class="wikitable sortable"`, `<syntaxhighlight lang="python">`,
> és `<details>` accordionokkal*

Ennyi. A wikitextet bemásolod a MediaWiki szerkesztőbe.

Más AI platformokhoz (ChatGPT, Claude web, Gemini, Claude Cowork) — beleértve
az előfizetés nélküli, ingyenes szintű receptet, amely minden chat UI-n
működik — lásd a [🌐 Más AI platformok](docs/hu/other-platforms.md) oldalt.

## 📄 Licenc

Ez a projekt az [MIT Licenc](LICENSE) alatt áll (Copyright © 2026
eggproject Kft., Magyarország).

Az MIT Licenc **minden személynek**, aki a szoftver egy példányát
megszerzi, megadja a jogot, hogy **ingyenesen** használja, másolja,
módosítsa, egyesítse, publikálja, terjessze, allicencbe adja és/vagy
eladja a szoftver másolatait. Az egyetlen feltétel, hogy a copyright
értesítést és az engedélyt meg kell őrizni minden másolatban vagy a
szoftver jelentős részében, és a szoftvert **"ahogy van"** biztosítjuk,
bármilyen garancia nélkül — a teljes szöveget lásd a [`LICENSE`](LICENSE)
fájlban.

## 🇬🇧 English version (angol nyelv)

The English documentation is here: [README.md](README.md).

A magyar verziójú skill a `hungarian-example/` mappában található, és
**kifejezetten példaként szolgál a magyar kollégáknak**, akik magyar
nyelvű rendszer-prompttal dolgoznak. Általános felhasználásra **az angol
SKILL.md-t ajánljuk** — a drágább modelleknél a nyelv kevésbé számít, de
olcsóbb vagy kisebb lokális modelleknél hosszabb, összetett feladatoknál
az angol prompt megbízhatóbban követett. A magyar verziót csak akkor
válaszd, ha a beszélgetés system promptja is magyar.
