# A skill struktúrája és referenciái

A skill a nyílt [Agent Skills specifikáció](https://agentskills.io/specification)
szerint készült. Ez az oldal dokumentálja a lemezen lévő elrendezést és
azt, hogy az egyes referencia fájlok mire valók.

## Lemezen lévő elrendezés

```
skills/
└── mediawiki-article-creator/      ← a név megegyezik a mappa nevével
    ├── SKILL.md                    # Fő belépési pont (workflow-k, szabályok)
    ├── references/                 # Igény szerinti dokumentáció
    │   ├── 01-syntax-cheatsheet.md
    │   ├── 02-design-patterns.md
    │   ├── 03-conversion-playbooks.md
    │   ├── 04-verification-checklist.md
    │   ├── 05-advanced-features.md
    │   └── 06-design-recipes.md
    └── workspace/                  # Opcionális munkatér (kezdetben üres)
```

A `SKILL.md` mindig betöltődik az LLM kontextusába, amikor a skill
triggerelődik. A `references/` fájlok **igény szerint** töltődnek be,
amikor az LLM-nek részletes információra van szüksége egy adott témában.
Ez tartja karcsún a kontextusablakot, és a
[progressive disclosure](https://agentskills.io/specification) minta
része, amelyet a specifikáció javasol.

## Mit tartalmaznak a referenciák?

| Fájl | Cél | Sorok |
|------|-----|-------|
| `01-syntax-cheatsheet.md` | Teljes MediaWiki szintaxis referencia példákkal. **Innen indulj, ha nem ismered a szintaxist.** | ~1100 |
| `02-design-patterns.md` | Tipográfia, színhasználat, kiemelések, accordionok, tabok, infobox-ok, sidebar-ok, hero fejlécek. | ~640 |
| `03-conversion-playbooks.md` | Lépésről-lépésre konverziós playbookok HTML riport, HTML slideshow, PDF, Word, Excel, PowerPoint formátumokhoz. | ~480 |
| `04-verification-checklist.md` | A pontos checklist, amelyet a független ügynök használ a wikitext ellenőrzésére. Tartalmazza a prompt sablont és a riport formátumot. | ~350 |
| `05-advanced-features.md` | TemplateStyles, Scribunto (Lua), ParserFunctions, Variables, Page Forms, Semantic MediaWiki, Cargo, biztonsági modell. | ~460 |
| `06-design-recipes.md` | 22 másolható design recept: kártyák, accordionok, tabok, breadcrumbs, infobox, sidebar, galéria, diagramok, stb. | ~810 |

## Hol találod meg, amit keresel

- *"Nem tudom, hogyan kell X-et wikitextben"* → `01-syntax-cheatsheet.md`
- *"Hogyan néz ki egy modern kiemelés?"* → `02-design-patterns.md`
- *"Hogyan konvertáljak HTML táblát wikitextre?"* → `03-conversion-playbooks.md`
- *"Mit ellenőriz valójában a verification ügynök?"* → `04-verification-checklist.md`
- *"Használhatok Lua-t / TemplateStyles-t?"* → `05-advanced-features.md`
- *"Adj egy copy-paste komponenst."* → `06-design-recipes.md`

## A `workspace/` mappa

A `workspace/` mappa a skillben üres munkatérként szerepel. A skill
használhatja átmeneti közbenső fájlok tárolására nagy dokumentumok
konvertálásakor. Biztonságosan üresen hagyható (és érdemes a
`workspace/**/*` mintát a `.gitignore`-ba tenni, ha forkolod a skillt).

## Magyar tükör

A skill magyar nyelvű változata a [`hungarian-example/`](../../hungarian-example/)
mappában található, kétnyelvű példaként. Ugyanaz a struktúra és ugyanaz
a hat referencia fájl, mind magyarul.

---

**Tovább:** [Kompatibilitás →](compatibility.md) · [Vissza a README-hez →](../../README.hu.md)
