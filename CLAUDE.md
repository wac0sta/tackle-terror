# Tackle Terror!

Een 2D voetbal-renspel (side-scroller), gemaakt door een vader en zoon (14). De speler rent automatisch, verzamelt ballen, ontwijkt tackles en sluit elk level af met een penalty. Uitleg voor spelers en een wijzigingslog staan in `README.md`.

## Taal en toon
- Alle tekst in het spel, de README, de commentaren in de code en de commitberichten zijn in het **Nederlands**.
- Houd uitleg begrijpelijk voor een 14-jarige: de README en het blok INSTELLINGEN zijn bedoeld om zelf mee te knutselen.

## Opbouw
- Alles zit in **één bestand**: `index.html` (HTML, CSS en JS in een IIFE met `'use strict'`).
- Geen externe afbeeldingen of geluidsbestanden: alles wordt op een `<canvas>` getekend en geluid wordt live gemaakt met de Web Audio API. Houd dat zo.
- Externe bronnen zijn alleen Google Fonts (Bungee en Barlow Condensed, met fallback-lettertypen) en de Firebase-SDK voor de ranglijst. Het spel moet ook zonder die twee speelbaar blijven.
- Er is een vaste logische resolutie van 960 × 540, die geschaald wordt naar de schermgrootte.
- De spelstatus `state` is een van: `menu`, `clMenu`, `intro`, `play`, `red`, `penalty`, `levelEnd`, `over`, `win`, `ranking` of `paused`. Er is één `update(dt)` en één `render()` per frame.
- `mode` is `league` (gewone competitie, `LEVELS`/`PLAYERS`) of `cl` (Champions League, `CL_LEVELS`/`CL_PLAYERS`). Gebruik `levels()`, `lvl()` en `lvlLabel()` in plaats van `LEVELS[lvlIdx]`. De Champions League komt vrij na het kampioenschap (sleutel `tackle-terror-cl`).
- Bovenaan het script staat het blok **INSTELLINGEN** (`PLAYERS`, `LEVELS`, `CL_PLAYERS`, `CL_LEVELS`, `OPP`, `REF`, `KEEPER`, `ADS`, `ACHTERGRONDGELUID`, `HYMNE_VOLUME`, `VERBODEN_DEEL`, `VERBODEN_WOORD`). Nieuwe instelbare waarden horen daar.
- Knoppen op het canvas worden elke frame opnieuw geregistreerd via `button()` / `buttons.push()`.

## Ranglijst en opslag
- Er zijn drie opslagplekken via één adapter `store` (`subscribe`, `add`, `getAll`). De volgorde is:
  1. **Firebase Firestore** (project `tackle-terror`, collectie `scores`). De SDK wordt dynamisch geladen van gstatic (`FIREBASE_VERSION`). De config staat in `FIREBASE_CONFIG` en is niet geheim.
  2. **Artifact-database** via `window.claude.use('db')` (als Firebase niet bereikbaar is, bijvoorbeeld binnen een artifact).
  3. **`localStorage`**.
- De Firestore-regels (in de Firebase-console, niet in de repo) staan alleen `create` toe, met exact de velden `name` (1 tot 16 tekens), `score` (int, 0 tot 5000), `speler` (≤ 20 tekens), `level` (int, 1 tot 3), `kampioen` (bool) en `datum` (== request.time, dus `serverTimestamp()`). Lezen mag iedereen, `update` en `delete` mag niemand. **Verander je de velden van een score, pas dan ook de regels aan**, anders wordt opslaan geweigerd.
- Champions League-scores passen binnen die regels: `speler` begint met `CL ` (dus CL-spelernamen ≤ 17 tekens) en `level` is 1 of 2 binnen de CL. Herkennen met `isCLRow()`.
- De live lijst (`ranking`, `RANK_LIMIT`) is alleen de top; voor bladeren haalt `refreshFull()` alle scores op (`fullRows`). `allRows()` voegt ze samen, filtert verboden namen (`isBadName()`) en splitst per tabblad.
- Er is geen lokaal record meer (`doorbraak-hi` wordt niet meer gebruikt, maar niet verwijderd).
- Testen kan met de REST API: `GET https://firestore.googleapis.com/v1/projects/tackle-terror/databases/(default)/documents/scores?key=<apiKey>`. Schrijf geen testscores naar de echte ranglijst, want die zijn alleen via de console te verwijderen.
- **Hernoem de opslagsleutels nooit** (`doorbraak-hi`, `doorbraak-naam`, `doorbraak-ranglijst`) en ook de artifact-collectie niet. Dat is de oude naam van het spel en zo blijven bestaande scores bewaard.
- `window.__doorbraak` onderaan is een testhaak.

## Controleren
Er is geen build of testsuite. Na een wijziging minstens de syntax controleren:

```
node -e "const h=require('fs').readFileSync('index.html','utf8');const s=h.split('<script>')[1].split('</script>')[0];new Function(s);console.log('syntax ok')"
```

Speel het spel daarna in een browser als het kan, of vraag de gebruiker om het te testen. Werkt het spel ook nog met touch (tablet of telefoon)? Daar zit aparte invoercode voor (`pointerdown` en de variabele `coarse`).

## Werkwijze
- **Commit en push geregeld naar GitHub** (`origin` = github.com/wac0sta/tackle-terror, branch `main`). Doe dat na elke afgeronde, werkende wijziging, zonder telkens toestemming te vragen. Gebruik korte Nederlandse commitberichten.
- Werk bij elke nieuwe versie het **wijzigingslog** in `README.md` bij ("Versie N: …"), en zo nodig de tabellen met spelregels en besturing.
- Kleine, gerichte wijzigingen hebben de voorkeur. Leg kort uit wat er voor de speler verandert.

## Openstaande ideeën (uit de code-review)
- De keeper redt bij een "verkeerde gok" een schot door het midden alsnog (spiegeling `960 - target` in `shoot()`).
- Modder geeft −5 punten, ook tijdens de turbo.
- "Langste combo" op het levelscherm geldt voor het hele potje, niet per level.
- Er is geen opbouw in tempo binnen een level.
- Er is geen knop voor volledig scherm op mobiel.
- Met spatie op het eindscherm sla je het invullen van je naam over (op het kampioensscherm gaat spatie naar de Champions League).
- Ranglijst: toon per naam alleen de beste score en begrens de lokale lijst.
- Het namenfilter werkt alleen in het spel; de Firestore-regels controleren geen woorden.
- De moeilijkheid van de Champions League-levels is nog niet uitgebreid getest.
- Snelheid: het publiek en de verlopen worden elke frame opnieuw getekend (vooraf tekenen als het op een tablet hapert).
