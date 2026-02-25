# LaTeX шаблон для отчётов о лабораторных и семинарских работах

Готовый шаблон для оформления отчётов. Настроен для работы с XeLaTeX, поддерживает русский язык и кастомные шрифты.

## Требования

- XeLaTeX
- VS Code с расширением LaTeX Workshop

## Файловая структура

```
latex-lab-template/
├── main.tex                 # Преамбула и структура документа
├── config.tex               # Параметры конкретной работы
├── pages/
│   ├── titlepage.tex        # Вёрстка титульного листа
│   └── bibliography.tex     # Вёрстка страницы со списком литературы
├── chapters/
│   └── chapter1.tex         # Содержимое отчёта
├── images/
│   └── logo.png             # Логотип университета
├── references.bib           # База данных источников литературы
├── .vscode/
│   ├── settings.json        # Настройки LaTeX Workshop
│   └── keybindings.json     # Шорткаты для ручной компиляции
├── .gitignore
└── README.md
```

### Описание файлов

**config.tex** — единственный файл, который нужно редактировать для каждой новой работы. Содержит булевы флаги для опциональных пакетов и все параметры документа: дисциплину, тему, группу, преподавателя и т.п.

**main.tex** — преамбула с подключением пакетов и структура документа. Редактируется только для добавления новых глав через `\input{chapters/...}`.

**pages/titlepage.tex** — вёрстка титульного листа. Использует переменные из `config.tex`.

**pages/bibliography.tex** — вёрстка страницы со списком литературы. Подключается автоматически при включении флага `\bibtrue` в `config.tex`.

**chapters/chapter1.tex** — шаблон содержимого с примером структуры (раздел, подраздел).

**references.bib** — файл для хранения источников в формате BibTeX.

**.vscode/settings.json** — конфигурация LaTeX Workshop с рецептами компиляции и правилами очистки вспомогательных файлов.

**.vscode/keybindings.json** — справочник шорткатов для ручной компиляции. VS Code не подхватывает его автоматически — чтобы применить, скопируйте содержимое в `~/.config/Code/User/keybindings.json`.

## Опциональные пакеты

Управляются булевыми флагами в `config.tex`. По умолчанию все отключены.

| Флаг | Включить | Пакеты |
|---|---|---|
| `\bibfalse` | `\bibtrue` | biblatex (список литературы) |
| `\plotsfalse` | `\plotstrue` | pgfplots (графики) |
| `\listingfalse` | `\listingtrue` | listings + xcolor (листинг кода) |

### Библиография

В `config.tex` установите `\bibtrue`, затем добавляйте источники в `references.bib`:

```bibtex
@book{key,
  author = {Фамилия, Имя},
  title  = {Название книги},
  year   = {2025}
}
```

В тексте ссылайтесь: `\cite{key}`.

При включённой библиографии используйте рецепт компиляции **xelatex → biber → xelatex × 2**.

## Как добавить новую главу

1. Создайте `chapters/chapter2.tex`:

```latex
\section{Название раздела}

\subsection{Подраздел}

Ваш текст здесь...
```

2. Подключите в `main.tex` после уже существующих глав:

```latex
\input{chapters/chapter2.tex}
```

## Вставка изображений

Все изображения кладите в папку `images/`. Путь указывать без папки:

```latex
\includegraphics[width=0.5\linewidth]{image_name.png}
```

## Рецепты компиляции

### xelatex × 2 (по умолчанию)

Два прохода XeLaTeX — нужны для корректного оглавления и ссылок.

```bash
xelatex main.tex
xelatex main.tex
```

### xelatex × 1

Быстрая проверка, когда оглавление не нужно.

### xelatex → biber → xelatex × 2

Используйте только при включённом флаге `\bibtrue` в `config.tex`.

```bash
xelatex main.tex
biber main
xelatex main.tex
xelatex main.tex
```


## Настройка шрифтов

В `main.tex` настроены шрифты для Linux:

```latex
\setmainfont{Liberation Serif}
\setmonofont{DejaVu Sans Mono}[Scale=MatchLowercase]
```

Для Windows/macOS раскомментируйте альтернативные строки в `main.tex`:

```latex
\setmainfont{Times New Roman}
\setmonofont{Courier New}
```

## Настройки VS Code

Ключевые параметры в `.vscode/settings.json`:

- `autoBuild.run: never` — автокомпиляция отключена, сборка запускается вручную (`Ctrl+Alt+B`)
- `autoClean.run: never` — вспомогательные файлы не очищаются (ускоряет повторную компиляцию; папка `build/` в `.gitignore`, поэтому в репозиторий они не попадают)
- `view.pdf.viewer: tab` — PDF в отдельной вкладке
- `synctex.afterBuild.enabled: true` — синхронизация кода и PDF (двойной клик в PDF переходит к строке в коде)
