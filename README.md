# $\LaTeX$ template for lab and seminar reports

A ready-to-use template for academic reports. Configured for $XeLaTeX$.

[English](README.md) | [Русский](README.ru.md)

## Requirements

- $XeLaTeX$
- VS Code with LaTeX Workshop extension

## File Structure

```
latex-lab-template/
├──.vscode/                  # Editor configuration directory
│   ├── settings.json            # LaTeX Workshop settings
│   └── keybindings.json         # Shortcuts for manual compilation
├── chapters/                # Directory with report content files
│   └── chapter1.tex             # Example file
├── figures/                 # Directory with plots, charts, diagrams, etc.
├── images/                  # Directory with various images
│   └── logo.png                 # University logo
├── pages/                   # Directory with static pages
│   ├── titlepage.tex            # Title page
│   └── bibliography.tex         # Bibliography page
├──.gitignore
├── config.tex               # Document parameters
├── macros.tex               # Custom commands
├── main.tex                 # Preamble and document structure
├── README.md
└── references.bib           # BibTeX source database
```

## Optional Packages

Controlled by boolean flags in `config.tex`. All disabled by default.

| Flag | Enable | Packages |
|---|---|---|
| `\bibfalse` | `\bibtrue` | biblatex (bibliography) |
| `\plotsfalse` | `\plotstrue` | pgfplots (function plots) |
| `\tikzfalse` | `\tikztrue` | tikz (diagrams and flowcharts) |
| `\listingfalse` | `\listingtrue` | minted (code listings) |

## Bibliography

Set `\bibtrue` in `config.tex`, then add sources to `references.bib`:

```bibtex
@book{key,
  author = {Last, First},
  title  = {Book Title},
  year   = {2025}
}
```

Cite in text with `\cite{key}`.

When bibliography is enabled, use the **xelatex $\to$ biber $\to$ xelatex $\times$ 2** compilation recipe.

## Inserting Images

Images are split across two directories:

- `images/` — static images (logo, screenshots, photos)
- `figures/` — plots and diagrams (preferably PDF or PNG)

Specify the file path without the directory:

```latex
\includegraphics[width=0.5\linewidth]{image_name.png}
```

It is advisable to use PDF for plots:

```python
plt.savefig("figures/plot.pdf", bbox_inches="tight")
```

## Compilation Recipes

| Recipe | When to use |
|---|---|
| xelatex $\times$ 2 | Default — two passes for correct TOC and references |
| xelatex $\times$ 1 | Quick check when TOC is not needed |
| xelatex $\to$ biber $\to$ xelatex $\times$ 2 | Only when `\bibtrue` is set in `config.tex` |

## VS Code Settings

Key parameters in `.vscode/settings.json`:

- `autoBuild.run: never` — auto-compilation disabled, build manually (`Ctrl+Alt+B`)
- `autoClean.run: never` — auxiliary files are not cleaned after build (speeds up recompilation; the `build/` directory is in `.gitignore`)
- `view.pdf.viewer: tab` — PDF opens in a VS Code tab
- `synctex.afterBuild.enabled: true` — syncs source and PDF
