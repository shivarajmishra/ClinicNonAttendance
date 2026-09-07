# Screening non-attendance risk dashboard

Click here to load the dashboard [Click here to load the dashboard](https://shivarajmishra.github.io/ClinicNonAttendance/screening_risk_dashboard.html).

A single-file browser tool that flags patients at higher risk of missing their
breast screening appointment, so clinic teams can target reminders and outreach
where they are most likely to make a difference.

Risk weights are the adjusted relative risk ratios (RRRs) published in
[Ellis et al., *Lancet Public Health* 2017](https://www.thelancet.com/journals/lanpub/article/PIIS2468-2667(17)30217-7/fulltext),
with the local government area factor adapted for the South Western Sydney Local
Health District (SWSLHD).

## Quick start

Open this html page in any modern browser, or visit the
[live version on GitHub Pages](https://shivarajmishra.github.io/ClinicNonAttendance/screening_risk_dashboard.html).
There is no build step, server, or install — it is one self-contained HTML file.

The dashboard loads with a set of example patients so you can see the model
working immediately. Use **Clear all** to start from an empty list.

## What it does

- **Patient list** — every loaded patient with their computed risk index and
  tier (Low / Moderate / High / Very high), sortable by any column and
  filterable by tier.
- **Patient detail** — a risk gauge, the per-factor RRR breakdown, and suggested
  interventions for that patient's tier.
- **Clinic summary** — aggregate view of the currently loaded list: risk tier
  mix, deprivation decile spread, and LGA breakdown.
- **Download PDF** — a one-page printable summary for a selected patient
  (uses the browser's print-to-PDF).

## Adding patients

- **Add patient** — enter one patient at a time via a form.
- **Import from Excel/CSV** — upload a `.csv`, `.xlsx`, or `.xls` file with one
  row per patient. Column headers are matched loosely against common names
  (e.g. `Age`, `SIMD`, `Delay (days)`, `LGA`, `Practice deprivation`,
  `Distance (km)`, `Prior DNAs`). A preview shows which columns were detected and
  which rows could and could not be parsed before anything is added.
  Rows with missing required values are skipped.
- **Load example patients** — appends the built-in demo cohort.

A CSV template is available from the "How to use this dashboard" panel inside the
app.

## How the risk score works

Each patient's **risk index** is the product of the RRRs for their category on
six factors, i.e. `exp(Σ ln(RRR))` across:

| Factor | Notes |
| --- | --- |
| Age band | 7 bands, 0–15 through 90+ |
| Deprivation decile (SIMD) | 1 = most deprived, 10 = least deprived (reference) |
| Appointment delay | days between booking and appointment |
| Local government area | SWSLHD's 6 LGAs (see caveat below) |
| Mean practice patient deprivation | practice-level SIMD band |
| Distance to practice | 0–2 km vs. more than 2 km |

Previous missed appointments (DNAs) are captured for context but are **not** part
of the score.

The index is relative to a reference profile (index ≈ 1.0). Tier cut-offs:

| Tier | Risk index |
| --- | --- |
| Low | ≤ 1.5 |
| Moderate | ≤ 3.0 |
| High | ≤ 6.0 |
| Very high | > 6.0 |

## Important caveats

- **Not locally calibrated.** Weights come straight from the study's negative
  binomial model and have not been fitted to local screening attendance data.
  Treat the tiers as a provisional starting point and recalibrate once local
  outcomes accrue.
- **The LGA factor is an unvalidated approximation.** The original study used
  Scotland's 8-level urban/rural classification. That scale has been remapped
  onto SWSLHD's 6 LGAs (Canterbury-Bankstown, Liverpool, Fairfield,
  Campbelltown, Camden, Wollondilly), ordered most to least urban, reusing 6 of
  the published RRRs in that order. Under Australia's own remoteness
  classification almost all of SWSLHD sits within "Major City", so this factor's
  contribution should be treated with particular caution.
- **Suggested actions are a starting point**, not a fixed protocol.

## Data & privacy

Patient records stay in the browser only — nothing is sent to a server.
**Clear all** deletes them permanently. The Excel importer loads a parsing
library (SheetJS) from a CDN on first use; CSV import works fully offline.

## Project layout

```
index.html   the entire application (HTML + CSS + JS)
```
