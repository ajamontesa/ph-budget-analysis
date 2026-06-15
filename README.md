# Philippine Departmental Budget Reviews

Long-form budget analyses of selected Philippine departments and agencies,
tracing each peso from the Executive's proposal (NEP) through enactment
(GAA), release (allotment), commitment (obligation), and final cash payment
(disbursement). Each report covers the Office of the Secretary, current-year
**New Appropriations only**, FY 2018-2026.

**Live site:** the reports are published via GitHub Pages from the `docs/`
folder. The landing page (`docs/index.html`) links to every published report.

## Repository structure

```
budget-analysis/
├── data/
│   ├── Compiled_-_NEP-GAA-SAAODB-ByPAPLabelled.xlsx   # source data (one sheet per agency)
│   └── Compiled_-_DPWH.xlsx                            # DPWH (separate structure: long-run + by operating unit)
├── reports/                       # R Markdown sources (each is fully self-contained)
│   ├── index.Rmd                  # landing page -> knits to index.html
│   ├── DepEd_OSEC_Budget_Analysis.Rmd
│   ├── DOH_OSEC_Budget_Analysis.Rmd
│   ├── DSWD_OSEC_Budget_Analysis.Rmd
│   ├── DAR_OSEC_Budget_Analysis.Rmd
│   ├── DA_Budget_Analysis.Rmd
│   ├── DENR_OSEC_Budget_Analysis.Rmd
│   └── DPWH_Budget_Analysis.Rmd
└── docs/                          # GitHub Pages root (published HTML output)
    ├── index.html
    ├── deped-osec.html
    ├── doh-osec.html
    ├── dswd-osec.html
    ├── dar-osec.html
    ├── da-osec.html
    ├── denr-osec.html
    └── dpwh.html
```

Every `.Rmd` is **self-contained**: it carries its own CSS and R setup, so it
knits with nothing but the file itself plus the data workbook. There are no
shared helper scripts or stylesheets to manage.

## Requirements

R (≥ 4.2) with `rmarkdown`, `knitr`, `dplyr`, `tidyr`, `ggplot2`, `scales`,
`kableExtra`, `readxl`, `stringr`, `DT`, plus Pandoc. (`index.Rmd` needs only
`rmarkdown` + `tibble`; it does not read the data workbook.)

## Workflow

1. **Update the data.** Add or revise the workbook in `data/`.
2. **Author a slide deck** for the new department in R Markdown (your step).
3. **Generate the long-form document** version of that report.
4. **Knit** each `.Rmd` to HTML and place the output in `docs/`:

   ```r
   rmarkdown::render("reports/DepEd_OSEC_Budget_Analysis.Rmd",
                     output_file = "deped-osec.html", output_dir = "docs")
   ```

   The report RMDs look for the workbook at `../data/...` (i.e. when knit from
   `reports/`) and at `data/...` (when knit from the repo root), so either
   works.
5. **Update the index.** Open `reports/index.Rmd`, edit the `agencies` table
   (add a row, or flip a `status` from `"planned"` to `"published"`), knit it
   to `index.html`, and place it in `docs/`. The landing page needs no data
   file.
6. **Commit and push.** GitHub Pages serves the update.

## Enabling GitHub Pages

Repository settings → Pages → set the source to the `main` branch and the
`/docs` folder. The site is served at `https://<user>.github.io/<repo>/`.
The empty `docs/.nojekyll` file tells Pages to serve the HTML and its assets
as-is.

## Data notes

- **Scope:** Current New Appropriations only — excludes Continuing and
  Automatic Appropriations and Special Purpose Fund transfers.
- **Coverage:** NEP/GAA for FY 2018-2026; execution measures (Adjusted
  Allotments, Obligations, Disbursements) for FY 2018-2025.
- **Source:** DBM-published budget and execution data.
