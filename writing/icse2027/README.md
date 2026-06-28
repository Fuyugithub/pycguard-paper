# ICSE 2027 LaTeX Environment

This directory contains the ICSE 2027 Research Track writing environment for:

Finding Bugs in Python Native Extensions via Safety Obligation Checking

## Official Formatting Requirements

The ICSE 2027 Research Track call states that submissions must use the IEEE conference proceedings template. LaTeX submissions must use:

```tex
\documentclass[10pt,conference]{IEEEtran}
```

Do not use `compsoc` or `compsocconf`.

Main text limit: 10 pages, inclusive of figures, tables, appendices, and other main content. Two additional pages may contain references only.

Review model: double-anonymous. Keep author information omitted in `main.tex` until camera-ready.

## Template Source

The complete `IEEEtran` distribution was downloaded from CTAN:

https://mirrors.ctan.org/macros/latex/contrib/IEEEtran.zip

The original template files are preserved under:

```text
template/IEEEtran/
```

The local compile entry point uses the copied official files:

```text
IEEEtran.cls
IEEEtran.bst
```

## Build

```sh
make
```

This requires a local LaTeX installation with `latexmk` and `pdflatex`.
