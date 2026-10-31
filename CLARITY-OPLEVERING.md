# Oplevering landingpage Landal De Wielsche Dreef

Datum: 26 oktober 2026

Bewerkte bestanden (identieke HTML/CSS/JS-wijzigingen in beide):

- `index-landgoed.html` (canonical, met echte formuliersecties)
- `index-landgoed-zonder-formulieren.html` (plak-versie voor GHL, met formulier-placeholders)

Alles is getest in de browser op 390px (mobiel) en op desktop. Geen console-fouten.

---

## 1. Wijzigingen per punt

### 1. Popup-triggerscript
Eén inline `<script>` onderaan de pagina. Bovenin staat:

```js
var TRIGGER_SELECTOR = '#wd-popup-trigger a, #wd-popup-trigger button, #wd-popup-trigger';
```

Gedrag: elke rondleiding-CTA met de class `js-open-popup` doet bij een klik `preventDefault()` + `stopPropagation()` en klikt vervolgens op `document.querySelector(TRIGGER_SELECTOR)`. Is die trigger er niet, dan scrollt de pagina soepel naar `#aanmelden`. De pagina breekt nooit als de popup of trigger afwezig is (alles in try/catch en met null-checks).

Gekoppeld (shared class `js-open-popup`, 13 elementen in totaal):
- header-knop "Plan uw rondleiding"
- balk-link "Rondleiding" (`.vw-banner-link`)
- hero-hoofdknop
- knop in het park-blok ("Kom langs op ...")
- knop in de villa-sectie ("Kom kijken op ...")
- knop in de interieur-sectie ("Plan uw rondleiding")
- knop in het verkoopweekend-blok ("Plan uw rondleiding *")
- footer-CTA "Plan uw rondleiding"
- 4 nieuwe villa-kaartknoppen
- nieuwe lightbox-knop

Brochure-knoppen (`#brochure`, "Liever eerst de brochure", "Brochure aanvragen") zijn bewust ongemoeid gelaten: die openen de popup niet.

### 2. CTA's waar de aandacht is
- Onder elke `.woning-card` (in `.woning-card-body`) staat nu een echte `<button>` "Bekijk deze villa op het verkoopweekend". Die opent de popup en doet `stopPropagation()`, dus hij triggert niet óók de lightbox van de kaart.
- De hele kaart blijft klikbaar en opent de lightbox voor dat type; klikken op de foto/overlay werkt (de klik bubbelt naar de kaart, alleen de nieuwe knop stopt dat). Getest: knop opent popup zonder lightbox, kaart-klik opent lightbox.
- In de lightbox staat nu een vaste knop "Plan uw rondleiding" onder de foto (in een `.lb-inner` wrapper). Hij is gestyled in forest/mint en goed zichtbaar op de donkere overlay. Bij een klik sluit de lightbox en opent de popup.

### 3. Hero (mobile-first, 390px)
Op het eerste scherm (390px) staan boven de vouw: de kop, de regel "Villa's vanaf € 313.500 excl. btw" (in `.hero-facts`), de datum (in de eyebrow: "Verkoopdagen 30 & 31 oktober 2026 • Eck en Wiel") en de knop "Plan uw rondleiding op 30 & 31 oktober".
- Hero-prijs gewijzigd van **€ 323.500** naar **€ 313.500** (de prijs-segment luidt nu "Villa's vanaf € 313.500 excl. btw"). De prijzen per type op de kaarten (323.500 / 384.500 / 517.500) zijn ongewijzigd.
- Mobiele hero-marges iets ingekort (`.hero-content` padding, h1/p/facts/bouwstatus/vrijblijvend) zodat alles ruim boven de vouw past.

Hoogte-redenering (390x844): header (54px) + balk (~27px) staan bij het laden in de flow; de hero begint op ~81px. Met de ingekorte marges eindigt de hoofdknop ruim binnen de 844px. In de browsertest (390x844) was de hoofdknop volledig zichtbaar met nog ruimte voor de outline-knop eronder.

### 4. Datum uit één constante
De automatische rollover is vervangen door één instelbare constante bovenin het datumscript:

```js
var VERKOOPWEEKEND = '30 & 31 oktober 2026';
```

Die vult `#vw-datum` (balk), `#vw-datum-hero` (eyebrow), `#vw-datum-card` (verkoopweekend-kaart) en de datum in CTA-knoppen die een datum bevatten (daar wordt automatisch het jaartal weggelaten, bijv. "Kom langs op 30 & 31 oktober"). De statische HTML-teksten zijn ook al bijgewerkt, zodat de datum ook zonder JS klopt.

Per cyclus hoeft u dus maar één regel aan te passen. Zie de twijfels-lijst voor de afweging automatisch vs. handmatig.

