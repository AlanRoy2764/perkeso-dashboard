# perkeso-dashboard

Static HTML reports built from [PERKESO LMX](https://lmx.perkeso.gov.my/) public labour-market statistics.

Hosted on GitHub Pages: <https://alanroy2764.github.io/perkeso-dashboard/>

The site is set to `noindex` — share the URL directly; it shouldn't surface in search results.

## Pages

**Page 1 — Quarterly labour market across six dimensions (2023 Q2 → 2026 Q1)**

Each page compares LOE / Job Vacancies / Job Placements side-by-side for one dimension.

- `index.html` — landing page linking to each report.
- `gender.html` (LOE, Placement — vacancy has no gender field)
- `age-group.html` (LOE, Placement — vacancy has no age field)
- `industry.html` — MSIC 2008 Section, 21 codes A–U
- `state.html` — 16 states + 3 Federal Territories
- `education.html`
- `salary.html`

**Other reports**

- `perkeso-q1-dashboard.html` — editorial Q1 dashboard, 3-year snapshot.
- `loe-by-age-group.html` — original LOE-only deep-dive.

## Source data

All figures come from the public ARTS API behind LMX (no auth):

- `https://arts.perkeso.gov.my/data/datamart.v_stat_loe`
- `https://arts.perkeso.gov.my/data/datamart.v_stat_vacancy_final`
- `https://arts.perkeso.gov.my/data/datamart.v_stat_jobseeker`
- `https://arts.perkeso.gov.my/data/datamart.v_stat_placement`

## Pipeline

1. Pull raw rows with the local `perkeso-lmx` skill (cached gzipped JSON).
2. `build_long_format.py` → normalises into long-format quarterly CSVs (`data/*.csv`).
   Excludes rows where the dimension value is `Revision Stage`. Industry labels
   canonicalised across datasets via MSIC section letter.
3. `push_to_sheet.py` → mirrors the CSVs into the
   [`perkeso-data-analysis-2026`](https://docs.google.com/spreadsheets/d/11IXCbodlq4ywV9nksagMFTWg_iejKI9FrCDIY0wLIHM/edit)
   Google Sheet (long-format warehouse).
4. `build_html.py` → reads `data/*.csv`, generates the six dimension pages and
   refreshes `index.html`. Data is embedded as JSON; no runtime API calls.

Build scripts are gitignored — they assume the local `perkeso-lmx` skill cache and
`gws` CLI auth.

## Data quality

Rows where the dimension value or its rank is `Revision Stage` (PERKESO's
placeholder for records being revised) are excluded. In Placement this is
material — for the 2023 Q2 → 2026 Q1 window: education ≈17%, salary ≈8.5%,
state ≈6.5%, industry ≈4.5% of total are dropped. LOE and Vacancy exclusions
are &lt;1%.

Vacancy salary = OFFERED by employer; LOE/Placement salary = REALISED. Compare
shapes, not levels.
