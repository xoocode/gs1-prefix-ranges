# GS1 prefix ranges, reconciled

A machine-readable table of GS1 prefix ranges and the GS1 member organisation
that issues numbers in each range. 144 ranges covering 122 countries.

## What a GS1 prefix is, and what it is not

The first digits of a GTIN identify **the GS1 member organisation that allocated
the number to the brand owner**. They do not identify where the product was
made. GS1 states this itself:

> Since GS1 user companies can manufacture products anywhere in the world, GS1
> Prefixes do not identify the country of origin for a given product.

This matters because the opposite belief is widespread. It circulates in chain
letters urging people to boycott a particular prefix, and most lookup services
reinforce it by labelling the field "country of origin". This dataset labels it
`issuing_organisation`, and `organisation_country` means the country that
organisation sits in — not the country the goods came from.

If you are building a lookup tool, please do not rename these columns back.

## Why this table rather than any other

It is reconciled against two sources rather than copied from one:

- GS1's own prefix list
- English Wikipedia, `List of GS1 country codes`

Where the two disagree, GS1's reading is used and **the disagreement is kept**
in the `note` field instead of being silently resolved. 2 rows carry
such a flag:

- `612` — single-sourced: appears in Wikipedia but not in GS1's own list
- `894` — GS1 lists this range as centrally managed; Wikipedia gives Bangladesh

The `note` column itself is in Swedish, because the same table renders in the
lookup tool linked below. Everything else is language-independent.

⚠️ One caution if you update this yourself: several GS1 member organisations
publish stale copies of the list. GS1 Slovakia's still lists "Serbia and
Montenegro". A stale source looks exactly like a current one, so check the date.

## Files

Everything under `public/` is served as-is, with CORS open so the data can be
fetched from other sites.

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

The files are generated, not hand-edited. The source of truth is a TypeScript
table in a private repo, exported by a script that also enforces the two rules
that matter: the column may not be renamed to `country_of_origin`, and a new
source disagreement without an English explanation fails the build rather than
publishing a Swedish note in an English file.

**Do not edit the data files directly** — regenerate and commit. Corrections to
the underlying data are very welcome as an issue.

## Columns

| Column | Meaning |
|---|---|
| `prefix_from` | First prefix in the range, three digits |
| `prefix_to` | Last prefix in the range, three digits |
| `issuing_organisation` | The GS1 member organisation, or empty for special series |
| `organisation_country` | Where that organisation sits. Empty when the range is not a country |
| `note` | Restricted-distribution ranges, and any source disagreement |

Note that some ranges are not countries at all: `020–029` and `200–299` are
restricted distribution decided locally, `040–049` is restricted to within a
company, and `977`–`979` are ISSN/ISBN/ISMN.

## Licence

CC BY 4.0. Attribution to <https://smartatest.se/guider/ean-landsprefix>.

## Source

Maintained alongside the Swedish-language lookup tool at <https://smartatest.se/guider/ean-landsprefix>,
which explains the origin-country myth in more detail.

Last updated 2026-08-17.
