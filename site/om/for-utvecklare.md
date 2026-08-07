---
title: För utvecklare
---

Allt innehåll på lagen.nu är öppna data och går att hämta maskinellt. Statlig
lagtext och myndighetsbeslut omfattas inte av upphovsrätt; rättsfallsreferat får
återges fritt med angivande av källan (Domstolsverket). Vi uppmuntrar
återanvändning.

Det finns fyra sätt att komma åt innehållet:

- **Länka** till läsvyer med de permanenta adresserna -- se
  [Hur man länkar till lagen.nu](/om/lankning).
- **REST-API** för att söka, lista och hämta enskilda dokument samt härledda
  vyer -- t.ex. vilka dokument som hänvisar till ett givet dokument
  (citeringsgrafen), versionshistorik och skillnader mellan lydelser.
- **Bulknedladdning** av hela korpusen som komprimerade NDJSON-filer, en rad per
  dokument.
- **MCP** för AI-verktyg, så att en chatbot kan slå upp gällande lagtext och
  följa citeringsgrafen istället för att svara ur minnet.

De två första beskrivs närmare under [API och bulkdata](/om/api), det sista
under [lagen.nu i AI-verktyg](/om/mcp).

Varje dokument identifieras överallt med sin kanoniska `https://lagen.nu/…`-adress
-- samma sträng är API-nyckel, rad-id i bulkdumparna och id i sökindexet.
JSON-artefakten är källan till sanning; sökindex och katalog är härledda och kan
byggas om ur artefakterna.

API:et ligger under `/api/v1`. Ett maskinläsbart schema serveras på
[`/openapi.json`](/openapi.json) och interaktiv dokumentation på
[`/docs`](/docs).

Plattformen bakom lagen.nu är öppen källkod och kan återanvändas för att bygga
andra tjänster som hanterar liknande informationssamlingar. Läs mer om
[tekniken bakom lagen.nu](/om/teknik).
