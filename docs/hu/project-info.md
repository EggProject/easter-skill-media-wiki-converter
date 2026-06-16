# Projekt info

Verziókezelés, közreműködés, licenc és köszönetnyilvánítás.

## 🔢 Verziókezelés

A projekt [Szemantikus Verziózást](https://semver.org/) használ —
`MAJOR.MINOR.PATCH`.

- **MAJOR** — a skill struktúrájában vagy workflow-jában bekövetkező
  törést okozó változások
- **MINOR** — új funkciók (új referencia, új workflow lépés, új példa)
- **PATCH** — bugfixek, elgépelések, kisebb pontosítások

Jelenlegi verzió: **1.0.0** (első kiadás)

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

## 📄 Licenc

Ez a projekt az [MIT Licenc](../../LICENSE) alatt áll
(Copyright © 2026 eggproject Kft., Magyarország).

Az MIT Licenc **minden személynek**, aki a szoftver egy példányát
megszerzi, megadja a jogot, hogy **ingyenesen** használja, másolja,
módosítsa, egyesítse, publikálja, terjessze, allicencbe adja és/vagy
eladja a szoftver másolatait. Az egyetlen feltétel, hogy a copyright
értesítést és az engedélyt meg kell őrizni minden másolatban vagy a
szoftver jelentős részében, és a szoftvert **"ahogy van"** biztosítjuk,
bármilyen garancia nélkül — a teljes szöveget lásd a
[`LICENSE`](../../LICENSE) fájlban.

## 🙏 Köszönetnyilvánítás

Ez a skill a hivatalos
[Claude skill-creator plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator)-
nal készült — ez egy meta-skill, amelyet Claude Code skill-ek
tervezésére, tesztelésére és csomagolására használnak.

- A [MediaWiki közösség](https://www.mediawiki.org/wiki/Community) — a
  kiváló dokumentációért, amelyet ez a skill tömörít
- A [Claude Code csapat](https://docs.claude.com/en/docs/claude-code) —
  a skill rendszerért, amely ezt lehetővé teszi
- A [hivatalos Claude skill-creator plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator) —
  a meta-skill workflow-ért, amely ezt a skillt tervezte és csomagolta
- A [Pandoc](https://pandoc.org/) és az [OpenOffice](https://www.openoffice.org/)
  — a konverziós eszközökért, amelyeket a skill ajánl

---

**Vissza:** [README →](../../README.hu.md)
