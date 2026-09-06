---
title: API och bulkdata
---

Hela korpusen går att hämta maskinellt, utan nyckel och utan registrering. Den
här sidan är en översikt; den fullständiga tekniska referensen finns i
[API-dokumentationen på
GitHub](https://github.com/staffanm/ferenda/blob/modernization/docs/api/README.md).

Två saker är värda att veta först:

- **Ett dokuments adress är dess identitet överallt.** Samma sträng
  (`https://lagen.nu/1975:635`) är länkadress, API-nyckel, rad-id i
  bulkfilerna och id i sökindexet. Adresserna är stabila över tid.
- **JSON-artefakten är källan till sanning.** Katalogen, sökindexet och sidorna
  är härledda ur den och kan byggas om. `GET /api/v1/document` returnerar
  artefakten oförändrad, och varje rad i en bulkfil *är* en artefakt.

## REST-API

API:et ligger under `/api/v1` på samma server som sidorna. Det är öppet för
anrop från andra webbplatser (CORS, endast GET). Interaktiv dokumentation finns
på [`/docs`](/docs) och ett maskinläsbart schema på
[`/openapi.json`](/openapi.json).

Dokumentadresser skickas alltid som frågeparametern `uri`, aldrig som del av
sökvägen -- lagen.nu-adresser innehåller både `:` och `/`.

| Jag vill... | Anrop |
|---|---|
| söka i fulltext | `GET /api/v1/search?q=…` |
| slå upp en hänvisning | `GET /api/v1/search?q=avtalslagen+36+§` |
| lista källor och storlekar | `GET /api/v1/sources` |
| räkna upp dokument | `GET /api/v1/documents?source=sfs` |
| bläddra efter facett | `GET /api/v1/facets`, `GET /api/v1/browse` |
| hämta ett dokument | `GET /api/v1/document?uri=…` |
| **se vilka som hänvisar hit** | `GET /api/v1/document/inbound?uri=…` |
| se vad ett dokument hänvisar till | `GET /api/v1/document/outbound?uri=…` |
| hämta grannskapet i citeringsgrafen | `GET /api/v1/graph?uri=…` |
| hitta kortaste kedjan mellan två dokument | `GET /api/v1/path?from=…&to=…` |
| lista tidigare lydelser | `GET /api/v1/document/versions?uri=…` |
| jämföra två lydelser | `GET /api/v1/document/diff?uri=…&from=…` |
| hämta en faksimilsida (PNG) | `GET /api/v1/facsimile?uri=…&sid=N` |

Två av anropen gör sådant som är svårt att få tag på någon annanstans:

**Sökningen förstår hänvisningar.** Ser frågan ut som en hänvisning --
`avtalslagen 36 §`, `BrB 12:1`, `GDPR art 32` -- löses den upp och det exakta
lagrummet fästs som första träff. Det är samma uppslagning som sökrutan på
sajten använder, och den hittar rätt även när namnet inte står någonstans i
texten.

**Citeringsgrafen är åtkomlig som data.** `inbound` svarar på vilka andra
dokument som hänvisar till exakt den här adressen. Skickar du med ett
fragment (`…#P6`) får du hänvisningarna till just den paragrafen, inte till
lagen som helhet.

## Bulkfiler

För att bearbeta hela korpusen är det bättre att hämta bulkfilerna än att
bläddra igenom API:et. De ligger under
[`/dumps/`](https://lagen.nu/dumps/), en fil per källa, och
`GET /api/v1/dumps` listar dem med filnamn och storlek.

Filerna är stora -- hela uppsättningen är ca 4,5 GB, varav förarbetena ensamma
är ca 3,5 GB. De serveras med stöd för delhämtning, så ett avbrutet nedladdande
går att återuppta.

Varje fil är gzippad NDJSON: en artefakt per rad, oförändrad -- samma objekt som
`GET /api/v1/document` returnerar under `artifact`. Eftersom varje artefakt bär
sina egna hänvisningar är en rad självständig; ingen katalog behövs för att
bearbeta materialet.

```sh
zcat sfs.ndjson.gz | head -1
```

Filerna byggs om tillsammans med resten av materialet, så de följer med när
korpusen uppdateras.

## Villkor

Statlig lagtext och myndighetsbeslut omfattas inte av upphovsrätt;
rättsfallsreferat får återges fritt med angivande av källan (Domstolsverket).
Återanvändning uppmuntras. Se vidare [För utvecklare](/om/for-utvecklare).

Vill du istället koppla materialet till ett AI-verktyg finns ett
MCP-gränssnitt -- se [lagen.nu i AI-verktyg](/om/mcp).