### 5. Bezwarenblok onder de hero
Direct onder de hero staat nu een blok met iconen + korte tekst (`.bezwaren`). Het vervangt de plek waar de Landal-strip stond (punt 8). Het blok bevat **2** punten (niet 3):
- **Grond in eigendom** (geen erfpacht)
- **Fiscaal voordeel bij aankoop** (btw terug bij verhuur, geen overdrachtsbelasting op nieuwbouw)

Het derde voorgestelde punt "Aankoop privé of via BV" is **weggelaten**: daar is nergens in de bestaande copy onderbouwing voor (zie twijfels-lijst). Eenvoudige inline-SVG-iconen, on-brand gestyled (forest/mint/zand), 1 kolom op mobiel.

### 6. Feitelijke correcties
- **"In aanbouw"-teksten:** de oude strings ("De bouw is in volle gang", "FASE 1 • 54 VILLA'S • NU IN AANBOUW") waren al weg (de hero was al herschreven). In plaats daarvan is de trust-regel **"Open sinds 31 juli 2026. De eerste villa's worden al verhuurd."** toegevoegd in de hero (als `.hero-bouwstatus` met het groene bolletje, net onder de belofte-tekst).
- **FAQ "Wanneer kan ik mijn villa in gebruik nemen?":** de zin "worden op dit moment gebouwd" is herschreven naar "Het park is open sinds 31 juli 2026 en de eerste villa's worden al verhuurd. Niet alle villa's van fase 1 zijn al opgeleverd ...".
- **Zwembad / parkgebouw als GEPLAND:** overal gemarkeerd als nog te bouwen, consistent met de eind-2027-note:
  - park-kenmerk "Subtropisch zwembad" → "Subtropisch zwembad (gepland)" + "in het geplande centrale parkgebouw. Oplevering naar verwachting eind 2027."
  - faciliteiten-sectie: label "Het parkgebouw" → "Het parkgebouw (gepland)"; intro "in het geplande centrale parkgebouw, met oplevering naar verwachting eind 2027. Klik op een impressie ...". De bestaande impressie-note (eind 2027) bleef staan.
- **Verhuurpool / "elke maand uw aandeel":** alle garantie-achtige formuleringen neutraal herschreven (zie OLD vs NEW in de twijfels-lijst). Kern: opbrengsten lopen via een gedeelde verhuurpool, het aandeel hangt af van verhuur en bezetting, geen gegarandeerd bedrag.

### 7. CLS (layout shift)
- Alle content-afbeeldingen hadden al een CSS `aspect-ratio` via hun class (hero is een background-image, geen `<img>`). Het logo had nog geen gereserveerde breedte: `width="150" height="66"` toegevoegd (intrinsieke maat uit de SVG), zodat de browser de ruimte reserveert; `width:auto` in CSS laat de breedte meeschalen met de ingestelde hoogte van 42px.
- Alle below-the-fold afbeeldingen hebben `loading="lazy"` (was al zo); hero-achtergrond blijft eager via de bestaande preload.
- Ruimte voor de sticky kop was al geregeld: zodra de kop sticky wordt, krijgt `body.kop-vast` een `padding-top` gelijk aan de kophoogte, dus de inhoud verspringt niet. Dit is ongewijzigd gelaten.

### 8. Landal-link verwijderd
De volledige "huur-strip" (de div met `<a href="https://www.landal.nl/...">Bekijk het aanbod op Landal.nl</a>`) is verwijderd. Er staat nergens meer een uitgaande link naar Landal. De body-copy noemt nog wel dat Landal de verhuur verzorgt (dat mag, alleen de externe link is weg). Op die plek staat nu het bezwarenblok (punt 5).

### 9. Clarity-events
Alleen binnen het popup-triggerscript toegevoegd (verder niets aan tracking gewijzigd):
- Bij elke rondleiding-CTA-klik: `clarity('event','cta_rondleiding')` + een bron-event `clarity('event','cta_bron_<bron>')`.
- Bij de fallback-scroll (geen trigger gevonden): `clarity('event','popup_fallback_scroll')`.
- Alles via een helper met `if (window.clarity)` + try/catch, dus veilig als Clarity (nog) niet geladen is.

Bron-waarden (`data-bron`): `hero`, `header`, `balk`, `villakaart`, `lightbox`, `verkoopweekendblok` (de zes gevraagde) plus `parkblok`, `villa_sectie`, `interieur`, `footer` voor de overige rondleiding-CTA's (zie twijfels-lijst).

---

## 2. GHL-plaatsing

