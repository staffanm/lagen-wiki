---
title: About lagen.nu
---

Lagen.nu is a non-profit, volunteer-run site that publishes Swedish legal
information — and, increasingly, the European and international law that Swedish
law refers to. Everything on it is free to read and free to reuse. This page is a
summary in English; the rest of the site is in Swedish.

The point of the site is not the texts themselves, which are public documents
available elsewhere. It is the links **between** them: 250 000 documents and
13.5 million resolved references, so that a provision can show you the case law
that applies it, the preparatory works that explain it and the EU article it
implements.

## What is here

- **Statutes (SFS)** — consolidated texts of the Swedish Code of Statutes,
  around 11 000 acts of which some 5 300 are in force. Historical consolidations
  are kept, so an act can be read as it stood at an earlier date.
- **Agency regulations** — nearly 13 000 regulations from about seventy agency
  code series, together with guiding decisions from the Parliamentary Ombudsmen
  (JO), the Chancellor of Justice (JK), the National Board for Consumer Disputes
  (ARN), the Competition Authority and the data protection authority (IMY), plus
  the agencies' own published legal positions.
- **Case law** — about 23 700 guiding decisions, from the Supreme Court and the
  Supreme Administrative Court down to the courts of appeal and the specialised
  appellate courts, from 1981 onward.
- **Preparatory works** — around 97 000 documents: government bills, official
  inquiries (SOU), ministry memoranda (Ds), terms of reference, committee
  reports. Bills reach back to 1867 and SOU to 1922, mostly as scanned and
  OCR-processed text.
- **EU law** — around 64 000 documents from EUR-Lex: regulations, directives,
  decisions and treaties, plus the Court of Justice's judgments and the Advocates
  General's opinions, back to the first cases in 1954.
- **International law** — the European Convention on Human Rights and the other
  Council of Europe treaties, about 7 000 judgments of the European Court of
  Human Rights, the international humanitarian law treaties, a selection of UN
  treaties, and decisions of the International Criminal Court.
- **Commentary and concepts** — a register of some 24 000 legal terms, most of
  them statutory definitions found automatically, several hundred with an
  explanation written by hand; and section-by-section commentary on a number of
  important acts.

## How the linking works

References in running text are read by a grammar rather than by pattern
matching, which means a reference resolves to the exact provision rather than to
the document — down to the subsection or the numbered point. The same grammar
runs over every source, which is why a court decision, a preparatory work and an
agency regulation can all point at precisely the same section.

The result is that beside any provision you are reading, a context panel shows
the case law and preparatory works that cite *that provision*, an extract from
the relevant passage of the bill that introduced it, the EU directive article it
transposes, and its amendment history.

Two of those connections are not written down anywhere in structured form and
had to be read out of the preparatory works themselves, in part with the help of
a language model:

- **Which directive article a Swedish provision implements**, read from the
  bill's section-by-section commentary.
- **Which provision of a new act corresponds to which provision of the act it
  replaced** — read from the comparison tables in the bill where those exist,
  otherwise from the commentary. This matters because a recodification otherwise
  severs a statute from decades of case law: reading a section of the new act,
  the panel also offers the decisions handed down under its predecessor,
  following the chain back several generations.

Both are shown together with the source they were read from, and both can be
wrong.

## Reuse

Swedish statutory text and the decisions of public authorities are not protected
by copyright. Case law summaries may be freely reproduced with attribution to
the National Courts Administration (Domstolsverket). We actively encourage
reuse, and there are four ways in:

- **Links.** Every document's URL is stable and documented — the Copyright Act
  (1960:729) is `https://lagen.nu/1960:729`, and section 12 of it is
  `https://lagen.nu/1960:729#P12`. See [Hur man länkar](/om/lankning).
- **A REST API** under `/api/v1`, with an OpenAPI schema at `/openapi.json` and
  interactive documentation at `/docs`. It exposes search, document retrieval,
  version history and — the interesting part — the citation graph in both
  directions.
- **Bulk downloads** of the whole corpus as gzipped NDJSON, one JSON document
  per line. See [API och bulkdata](/om/api).
- **An MCP interface**, so an AI assistant can look up current statutory text and
  follow the citation graph instead of answering from memory. See
  [lagen.nu i AI-verktyg](/om/mcp).

A document's canonical URL is its identity everywhere: link target, API key,
line id in the bulk files and id in the search index.

## The code

Running a site like this on a volunteer budget means automating nearly
everything — downloading, parsing, structuring and cross-linking the material.
The code is Python, released as open source under a BSD-style licence, and is at
[GitHub](https://github.com/staffanm/ferenda/). Anyone wanting to build
something similar, for Swedish law or for another body of documents, is welcome
to it.

## Caveats

The material is processed automatically from what public bodies publish, and both
the sources and our processing can introduce errors. For anything that matters,
check against the official publication. See
[ansvarsfriskrivning](/om/ansvarsfriskrivning) (in Swedish) for the detail.

## Contact

Staffan Malmgren, staffan.malmgren@gmail.com. Please note that we cannot answer
substantive legal questions.
