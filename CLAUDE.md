# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an NTU (National Taiwan University) thesis repository using the NTU-Thesis LaTeX template with GitHub Actions for automated compilation and deployment. The thesis is configured for English language master's degree work in the Institute of Industrial Engineering.

## Document Structure

The thesis follows a modular structure with separate directories for different sections:

- `main.tex` - Root document that orchestrates the entire thesis
- `ntusetup.tex` - Configuration file containing thesis metadata (title, author, advisor, dates, keywords) and package imports
- `front/` - Front matter (acknowledgement, abstract, denotation/symbols)
- `contents/` - Main thesis chapters (chapter01.tex through chapter04.tex)
- `back/` - Back matter (references.bib, appendices)
- `figures/` - Image assets (cloned from template during CI)
- `fonts/` - Font files (cloned from template during CI)

**Important**: Note that `main.tex:42` includes chapter03.tex twice (lines 41 and 42). This appears to be a mistake that should be reviewed.

## Building the Thesis

### Local Compilation

The thesis uses XeLaTeX and BibTeX. Compile with:

```bash
latexmk -synctex=1 -interaction=nonstopmode -file-line-error -xelatex main.tex -f
```

Or if using a TeX editor, the build instructions are specified in the first lines of `main.tex`:
- TeX program: xelatex
- BIB program: bibtex
- Encoding: UTF-8 Unicode

### Prerequisites

Before compiling locally, you need the NTU-Thesis template files. The CI workflow clones these automatically, but for local development you'll need:
- `ntuthesis.cls` - The document class file from https://github.com/Hsins/NTU-Thesis
- `fonts/` directory from the template
- `figures/` directory from the template (or create your own)

### CI/CD Pipeline

GitHub Actions automatically compiles and deploys the PDF on every push to master:
1. Clones the NTU-Thesis template (https://github.com/Hsins/NTU-Thesis)
2. Copies fonts, figures, and ntuthesis.cls from the template
3. Compiles with latexmk using XeLaTeX
4. Creates a GitHub release with the compiled PDF
5. Commits the compiled PDF back to the repository

The workflow is defined in `.github/workflows/compile.yml`.

## Configuration

Edit `ntusetup.tex` to update:
- Thesis metadata: title (English and Chinese), author, advisor, student ID
- Dates: submission date and oral defense date
- Keywords (English and Chinese)
- DOI if applicable
- Document class options in `main.tex` lines 5-13:
  - `degree` (master or doctor)
  - `language` (chinese or english)
  - `fontset` (default, template, system, or overleaf)
  - `watermark` (true or false)
  - `doi` (true or false)

## References

Bibliography entries are stored in `back/references.bib` using BibTeX format. The thesis uses the `abbrv` bibliography style and includes natbib package with sort&compress option for citation management.

## Common LaTeX Packages

The template includes these commonly used packages (see `ntusetup.tex:32-40`):
- natbib - Bibliography management with sort&compress
- amsmath, amsthm, amssymb - Mathematical environments
- booktabs, multirow, diagbox, array, longtable - Table formatting
- ulem, CJKulem - Text decoration (underlines, etc.)
- paralist - Enhanced list environments

## Adding Content

- New chapters: Create `contents/chapterXX.tex` and add `\input{contents/chapterXX}` to `main.tex`
- New appendices: Create `back/appendixXX.tex` and add `\input{back/appendixXX}` to `main.tex`
- Images: Place in `figures/` directory (will need to be created locally or handled in CI)
- References: Add BibTeX entries to `back/references.bib`

## Key Files to Modify

- `ntusetup.tex` - Thesis metadata and package configuration
- `front/abstract.tex` - Chinese and English abstracts
- `front/acknowledgement.tex` - Acknowledgements section
- `front/denotation.tex` - List of symbols and notations
- `contents/chapter*.tex` - Main thesis content
- `back/references.bib` - Bibliography entries
- `back/appendix*.tex` - Supplementary material