- De volledige, bijgewerkte pagina gaat in het bestaande custom-code-element. Gebruik hiervoor **`index-landgoed-zonder-formulieren.html`** (dat is de plak-klare versie met de GHL-formulier-placeholders in plaats van echte formulieren). Vervang de volledige inhoud van het custom-code-element door dit bestand.
- **Verborgen GHL-knop (de trigger):** maak in GHL een knop (of een element) met **CSS-ID `wd-popup-trigger`**. Zet die knop zo in dat hij de GHL-popup opent waarin uw GHL-formulier (rondleiding aanmelden) staat. Verberg de knop visueel (bijv. via de GHL-instellingen of custom CSS `#wd-popup-trigger{position:absolute;left:-9999px;}`), maar houd hem klikbaar in de DOM. Het script klikt deze knop aan zodra een bezoeker op een rondleiding-CTA klikt.
- **De popup zelf** is een GHL-popup/modal met daarin uw GHL-rondleidingformulier. De trigger-knop (`#wd-popup-trigger`) is het enige wat het script aanstuurt; de rest regelt GHL.
- **Fallback:** als er (nog) geen `#wd-popup-trigger` is, scrollen de CTA's netjes naar de sectie met id `#aanmelden`. Zorg dus dat de GHL-sectie met het aanmeldformulier het id/Custom-ID `aanmelden` houdt (en de brochure-sectie `brochure`), net als nu.
- De formulier-placeholders in `index-landgoed.html` (de secties `#aanmelden` en `#brochure`) zijn alleen voor lokaal testen; die horen niet in GHL (daar leveren de GHL-secties die anchors).

---

## 3. Testlijst

### Mobiel (390px)
- [ ] Hero toont boven de vouw: kop, "Villa's vanaf € 313.500 excl. btw", datum (eyebrow 30 & 31 oktober 2026) en de knop "Plan uw rondleiding". (Geverifieerd in browsertest.)
- [ ] Datum staat overal goed: balk, eyebrow, verkoopweekend-kaart, en in de datum-knoppen (zonder jaartal).
- [ ] Elke rondleiding-CTA opent de GHL-popup (header, balk, hero, park, villa-sectie, interieur, verkoopweekend-blok, footer, 4 villa-kaartknoppen, lightbox-knop).
- [ ] Zonder `#wd-popup-trigger`: elke CTA scrollt soepel naar `#aanmelden`. (Geverifieerd: fallback actief.)
- [ ] Villa-kaartknop opent de popup; de rest van de kaart opent de lightbox. (Geverifieerd.)
- [ ] Lightbox-knop "Plan uw rondleiding" is zichtbaar onder de foto en opent de popup (en sluit de lightbox). (Geverifieerd.)
- [ ] Brochure-knoppen openen NIET de popup.
- [ ] Geen console-fouten. (Geverifieerd.)

### Desktop
- [ ] Zelfde CTA- en popup-checks als hierboven.
- [ ] Villa-kaarten: hover + klik openen de lightbox; de knop onderin de kaart opent de popup zonder de lightbox.
- [ ] Lightbox: pijltjes, sluiten (x), Escape en achtergrond-klik werken nog; nieuwe knop onder de foto werkt.
- [ ] Clarity-events verschijnen in Clarity (cta_rondleiding + cta_bron_* ; popup_fallback_scroll bij fallback).
- [ ] Geen layout-shift bij het sticky worden van de kop; afbeeldingen reserveren hun ruimte.

---

## 4. Twijfels / claims

### Weggelaten bezwaren-punt
- **"Aankoop privé of via BV"** is weggelaten. Reden: nergens in de bestaande copy of FAQ wordt iets gezegd over aankoop privé versus via een BV/rechtspersoon (grep op "BV", "privé", "zakelijk", "rechtspersoon" leverde niets op). Conform de opdracht (geen nieuwe claims toevoegen) is dit punt geschrapt. Wilt u het terug? Lever dan één onderbouwende zin aan (bijv. in de FAQ), dan kan het als derde bezwaar-item terug.

### Datum-constante vs. automatische rollover
- De automatische 3-weken-rollover is bewust vervangen door één constante (`VERKOOPWEEKEND`). Voordeel: u bepaalt exact welke datum er staat (de rollover kon een datum tonen die niet overeenkwam met een gepland weekend). Nadeel: u moet nu elke cyclus zelf één regel aanpassen. Wilt u liever automatisch doorrollen, dan kan het oude script terug; zeg het en ik zet het terug (eventueel met de juiste ankerdatum/cyclus).

### OLD vs NEW: verhuurpool / "elke maand uw aandeel"
Standaard is de neutrale (NEW) versie geïmplementeerd. Kiest u liever anders, laat het weten.

**1. Rendement-intro (boven de checklist)**
- OLD: "Wanneer u uw villa niet gebruikt, wordt deze professioneel verhuurd via Landal. U ontvangt maandelijks uw aandeel uit de verhuurpool, terwijl beheer, marketing en gastenservice voor u worden verzorgd."
- NEW: "Wanneer u uw villa niet gebruikt, wordt deze professioneel verhuurd via Landal. De opbrengsten lopen via een gedeelde verhuurpool: uw aandeel hangt af van de verhuur en de bezetting. Beheer, marketing en gastenservice worden voor u verzorgd."

