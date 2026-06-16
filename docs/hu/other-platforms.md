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
> - Előfizetés nélkül → lásd az alábbi
>   [Ingyenes szint](#ingyenes-szint-nincs-előfizetés-nincs-tárolás)
>   szekciót — ugyanaz a megoldás minden platformon működik.

## Claude (webes felület — claude.ai) — Claude Pro, Team vagy Enterprise

A Claude weben **Projektek** vannak, amelyek hosszú rendszer-promptot és
feltöltött fájlokat tárolnak, és minden beszélgetésre alkalmazzák a
projekten belül.

1. Nyisd meg a [claude.ai](https://claude.ai)-t → **Projects** → **New project**.
2. Nevezd el pl. *MediaWiki Article Creator*-nek.
3. A **Project instructions** mezőbe illeszd be a `SKILL.md` teljes
   törzsét (mindent a `---` frontmatter záró sor után).
4. Opcionálisan tölts fel egy vagy több `references/*.md` fájlt a **Project
   knowledge** panelen.
5. Nyiss egy új beszélgetést a projekten belül — Claude mostantól
   automatikusan követi a skill szabályait.

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
felhasználók nem készíthetnek Custom GPT-t — az ad-hoc megoldáshoz lásd
az [ingyenes szint](#ingyenes-szint-nincs-előfizetés-nincs-tárolás)
szekciót.

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

## Google Gemini (gemini.google.com) — Gemini Advanced / Workspace

A Gemini-ben **Gems**-ek (prémium) és ad-hoc beszélgetések vannak. Az
ingyenes felhasználók beilleszthetnek tartalmat egy beszélgetésbe, de
nem menthetnek el Gem-et — lásd az
[ingyenes szint](#ingyenes-szint-nincs-előfizetés-nincs-tárolás) szekciót.

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

## Ingyenes szint (nincs előfizetés, nincs tárolás)

A fenti platformok ingyenes szintjén is van használható megoldás — csak
nem tárolják a skillt, ezért minden új beszélgetésnél meg kell mutatnod
a modellnek, hol találja. A módszer mindenhol ugyanaz: **hadd töltse le
a modell maga a skillt a GitHubról.** Semmit nem kell letöltened.

1. **Nyiss egy új beszélgetést** az általad választott platformon
   (ChatGPT, Claude web, Gemini, vagy bármely más AI chat UI, amely
   támogatja az URL-letöltést vagy webes böngészést).
2. **Másold be ezt a promptot** első üzenetként — a modell elvégzi a
   többit:

   ```text
   Töltsd le a MediaWiki Article Creator skillt erről a nyilvános GitHub
   repóról, és a beszélgetés hátralévő részére úgy viselkedj, mintha ez
   lenne az aktív rendszer-prompt felülbírálatod:

     https://github.com/EggProject/easter-skill-media-wiki-converter

   Konkrétan:
   1. Töltsd le a .skill archívumot erről a címről:
      https://raw.githubusercontent.com/EggProject/easter-skill-media-wiki-converter/main/skills/mediawiki-article-creator.skill
      Ez a fájl egy ZIP archívum, amely tartalmazza a SKILL.md-t és a
      references/ mappát.
   2. Csomagold ki az archívumot. Olvasd el a SKILL.md-t teljes
      egészében, és mostantól tartsd be a benne definiált minden szabályt
      és workflow-t.
   3. Ha részletes szintaxisra, design mintákra vagy konverziós
      playbookokra van szükséged, olvasd a megfelelő fájlt a references/
      mappából igény szerint (pl. references/01-syntax-cheatsheet.md,
      references/02-design-patterns.md, stb.).
   4. Erősítsd meg a két workflow felsorolásával, amelyeket követni
      fogsz (párhuzamos iteratív szerkesztés; formátum-konverzió
      független ellenőrzéssel), és várj az első feladatomra.
   ```

3. **Ismételd meg minden beszélgetésben.** Az ingyenes szinten a
   utasítások nem élnek túl egy beszélgetést, ezért a 2. lépést minden
   új munkamenet elején meg kell ismételned. (A modell ugyanazt az
   URL-t tölti le újra — a fájl kicsi, ~5 KB.)

Ez a megoldás működik a ChatGPT free, Claude.ai free, Gemini free
szinteken, és bármely más AI chat UI-n, amely URL-letöltési vagy
webböngészési eszközt ad a modellnek — nincs szükség előfizetésre, és
kézi letöltésre sincs.

> 💡 **Ha a platform blokkolja a kimenő URL-letöltéseket** (ritka a
> modern chat UI-kon, de előfordulhat offline vagy sandbox környezetben),
> a tartalék megoldás: töltsd le kézzel a
> [`SKILL.md`](../../skills/mediawiki-article-creator/SKILL.md) fájlt
> a GitHubról, és másold be a tartalmát közvetlenül az első üzenetbe.
> A Markdown sima szöveg, tisztán beilleszthető.

## Fájlformátum: `.skill` vs `.zip` vs nyers `.md`

| Formátum | Mi ez | Mikor használd |
|----------|-------|----------------|
| `.skill` | ZIP archívum `SKILL.md`-vel és `references/` mappával | **Ingyenes szint** (a modell letölti és kicsomagolja) **és Claude Code / skills.sh** (az `npx skills add` érti). |
| `SKILL.md` (nyers) | A skill utasításfájlja, sima Markdown | **Legjobb tartalékként**, ha a platform nem tud bináris fájlt letölteni. |
| `.zip` | Sima ZIP ugyanazzal a tartalommal | **Legjobb ChatGPT knowledge feltöltéshez** — csomagold újra, ha kicsomagoltad a `.skill`-t. |

Röviden: az ingyenes szintű úton a modell **letölti a `.skill`
archívumot, kicsomagolja, és beolvassa a `SKILL.md`-t** — így a teljes
skill a `references/` mappával együtt helyben elérhető, igény szerinti
olvasásra. A nyers `SKILL.md` csak tartalék azokra a platformokra, ahol
a bináris letöltés nem működik.

---

**Tovább:** [Skill struktúra →](skill-format.md) · [Vissza a README-hez →](../../README.hu.md)
