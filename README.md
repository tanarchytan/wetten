# Wettenbank-corpus

**ELI-georganiseerde markdown-spiegel van het Nederlandse Basis Wetten Bestand (BWB).**

Auto-gegenereerd door [Wettenbank.online](https://wettenbank.online) uit het KOOP Basis Wetten Bestand. Eén map per regeling onder `<type>/<jaar>/<slug>/` met één markdown-bestand per geldigheidsperiode + een `README.md` als landingspagina.

## Databronnen + actualiteit

> ⚠️ **De huidige snapshot is van 3 september 2025**. Wijzigingen daarna zijn nog niet doorgevoerd.

| Bron | Bestand / endpoint | Snapshot tot | Volgende verversing |
|---|---|---|---|
| KOOP BWB initial dump | `BWB_20250903_102559.7z` | 2025-09-03 | éénmalig — geen periodieke vervanging |
| KOOP SRU delta-feed | `xml.overheid.nl/sru/bwb` (CQL `dt.modified>=…`) | nog niet aangezet | dagelijks zodra cron actief is in [wettenbank-online](https://github.com/tanarchytan/wettenbank) |

De drift tussen 2025-09-03 en vandaag wordt ingehaald via `bin/sync-delta.ts` in de wettenbank repo.

## Coverage

- **45 607** BWB-entiteiten:
  - 41 941 BWBR (regelingen — wetten / AMvB / MinR / beleidsregels / circulaires / ZBO / etc.)
  - 3 662 BWBV (verdragen)
  - 4 BWBW
- **117 898** unieke staten (geldigheidsperiodes)
- **~6,3 M** artikelen
- **~1,1 M** citation-edges

## Mapstructuur

```
<type>/<jaar>/<slug>/
├── README.md                 — landingspagina met alle staten
├── 1994-01-01.md             — één bestand per staat (validFrom)
├── 1994-04-01.md
└── 2025-09-01.md             — laatst geldige staat
```

Elk markdown-bestand heeft YAML-frontmatter met `bwbId`, `validFrom`, `validTo`, `ministry`, `citetitle`, `eliUri`, `prevState`, `nextState` — geschikt voor static-site generators of als platte AI-context.

## Slug-collisions

4 195 regelingen delen een citetitel-slug (b.v. jaarlijkse "Regeling vaststelling tarieven"). Pre-resolved door `bin/index-eli.ts` in wettenbank-online:

- Unieke citetitels: `/eli/nl/wet/1815/grondwet`
- Conflicten: `/eli/nl/wet/2018/regeling-tarieven-bwbr0042001` (BWB-id suffix)

## Licentie

Alle wettekst valt in het **publiek domein** conform Auteurswet artikel 11 en wordt door KOOP gepubliceerd onder [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/deed.nl).

Deze gegenereerde markdown-versie wordt onder dezelfde licentie aangeboden.

## Genereren

De code die deze repo bouwt staat in [tanarchytan/wettenbank](https://github.com/tanarchytan/wettenbank):

```bash
bun run bin/koop-to-markdown.ts --source /pad/naar/wetten --out /pad/naar/wetten-corpus
```

## Niet officieel

Voor juridisch bindende publicatie raadpleeg het Tractatenblad, Staatsblad, Staatscourant of [wetten.overheid.nl](https://wetten.overheid.nl).
