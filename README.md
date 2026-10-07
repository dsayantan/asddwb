# West Bengal SIR - ASDD List

Static search over the **ASDD list** published by the CEO West Bengal
(https://ceowestbengal.wb.gov.in/asd_sir): voters whose enumeration forms were
not received during the Special Intensive Revision (Absent / Shifted / Dead /
Already enrolled).

**24 districts, 294 assemblies, 5,820,899 records, 80,680 booths.**

## Layout

- `index.html` - **landing page: summary dashboard** (Bangla by default). A table
  of districts that expands to assemblies and then to booths, with totals, the
  split by reason, female and age shares, the estimated Muslim-name share with
  the **Census 2011 Muslim share in brackets** (district and state only - the
  census publishes religion no lower than district), and top surnames. Sortable
  by any column. Booth rows load from `summary/<district>.js` on first expand.
  A **Search a name** button opens `search.html`.
- `search.html` - choose a **district and an assembly** and continue to that
  assembly's page (no links; a button returns to the summary).
- `<district>/AC<NNN>_<NAME>.html` - one **self-contained** page per assembly,
  with two tiles, and links back to `search.html` and `index.html`. Records are
  embedded as gzip+base64, so a page needs no server and no other files.

The language choice (বাংলা / English) is remembered across pages.
Total about 145 MB; largest single page 1.5 MB.

## Tile 1 - search for a person

Four boxes, all optional; results must match **all** that were filled.
Each box matches only its own column.

| Box | Minimum | Matching |
|---|---|---|
| Elector Name | 3 letters | substring, then fuzzy >= 80%, then sound-alike |
| Guardian Name | 3 letters | substring, then fuzzy >= 80%, then sound-alike |
| EPIC Number | 5 characters | substring (slashes ignored) |
| Booth / Part No(s) | - | exact; a list or range such as `5,7,12-15` |

The PDFs are in **Bengali** for most assemblies, **Hindi** for the Darjeeling
hills and **English** for some Kolkata seats. A query in the same script is
matched by spelling; a query in a different script (typically English against
Bengali names) is matched by **sound**: both sides are reduced to a consonant
skeleton (aspirates folded, s/sh, b/v/w folded, vowels dropped), so
`BISWAS` finds বিশ্বাস and `MONDAL` finds মণ্ডল. Such rows are badged
`sounds like N%`.

An automatic English transliteration is shown under every Bengali/Hindi name,
for reading only.

## Tile 2 - show a whole booth

Pick a booth from the dropdown; every name in that part is listed, with the
total and the split by reason.

## Publishing on GitHub Pages

Commit this folder, enable Pages on the branch root, share
`https://<user>.github.io/<repo>/`. `.nojekyll` is included; all links are
relative.

Decoding uses `DecompressionStream` (Chrome 80+, Firefox 113+, Safari 16.4+).
