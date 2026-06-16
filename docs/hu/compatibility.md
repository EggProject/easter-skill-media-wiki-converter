# Kompatibilitás és követelmények

A skill a **MediaWiki 1.39+** verziót célozza a teljes funkcionalitáshoz,
és minden olyan ügynökkel működik, amelyik követi az
[Agent Skills specifikációt](https://agentskills.io/specification).

## MediaWiki verzió

| Funkció | Minimális MediaWiki verzió |
|---------|----------------------------|
| `class="wikitable sortable"` | 1.27+ |
| `class="wikitable striped"` | 1.39+ |
| `<details>` / `<summary>` | 1.40+ |
| `class="skin-invert-image"` | Vector 2022 skin |
| `{{short description}}` magic word | 1.38+ |

Régebbi MediaWiki verzióknál a skill visszafelé kompatibilis natív
szintaxist használ, és kerüli az újabb funkciókat. A skill ellenőrzi a
verziót, amellyel beszél (explicit átadható neki), és graceful downgradet
végez.

## MediaWiki kiterjesztések

A skill ezeket a kiterjesztéseket használja, ha elérhetők. Csak a
**SyntaxHighlight**-ot feltételezi, hogy mindenhol jelen van; a többit
jelzi, ha a skill használja egy adott kimenetben.

| Kiterjesztés | Kötelező? | Erre használja |
|--------------|-----------|----------------|
| **SyntaxHighlight** | Szinte mindig elérhető | `<syntaxhighlight>` blokkok |
| **ParserFunctions** | Szinte mindig elérhető | `#if`, `#switch`, stb. |
| **TemplateStyles** | Opcionális | Egyedi CSS kártyákhoz, hero fejlécekhez |
| **Scribunto** | Opcionális | Lua modulok |
| **TabberNeue** | Opcionális | Tabok |
| **Mermaid** | Opcionális | Diagramok |
| **Math** | Általában elérhető | LaTeX |
| **Variables** | Opcionális | `#vardefine` |
| **Page Forms** | Opcionális | Űrlap-alapú szerkesztés |
| **Semantic MediaWiki** vagy **Cargo** | Opcionális | Strukturált adatok |

A skill figyelmeztet, ha olyan kiterjesztést használ, amely nem biztos,
hogy elérhető, és felajánl egy alternatívát (pl. egyszerű wikitable a
TabberNeue tab helyett, kódblokk szintaxis-kiemelés helyett).

## Támogatott ügynök hosztok

| Ügynök | Telepítés | Megjegyzések |
|--------|-----------|--------------|
| Claude Code | `npx skills add` vagy `.claude/skills/` | Teljes funkciótámogatás, beleértve az `Agent` tool-t az ellenőrzéshez. |
| Claude Cowork | `npx skills add -a claude-code` | Ugyanaz, mint a Claude Code. |
| Claude web (Projects) | SKILL.md beillesztése a Project instructions-ba | Nincs `Agent` tool — az ellenőrzést a fő modell végzi. |
| OpenAI Codex | `npx skills add -a codex` | |
| OpenCode | `npx skills add -a opencode` | |
| Cursor | `npx skills add -a cursor` | |
| ChatGPT (Custom GPT) | SKILL.md beillesztése az Instructions-ba | Nincs `Agent` tool. |
| Gemini (Gems) | SKILL.md beillesztése az Instructions-ba | Nincs `Agent` tool. |

## Skill specifikáció

A `SKILL.md` frontmatter érvényes az
[Agent Skills specifikáció](https://agentskills.io/specification) szerint:

```yaml
---
name: mediawiki-article-creator
description: |
  ... (max 1024 karakter, nincs < vagy >)
---
```

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
- **Meg kell egyezzen a szülő mappa nevével** (a skill a
  `skills/mediawiki-article-creator/` útvonalon van, tehát a neve
  `mediawiki-article-creator`)

---

**Tovább:** [Publikálás →](publishing.md) · [Vissza a README-hez →](../../README.hu.md)
