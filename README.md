# perkeso-dashboard

Static HTML reports built from [PERKESO LMX](https://lmx.perkeso.gov.my/) public labour-market statistics.

Hosted on GitHub Pages: <https://alanroy2764.github.io/perkeso-dashboard/>

The site is set to `noindex` — share the URL directly; it shouldn't surface in search results.

## Pages

- `index.html` — landing page linking to each report.
- `loe-by-age-group.html` — Loss of Employment claims by 5-year age band, last 9 quarters (Q1 2024 – Q1 2026), with a trendline chart.
- `perkeso-q1-dashboard.html` — Q1 dashboard across LOE / vacancy / jobseeker / placement.
- `perkeso-q1-dashboard-email.html` — inline-styled, table-based version of the Q1 dashboard for email rendering.

## Source data

All figures come from the public ARTS API behind LMX (no auth):

- `https://arts.perkeso.gov.my/data/datamart.v_stat_loe`
- `https://arts.perkeso.gov.my/data/datamart.v_stat_vacancy_final`
- `https://arts.perkeso.gov.my/data/datamart.v_stat_jobseeker`
- `https://arts.perkeso.gov.my/data/datamart.v_stat_placement`

Pulled and pivoted with the local `perkeso-lmx` skill.
