# Rapport: inaktuellt, felaktigt och ofullständigt i `concept/` och `commentary/`

Granskningen omfattar 571 begreppsfiler i `concept/` och 91 kommentarfiler i
`commentary/` (89 SFS, 2 EU-rättsakter). Texterna är i huvudsak skrivna
2010–2012. Aktuell laglydelse har hämtats från lagen.nu och korsställts mot
varje kommenterad paragraf maskinellt; de substantiella exemplen är därefter
lästa och verifierade för hand.

Inga filer har ändrats.

---

## 0. Först: lagen-mcp fungerar bara till hälften just nu

MCP-servern var inte inkopplad i sessionen. Adressen fanns dokumenterad i
repot självt (`site/om/mcp.md`), så jag kopplade in den — men textuthämtningen
är nere serversidan:

| Verktyg | Status |
|---|---|
| `search`, `list_documents`, `list_sources`, `get_incoming_citations`, `get_outgoing_citations` | fungerar |
| `get_document`, `fetch`, `resolve_citation` | **fel för samtliga dokument, alla källor** |

`list_documents(source="sfs")` räknar upp 11 214 författningar, men varje
försök att hämta texten bakom en av dem misslyckas. Katalogindexet finns
alltså, medan artefaktlagret det pekar på inte går att läsa — ser ut som en
volym som inte är monterad i containern, inte som ett trasigt index.

Felet gäller alla källor, inte bara SFS:

```
get_document uri=https://lagen.nu/1962:700 pinpoint=K3P1
  -> Error executing tool get_document: /app/catalog/artifact/sfs/1962/700.json
get_document uri=https://lagen.nu/dom/nja/2015s899
  -> Error executing tool get_document: /app/catalog/artifact/dom/NJA_2015_s_899.json
resolve_citation citation="avtalslagen 36 §"
  -> Error executing tool resolve_citation: /app/catalog/artifact/sfs/1915/218.json
```

Jag har därför hämtat gällande lydelse från produktionswebben (`lagen.nu`),
som är aktuell — LAS visas t.ex. i lydelse enligt SFS 2022:836. Underlaget i
rapporten är alltså korrekt, men det kom inte via MCP.

### Sidoiakttagelse: interna sökvägar läcker ut i felmeddelanden

Serverns felhantering skickar `str(e)` rakt till klienten:

```json
{"result": {"content": [{"text": "Error executing tool get_document: /app/catalog/artifact/sfs/1962/700.json",
                         "type": "text"}], "isError": true}}
```

Undantaget verkar bära sökvägen som enda argument, så det som når klienten är
en naken containersökväg utan förklarande text. Jämför med den kontrollerade
varianten, som finns för katalogsökningen:

```
get_document uri=https://lagen.nu/9999:1
  -> Error executing tool get_document: no document 'https://lagen.nu/9999:1' in the catalog
```

Katalogslagningen har ett avsiktligt formulerat fel; artefaktinläsningen har
inget alls. Värt att fånga in — dels läcker det driftdetaljer, dels är
meddelandet obegripligt för den som använder verktyget.

---

## 1. Allvarligast: kommentarer sitter på fel paragraf

Detta är det enda felet som är *aktivt vilseledande* snarare än bara föråldrat,
och det syns inte i någon enkel kontroll: paragrafankaret finns kvar, så
kommentaren renderas på lagen.nu — men paragrafen har fått ett annat innehåll.

**Tryckfrihetsförordningen 2 kap. numrerades om helt genom SFS 2018:1801**
(i kraft 1 januari 2019). `commentary/sfs/1949/105.md` följer den gamla
numreringen genomgående. 53 av kapitlets kommenterade paragrafer berörs.

| Kommentaren står under | Kommentaren handlar om                | Vad paragrafen är idag          | Rätt paragraf nu |
|------------------------|---------------------------------------|---------------------------------|------------------|
| 2 kap. 3 §             | de tre rekvisiten för allmän handling | definition av "handling"        | 2 kap. 4 §       |
| 2 kap. 6 §             | när en handling är *inkommen*         | när en upptagning är *förvarad* | 2 kap. 9 §       |
| 2 kap. 12 §            | utlämnande på stället                 | minnesanteckningar              | 2 kap. 15 §      |

