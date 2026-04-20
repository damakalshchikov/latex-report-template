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
├──.vscode/                          # Каталог с настройками
│   ├── .settings-for-linux.json         # Шаблон настроек для Linux
│   ├── .settings-for-macos.json         # Шаблон настроек для macOS
│   ├── keybindings.json                 # Шорткаты для ручной компиляции
│   └── settings.json                    # Локальные настройки (не отслеживаются git)
├── chapters/                        # Каталог с частями, из которых состоит отчёт
│   └── chapter1.tex                     # Файл в качестве примера
├── figures/                         # Каталог с графиками функций, схемами и т. п.
├── images/                          # Каталог с различными изображениями
│   └── logo.png                         # Логотип университета
├── pages/                           # Каталог со статичными страницами
│   ├── titlepage.tex                    # Титульная страница
│   └── bibliography.tex                 # Страница со списком используемой литературы
├──.gitignore
├── config.tex                       # Параметры документа и шрифты
├── macros.tex                       # Собственные команды
├── main.tex                         # Преамбула и структура документа
├── README.md
└── references.bib                   # База данных источников литературы
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

При включённой библиографии используйте рецепт компиляции **xelatex -> biber -> xelatex x 2**.

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
| xelatex x 2 | По умолчанию — два прохода для корректного оглавления и ссылок |
| xelatex x 1 | Быстрая проверка, когда оглавление не нужно |
| xelatex -> biber -> xelatex x 2 | Только при включённом флаге `\bibtrue` в `config.tex` |

## Шрифты

Шрифты задаются в `config.tex`:

```latex
\newcommand{\mainfont}{Times New Roman}  % для Windows/macOS
% \newcommand{\mainfont}{Liberation Serif}  % для Linux

\newcommand{\monofont}{Courier New}      % для Windows/macOS
% \newcommand{\monofont}{Liberation Mono}   % для Linux
```

На Linux раскомментируйте строки для Linux и закомментируйте строки для Windows/macOS.

## Настройки VS Code

`settings.json` не отслеживается git — каждый пользователь создаёт его локально, скопировав нужный шаблон из `.vscode/`:

- `.settings-for-linux.json` — для Linux
- `.settings-for-macos.json` — для macOS

Ключевые параметры:

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