**2. Rendement-checklist-item**
- OLD: titel "Maandelijkse verhuurinkomsten" + "Via de verhuurpool van Landal ontvangt u elke maand uw aandeel, ongeacht de bezetting van uw villa."
- NEW: titel "Inkomsten uit de verhuurpool" + "Uw villa draait mee in een gedeelde verhuurpool van Landal. Uw aandeel hangt af van de verhuuropbrengsten en de bezetting; er is geen gegarandeerd bedrag."

**3. Rendement-aside (groen kader)**
- OLD: "... Bij aankomst ligt de tuin er netjes bij en is de villa schoon. En u ontvangt elke maand uw aandeel uit de verhuur."
- NEW: "... Bij aankomst ligt de tuin er netjes bij en is de villa schoon. Uw villa draait mee in de gedeelde verhuurpool; uw aandeel hangt af van de verhuur en de bezetting."

**4. FAQ "Wie regelt de verhuur?"**
- OLD: "Landal neemt boekingen, gasten, schoonmaak en beheer volledig over. U ontvangt maandelijks uw aandeel uit de verhuurpool, ongeacht de bezetting van uw eigen villa."
- NEW: "Landal neemt boekingen, gasten, schoonmaak en beheer volledig over. De opbrengsten lopen via een gedeelde verhuurpool; uw aandeel hangt af van de verhuur en de bezetting."

### Gemarkeerde / verzachte claims (faciliteiten)
- "Subtropisch zwembad" (park-kenmerk) is nu "Subtropisch zwembad (gepland)" + verwijzing naar oplevering eind 2027.
- Faciliteiten-sectie: label "Het parkgebouw" → "Het parkgebouw (gepland)"; intro meldt nu expliciet "geplande centrale parkgebouw" + "oplevering naar verwachting eind 2027" en spreekt van "impressie" in plaats van "beeld".
- Kop en h2 van de faciliteiten-sectie ("Alles wat u nodig hebt, op loopafstand") zijn ongewijzigd; met het toegevoegde "(gepland)" in label en intro is de status nu duidelijk. Wilt u ook de h2 aanpassen (bijv. "... op loopafstand, zodra het parkgebouw er staat"), dan hoor ik het.

### Overige aandachtspunten
- **Palet uit sync tussen de twee bestanden.** `index-landgoed.html` gebruikt het nieuwe forest/mint/zand/goud-palet (in de code gemarkeerd als "ontwerpvoorstel demo"); de GHL-plak-versie `index-landgoed-zonder-formulieren.html` gebruikt nog het oudere teal-palet (#00857A) en heeft geen `--gold`/`--gold-deep` en geen gouden focus-outline. Ik heb de paletten NIET aangepast (buiten de opdracht van punten 1-9 en mogelijk bewust: de plak-versie is de live-versie). Alle nieuwe elementen gebruiken alleen variabelen die in beide bestanden bestaan (`--teal`, `--teal-light`, `--cream`, `--border`, `--text`, `--text-muted`, `--white`), dus beide renderen correct in hun eigen stijl. **Let op:** als u wilt dat de live GHL-pagina er net zo uitziet als het ontwerpvoorstel, moet het forest-palet nog naar de plak-versie worden overgezet. Zeg het, dan sync ik de paletten.
- **"geruststellend rendement"** staat nog in de rendement-kop ("... een professioneel verhuurconcept met een geruststellend rendement."). Dit bevat niet de frase "elke maand uw aandeel", dus ik heb het conform de opdracht laten staan. Het kan echter als rendementsbelofte worden gelezen; overweeg "... met een professioneel verhuurconcept" (zonder "geruststellend rendement"). Ik pas het graag aan op uw woord.
- **Bron-events buiten de zes genoemde.** De opdracht noemde zes bron-waarden; er zijn echter meer rondleiding-CTA's (park, villa-sectie, interieur, footer). Die hebben elk een eigen, duidelijk benoemde bron gekregen (`cta_bron_parkblok`, `cta_bron_villa_sectie`, `cta_bron_interieur`, `cta_bron_footer`) voor volledige attributie. Wilt u strikt alleen de zes, dan kan ik de overige CTA's op een van die zes laten mappen.
- **"Vrijblijvend. Een adviseur belt u binnen één werkdag ..."** (hero) en "* Een adviseur belt u op om de afspraak in te plannen." zijn ongewijzigd. Bevestig graag dat "binnen één werkdag" klopt; zo niet, dan pas ik de termijn aan.