En läsare som slår upp 2 kap. 12 § TF får alltså en utförlig och i sak
välskriven text om rätten att ta del av handlingar på plats — placerad under
paragrafen om minnesanteckningar. Kommentaren till 2 kap. 3 § räknar dessutom
upp undantag i "6, 7, 8 och 9 §§", paragrafnummer som efter omnumreringen pekar
någon helt annanstans.

Kapitlet behöver mappas om paragraf för paragraf, inte lagas styckvis. Samma
kontroll bör göras för RF 2 kap. (omnumrerat genom SFS 2010:1408) innan något
annat rörs.

---

## 2. Elva kommentarer beskriver upphävda lagar

Filerna ligger kvar och publiceras. Efterföljaren är verifierad mot katalogen i
varje fall.

| Fil                                                 | Upphävd lag                                               | Ersatt av                                                |
|-----------------------------------------------------|-----------------------------------------------------------|----------------------------------------------------------|
| `commentary/sfs/1998/204.md` (28,5 kB)              | Personuppgiftslagen (1998:204)                            | GDPR + dataskyddslagen (2018:218)                        |
| `commentary/sfs/1986/223.md` (32,5 kB)              | Förvaltningslagen (1986:223)                              | Förvaltningslag (2017:900)                               |
| `commentary/sfs/1991/900.md` (6,8 kB, 287 rubriker) | Kommunallagen (1991:900)                                  | Kommunallag (2017:725)                                   |
| `commentary/sfs/1990/932.md` (29,4 kB)              | Konsumentköplagen (1990:932)                              | Konsumentköplag (2022:260)                               |
| `commentary/sfs/2007/1091.md` (6,7 kB)              | LOU (2007:1091)                                           | LOU (2016:1145) — kommentar finns redan                  |
| `commentary/sfs/1995/400.md`                        | Fastighetsmäklarlagen (1995:400)                          | 2011:666, därefter 2021:516                              |
| `commentary/sfs/1982/670.md`                        | Namnlagen (1982:670)                                      | Lag (2016:1013) om personnamn                            |
| `commentary/sfs/1997/238.md`                        | Lagen om arbetslöshetsförsäkring (1997:238)               | Lag (2024:506)                                           |
| `commentary/sfs/1991/572.md`                        | Lagen om särskild utlänningskontroll (1991:572)           | Lag (2022:700) om särskild kontroll av vissa utlänningar |
| `commentary/sfs/2000/832.md`                        | Lagen om kvalificerade elektroniska signaturer (2000:832) | eIDAS (910/2014) + lag (2016:561)                        |
| `commentary/sfs/1937/81.md`                         | IDL (1937:81)                                             | Arvsförordningen (650/2012) + lag (2015:417)             |

PUL-kommentaren är den mest problematiska: 28,5 kB som beskriver
missbruksmodellen, anmälan till Datainspektionen på blankett, och
tredjelandsöverföring enligt en ordning som upphörde 2018. Den citeras
dessutom från sju andra filer.

---

## 3. Konkreta sakfel i gällande lagar

Verifierade mot lydelsen på lagen.nu. Detta är stickprov ur de 589 kommenterade
paragrafer som ändrats sedan 2012 — inte en uttömmande lista.

**LAS 22 § — kvantitativt fel med praktisk betydelse.**
Kommentaren: *"Om en arbetsgivare har högst tio anställda kan två nyckelpersoner
undantas från turordningen."* Sedan SFS 2022:835 gäller detta **alla**
arbetsgivare oavsett storlek, och undantaget omfattar **tre** arbetstagare.
Både tröskeln och antalet är fel. Nya 22 a § saknas helt.

**LAS 7 § — begreppet finns inte längre.**
Kommentaren bygger på "saklig grund". Lagtexten säger sedan 2022:835 "sakliga
skäl", och det var en avsedd materiell förändring, inte en språklig putsning.
Kommentaren missar också regeln att omplaceringsskyldigheten anses uppfylld om
arbetsgivaren en gång omplacerat av personliga skäl, om det inte finns
särskilda skäl.

