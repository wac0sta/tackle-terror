# Tackle Terror! ⚽

Een 2D voetbal-renspel in de stijl van een side-scroller. Je kiest een speler, rent automatisch van links naar rechts over het veld, verzamelt ballen, ontwijkt tackles en sluit elk level af met een penalty.

Het hele spel zit in één bestand (`index.html`): HTML, CSS en JavaScript, zonder externe afbeeldingen of geluidsbestanden. Alle graphics worden getekend op een `<canvas>` en alle geluiden worden live gemaakt met de Web Audio API.

Gemaakt door een vader en zoon (14), samen met Claude.

## Spelen

Open `index.html` in een browser. Er is geen installatie of server nodig.

| Actie | Toetsenbord | Tablet / telefoon |
|---|---|---|
| Springen (houd vast voor hoger) | Spatie, ↑ of W | Tik rechts op het veld |
| Sliding | ↓ of S | Houd links ingedrukt |
| Penalty mikken | ↑ en ↓ | Tik op de plek in het doel |
| Penalty schieten | Spatie of Enter | Tik |
| Pauze | P of Esc | Knop onder het spel |
| Geluid aan/uit | M | Knop onder het spel |
| Ranglijst openen | R (menu en eindscherm) | Knop "Ranglijst" |
| Ranglijst bladeren | ← en → | Pijlen naast de lijst |
| Wisselen tussen competitie en Champions League in de ranglijst | Tab | Knoppen bovenaan |
| Champions League openen (na het kampioenschap) | L | Knop "Champions League" |

## Spelregels

**Spelers.** Er zijn drie spelers, elk met een eigen kracht. Ze krijgen allemaal evenveel punten per bal, zodat je met elke speler even hoog kunt scoren:

| Speler | Kracht | Effect |
|---|---|---|
| Rik Raket (#7) | Snel | Rent 10% sneller, turboschoenen werken twee keer zo lang |
| Bram de Muur (#4) | Sterk | Begint elk level met een schild dat één tackle opvangt |
| Joey Veer (#11) | Springer | Kan in de lucht nog een keer springen |

**Obstakels.**

| Obstakel | Hoe ontwijk je het | Gevolg bij botsing |
|---|---|---|
| Sliding tackle | Springen | Kaart |
| Vliegende tegenstander (hoge trap) | Sliding | Kaart |
| Pion | Springen | Kaart |
| Scheidsrechter (level 2 en 3) | Springen | Kaart |
| Modderplas (level 2 en 3) | Springen | −5 punten, combo weg |

De eerste botsing in een level geeft een gele kaart, de tweede een rode kaart en dan is het game over. Kaarten worden per level weer op nul gezet.

**Punten verzamelen.**

| Item | Effect |
|---|---|
| Bal | 1 punt |
| Gouden bal | 5 punten |
| Combo van 10 ballen achter elkaar | +5 bonus en juichend publiek |
| Tegenstander wegbeuken met turbo | +3 punten |
| Penalty raak | +25 / +50 / +75 punten (level 1 / 2 / 3), in de Champions League +100 / +150 |

**Power-ups.**

| Power-up | Duur | Effect |
|---|---|---|
| Turboschoenen | 5 sec (Rik 10, Kiki 7,5) | Sneller en onkwetsbaar, je beukt tegenstanders weg |
| Aanvoerdersband | Tot de eerste tackle | Schild dat één tackle opvangt |
| Ballenmagneet | 8 sec | Trekt ballen in de buurt naar je toe |

**Levels.**

| Level | Naam | Lengte | Snelheid | Extra |
|---|---|---|---|---|
| 1 | Trainingsveld | 7000 | 340 | Rustig publiek langs de lijn |
| 2 | Dorpsclub in de regen | 8500 | 400 | Regen, modder, scheidsrechter, kantine |
| 3 | Stadion bij avond | 10000 | 460 | Lichtmasten, volle tribunes |

Aan het eind van elk level volgt een penalty. Het richtkruis beweegt van links naar rechts; met ↑ en ↓ kies je de hoogte. Hoog in de hoek is het lastigst voor de keeper.

**Champions League.** Ben je kampioen geworden (alle drie de levels gehaald), dan komt de Champions League vrij. Je ziet dan een knop op het eindscherm en in het menu. Daar speel je met een van twee topspelers twee hele moeilijke levels:

| Speler | Kracht | Effect |
|---|---|---|
| Kiki Bliksem (#10) | Bliksem | Iets sneller, dubbele sprong, turbo werkt langer |
| Harry Kanon (#9) | Kanon | Begint met een schild, iets trager, de keeper gokt bij zijn penalty's vaker fout |

| Ronde | Naam | Lengte | Snelheid | Extra |
|---|---|---|---|---|
| Halve finale | Europese avond in de stromende regen | 11000 | 520 | Regen, donker, snellere tackles |
| Finale | Het grootste stadion van Europa | 12000 | 570 | Nog snellere tackles, veel scheidsrechters |

Wie de finale wint, wordt **Europees kampioen** en krijgt de zilveren beker. Het spel onthoudt in de browser dat je de Champions League hebt vrijgespeeld.

## Ranglijst en opslag

Na elk potje kun je je naam invullen en je score op de ranglijst zetten. De ranglijst heeft twee tabbladen: **Competitie** en **Champions League**. Je ziet 10 scores per pagina, met medailles voor de eerste drie, en met de pijlen blader je terug door alle scores.

Namen met scheldwoorden of foute woorden (ook geschreven als `k4nk3r` of `k.a.n.k.e.r`) worden geweigerd. De lijst met verboden woorden staat in INSTELLINGEN (`VERBODEN_DEEL` en `VERBODEN_WOORD`). Oude scores met zo'n naam worden niet meer getoond.

Waar de scores worden bewaard, hangt af van waar het spel draait:

- **Online via Firebase** (normaal, bijvoorbeeld via GitHub Pages of als je het bestand los opent met internet): iedereen ziet dezelfde ranglijst, op elke computer, tablet of telefoon. Nieuwe scores verschijnen meteen.
- **Als Claude-artifact**: als Firebase daar niet bereikbaar is, gebruikt het spel de database van het artifact.
- **Zonder internet**: in de `localStorage` van de browser. Dan staat er onder de ranglijst de melding dat de scores alleen in die browser bewaard worden.

Via "Download als bestand" kun je alle scores als CSV exporteren (te openen in Excel).

**Firebase beheren.** De scores staan in het Firebase-project `tackle-terror` (https://console.firebase.google.com), onder Firestore Database, collectie `scores`. Daar kun je ook valse of foute scores verwijderen. De beveiligingsregels zorgen dat het spel alleen nieuwe, netjes ingevulde scores mag toevoegen (naam van 1 tot 16 tekens, 0 tot 5000 punten, level 1 tot 3, datum van de server) en dat niemand via het spel scores kan wijzigen of wissen.

Champions League-scores herken je in Firebase aan `CL ` voor de spelernaam (bijvoorbeeld `CL Kiki Bliksem`), met `level` 1 (halve finale) of 2 (finale). Zo passen ze binnen de bestaande regels.

## Zelf aanpassen

Bovenaan het `<script>`-blok in `index.html` staat een blok **INSTELLINGEN**. Daar kun je zonder veel programmeerkennis dingen veranderen:

- `PLAYERS`: namen, rugnummers, shirt-, broek-, sok-, huid- en haarkleur, en de krachten (`speedMul`, `scoreMul`, `turboMul`, `doubleJump`, `shieldStart`).
- `LEVELS`: lengte van het veld, snelheid, tijd tussen obstakels (`gap`), kleuren van lucht en gras, regen, nacht, welke obstakels voorkomen, en de penalty (`bonus`, `keeperFout`, `mikSnelheid`, `keeperSnelheid`).
- `CL_PLAYERS` en `CL_LEVELS`: de spelers en levels van de Champions League (met `tackleMul` voor snellere tackles en `penaltyKiller`).
- `VERBODEN_DEEL` en `VERBODEN_WOORD`: namen die niet op de ranglijst mogen.
- `OPP`, `REF`, `KEEPER`: de kleuren van tegenstanders, scheidsrechter en keeper.
- `ADS`: de teksten op de reclameborden.

Andere handige plekken in de code:

- `JUMP_V`, `JUMP2_V`, `GRAV`: hoe hoog en hoe zwaar de speler springt.
- `updatePenalty()`: hoe groot het bereik van de keeper is.
- `SFX` en `whistleBlast()`: alle geluidseffecten.
- `startAmbience()` en `updateAmbience()`: het stadiongeluid.

## Opbouw van de code

| Onderdeel | Functies |
|---|---|
| Instellingen | `PLAYERS`, `LEVELS`, `CL_PLAYERS`, `CL_LEVELS`, `OPP`, `REF`, `KEEPER`, `ADS`, `VERBODEN_DEEL` |
| Geluid | `audio()`, `tone()`, `noise()`, `whistleBlast()`, `startAmbience()`, `SFX` |
| Level bouwen | `genLevel()`, `addObstacle()`, `addBall()` |
| Spelverloop | `newGame()`, `startLevel()`, `update()`, `updatePlay()`, `collide()` |
| Penalty | `startPenalty()`, `updatePenalty()`, `shoot()`, `drawPenalty()` |
| Ranglijst | `saveScore()`, `isBadName()`, `subscribeRanking()`, `allRows()`, `refreshFull()`, `drawRanking()`, `downloadRanking()` |
| Tekenen | `drawFootballer()`, `drawBall()`, `drawBackground()`, `drawHUD()`, `drawMenu()`, `drawCLMenu()`, `drawCards()`, `drawEnd()` |
| Invoer | `keydown`/`keyup`-listeners, `pointerdown`/`pointerup` op het canvas |

Het spel werkt met een vaste logische resolutie van 960 × 540 pixels die wordt geschaald naar de schermgrootte. De spelstatus (`state`) is een van: `menu`, `clMenu`, `intro`, `play`, `red`, `penalty`, `levelEnd`, `over`, `win`, `ranking` of `paused`. De variabele `mode` is `league` (competitie) of `cl` (Champions League).

## Wijzigingslog

### Versie 10: Champions League
- **Eerlijkere spelers.** Rik Raket kreeg 1,5× punten per bal en won daardoor bijna altijd. Nu krijgt iedereen evenveel punten. Rik rent 10% sneller (was 15%) en zijn turboschoenen werken twee keer zo lang.
- **Champions League.** Na het kampioenschap komt een nieuwe spelmodus vrij met twee topspelers (Kiki Bliksem en Harry Kanon) en twee hele moeilijke levels: de halve finale en de finale. Wie wint, is Europees kampioen.
- **Ranglijst.** Aparte tabbladen voor de competitie en de Champions League, en bladeren door alle scores (10 per pagina). Na het opslaan opent de ranglijst op de pagina met jouw score.
- **Namenfilter.** Scheldwoorden en foute woorden worden geweigerd als naam, en oude scores met zo'n naam worden verborgen.
- Het lokale record ("Record op dit apparaat") is weg, want de online ranglijst houdt alles bij.

### Versie 9: makers
- Onder de besturingsinstructies staat nu "Gemaakt door Sverre, Tim en Claude".

### Versie 8: achtergrondgeluid uit
- Het doorlopende stadiongeluid (publiek en klapritme) staat voorlopig uit. De geluidseffecten, het gejuich bij combo's en goals en de fluit blijven. Weer aanzetten kan met `ACHTERGRONDGELUID = true` in het blok INSTELLINGEN.

### Versie 7: online ranglijst met Firebase
- Scores worden nu online bewaard in Firebase (Firestore). Iedereen ziet dezelfde ranglijst, ook op GitHub Pages.
- Lukt Firebase niet, dan valt het spel terug op de artifact-database en daarna op de browseropslag.
- Een score wordt nooit dubbel opgeslagen als het opslaan een tweede keer geprobeerd wordt.
- "Download als bestand" werkt nu ook buiten een Claude-artifact.

### Versie 6: eerlijkere penalty's en turbo
- Een penalty schiet niet meer per ongeluk: de eerste halve seconde telt geen invoer, zodat een late sprong niet meteen schiet. Op touch schiet je alleen nog met een tik in het doel.
- Als de turboschoenen aflopen, ben je nog een halve seconde onkwetsbaar (je knippert even). Zo krijg je geen kaart van een tegenstander die op dat moment toevallig op je staat.
- Punten zijn altijd hele getallen: Rik krijgt nu +8 voor een gouden bal (was +7,5) en +5 voor wegbeuken (was +4,5).

### Versie 5: nieuwe naam
- Het spel heet nu **Tackle Terror!** (was: Doorbraak!). Aangepast in het titelscherm, de paginatitel, de schermlezer-beschrijving en de bestandsnaam van de geëxporteerde ranglijst (`tackle-terror-ranglijst.csv`).
- De opslagsleutels in de browser (`doorbraak-hi`, `doorbraak-naam`, `doorbraak-ranglijst`) en de ranglijst-database zijn bewust niet hernoemd, zodat bestaande records en scores bewaard blijven.

### Versie 4: penalty-hoogte en ranglijst
- Penalty's kunnen nu hoog of laag geplaatst worden met ↑ en ↓ (op touch: tikken op de plek in het doel). Hoog in de hoek verkleint het bereik van de keeper.
- Pijltje omhoog schiet niet meer bij de penalty; schieten gaat met spatie of Enter.
- Na elk potje kun je je naam invullen en je score op de ranglijst zetten. Je ziet meteen je plek.
- Nieuw scherm "Ranglijst" met top 10, medailles, gebruikte speler en bereikt level. Bereikbaar via het menu, het eindscherm en de toets R.
- Scores worden opgeslagen in de database van het artifact; zonder database valt het spel terug op browseropslag.
- Knop "Download als bestand" exporteert de ranglijst als CSV.
- Eindscherm opnieuw ingedeeld met drie knoppen: Opnieuw spelen, Andere speler, Ranglijst.

### Versie 3: makkelijkere penalty's, echte sliding, sfeer en fluit
- Penalty's in level 2 en 3 makkelijker: trager richtkruis, tragere duik van de keeper, kleiner bereik en vaker een verkeerde gok.
- Pijltje omlaag maakt nu een echte sliding (naar achteren leunen met de benen vooruit) in plaats van naar voren bukken, met opspattend gras.
- Levels langer gemaakt: 5200 → 7000, 6400 → 8500, 7600 → 10000.
- Doorlopend stadiongeluid toegevoegd: publiek dat aanzwelt en wegzakt, met af en toe een klapritme. Zachter op het trainingsveld, het hardst in het stadion. Extra gejuich bij combo's en goals.
- Piepjes vervangen door een echte scheidsrechtersfluit met triller: één fluit bij geel, twee kort en één lang bij rood, dubbele fluit bij de aftrap.

### Versie 2: eerste speelbare versie
- Keuzescherm met drie spelers, elk met een eigen kracht.
- Automatisch rennen met springen (variabele hoogte) en duiken.
- Obstakels: sliding tackles, hoge trappen, pionnen, scheidsrechter, modder.
- Ballen, gouden ballen, combo's en drie power-ups.
- Gele en rode kaart als levenssysteem.
- Drie levels met eigen sfeer: trainingsveld, dorpsclub in de regen, stadion bij avond.
- Penalty-minigame aan het eind van elk level.
- Scorebord, voortgangsbalk naar het doel, highscore in de browser.
- Besturing met toetsenbord en touch, pauze en geluid aan/uit.

### Versie 1: briefing
Het oorspronkelijke idee: een 2D-spel zoals een side-scroller, je begint links en gaat vanzelf door het speelveld. Voetbalstijl, eerst een speler kiezen, ballen verzamelen en tackles ontwijken. Aangevuld met ideeën voor spelerskrachten, power-ups, kaarten als levens, een penalty aan het eind van elk level en levels met een eigen sfeer.
