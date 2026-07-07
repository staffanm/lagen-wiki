---
title: Hur ser det ut bakom kulisserna?
---

Lagen.nu består av hundratusentals sammanlänkade dokument. För att kunna hantera
en så stor informationsmängd med mycket begränsade resurser är nästan allt
automatiserat.

Varje källa (SFS, domstolsavgöranden, förarbeten, EU-rätt, myndighetsföreskrifter
med flera) har en egen bearbetningskedja i fyra steg:

1. **Hämtning** -- råmaterialet laddas ner från källan (Regeringskansliets
   rättsdatabaser, domstolarnas publiceringstjänst, regeringen.se, EUR-Lex,
   myndigheternas egna webbplatser, m.fl.).
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

Den viktigaste funktionen -- att bredvid varje paragraf visa vilka rättsfall och
förarbeten som hänvisar till den -- kommer ur citeringsgrafen som byggs i steg 4.

Webbplatsen serveras som statiska sidor, och samma process driver även ett
[REST-API och bulknedladdningar](/om/for-utvecklare) av hela materialet.

Kommentarerna och begreppsförklaringarna skrivs som versionshanterad markdown i
ett separat innehållsförråd, och kan även redigeras direkt på sajten av inloggade
skribenter.

Koden är skriven i Python och är släppt som öppen källkod, så att den kan
återanvändas för att bygga andra tjänster som hanterar liknande
informationssamlingar. Mer bakgrund finns i "lagen.nu"-kategorin på
[min blogg](http://blog.tomtebo.org/tag/lagennu/).
