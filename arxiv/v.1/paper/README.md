# v.1 Paper Workspace

This folder contains the initial arXiv draft for the
`behavior-hybrid-substrate` repo.

Contents:

- `main.tex`: primary LaTeX source
- `references.bib`: BibTeX database

Local build:

```bash
cd arxiv/v.1/paper
pdflatex -interaction=nonstopmode -halt-on-error main.tex
bibtex main
pdflatex -interaction=nonstopmode -halt-on-error main.tex
pdflatex -interaction=nonstopmode -halt-on-error main.tex
```

Draft role in the repo sequence:

- `symbolic-logic`: historical and architectural argument
- `aibo-rcode`: behavioral-state extraction from preserved robot code
- `behavior-hybrid-substrate`: executable implementation path through
  `SOFTSIM` and `GA144`
