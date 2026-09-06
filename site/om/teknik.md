---
title: Hur ser det ut bakom kulisserna?
---

Lagen.nu består av hundratusentals sammanlänkade dokument. För att kunna hantera
en så stor informationsmängd med mycket begränsade resurser är nästan allt
automatiserat.

Varje källa (SFS, domstolsavgöranden, förarbeten, EU-rätt, folkrätt,
myndighetsföreskrifter med flera) har en egen bearbetningskedja i fyra steg:

1. **Hämtning** -- råmaterialet laddas ner från källan (Regeringskansliets
   rättsdatabaser, domstolarnas publiceringstjänst, regeringen.se, EUR-Lex,
   Europadomstolens HUDOC, myndigheternas egna webbplatser, m.fl.). Hämtningen
   är inkrementell: den går bakåt tills den träffar sådant som redan finns.
2. **Tolkning** -- den oformatterade texten (HTML, PDF eller Word) analyseras och
   struktureras: kapitel, paragrafer, stycken, rubriker och listor identifieras,
   och hänvisningar till andra lagar, rättsfall och förarbeten känns igen och
   länkas.
3. **Artefakt** -- resultatet sparas som en **JSON-fil per dokument**. Den filen
   är källan till sanning för allt som utvunnits ur dokumentet (struktur,
   metadata och länkar).
4. **Härledning och publicering** -- ur alla JSON-artefakter byggs en katalog
   (citeringsgrafen, dvs. vilka dokument som hänvisar till vilka), ett sökindex,
   bulknedladdningar och slutligen de statiska, sammanlänkade webbsidorna.

Varje steg är i sig inkrementellt -- ett dokument som inte ändrats tolkas inte
om, och en sida vars underlag inte ändrats renderas inte om. Det är vad som gör
det möjligt att köra om hela kedjan varje natt.

Den viktigaste funktionen -- att bredvid varje paragraf visa vilka rättsfall och
förarbeten som hänvisar till den -- kommer ur citeringsgrafen som byggs i steg 4.

## Att känna igen en hänvisning

Hänvisningar i löpande text ("enligt 4 kap. 3 § andra stycket lagen (1960:729)
om upphovsrätt", "jfr NJA 2015 s. 3", "artikel 6.1 c i dataskyddsförordningen")
läses av en grammatik snarare än av mönstermatchning. Det gör att en hänvisning
kan lösas upp ända ner till rätt stycke eller punkt, och att den kan avvisas när
den inte går ihop, istället för att bli en gissning. Samma grammatik används på
alla källor, vilket är skälet till att ett rättsfall, ett förarbete och en
myndighetsföreskrift alla kan peka på exakt samma paragraf.

Ett dokument som citeras men som inte finns i tjänsten länkas inte -- texten
lämnas som den är.

## Sök

Sökmotorn indexerar varje paragraf, artikel och avsnitt för sig, så att en
träff kan peka på stället i dokumentet och inte bara på dokumentet. En sökning
som ser ut som en hänvisning besvaras dessutom direkt med det lagrum den pekar
på.

## Publicering

Webbplatsen serveras som statiska sidor, och samma process driver även ett
[REST-API och bulknedladdningar](/om/for-utvecklare) av hela materialet. Sidorna
laddar inga resurser från tredje part -- typsnitt, skript och stilmallar
levereras från samma server, och det finns varken mätverktyg eller inbäddat
innehåll utifrån.

Kommentarerna och begreppsförklaringarna skrivs som versionshanterad markdown i
ett separat innehållsförråd.

## Maskinellt utläst material

De mest värdefulla kopplingarna är de som ingen har skrivit ner i strukturerad
form. De står i klartext i förarbetena, och läses därifrån -- i två fall med
hjälp av en språkmodell:

- **Vilken direktivartikel en paragraf genomför**, utläst ur propositionens
  författningskommentar.
- **Vilken paragraf i en ny lag som motsvarar vilken i den gamla.** Där
  propositionen har en jämförelsetabell läses den mekaniskt; annars läses
  motsvarigheterna ur författningskommentaren. Varje kant kontrolleras mot båda
  lagarnas faktiska paragrafförteckningar, så en motsvarighet till en paragraf
  som inte finns avvisas istället för att publiceras.

Sådant visas alltid tillsammans med den källa det lästs ur, så att det går att
kontrollera, och kan vara fel.

## Koden

Koden är skriven i Python och är släppt som öppen källkod, så att den kan
återanvändas för att bygga andra tjänster som hanterar liknande
informationssamlingar. Mer bakgrund finns på
[min blogg](http://blog.tomtebo.org/).
