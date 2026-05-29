# Wettenbank-corpus

**ELI-georganiseerde markdown-spiegel van het Nederlandse Basis Wetten Bestand (BWB).**

Auto-gegenereerd door [Wettenbank.online](https://wettenbank.online) uit het KOOP Basis Wetten Bestand. Eén map per regeling onder `<type>/<jaar>/<slug>/` met één markdown-bestand per geldigheidsperiode + een `README.md` als landingspagina.

## Databronnen + actualiteit

| Bron | Endpoint / bestand | Coverage | Frequentie |
|---|---|---|---|
| KOOP BWB initial dump | `BWB_20250903_102559.7z` | 2025-09-03 baseline | éénmalig |
| **KOOP FRBR feed** | `repository.officiele-overheidspublicaties.nl/bwb/<BWBR>` | doorlopend t/m laatste KOOP-publicatie | **elke 12u via `bin/koop-bwb-sync.ts`** |

Deze corpus wordt automatisch bijgewerkt door [tanarchytan/wettenbank](https://github.com/tanarchytan/wettenbank) — twice-daily wordt per BWB-id `manifest.xml` opgevraagd met `If-Modified-Since`, en alleen gewijzigde regelingen + nieuwe states worden gedownload via:

```
https://repository.officiele-overheidspublicaties.nl/bwb/<BWBR>/<YYYY-MM-DD>_<rev>/xml/<file>.xml
```

Tier-based scheduling (12u/3d/14d/30d op basis van wijzigings-activiteit) minimaliseert de load op de KOOP-servers. Discovery van dit endpoint en de werkende grammar staat gedocumenteerd in de wettenbank-repo onder `docs/koop-bwb-feed-discovery.md`.

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
