# skills.sh megfelelőség és publikálás

Ez a skill úgy van kialakítva, hogy telepíthető legyen a
[skills.sh](https://skills.sh)-n, a nyílt agent-skills ökoszisztémán
keresztül. Az alábbiakban a felfedezési szabályok, a frontmatter
követelmények és a validációs lépések találhatók.

## Kötelező frontmatter

A `SKILL.md` frontmatter érvényes az
[Agent Skills specifikáció](https://agentskills.io/specification) szerint:

| Mező | Kötelező | Érték |
|------|----------|-------|
| `name` | igen | `mediawiki-article-creator` — meg kell egyezzen a szülő mappa nevével |
| `description` | igen | ≤ 1024 karakter, nincs `<` vagy `>`, leírja **mit csinál és mikor használd** |
| `license` | nem | `MIT` (ebben a projektben) |
| `compatibility` | nem | `Designed for Claude Code (or similar products).` |
| `metadata` | nem | `author: eggp`, `version: 1.0.0` |
| `allowed-tools` | nem | nincs beállítva (a skill az alapértelmezett toolkészletet használja) |

## A `name` mező szabályai

- Csak kisbetűk, számok és kötőjelek
- 1-64 karakter
- Nem kezdődhet és nem végződhet kötőjellel
- Nem tartalmazhat dupla kötőjelet
- **Meg kell egyezzen a szülő mappa nevével** (a skill a
  `skills/mediawiki-article-creator/` útvonalon van, tehát a neve
  `mediawiki-article-creator`)

## Felfedezési útvonalak ebben a repóban

A skill a kanonikus `skills/<név>/` flat layout alatt van, amelyet a
skills.sh automatikusan felfedez (egy szint mélyen):

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

## Telepítési parancs

```bash
# Ebből a GitHub repóból
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator
```

A `--list` kapcsolóval először listázhatod az összes skillt, a `-g`
kapcsolóval globálisan telepítheted, a `-a` kapcsolóval pedig adott
ügynökökhöz.

## Publikálás — nincs szükség külön regisztrációra

A
[Vercel agent skills útmutató](https://vercel.com/kb/guide/agent-skills-creating-installing-and-sharing-reusable-agent-context)
szerint a skills.sh-hoz **nincs formális publikálási folyamat**. Ahhoz,
hogy egy skill felfedezhető legyen:

1. Tedd egy publikus GitHub repóba
2. Győződj meg róla, hogy a `SKILL.md` érvényes (a fenti specifikáció
   szerint)
3. Adj hozzá egy `README.md` fájlt (ez a fájl pontosan az)
4. Opcionálisan csatolj egy `LICENSE` fájlt
5. Oszd meg a telepítési parancsot — a telepítések organikusan megjelennek
   a skills.sh leaderboardján az install telemetria alapján

A skill **telepíthető** amint a CLI eléri a repót, még mielőtt megjelenne
a nyilvános leaderboardon.

## Validáció

A frontmatter lokálisan validálható a
[skills-ref validátorral](https://github.com/agentskills/agentskills/tree/main/skills-ref):

```bash
npx skills-ref validate skills/mediawiki-article-creator
```

Vagy használd a kézi checklistet:

- [x] `name` mező jelen van, kebab-case, ≤ 64 karakter, megegyezik a mappa nevével
- [x] `description` mező jelen van, ≤ 1024 karakter, nincs `<` vagy `>`, leírja a mit és a mikor
- [x] Opcionális mezők érvényesek (`license`, `compatibility`, `metadata`)
- [x] Nincsenek váratlan top-level kulcsok a frontmatterben
- [x] A fájl neve `SKILL.md` (case-sensitive)

## A `.skill` archívum építése

A csomagolt `skills/mediawiki-article-creator.skill` a
`mediawiki-article-creator/` mappa sima ZIP-je. Szerkesztés utáni
újraépítéshez:

```bash
cd skills
zip -r mediawiki-article-creator.skill mediawiki-article-creator/ -x "*.DS_Store"
```

A skill-creator plugin a "Skill csomagolása .skill fájlba" workflow-ján
keresztül szintén tudja csomagolni a skillt.

---

**Tovább:** [Projekt info →](project-info.md) · [Vissza a README-hez →](../../README.hu.md)
