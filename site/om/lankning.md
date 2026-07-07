---
title: Hur man länkar till lagen.nu
---

Om du har nytta av lagen.nu och vill hjälpa tjänsten får du gärna länka till den från din hemsida, via din blogg, i dina tweets eller på annat sätt. Varje länk hit gör att fler personer kan upptäcka tjänsten, och det är den bästa hjälp den kan få. Du behöver aldrig fråga om lov eller ens upplysa mig om att du har länkat till lagen.nu.

Du kan länka till enskilda lagar, och även enskilda paragrafer i en lag. För att länka till lagen om elektronisk kommunikation (LEK), som har SFS-nummer 2003:389, använder du följande address:

```
https://lagen.nu/2003:389
```

Du skriver alltså bara "https://lagen.nu/" följt av SFS-numret.

Samma mönster -- prefixet följt av dokumentets egen beteckning -- gäller även för andra dokumenttyper:

- **Lag / förordning (SFS):** `https://lagen.nu/<SFS-nummer>`, t.ex. `https://lagen.nu/2003:389`
- **Proposition:** `https://lagen.nu/prop/<beteckning>`, t.ex. `https://lagen.nu/prop/2020/21:22`
- **Rättsfall:** `https://lagen.nu/dom/<domstol>/<referat>`, t.ex. `https://lagen.nu/dom/ad/1993:100`
- **EU-rättsakt:** `https://lagen.nu/celex/<CELEX>`, t.ex. `https://lagen.nu/celex/32016R0679`

En länk till en enskild paragraf gör att webläsaren laddar hela lagtexten, och automatiskt hoppar till den specifika paragrafen.  För att länka till 6 kap. 18 § i samma lag använder du följande address:

```
https://lagen.nu/2003:389#K6P18
```

Du skriver alltså adressen till själva lagen, följt av "#"-tecknet, följt av "K6" (för 6 kap.) och "P18" (för 18 §).

Adresserna är utformade enligt följande mönster:

```
https://lagen.nu/<SFS-nummer>[#<kapitel>][P<paragraf>]
```

Om lagen i fråga inte har kapitelindelning, eller om paragrafnumreringen är kontinuerlig över kapitelgränserna (som i [Upphovsrättslagen](https://lagen.nu/1960:729)), använd inte <kapitel>-biten. För att länka till 9 § upphovsrättslagen blir alltså adressen:

```
https://lagen.nu/1960:729#P9
```

Om kapitlet eller paragrafen har en bokstav i sin numrering (vilket händer när nya bestämmelser införs i en befintlig lag), exv "26 a §", skriver man ihop siffran och bokstaven, "P26a":

```
https://lagen.nu/1960:729#P26a
```

Ibland kan man vilja länka till ett visst stycke eller t.o.m. en viss punkt i en punktlista. För att länka till exv. andra stycket i en paragraf lägger man till "S2", och för att länka till tredje punkten i en punktlista lägger man till "N3". För att länka till 58 kap. 1 § första stycket 3 p. rättegångsbalken skriver man:

```
https://lagen.nu/1942:740#K58P1S1N3
```

Ankartecknens betydelse är alltså **K** = kapitel, **P** = paragraf, **S** = stycke, **N** = numrerad punkt.

## Ankare i EU-rättsakter

En EU-artikel pekas ut med sitt nummer, och finare delar med punktnotation. För att länka till artikel 6 i dataskyddsförordningen (CELEX 32016R0679):

```
https://lagen.nu/celex/32016R0679#6
```

- Artikel 6: `#6`
- Artikel 6.2: `#6.2`
- En definition (t.ex. artikel 2.21): `#2.21`
- Skäl 15: `#recital-15`

## Övrigt

Tänk på att om du länkar till en paragraf långt nere i en lång lag så kommer webläsaren visa början av lagtexten ända tills rätt paragraf hämtats över Internet. Först då hoppar läsaren ner till rätt ställe. Detta märks särskilt tydligt om man använder en långsam internetuppkoppling.
