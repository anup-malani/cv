# Maintaining `anup-malani/cv`

This repo holds the LaTeX source for Anup Malani's CV. Compiled PDFs are published from this source into the `anup-malani/website_personal` repo (and from there linked from anupmalani.com).

## Architecture

```
anup-malani/cv (this repo)         anup-malani/website_personal       anup-malani/anup-malani.github.io
─────────────────────────          ─────────────────────────          ────────────────────────────────
main_sorted.tex                                                        _pages/cv.md links at →
Pubs_Sorted.tex                    Malani Resume Sorted YYMM.pdf  ←─── (raw GitHub URL of latest dated CV)
Pubs_Other.tex                     publications/*.pdf
                                   (paper PDFs)
       │
       │ latexmk
       │ produces a YYMM-dated PDF →
       │ → copied + committed to website_personal
       ↓
```

The compiled PDF is **not** stored in this repo — `.gitignore` excludes `main_sorted.pdf`. Each release is a YYMM-stamped copy in `website_personal`.

## Build locally

```bash
latexmk -pdf -interaction=nonstopmode main_sorted.tex
```

Produces `main_sorted.pdf` in the working directory. Verify clean compile (no `Misplaced alignment tab character &` or `Missing $ inserted` errors). Page count should be 15 ± 1.

Clean auxiliary files: `latexmk -c main_sorted.tex`

## Add a publication (manual workflow)

> **AI-assisted alternative.** The `/add-paper` skill at `~/UChicago Law Dropbox/Anup Malani/assistants/research-manager/projects/website/skills/add-paper/SKILL.md` automates this. Use it if available.

1. **Edit `Pubs_Sorted.tex`** (peer-reviewed) **or** `Pubs_Other.tex` (working papers / books / book chapters / legal briefs / reports).

   Find the right `\subsection*{...}` and the right year position. Insert at the top of the year (newest first within each year):
   ```latex
   \item[YYYY] \tab{}APA-formatted citation. \href{https://doi.org/...}{https://doi.org/...} \href{https://github.com/anup-malani/website_personal/blob/main/publications/<canonical-name>.pdf}{[PDF]}
   ```

2. **LaTeX-escape any special characters** — `&` → `\&`, `_` (in `\href{}{}` link text only) → `\_`, `#` → `\#`, `%` → `\%`.

3. **Verify clean compile** with `latexmk -pdf -interaction=nonstopmode main_sorted.tex`.

4. **Commit and push** to git. (See "Release a new dated CV PDF" below for shipping the compiled PDF.)

## Release a new dated CV PDF

After paper / position / talk edits are committed:

```bash
cd ~/github/cv
latexmk -pdf -interaction=nonstopmode main_sorted.tex

YYMM=$(date +%y%m)
NEW_NAME="Malani Resume Sorted ${YYMM}.pdf"
cp main_sorted.pdf "$HOME/github/website_personal/$NEW_NAME"

cd ~/github/website_personal
git add "$NEW_NAME"
git commit -m "Update CV: ${YYMM} rebuild"
git push

# Then update the website's CV link
cd ~/github/anup-malani.github.io
# Edit _pages/cv.md — replace previous YYMM in the GitHub blob URL with ${YYMM}
git add _pages/cv.md
git commit -m "Update CV link: ${YYMM}"
git push
```

GitHub Action rebuilds the site in ~60–90s; `/cv/` page now serves the new PDF. Old dated CV PDFs in `website_personal` are preserved (never delete — they're a historical record).

## CV publication entries vs. CV biographical entries

The CV mixes two kinds of edits:

| What changes | Edit | Skill |
|---|---|---|
| New paper, working paper, book | `Pubs_Sorted.tex` or `Pubs_Other.tex` | `/add-paper` |
| New position, award, invited talk, board membership | `main_sorted.tex` (specifically the relevant `\section*{...}` block) | `/update-cv-only` |
| Bio paragraph, research areas, teaching areas | `main_sorted.tex` (top, before `\input{Pubs_Sorted}`) | manual |

## Overleaf integration

Anup uses [Overleaf](https://overleaf.com) for occasional collaborative editing and visual preview. Overleaf imports from this GitHub repo (one-way: GitHub → Overleaf, free tier).

To set up:
1. In Overleaf, click "New Project" → "Import from GitHub"
2. Authorize Overleaf to read GitHub repos
3. Select `anup-malani/cv`
4. Project syncs read-only — edits in Overleaf don't propagate back. Make actual edits via git (Claude Code, VS Code, or `gh` CLI), then re-sync in Overleaf.

For two-way sync (edit either git or Overleaf, with bi-directional propagation), upgrade to Overleaf Premium and configure `git push`/`git pull` from inside the Overleaf project.

## Tooling assumptions

- TeX Live 2024 (`pdflatex`, `latexmk`) — `/Library/TeX/texbin/` on macOS via MacTeX.
- Standard LaTeX packages used: `csquotes`, `babel`, `microtype`, `datetime`, `tabto`, `hyperref`, `geometry`, `enumitem`, `titlesec`, `setspace`, `ebgaramond`, `tgheros`. All ship with TeX Live full install.

## See also

- [`anup-malani/website_personal`](https://github.com/anup-malani/website_personal) — PDF host repo (compiled CV releases + paper PDFs)
- [`anup-malani/anup-malani.github.io`](https://github.com/anup-malani/anup-malani.github.io) — al-folio Jekyll site (publishes anupmalani.com)
- AI-assisted workflow: `~/UChicago Law Dropbox/Anup Malani/assistants/research-manager/projects/website/skills/`
