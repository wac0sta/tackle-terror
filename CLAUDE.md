# Tackle Terror!

Een 2D voetbal-renspel (side-scroller), gemaakt door een vader en zoon (14). De speler rent automatisch, verzamelt ballen, ontwijkt tackles en sluit elk level af met een penalty. Uitleg voor spelers en een wijzigingslog staan in `README.md`.

## Taal en toon
- Alle tekst in het spel, de README, de commentaren in de code en de commitberichten zijn in het **Nederlands**.
- Houd uitleg begrijpelijk voor een 14-jarige: de README en het blok INSTELLINGEN zijn bedoeld om zelf mee te knutselen.

## Opbouw
- Alles zit in **één bestand**: `index.html` (HTML, CSS en JS in een IIFE met `'use strict'`).
- Geen externe afbeeldingen of geluidsbestanden: alles wordt op een `<canvas>` getekend en geluid wordt live gemaakt met de Web Audio API. Houd dat zo.
- De enige externe bron is Google Fonts (Bungee en Barlow Condensed), met fallback-lettertypen.
- Er is een vaste logische resolutie van 960 × 540, die geschaald wordt naar de schermgrootte.
- De spelstatus `state` is een van: `menu`, `intro`, `play`, `red`, `penalty`, `levelEnd`, `over`, `win`, `ranking` of `paused`. Er is één `update(dt)` en één `render()` per frame.
- Bovenaan het script staat het blok **INSTELLINGEN** (`PLAYERS`, `LEVELS`, `OPP`, `REF`, `KEEPER`, `ADS`). Nieuwe instelbare waarden horen daar.
- Knoppen op het canvas worden elke frame opnieuw geregistreerd via `button()` / `buttons.push()`.

## Ranglijst en opslag
- Als het spel als Claude-artifact draait, gaan de scores via `window.claude.use('db')` naar de collectie `scores`. Downloads lopen via `window.claude.use('downloads')`.
- Zonder artifact valt het spel terug op `localStorage`.
- **Hernoem de opslagsleutels nooit** (`doorbraak-hi`, `doorbraak-naam`, `doorbraak-ranglijst`) en ook de databasecollectie niet. Dat is de oude naam van het spel en zo blijven bestaande scores bewaard.
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
- Met spatie op het eindscherm sla je het invullen van je naam over.
- Ranglijst: toon per naam alleen de beste score en begrens de lokale lijst.
- Snelheid: het publiek en de verlopen worden elke frame opnieuw getekend (vooraf tekenen als het op een tablet hapert).
- Opruimen: `ctx.measureText(p.kracht).width * 0 + 86` in `drawMenu()`.
