# Anup Malani — CV (LaTeX source)

This repo holds the LaTeX source for Anup Malani's curriculum vitae. The compiled, dated PDFs live in [`anup-malani/website_personal`](https://github.com/anup-malani/website_personal) and are linked from [anupmalani.com/cv/](https://anupmalani.com/cv/).

See [`MAINTAINING.md`](MAINTAINING.md) for the build and update workflow.

## Quick build

```bash
latexmk -pdf -interaction=nonstopmode main_sorted.tex
```

Produces `main_sorted.pdf` in the working directory (gitignored — compiled releases land in `anup-malani/website_personal` as `Malani Resume Sorted YYMM.pdf`).

## Files

- `main_sorted.tex` — main CV document (subject-sorted; only variant maintained as of 2026-04-27)
- `Pubs_Sorted.tex` — peer-reviewed publications grouped by topic (Health, Statistics, Economic Growth & Development, Blockchain, Law & Regulation, and Other Publications)
- `Pubs_Other.tex` — books and book chapters, conference proceedings, reports, legal briefs, and working papers
- `archive/` — legacy CV variants (chronological `main.tex` + `Pubs_Unsorted.tex`), per-decade BibTeX files, and prior `Sorted_*.tex` formats. Not maintained; kept for historical reference.
