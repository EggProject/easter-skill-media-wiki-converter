# A két workflow

A skill két különböző szerkesztési folyamatot támogat. A kiindulási pont
alapján válassz az alábbiak közül.

## Workflow 1 — Párhuzamos, iteratív cikk-készítés

Akkor használd, amikor a nulláról indulsz, és az LLM-mel szeretnél
brainstormolni.

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

A skill négy dolgot kérdez meg előre: **cél, közönség, hossz, szükséges
elemek**. Aztán egyben, nem darabokban állítja elő a teljes cikket, hogy
lásd a teljes képet, és holisztikus visszajelzést tudj adni.

Ez a megközelítés a
[skill-creator plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator)
metodológiáját tükrözi: előbb írd meg az egészet, aztán iterálj. Az ügynök
minden revíziónál újraírja a teljes blokkot (nem pedig foltonként javít),
így a wikitext végig konzisztens marad.

## Workflow 2 — Formátum-konverzió

Akkor használd, amikor más formátumban (HTML, PDF, Word stb.) van
tartalmad, és MediaWiki-be akarod vinni.

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
típussal, a
[`references/04-verification-checklist.md`](../../skills/mediawiki-article-creator/references/04-verification-checklist.md)
checklist alapján. Az ügynök jelenti a szövegeltéréseket (akár egyetlen
szót is!), a formátum- és design-eltéréseket, valamint a szintaxis hibákat.

**Miért fontos a független ellenőrzés:** amikor egy LLM átírja a szöveget,
finoman parafrazál — még akkor is, ha kifejezetten megkérik, hogy ne. Egy
friss ügynök szigorú checklist-tel kiszúrja ezeket, és javítást kényszerít
ki, mielőtt a cikk megjelenik.

## Melyik workflow-t mikor?

| Van... | Használd |
|--------|----------|
| Egy üres oldal és egy téma a fejedben | **Workflow 1** — iterálj a nulláról |
| Meglévő dokumentum (HTML, PDF, DOCX, PPTX, XLSX) | **Workflow 2** — konvertálás |
| Piszkozat MediaWiki cikk, amit csiszolni szeretnél | **Workflow 1** stílusban — review és iteráció |
| Egyedi wiki, amit migrálni kell | **Workflow 2** — oldalankénti konverzió |

---

**Tovább:** [Használati példák →](usage-examples.md) · [Vissza a README-hez →](../../README.hu.md)
