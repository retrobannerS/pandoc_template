# Универсальный шаблон для написания документов и конспектов

## Требования

Установленные *Python*, *Pandoc* и *xeLaTex*(один из теховских движков).

## Необходимые библиотеки

Все требуемые библиотеки для *Python* записаны в *requirements.txt*. Для установки нужно написать в терминал(находясь в папке с проектом):

```bash
pip install -r src/requirements.txt
```

Все требуемые библиотеки для *LaTeX* записаны в *requirements.txt*. Для установки нужно написать в терминал(находясь в папке с проектом):

```bash
sudo xargs tlmgr install < tex_requirements.txt
```

## Поддерживаемые языки верстки

- LaTeX.
- MarkDown.

## Настройка

Вся настройка данного способа писать документы находится в конфигурационном файле *build-conf.toml*.
Для начала будет достаточно поменять название документа и автора в полях *title* и *author*.
Обо всех флагах в настройке можно почитать в [документации pandoc](https://pandoc.org/MANUAL.html).

Настройка титульной страницы производится внутри шаблона. Для *eisvogel-custom_mephi_titlepage.tex* это 939-1024 строки. Автор, дата и название документа вставляются автоматически из *metadata*.

## Использование

В папке *src/* пишутся файлы в поддерживаемых форматах. Далее *Python* скрипт *build.py* вытягивает настройки из конфигурационного файла *build-conf.toml*, объединяет их во временный файл *metadata.yaml* и выполняет *bash* команду, создающую выходной файл.

Для соединения в единый PDF файл используется утилита [pandoc](https://pandoc.org/index.html). Он под капотом переводит, если необходимо, *markdown* файлы в единый *.tex* файл, добавляет к ним шаблон и настройки и формирует единый PDF.

Для конвертирования исходников в один PDF файл достаточно в конфигурационном файле *build-conf.toml* сделать следующие изменения:

- Написать список исходных файлов, которые будут входить в PDF, в поле *src*.
- Написать имя выходящего файла в поле *out-name*. Формат выходящего файла важен.

**ВАЖНО!** Исходники должны быть одного и того же формата: либо с расширением *".tex"*, либо с расширением *".md"*.

Запуск *Python* скрипта создает выходной PDF:

```bash
python3 ./scripts/build.py
```

Папка *examples* независима и может быть удалена для личного использования этого шаблона. То же самое касается *README.md*, *title-page-01.jpg* и *title-page-02.jpg*

## Примеры

В корневой папке вы можете встретить *examples* - примеры сборки файлов в один PDF на основе *LaTeX* и *MarkDown*.

## Возможные выводы в терминал

1.

```bash 
[WARNING] Unusual conversion: to convert a .tex file to PDF, you get better results by using pdflatex (or lualatex or xelatex) directly, 
try `pdflatex src/ ... .tex` instead of `pandoc src/ ... .tex -o ... .pdf`.
```

В данном случае *Pandoc* советует использовать движок *LaTeX* напрямую, мол зачем ты используешь *Pandoc*, если ты все равно пишешь на чистом *LaTeX*, можешь использовать другую программу, которая специально для этого предназначалась, она еще и эффективнее будет.

Я не обращаю внимание на этот вывод, потому что работать с *Pandoc* более удобно, чем с движком напрямую, хоть и идет потеря по времени компиляции.

2. 

```bash 
[WARNING] [makePDF] LaTeX Warning: Command \underbar has changed. Check if current package is valid.
[WARNING] [makePDF] LaTeX Warning: Command \underline has changed. Check if current package is valid.
```

Этот вывод возникает при использовании следующих дополнительных включений *LaTeX* в конфигурационный файл *build-conf.toml*:

```latex
\usepackage{sectsty}
\sectionfont{\clearpage}
```

Под капотом происходит переопределение команд $underbar$ и $underline$, на которое мы никак не можем повлиять. Поэтому стоит игнорировать этот вывод.

## Шаблоны

В проекте приведены два шаблона, основанные на шаблоне [eisvogel](https://github.com/enhuiz/eisvogel?ysclid=m1wjhh6qka717625755).

*eisvogel-custom.tex:*

- Адаптированная версия оригинала для русского языка.
- Мелкие обновления, связанные с цветами подписей к картинкам/блокам кода.
- Правка, связанная с *[обновлением pandoc](https://pandoc.org/releases.html#pandoc-3.2.1-2024-06-24)*. Там изменилась вставка картинок, а шаблон не обновлялся с 2020.

*eisvogel-custom_mephi_titlepage.tex:*

- Добавлена титульная страница - пародия на ГОСТ:

| без логотипа                                         | с логотипом                                         |
| ---------------------------------------------------- | --------------------------------------------------- |
| ![титульник без логотипа](/assets/title-page-01.jpg) | ![титульник с логотипом](/assets/title-page-02.jpg) |
