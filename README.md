# GS1 prefix ranges, reconciled

144 GS1 prefix ranges and the member organisation that issues numbers
in each one, covering 122 countries. CSV, JSON and DCAT-AP.

## What a GS1 prefix is

The first digits of a GTIN identify **the GS1 member organisation that allocated
the number to the brand owner**. They say nothing about where the product was
made. GS1 puts it plainly:

> Since GS1 user companies can manufacture products anywhere in the world, GS1
> Prefixes do not identify the country of origin for a given product.

The opposite belief is common. It travels in chain letters urging people to
boycott a particular prefix, and most lookup services feed it by labelling the
field "country of origin". Here the field is `issuing_organisation`, and
`organisation_country` is where that organisation sits, not where the goods
came from. If you build a lookup on this data, keep those names.

## How the table was made

Two sources, read against each other:

- GS1's own prefix list
- English Wikipedia, `List of GS1 country codes`

Where they disagree, GS1's reading wins and **the disagreement stays in the
record**. 2 ranges are marked:

- `612`: Appears in Wikipedia but not in GS1's own list
- `894`: GS1 lists this range as centrally managed; Wikipedia gives Bangladesh

Both carry `source_disagreement` as `true`, so you can filter for them without
reading prose.

If you maintain your own copy, check the date on your source. Several GS1 member
organisations publish stale lists; GS1 Slovakia's still names Serbia and
Montenegro. A stale list looks exactly like a current one.

## Files

Everything under `public/` is served as-is, with CORS open, so you can fetch the
data straight from your own page.

| File | Contents |
|---|---|
| `public/gs1-prefix.csv` | The table, RFC 4180 |
| `public/gs1-prefix.json` | The same, with source metadata |
| `public/dcat-ap.jsonld` | DCAT-AP catalogue record |
| `public/index.html` | A browsable, filterable view of the table |

Stable URLs, safe to hotlink:

- `https://gs1-prefix-ranges.vercel.app/gs1-prefix.csv`
- `https://gs1-prefix-ranges.vercel.app/gs1-prefix.json`

## How this is built

**The data files are generated, so edits to them get overwritten by the next
export.** They come from a TypeScript table in a private repo, via a script that
refuses to publish three things: a column renamed to `country_of_origin`, a note
or country name with no English form, and an ISO code that fails to round-trip to
the country it came from.

Corrections to the data are welcome as an issue.

## Columns

| Column | Meaning |
|---|---|
| `prefix_from` | First prefix in the range, three digits |
| `prefix_to` | Last prefix in the range, three digits |
| `issuing_organisation` | The GS1 member organisation, or empty for special series |
| `organisation_country` | Where that organisation sits. Empty when the range is not a country |
| `organisation_country_code` | ISO 3166-1 alpha-2, for joining. Empty for `540`, which covers two countries |
| `note` | What the range is for, when it is not a plain country allocation |
| `source_disagreement` | `true` where GS1 and Wikipedia disagree |

Some ranges are not countries. `020–029` and `200–299` are restricted
distribution decided locally, `040–049` is restricted to within one company, and
`977`–`979` are ISSN, ISBN and ISMN.

## Licence

CC BY 4.0. Attribution to <https://smartatest.se/guider/ean-landsprefix>.

## Source

Maintained alongside the Swedish-language lookup tool at <https://smartatest.se/guider/ean-landsprefix>,
which goes further into the origin-country myth.

Last updated 2026-08-17.
