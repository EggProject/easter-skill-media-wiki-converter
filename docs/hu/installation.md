# Telepítés

Három telepítési útvonal támogatott, ajánlott sorrendben.

## 1. opció — Telepítés a skills.sh-ról (ajánlott)

A skill publikálva van a nyílt [skills.sh](https://skills.sh) rendszerben.
Telepítés az `npx skills add` paranccsal:

```bash
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator
```

A CLI átmásolja vagy szimbolikusan linkeli a skillt az ügynök lokális skills
mappájába (`./<agent>/skills/` alapértelmezetten).

### Globális telepítés

A `-g` kapcsolóval egyszer telepítheted a felhasználódhoz, és minden
projektedben elérhető:

```bash
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator -g
```

A skill ekkor a `~/<agent>/skills/` mappába kerül.

### Rögzítés egy adott ügynökhöz

A `-a` kapcsolóval csak egy adott ügynökhöz telepítheted (hasznos, ha
több is telepítve van, pl. `claude-code`, `codex`, `opencode`, `cursor`):

```bash
npx skills add eggp/easter-skill-media-wiki-converter \
  --skill mediawiki-article-creator \
  -a claude-code
```

Telepítés után indítsd újra a Claude Code-ot, hogy felismerje az új skillt.

## 2. opció — A `.skill` fájl bemásolása a projekt `skills/` mappájába

A csomagolt `mediawiki-article-creator.skill` archívum valójában egy
sima ZIP.

```bash
# A projekt gyökeréből
mkdir -p skills
cp /eleresi/ut/mediawiki-article-creator.skill skills/
cd skills
unzip mediawiki-article-creator.skill
```

A Claude Code automatikusan felismeri a `skills/` mappában lévő skill-eket.

## 3. opció — A kicsomagolt mappa bemásolása a `.claude/skills/` mappába

Ha már kicsomagoltad az archívumot (vagy klónoztad a repót), a kicsomagolt
mappát közvetlenül is átmásolhatod:

```bash
mkdir -p .claude/skills
cp -r /eleresi/ut/mediawiki-article-creator .claude/skills/
```

A skill a `.claude/skills/mediawiki-article-creator/` mappából töltődik be.

## A telepítés ellenőrzése

Bármelyik telepítési módszer után indíts egy friss Claude Code munkamenetet,
és kérdezd meg:

> "Ismered a mediawiki-article-creator skillt?"

Claude-nak meg kell erősítenie, hogy a skill elérhető, és továbbléphetsz
a [workflow-k](workflows.md) oldalra.

---

**Tovább:** [Workflow-k →](workflows.md) · [Más platformok →](other-platforms.md)
