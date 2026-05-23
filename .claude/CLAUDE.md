# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A XeLaTeX template for Russian-language academic lab and seminar reports, configured for VS Code + LaTeX Workshop. Compiled with `-shell-escape` (required for `ifplatform` font detection).

## Building

Output goes to `build/` (in `.gitignore`). Create it before compiling:

```bash
mkdir -p build
xelatex -shell-escape -output-directory=build main.tex
```

For bibliography: run `xelatex → biber build/main → xelatex × 2`.

CI uses `latexmk` with `latexmk_use_xelatex: true` and `latexmk_shell_escape: true` via `xu-cheng/latex-action@v3`.

## Architecture

The template is split across three config layers:

1. **`config.tex`** — loaded first. Defines fonts (via `ifplatform`), document metadata (`\discipline`, `\topic`, `\teacher`, etc.), and feature flags.
2. **`main.tex`** — preamble only. Loads packages conditionally based on flags from `config.tex`, then assembles pages and chapters via `\input{}`.
3. **`macros.tex`** — custom commands (`\unnsection`, `\unnsubsection`, `\unnsubsubsection` — unnumbered sections that still appear in TOC).

## Preamble Packages

Packages in `main.tex` are intentionally pre-loaded for end users who build reports on top of this template. A package being absent from the template's own `.tex` files does not mean it is unused — it means it is provided for the user. Do not flag pre-existing preamble packages as unused.

Only flag a package as problematic if it was **added in the current change** and is functionally redundant (already provided by another loaded package) or conflicts with existing packages.

## Feature Flag System

Optional packages are gated by boolean flags in `config.tex`:

```latex
\newif\ifbib     \bibfalse    % biblatex + biber
\newif\ifplots   \plotsfalse  % pgfplots
\newif\iftikz    \tikzfalse   % tikz
\newif\iflisting \listingfalse % minted
```

**Any new optional package must follow this pattern** — guarded by a `\newif` flag, never loaded unconditionally. Adding a flag also requires updating both `README.md` and `README.ru.md`.

## Font Selection

`config.tex` uses `ifplatform` to select fonts automatically:
- **Linux**: Liberation Serif / Liberation Mono
- **Windows / macOS**: Times New Roman / Courier New

`ifplatform` requires `-shell-escape` to detect the OS. Without it, `\iflinux` is false and it falls back to Times New Roman (which fails on Linux CI).

## GitHub Actions

| Workflow | Trigger | Purpose |
|----------|---------|---------|
| `build-latex.yml` | push/PR to `main` | Compile `main.tex`, upload PDF artifact |
| `check-docs.yml` | PR touching `config.tex`, `main.tex`, `macros.tex`, or READMEs | Claude checks flag/font/file-structure consistency between source and READMEs |
| `claude.yml` | `@claude` mention in issue/PR | General Claude Code assistant |
| `claude-code-review.yml` | Every PR | Automated code review |
