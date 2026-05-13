# MCAF R10 MISRA Compliance Progress

A lightweight dashboard hosted on GitHub Pages that tracks MISRA violation counts for the MCAF R10 project over time.

Live page: `https://<your-org>.github.io/misra-progress-reviwer/`

## How it works

- `docs/data.csv` holds the violation history — one row per date.
- `docs/index.html` reads the CSV and renders an interactive line chart using [Chart.js](https://www.chartjs.org/).
- A GitHub Actions workflow appends a new row every day at **5:00 PM MST** (00:00 UTC), carrying forward the last known value if no manual update was made.

## Updating the violation count

After a MISRA check run, append a new row to `docs/data.csv`:

```csv
date,violations_left
2026-05-11,891
2026-05-12,600
2026-05-13,580   ← add new row here
```

Then commit and push:

```bash
git add docs/data.csv
git commit -m "Update MISRA violations YYYY-MM-DD"
git push
```

The chart on GitHub Pages will update within a minute or two.

## Baseline

The start point (baseline) is based on the **33CK device default build** of MCAF R9 RC31.
Full baseline report: [MCAF R9 RC31 MISRA — 33CK Baseline](https://confluence.microchip.com/display/MCU16APP/MCAF+R9+RC31+MISRA+-+33CK+Baseline)

## Repo structure

```
.github/
  workflows/
    daily-misra-update.yml   # daily automation workflow
docs/
  index.html                 # dashboard page
  data.csv                   # violation history
  .nojekyll                  # disables Jekyll processing
README.md
```

## Automation schedule

The daily workflow runs at **00:00 UTC (5:00 PM MST)** and:
1. Checks if today's date already exists in `data.csv`.
2. If not, appends a row with the last known violation count.
3. Commits and pushes the change automatically.

To trigger it manually: GitHub → Actions → **Daily MISRA progress entry** → **Run workflow**.
