# $\LaTeX$ шаблон для отчётов о лабораторных и семинарских работах

Готовый шаблон для оформления отчётов. Настроен для работы с VS Code. Вот пример [скомпилированного шаблона](main.pdf).

[English](README.md) | [Русский](README.ru.md)

## Требования

- XeLaTeX
- VS Code
- LaTeX Workshop
- LTeX+ (опционально)

## Файловая структура

```
latex-lab-template/
├──.vscode/                  # Каталог с настройками 
│   ├── settings.json            # Настройки LaTeX Workshop и LTeX+
│   └── keybindings.json         # Шорткаты для ручной компиляции
├── chapters/                # Каталог с частями, из которых состоит отчёт
│   └── chapter1.tex             # Файл в качестве примера
├── figures/                 # Каталог с графиками функций, схемами и т. п.
├── images/                  # Каталог с различными изображениями
│   └── logo.png                 # Логотип университета
├── pages/                   # Каталог со статичными страницами
│   ├── titlepage.tex            # Титульная страница
│   └── bibliography.tex         # Страница со списком используемой литературы
├──.gitignore
├── config.tex               # Параметры конкретной работы
├── macros.tex               # Собственные команды
├── main.tex                 # Преамбула и структура документа
├── README.md
└── references.bib           # База данных источников литературы
```

## Опциональные пакеты

Управляются булевыми флагами в `config.tex`. По умолчанию все отключены.

| Флаг | Включить | Пакеты |
|---|---|---|
| `\bibfalse` | `\bibtrue` | biblatex (список литературы) |
| `\plotsfalse` | `\plotstrue` | pgfplots (графики функций) |
| `\tikzfalse` | `\tikztrue` | tikz (схемы и диаграммы) |
| `\listingfalse` | `\listingtrue` | minted (листинг кода) |

## Библиография

В `config.tex` установите `\bibtrue`, затем добавляйте источники в `references.bib`:

```bibtex
@book{key,
  author = {Фамилия, Имя},
  title  = {Название книги},
  year   = {2025}
}
```

В тексте ссылайтесь: `\cite{key}`.

При включённой библиографии используйте рецепт компиляции **xelatex $\to$ biber $\to$ xelatex $\times$ 2**.

## Вставка изображений

Изображения распределены по двум каталогам:

- `images/` — статичные изображения (логотип, скриншоты, фото)
- `figures/` — графики и схемы (желательно в формате PNG или PDF)

Путь к файлу указывайте без каталога:
```latex
\includegraphics[width=0.5\linewidth]{image_name.png}
```

Для графиков желательно использовать PDF:
```python
plt.savefig("figures/plot.pdf", bbox_inches="tight")
```

## Рецепты компиляции

| Рецепт | Когда использовать |
|---|---|
| xelatex $\times$ 2 | По умолчанию — два прохода для корректного оглавления и ссылок |
| xelatex $\times$ 1 | Быстрая проверка, когда оглавление не нужно |
| xelatex $\to$ biber $\to$ xelatex $\times$ 2 | Только при включённом флаге `\bibtrue` в `config.tex` |

## Настройки VS Code

Ключевые параметры в `.vscode/settings.json`:

**LaTeX Workshop:**

- `autoBuild.run: never` — автокомпиляция отключена, сборка запускается вручную (`Ctrl+Alt+B`)
- `autoClean.run: never` — вспомогательные файлы не очищаются (ускоряет повторную компиляцию; каталог `build/` в `.gitignore`, поэтому в репозиторий они не попадают)
- `view.pdf.viewer: tab` — PDF в отдельной вкладке
- `synctex.afterBuild.enabled: true` — синхронизация кода и PDF

**LTeX+** (проверка орфографии и грамматики):

- `ltex.language: ru-RU` — проверка на русском языке
- `ltex.enabled: [latex, bibtex]` — включён для файлов `.tex` и `.bib`
- `ltex.latex.environments` — листинги кода и математические окружения игнорируются
- `ltex.latex.commands` — кастомные команды шаблона игнорируются
- `ltex.additionalRules.motherTongue: ru-RU` — улучшает проверку русской грамматики
