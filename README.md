# Philippine Departmental Budget Reviews

Long-form budget analyses of selected Philippine departments and agencies,
tracing each peso from the Executive's proposal (NEP) through enactment
(GAA), release (allotment), commitment (obligation), and final cash payment
(disbursement). Each report covers the Office of the Secretary, current-year
**New Appropriations only**, FY 2018-2026.

**Live site:** <https://ajamontesa.github.io/ph-budget-analysis/index.html> —
the home page links to every published report. The site is published via GitHub
Pages from the `docs/` folder.

## Repository structure

```
budget-analysis/
├── data/
│   ├── Compiled_-_NEP-GAA-SAAODB-ByPAPLabelled.xlsx   # source data (one sheet per agency)
│   ├── Compiled_-_DPWH.xlsx                            # DPWH (separate structure: long-run + by operating unit)
│   └── Compiled_-_NEP-GAA-NoFAR1.xlsx                  # agencies without FAR No.1 (P/A/P NEP-GAA + agency-level SAAODB)
├── reports/                       # R Markdown sources (each is fully self-contained)
│   ├── index.Rmd                  # landing page -> knits to index.html
│   ├── DAR_OSEC_Budget_Analysis.Rmd
│   ├── DA_Budget_Analysis.Rmd
│   ├── DepEd_OSEC_Budget_Analysis.Rmd
│   ├── DENR_OSEC_Budget_Analysis.Rmd
│   ├── DOH_OSEC_Budget_Analysis.Rmd
│   ├── DSWD_OSEC_Budget_Analysis.Rmd
│   ├── DPWH_Budget_Analysis.Rmd
│   ├── NCIP_Budget_Analysis.Rmd
│   └── NCMF_Budget_Analysis.Rmd
└── docs/                          # GitHub Pages root (published HTML output)
    ├── index.html
    ├── dar-osec.html
    ├── da-osec.html
    ├── deped-osec.html
    ├── denr-osec.html
    ├── doh-osec.html
    ├── dswd-osec.html
    ├── dpwh.html
    ├── ncip.html
    ├── ncmf.html
    └── slides/                     # briefing decks (standalone HTML, one per agency)
        ├── DAR_OSEC_Budget_Analysis.html
        ├── DA_Budget_Analysis.html
        ├── DepEd_OSEC_Budget_Analysis.html
        ├── DENR_OSEC_Budget_Analysis.html
        ├── DOH_OSEC_Budget_Analysis.html
        ├── DSWD_OSEC_Budget_Analysis.html
        ├── NCIP_Budget_Analysis.html
        └── NCMF_Budget_Analysis.html
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
6. **(Optional) Publish the briefing slides.** Put the deck's knitted HTML in
   `docs/slides/` (the current decks keep their source-style names, e.g.
   `docs/slides/DepEd_OSEC_Budget_Analysis.html`), then add one line to the
   `slides_paths` map in `reports/index.Rmd` —
   `DepEd = "slides/DepEd_OSEC_Budget_Analysis.html"` — and re-knit `index.html`.
   A "Briefing slides" link then appears on that agency's row (it works for
   `planned` agencies too, so slides can go up before the long-form report).
   Leave a code out of `slides_paths` to keep its link hidden, so the page never
   shows a dead link.

   > **Slide decks must be self-contained.** xaringan knits by default to a small
   > HTML that depends on a sibling `libs/` folder; that single file renders
   > blank on its own. Knit with `self_contained: true` in the deck's YAML to get
   > one portable file (the decks currently in `docs/slides/` are already built
   > this way — remark.js and images are inlined, so they work even offline).
7. **Commit and push.** GitHub Pages serves the update.

## Data notes

- **Scope:** Current New Appropriations only — excludes Continuing and
  Automatic Appropriations and Special Purpose Fund transfers.
- **Coverage:** NEP/GAA for FY 2018-2026; execution measures (Adjusted
  Allotments, Obligations, Disbursements) for FY 2018-2025.
- **Source:** DBM-published budget and execution data.
