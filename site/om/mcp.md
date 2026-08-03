---
title: lagen.nu i AI-verktyg (MCP)
---

Lagen.nu har ett **MCP-gränssnitt** -- ett standardiserat sätt för AI-verktyg
att hämta uppgifter ur en extern källa istället för att svara ur minnet. Kopplar
du in det kan chatboten slå upp den gällande lydelsen av en paragraf, hitta de
rättsfall som tillämpat den, och hänvisa till exakt rätt ställe.

Det är värt något just för juridik. En språkmodell har lagtext i sitt
träningsmaterial, men lagar ändras, och modellen vet inte vilken lydelse den
minns. Den kan inte heller veta vad som hänvisar till en viss paragraf --
citeringsgrafen finns inte i löptexten någonstans, den är uträknad.

## Adress

```
https://ferenda.lagen.nu/mcp
```

Transport är **Streamable HTTP**. Gränssnittet är öppet, kräver ingen nyckel och
kan bara läsa -- det finns inget verktyg som ändrar något.

(Adressen är `ferenda.lagen.nu`, inte `lagen.nu`. Det är den nya versionen av
tjänsten, och den ligger tills vidare på den adressen.)

## Koppla in det

**Claude Code:**

```sh
claude mcp add --transport http lagen-nu https://ferenda.lagen.nu/mcp
```

**Claude (webb och skrivbordsapp):** Inställningar → Kopplingar (*Connectors*) →
lägg till en egen koppling, med namn `lagen.nu` och adressen ovan. I ett Team-
eller Enterprise-konto kan det behöva göras av en administratör för hela
organisationen.

**ChatGPT:** Inställningar → Kopplingar (*Connectors*), lägg till en egen
MCP-server med adressen ovan. Egna kopplingar är i skrivande stund begränsade
till betalkonton, och i Business- och Enterprise-konton läggs de till av en
administratör.

**Andra verktyg:** allt som talar MCP över Streamable HTTP fungerar -- ange
adressen ovan, ingen autentisering. Verktyg som bara klarar den äldre
stdio-transporten behöver en brygga (t.ex. `mcpo` eller `mcp-remote`).

Menyerna i de här produkterna ändras ofta. Hittar du inte rätt, leta efter
"Connectors", "MCP" eller "Verktyg" i inställningarna.

## Vad verktygen kan

| Verktyg | Gör |
|---|---|
| `search` | fritextsökning i hela korpusen, ner till den paragraf som matchar |
| `resolve_citation` | en hänvisning skriven i text (`avtalslagen 36 §`, `GDPR art 32`) till dokumentets adress |
| `get_document` | ett dokuments text, hela eller en utpekad paragraf |
| `fetch` | texten bakom en sökträff |
| `list_documents` | räkna upp dokument, filtrerat på källa och typ |
| `get_incoming_citations` | vilka dokument som hänvisar hit |
| `get_outgoing_citations` | vad dokumentet självt hänvisar till |
| `list_sources` | vilka källor som finns och hur stora de är |

Det verktygen tillsammans gör möjligt är att gå från en fråga till en paragraf,
och därifrån till de domar och föreskrifter som tillämpar den -- att följa
kopplingarna, alltså, vilket är det en vanlig webbsökning inte klarar.

## Ha kvar omdömet

Att svaret är grundat i rätt källa gör det inte till en rättsutredning. En
språkmodell kan hämta rätt paragraf och ändå dra fel slutsats av den, och den
väger inte rättskällor mot varandra så som en jurist gör -- se [om
rättskällelära och juridisk metod](/om/rattskallor). Kontrollera hänvisningarna;
de går alltid tillbaka till en sida här som i sin tur länkar till utgivarens
egen publicering.

Se även [ansvarsfriskrivning](/om/ansvarsfriskrivning) och
[API och bulkdata](/om/api) för samma material utan AI-lager.
