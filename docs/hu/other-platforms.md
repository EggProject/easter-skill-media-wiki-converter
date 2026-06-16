# A skill használata más AI platformokon

Ez a skill a nyílt [Agent Skills specifikáció](https://agentskills.io/specification)
köré épül, ezért — legalább részben — bármely AI asszisztensen
használható, amelyik lehetővé teszi egy hosszú rendszer-prompt vagy
utasítás-blokk befecskendezését. Az alábbiakban a leggyakoribb platformokon
mutatjuk be a használatot.

> **Gyors áttekintés:**
>
> - Előfizetéssel → a platform *tárolja* a skillt, és automatikusan
>   alkalmazza minden beszélgetésben.
> - Előfizetés nélkül → a platform *nem tárolja* a skillt, ezért minden
>   prompt elejére be kell illesztened. A skill tartalma sima Markdown, így
>   tisztán beilleszthető.
> - A `.skill` fájl valójában egy ZIP — csak a Claude Code és a skills.sh-t
>   ismerő eszközök értik natívan. Más platformokhoz csak a `SKILL.md`
>   tartalom kell (vagy azok a `references/*.md` fájlok, amelyeket
>   elérhetővé akarsz tenni a modellnek).

## Claude (webes felület — claude.ai) — Claude Pro, Team vagy Enterprise

A Claude weben **Projektek** vannak, amelyek hosszú rendszer-promptot és
feltöltött fájlokat tárolnak, és minden beszélgetésre alkalmazzák a
projekten belül.

**Előfizetéssel (ajánlott):**

1. Nyisd meg a [claude.ai](https://claude.ai)-t → **Projects** → **New project**.
2. Nevezd el pl. *MediaWiki Article Creator*-nek.
3. A **Project instructions** mezőbe illeszd be a `SKILL.md` teljes
   törzsét (mindent a `---` frontmatter záró sor után).
4. Opcionálisan tölts fel egy vagy több `references/*.md` fájlt a **Project
   knowledge** panelen.
5. Nyiss egy új beszélgetést a projekten belül — Claude mostantól
   automatikusan követi a skill szabályait.

**Előfizetés nélkül (ad-hoc):**

1. Töltsd le a `skills/mediawiki-article-creator.skill` fájlt ebből a
   repóból (vagy nyisd meg a `SKILL.md`-t közvetlenül a GitHubon).
2. Másold a vágólapra a `SKILL.md` Markdown törzsét.
3. Új beszélgetésben illeszd be az első üzenetként, ezzel a bevezetővel:
   *„Kövesd az alábbi utasításokat a beszélgetés hátralévő részében:"*.
4. A skill csak erre a beszélgetésre lesz aktív. A claude.ai ingyenes
   szintjén nincs tárolt projekt, ezért minden munkamenetben újra be kell
   illesztened.

## Claude Cowork

A Claude Cowork az Anthropic ügynöki asztali terméke. A skill-ek ugyanúgy
telepíthetők, mint a Claude Code-ban:

```bash
# Telepítés Cowork-hoz
npx skills add eggp/easter-skill-media-wiki-converter \
  --skill mediawiki-article-creator -a claude-code
```

A Cowork-ban a kicsomagolt skill mappát közvetlenül is behúzhatod abba a
skills mappába, amelyet az app figyel (lásd a Cowork dokumentációját az
egyes operációs rendszerekhez tartozó pontos útvonalakért). Újraindítás
után a skill elérhetővé válik meghívható workflow-ként.

## ChatGPT (OpenAI) — Plus, Team vagy Enterprise

A ChatGPT-ben **Custom GPT-k** vannak az előfizetőknek. Az ingyenes
felhasználók nem készíthetnek Custom GPT-t, de a skill tartalmát bármely
beszélgetésbe beilleszthetik.

**Előfizetéssel (Plus / Team / Enterprise):**

1. Nyisd meg a [chatgpt.com](https://chatgpt.com)-ot → **Explore GPTs** →
   **Create**.
2. A **Configure** → **Instructions** mezőbe illeszd be a `SKILL.md`
   törzsét.
3. (Opcionális) A **Knowledge**-ba töltsd fel a `SKILL.md`-t és bármely
   `references/*.md` fájlt, amelyet a GPT-nek referenciaként szeretnél
   adni. A ChatGPT elfogad PDF, TXT és Markdown formátumot; egy `.zip`
   feltöltése is működik sok fájl csoportosítására, de egyetlen knowledge
   bejegyzésnek számít.
4. Mentsd el *Only me* (vagy oszd meg).

> ⚠️ **Fájlformátum-megjegyzés:** A ChatGPT **nem** olvassa a Claude-féle
> `.skill` zip formátumot. Töltsd fel az egyes `.md` fájlokat vagy egy
> sima `.zip`-et, amely tartalmazza azokat. A frontend kicsomagolja az
> archívumot feltöltéskor, és indexeli a szöveges tartalmat.

**Előfizetés nélkül (Free tier):**

1. Nyisd meg a `SKILL.md`-t a GitHubon, és másold ki a Markdown törzsét.
2. Indíts egy új beszélgetést, és illeszd be az első üzenetként:
   > *Te egy tapasztalt MediaWiki cikkszerző vagy. Tartsd be az alábbi
   > szabályokat a beszélgetés hátralévő részében:*
   >
   > *<ide illeszd a SKILL.md törzsét>*
3. Minden új beszélgetés elején illeszd be újra — a ChatGPT ingyenes
   verziója nem őrzi meg az utasításokat a beszélgetések között.

## Google Gemini (gemini.google.com) — Gemini Advanced / Workspace

A Gemini-ben **Gems**-ek (prémium) és ad-hoc beszélgetések vannak. Az
ingyenes felhasználók beilleszthetnek tartalmat egy beszélgetésbe, de
nem menthetnek el Gem-et.

**Előfizetéssel (Gemini Advanced / Workspace):**

1. Nyisd meg a
   [gemini.google.com/gems/create](https://gemini.google.com/gems/create)-et.
2. Az **Instructions** mezőbe illeszd be a `SKILL.md` törzsét. A Gems
   hosszú utasításokat fogad el — egy teljes SKILL.md elfér.
3. (Opcionális) A **Knowledge** alá tölts fel legfeljebb 10 fájlt
   (PDF, DOCX, TXT, MD) — a limit a
   [2024. novemberi Workspace frissítés](https://workspaceupdates.googleblog.com/2024/11/upload-google-docs-and-other-file-types-to-gems.html)
   szerint.
4. Mentsd el a Gem-et. Mostantól elérhető a Gems oldalsávban bármely
   beszélgetéshez.

**Előfizetés nélkül (Free tier):**

1. Töltsd le vagy másold a `SKILL.md`-t ebből a repóból.
2. Indíts egy új beszélgetést, és illeszd be a `SKILL.md` törzsét az
   első felhasználói üzenetként, egy rövid bevezetővel.
3. Minden beszélgetésben illeszd be újra. A Gems (mentett utasítások)
   nem érhetők el az ingyenes szinten.

## Fájlformátum: `.skill` vs `.zip` vs nyers `.md`

| Formátum | Mi ez | Mikor használd |
|----------|-------|----------------|
| `SKILL.md` (nyers) | A skill utasításfájlja, sima Markdown | **Legjobb prompt-ba illesztéshez** bármely platformon. |
| `.skill` | ZIP archívum `SKILL.md`-vel és `references/` mappával | **Legjobb Claude Code-hoz / skills.sh-hoz** — az `npx skills add` érti. |
| `.zip` | Sima ZIP ugyanazzal a tartalommal | **Legjobb ChatGPT knowledge feltöltéshez** — csomagold újra, ha kicsomagoltad a `.skill`-t. |

Röviden: a fenti platformokhoz szinte mindig a **nyers `SKILL.md`
tartalmat** kell a promptba illeszteni, és opcionálisan néhány
`references/*.md` fájlt a platform knowledge paneljére feltölteni. A
`.skill` / `.zip` formátumnak csak a Claude Code és az Agent Skills
protokollt ismerő eszközök számára van jelentősége.

## Ajánlott prompt-preamble

Ha a `SKILL.md`-t olyan platformra illeszted be, ahol nincs dedikált
skill slot, használd ezt a sablont a stabil viselkedés fenntartásához:

```text
Te egy tapasztalt MediaWiki szerző és szerkesztő vagy. Mostantól tartsd be
az alábbi dokumentumban található szabályokat és workflow-kat. Töltsd be
a referenciákat, ha részletes szintaxisra, design mintákra vagy konverziós
playbookokra van szükséged.

<IDE ILLESZD A SKILL.md TELJES TARTALMÁT>

Erősítsd meg a két workflow felsorolásával, amelyeket követni fogsz, és
várj az első feladatomra.
```

Ez a preamble felkészíti a modellt, hogy a beillesztett tartalmat tartós
rendszer-prompt felülbírálatként kezelje, ne pedig egyszeri kérdésként.

---

**Tovább:** [Skill struktúra →](skill-format.md) · [Vissza a README-hez →](../../README.hu.md)