**BrB 6 kap. 1 § — hela brottskonstruktionen är utbytt.**
Kommentaren beskriver tvångsmodellen före 2018: *"Första stycket avser fall där
gärningsmannen använder våld"*, *"andra stycket då offret är i ett hjälplöst
tillstånd"*, *"att tjata till sig sex kan aldrig bedömas som våldtäkt"*.
Bestämmelsen bygger sedan SFS 2018:618 på frivillighet. Vidare:

- samlagsdefinitionen i kommentaren (*"då en man och en kvinnas könsorgan
  kommer i beröring"*, samkönade handlingar utgör inte samlag) är ersatt av
  lagtextens "vaginalt, analt eller oralt samlag";
- straffskalans minimum är höjt från två till tre års fängelse;
- paragrafen har ändrats ytterligare genom SFS 2026:1318, i kraft 1 augusti
  2026 — alltså förra veckan.

**ÄktB 2 kap. 1 § — kommentaren handlar bara om en möjlighet som är borttagen.**
Hela kommentaren rör dispens för underåriga och RÅ 1988 ref. 100. Dispensen
avskaffades genom SFS 2014:376; lagtexten lyder nu i sin helhet *"Den som är
under 18 år får inte ingå äktenskap."* Kommentaren till 2 kap. 3 § är
däremot korrekt — tillstånd för halvsyskon prövas alltjämt av länsstyrelsen
enligt 15 kap. 1 §.

**Marknadsstörningsavgift — fel SFS-nummer.**
`concept/Marknadsstörningsavgift.md` anger marknadsföringslagen som
"(2008:408)". Rätt nummer är 2008:486. Filen anger också beloppsintervallet
5 000 kr – 5 miljoner kr, vilket ändrats.

**Sekretesslagen skriven som "(1980:00)".**
Sju gånger i `commentary/sfs/2009/400.md`. Ska vara 1980:100. (De 52
hänvisningarna till sekretesslagen i övrigt är motsvarighetsuppgifter och
korrekta som historik.)

**`concept/Personlig integritet.md`** anger som lagrum PUL, RF 2 kap. 3, 6 och
12 §§ samt EKMR art. 8. PUL är upphävd; RF 2 kap. 12 § är efter 2010 års
omnumrering minoritetsskyddet, inte något integritetsstadgande. Det relevanta
är numera RF 2 kap. 6 § andra stycket, GDPR och dataskyddslagen.

---

## 4. Omfattningen av det tysta åldrandet

589 kommenterade paragrafer har ändrats efter 2012. 311 av dem har en
kommentar på mer än 300 tecken, alltså tillräckligt mycket text för att ett
sakfel ska hinna uppstå.

Fördelat på ändringsår — notera att 2026 är näst största posten, och året är
inte slut:

```
2012  16    2015   9    2018  93    2021  35    2024  33
2013  33    2016  17    2019  16    2022  72    2025  27
2014  60    2017  34    2020  17    2023  18    2026 109
```

Värst drabbade filer:

| Fil | Ändrade paragrafer | Av totalt kommenterade |
|---|---|---|
| `commentary/sfs/1962/700.md` (BrB) | 158 | 261 |
| `commentary/sfs/2005/716.md` (UtlL) | 58 | 85 |
| `commentary/sfs/1949/105.md` (TF) | 53 | 53 |
| `commentary/sfs/2010/610.md` (Fängelselagen) | 39 | 105 |
| `commentary/sfs/1949/381.md` (FB) | 31 | 84 |
| `commentary/sfs/2009/400.md` (OSL) | 30 | 65 |
| `commentary/sfs/1984/387.md` (Polislagen) | 28 | 44 |
| `commentary/sfs/1991/900.md` (KL, upphävd) | 28 | 287 |

Brottsbalken sticker ut: 60 % av de kommenterade paragraferna har ändrad
lydelse, och SFS 2026:1318 har nyligen skrivit om stora delar av 3–15 och 20–34
kap. TF ligger på 53 av 53.

Utöver detta pekar **33 rubriker på paragrafer som inte finns kvar alls** —
kommentaren renderas då inte någonstans. Bl.a. BrB 8 kap. 3 §, 17 kap. 7, 14
och 17 §§, UtlL 2 kap. 1, 3, 3 a, 4 och 8 §§, medborgarskapslagen 5, 7, 9, 24
och 25 §§.

---

## 5. Föråldrade myndigheter och domstolar

Utfallet av en genomsökning; siffrorna avser träffar som vid genomläsning är
verkliga problem, inte historiska omnämnanden.

| Vad                                                | Sedan | Var                                                           |
|----------------------------------------------------|-------|---------------------------------------------------------------|
| Regeringsrätten -> Högsta förvaltningsdomstolen    | 2011  | 45 träffar i 15 filer; egen sida `concept/Regeringsrätten.md` |
| Datainspektionen -> IMY                            | 2021  | 6 träffar, samtliga i PUL-kommentaren                         |
| Rikspolisstyrelsen -> Polismyndigheten             | 2015  | egen sida `concept/Rikspolisstyrelsen.md`                     |
| Marknadsdomstolen -> Patent- och marknadsdomstolen | 2016  | `concept/Specialdomstol.md`, `concept/Domstol.md`             |
| Patentbesvärsrätten avskaffad                      | 2016  | `concept/Specialdomstol.md`                                   |
| Miljödomstol -> mark- och miljödomstol             | 2011  | `concept/Specialdomstol.md` m.fl.                             |
| Fastighetsdomstolarna avskaffade                   | 2011  | `concept/Specialdomstol.md`                                   |
| Statens va-nämnd avvecklad                         | 2016  | `commentary/sfs/1942/740.md`                                  |
| Lantmäteriverket -> Lantmäteriet                   | 2008  | `concept/Datapantbrev.md`                                     |
| LVFS -> HSLF-FS                                    | 2016  | `concept/Narkotika.md` m.fl.                                  |

`concept/Specialdomstol.md` är den enskilt mest inaktuella begreppssidan: den
räknar upp Marknadsdomstolen, Patentbesvärsrätten, miljödomstolarna och
fastighetsdomstolarna som existerande. Fyra av uppräkningens poster stämmer
inte längre.

`concept/Författningssamling.md` listar bl.a. Arbetsmarknadsstyrelsens och
Banverkets författningssamlingar; 43 av sidans länkar ger 404.

Därtill 16 förekomster av EG-terminologi (EG-domstolen, EG-rätten,
EG-fördraget) som Lissabonfördraget gjorde inaktuell 2009, plus en egen sida
`concept/EG-rätt.md`.

---

## 6. Länkar

840 distinkta externa URL:er, varav **261 svarar med annat än 200**: 132 rena
404, resten timeouts, namnuppslagsfel och certifikatfel.

Värst: eur-lex.europa.eu (34 — gamla `LexUriServ`-URL:er), domstol.se (19),
datainspektionen.se (15), regeringen.se (12), skatteverket.se (10).

Bland dessa finns **34 döda länkar till lagen.nu självt**, i huvudsak till den
avvecklade MediaWiki-delen (`lagen.nu/wiki/...`, ger 403) och till
`wiki.lagen.nu`. De förekommer bl.a. i `author`-fältet i frontmatter på ett
tjugotal kommentarfiler, som pekar på användarprofiler som inte finns kvar.

---

## 7. Interna begreppslänkar

865 `begrepp:`-hänvisningar pekar på 612 mål som saknar fil i `concept/`. De
vanligaste: `hävning` (19), `förvaltningsrätt` (14), `sakrätt` (10), `lös sak`
(8), `hyresnämnden` (8), `culpa` (6), `fullmäktig` (6).

Detta bör kontrolleras mot hur `begrepp:`-länkar faktiskt löses vid
publicering innan något åtgärdas — korpusen innehåller 28 928
begreppsdokument, betydligt fler än de 571 filerna här, så en del av dessa
länkar kan mycket väl fungera på webben. Men mönstret att centrala civilrättsliga
termer som `hävning` och `culpa` saknar egen sida är i sig en lucka.

---

## 8. Ofullständigt: vad som saknas helt

**Sakområden utan kommentar där lagen bytts ut.** Konsumentköplagen (2022:260),
förvaltningslagen (2017:900), kommunallagen (2017:725) och lagen om personnamn
(2016:1013) har alla en kommentar till *föregångaren* men ingen till den
gällande lagen. LOU är undantaget — där finns både 2007:1091 och 2016:1145.

**Stora reformer utan spår i texterna.** Upphovsrättslagen har fått nya 3 a och
5 a kap. genom DSM-genomförandet (2022); kommentaren nämner dem inte.
Skadeståndslagen, MBL 39 § (ändrad 2026:886) och JB 12 kap. (hyresreglerna,
ändrade 2026:773 och 2026:844) är i samma läge.

**116 stubbar** i `concept/` med under 160 tecken brödtext, varav tre helt
tomma: `Första hjälpen-tavlor.md`, `Kommun.md`, `Socialtjänst.md`. De två
sistnämnda är knappast marginella begrepp.

**Nio filer i `concept/` är inte begrepp alls** utan kvarlämnat wiki-material:

- `Källdokument:The Treaty on the Functioning of the European Union.md`
  (337 kB), `Källdokument:The Treaty on European Union.md` (78 kB) och
  `Källdokument:Europakonventionen.md` (62 kB) — hela fördragstexter,
  senast uppdaterade från källan 2010-01-04 respektive 2009-11-27. Korpusen har
  dessa i `eurlex` och `coe` i underhållen form.
- `Immaterialrätt checklista.md` inleds med rå wikitabellsyntax (`{|style=...`)
  och ett meddelande från dig till en användare, daterat 15 december 2011, om
  att sidan syns utåt på lagen.nu. Den gör den fortfarande.
- `En samling med författningar av intresse för migrationsrätt återfinns här..md`
  — en mening som filnamn, 70 länkar i MediaWiki-format, 12 av dem döda.
- `Praxis från europadomstolen 2011 Jul-Dec.md` och tre systerfiler — tidsstämplade
  ögonblicksbilder som slutar mitt i 2013.
- `Legacy talk:Robots.txt-artikeln.md`.

**Kvarlämnad MediaWiki-syntax i 37 filer**: `[url text]` istället för
markdown-länkar, och `\#` som listmarkör (som renderas som en bokstavlig
brädgård, se `concept/Författningssamling.md` och kommentaren till ÄktB 4 kap. 2 §,
där de fyra vigselrekvisiten radas upp som `\# ... # ... # ...`).

---

## 9. Förslag på ordning att ta det i

1. **TF 2 kap.** — felplacerade kommentarer är värre än inga. Kontrollera RF
   2 kap. på samma sätt.
2. **De 33 rubrikerna som pekar på borttagna paragrafer** — mekaniskt
   identifierbara, kommentaren är osynlig idag.
3. **De elva upphävda lagarna** — besluta per fil: flytta till efterföljaren,
   markera som historik, eller ta bort. PUL och gamla konsumentköplagen först.
4. **De verifierade sakfelen** i avsnitt 3 — få till antalet, stor
   konsekvens, särskilt LAS 22 §.
5. **`Specialdomstol.md`, `Domstol.md`, `Författningssamling.md`,
   `Rikspolisstyrelsen.md`, `Regeringsrätten.md`** — myndighetsuppräkningar som
   är enkla att rätta.
6. **Wiki-kvarlevorna i `concept/`** — ta bort eller flytta ut; de har inget
   läsvärde och tre av dem är på tillsammans 477 kB.
7. **BrB och UtlL** — det stora arbetet. 158 respektive 58 ändrade paragrafer;
   här går det inte att laga styckvis utan en genomgång kapitel för kapitel.

Punkt 1–5 är avgränsade och verifierade. Punkt 7 är i praktiken en omskrivning.

---

### Metodnot

Underlaget finns i sessionens scratchpad: `stale.json` (589 ändrade
paragrafer med fil, radnummer och ändrings-SFS), `match.json` (rubrik mot
paragrafstruktur), `links.json` (840 URL:er med statuskod), `dead_refs.json`,
`status.json` (upphävandestatus per lag) samt `lawtext/` (gällande lydelse för
samtliga 89 kommenterade lagar).

Ändringsmarkörerna är hämtade ur lagtextens egna `Lag (ÅÅÅÅ:NNN).`-noteringar.
En del 2026-ändringar har ikraftträdande senare i år — lagen.nu markerar dessa
med `/Träder i kraft I: .../`, och de bör kontrolleras individuellt innan de
skrivs in som gällande rätt.
