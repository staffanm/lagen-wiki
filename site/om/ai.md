---
title: Hur lagen.nu använder AI
---

Lagen.nu använder AI i form av språkmodeller på två sätt. Det ena är några
avgränsade steg när materialet bearbetas. Det andra är när programvaran som
driver tjänsten utvecklas och testas. Den här sidan beskriver båda.

Rättskällorna själva -- lagtexter, förarbeten, domar, föreskrifter och
EU-rättsakter -- återges som de är publicerade. Ingen AI skriver om,
sammanfattar eller översätter dem. Hänvisningar i texten känns igen av en
grammatik och inte av en språkmodell (se [Tekniken bakom lagen.nu](/om/teknik)).
Sökningen är en vanlig fritextsökning. Kommentarerna och
begreppsförklaringarna är skrivna av människor. Det finns ingen chattfunktion på
lagen.nu. [MCP-gränssnittet](/om/mcp) låter ditt eget AI-verktyg hämta
uppgifter härifrån, men själva AI:n är då din, inte lagen.nu:s.

## Var AI används i bearbetningen

Språkmodeller används i sju avgränsade steg. I samtliga får modellen en sluten
uppgift: att välja ur en lista, att peka ut ett ställe i en text eller att
klassificera något. Varje svar kontrolleras maskinellt innan det sparas. En
hänvisning måste peka på en paragraf, en artikel eller ett avsnitt som finns,
och ett citat måste stå ordagrant i källan. Ett svar som inte klarar
kontrollen kastas.

1. **Skälen i EU-rättsakter.** För en EU-förordning eller ett direktiv
   grupperar en språkmodell skälen i preambeln efter tema. Den ger varje grupp
   en kort rubrik och kopplar skälen till de artiklar de förklarar. Rubrikerna
   är det enda på lagen.nu som en språkmodell har formulerat med egna ord.
   Varje rubrik är märkt med **AI**.
2. **Remissvar.** För ett betänkande placerar en språkmodell varje
   remissinstans synpunkter på de avsnitt de gäller. Den bedömer instansens
   inställning och väljer ett citat som visar skälet för den. Citatet är
   ordagrant. Inställningen (symbolerna ▲ △ ◇ ▽ ▼) och sammanfattningen över
   ett avsnitt bygger på modellens bedömning. Panelen med remissvar säger det.
3. **Genomförande av EU-direktiv.** En språkmodell läser
   författningskommentaren i en proposition och avgör vilka artiklar i ett
   direktiv som varje paragraf genomför. Resultatet visas som länkar under
   "Genomför EU-rätt".
4. **När en lag görs om.** När en lag ersätts av en ny lag med annan
   indelning läser en språkmodell propositionen och avgör vilken gammal
   paragraf som motsvarar vilken ny (se [När lagen görs om](/om/praxis-over-tid)).
5. **Normkedjor.** En språkmodell hjälper till att avgöra vilka förordningar
   och föreskrifter som fyller ut vilka bestämmelser i en lag, och vilken roll
   en bestämmelse har -- om den till exempel definierar ett begrepp, ålägger
   en skyldighet eller delegerar normgivning (se
   [Normkedjor och regleringshierarki](/om/normkedjor)).
6. **Bilder och tabeller som saknas.** Den konsoliderade lagtexten saknar
   bilder, kartor och vissa tabeller som finns i den tryckta författningen. En
   språkmodell som kan läsa bilder hittar rätt sida och rätt område i den
   officiella PDF-filen. Det som visas är ett utsnitt ur PDF-filen, inte något
   som modellen har skapat. Ett utsnitt publiceras först när en människa har
   kontrollerat det.
7. **Vägledningar till EU-rättsakter.** En språkmodell kopplar avsnitt i
   officiella vägledningar, till exempel kommissionens frågor och svar, till
   de artiklar som avsnitten förklarar. Funktionen är under uppbyggnad.

I steg 3–7 visar lagen.nu bara kopplingar mellan texter och utsnitt ur
officiella dokument. Ingen text som en modell har formulerat visas. Därför är
de kopplingarna inte märkta var för sig.

## Granskning

Allt som en språkmodell har tagit fram sparas i en egen fil, skild från
rättskällorna, i ett [öppet git-arkiv](https://github.com/staffanm/lagen-wiki/tree/main/ann).
Filen anger vilken modell som användes och när. En människa kan granska, rätta
och godkänna resultatet, och git-historiken visar vad som har ändrats.

Utsnitten i steg 6 publiceras först efter granskning. Övriga resultat
publiceras utan att någon har granskat dem var för sig. Hittar du ett fel,
[hör gärna av dig](/om/kontakt).

## Var modellerna körs

Modellerna är öppna språkmodeller. De körs hos en tjänsteleverantör inom EU
eller på egen hårdvara. Det som skickas till modellerna är offentliga
dokument: lagtext, förarbeten, remissvar och EU-rättsakter. Inga uppgifter om
besökarna skickas.

## Hur AI-innehållet är märkt

- **På sidan.** Rubrikerna över skälen i EU-rättsakter har märket **AI**. I
  panelen med remissvar står det att inställningen är bedömd med AI och att
  citaten är ordagranna. Märket och meningen länkar hit.
- **Maskinläsbart.** De HTML-element som innehåller AI-genererat innehåll har
  attributet `data-ai-generated`. Den som återanvänder sidorna kan använda
  attributet för att hitta innehållet och behålla märkningen.
- **API, bulkdata och MCP.** De innehåller dokumenten som de har tolkats,
  utan något av det som språkmodellerna har tagit fram. Där behövs därför
  ingen märkning.

## Utveckling och kvalitetskontroll

Programvaran bakom lagen.nu, [ferenda](https://github.com/staffanm/ferenda),
utvecklas med hjälp av AI-assistenter för programmering. De skriver och ändrar
kod, skriver tester och granskar ändringar. De används också för
kvalitetskontroll: att jämföra det som tolkats ur ett dokument med originalet,
att leta efter tolkningsfel och att granska om kommentarer och
begreppsförklaringar har blivit inaktuella.

Det är en människa som beslutar vilka ändringar som tas in. Att tolkningen
blir rätt kontrolleras mot en fast uppsättning referensdokument och
automatiska tester, inte genom att någon litar på vad en AI-assistent säger.
