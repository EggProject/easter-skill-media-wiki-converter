# Használati példák

Nyolc valósághű prompt, és hogy mit készít a skill mindegyikre. Ezek a
[hivatalos skill-creator plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator)
tesztkészletének példáit tükrözik.

## 1. példa — Cikk készítése a nulláról (Workflow 1)

> "Írj egy modern MediaWiki cikket a vektor-adatbázisokról. Legyen benne
> egy összehasonlító táblázat (Pinecone vs. Weaviate vs. Qdrant vs. Milvus),
> egy Python kód példa Qdrant-tal, és egy GYIK szekció."

A skill egyetlen wikitext blokkot készít, `__TOC__`, `class="wikitable
sortable"`, `<syntaxhighlight lang="python">`, `<details>` accordionok, és
`[[Category:Databases]]` elemekkel.

## 2. példa — HTML negyedéves riport konvertálása (Workflow 2)

> "Konvertáld át ezt a HTML riportot MediaWiki-be. Ne változtasd meg a
> szöveget, csak a formátumot. Ellenőrizd egy független ügynökkel."

A skill:
1. Végigolvassa a HTML-t.
2. Leképezi: `<h2>` → `==`, `<h3>` → `===`, `<table>` → `{| class="wikitable"`,
   `<strong>` → `'''...'''`, `<div class="warning">` → `{{Warning|...}}`.
3. Elindít egy független ügynököt a konverzió ellenőrzésére.
4. Jelenti az eltéréseket és javítja őket.

## 3. példa — Word dokumentum (DOCX) konvertálása

> "Van egy Word dokumentumom itt: `/docs/architecture.docx`. Konvertáld
> MediaWiki-be, és a wikitextet tedd az `output.wiki` fájlba."

A skill elmagyarázza, hogy először `pandoc`-ot vagy `OpenOffice`-ot kell
futtatnod, hogy nyers MediaWiki-formátumú szöveget kapj, majd visszaadod
neki. A skill kitisztítja a pandoc kimenetét, és modern designt alkalmaz.

```bash
# 1. lépés — nyers MediaWiki szöveg kinyerése pandoc-kal
pandoc /docs/architecture.docx -t mediawiki -o /tmp/architecture.wiki

# 2. lépés — fájl odaadása a skillnek
> "Itt van a pandoc kimenet: /tmp/architecture.wiki. Tisztítsd meg és
>  alkalmazd a modern design mintákat a referenciákból."
```

## 4. példa — PowerPoint prezentáció konvertálása

> "Konvertáld ezt a PPTX-et MediaWiki cikkké. Minden dia legyen egy
> szekció."

A skill minden dia címét `==` címsorrá, a tartalmat listákká, a képeket
`[[File:...|thumb]]` hivatkozásokká, a jegyzeteket `{{Note|...}}` formába
alakítja.

## 5. példa — Modern design kártyákkal

> "Csinálj egy landing-page stílusú cikket három egymás melletti feature
> kártyával, egy GYIK accordionnal, és egy hero fejléccel."

A skill TemplateStyles-alapú hero-t, `card-grid` három `.card` divvel, és
`<details>`-alapú GYIK-ot készít. A pontos CSS-t lásd a
[`references/06-design-recipes.md`](../../skills/mediawiki-article-creator/references/06-design-recipes.md)
fájlban.

## 6. példa — MediaWiki saját sablonjainak használata

> "Tedd a megfelelő helyekre a kiemelő dobozokat (`{{Note}}`, `{{Warning}}`,
> `{{Tip}}`) ebben a cikkben."

A skill azonosítja, hogy milyen típusú kiemelés illik az adott helyre, és
hozzáadja őket a megfelelő súlyossággal:

- `{{Note}}` — info, kiegészítő részlet
- `{{Tip}}` — best-practice javaslat
- `{{Warning}}` — figyelmeztetés, lehetséges buktató
- `{{Caution}}` — kritikus, ha figyelmen kívül hagyod, elromlik

## 7. példa — Migráció egyedi wikiből

> "Van egy egyedi wikink 200 oldallal. Segíts migrációs tervet készíteni."

A skill végigvezet a migráción:

1. Leltár a kiterjesztés-specifikus funkciókról (egyedi tagek, magic
   word-ök, Lua modulok) a forrás wikiben.
2. Sablononkénti konverziós térkép építése (mit jelent a `{{MyTemplate}}`
   a standard MediaWiki-ban?).
3. `pandoc` futtatása oldalanként, majd minden kimenet átvezetése a
   skillen tisztításra.
4. Az első 5-10 konvertált oldal ellenőrzése a független ügynökkel.
5. A többi kötegelt feldolgozása.

## 8. példa — Best-practice review

> "Itt van a MediaWiki cikkem piszkozata. Mondd el, mit javítanál design
> és akadálymentesség szempontjából."

A skill a
[`references/02-design-patterns.md`](../../skills/mediawiki-article-creator/references/02-design-patterns.md)
design checklistjét és a
[`references/04-verification-checklist.md`](../../skills/mediawiki-article-creator/references/04-verification-checklist.md)
ellenőrző listáját alkalmazza. Az eredmény egy markdown riport, amely
felsorolja a konkrét szerkesztéseket, sorszámokkal és javasolt
cserékkel.

---

**Tovább:** [Más platformok →](other-platforms.md) · [Vissza a README-hez →](../../README.hu.md)
