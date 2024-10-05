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

Настройка титульной страницы производится внутри шаблона. Для *eisvogel-custom_mephi_titlepage.tex* это 935-1020 строки.

## Использование

В папке *src/* пишутся файлы в поддерживаемых форматах. Далее *Python* скрипт *build.py* вытягивает настройки из конфигурационного файла *build-conf.toml*, объединяет их во временный файл *metadata.yaml* и выполняет *bash* команду, создающую выходной файл.

Для соединения в единый PDF файл используется утилита [pandoc](https://pandoc.org/index.html). Он под капотом переводит, если необходимо *markdown* файлы в единый *.tex* файл, добавляет к ним шаблон и настройки и формирует единый PDF.

Для конвертирования исходников в один PDF файл достаточно в конфигурационном файле *build-conf.toml* сделать следующие изменения:

- Написать список исходных файлов, которые будут входить в PDF, в поле *src*.
- Написать имя выходящего файла в поле *out-name*. Формат выходящего файла важен.

**ВАЖНО!** Исходники должны быть одного и того же формата: либо с расширением *".tex"*, либо с расширением *".md"*.

## Шаблоны

В проекте приведены два шаблона, основанные на шаблоне [eisvogel](https://github.com/enhuiz/eisvogel?ysclid=m1wjhh6qka717625755).

*eisvogel-custom.tex:*

- Адаптированная версия оригинала для русского языка.

*eisvogel-custom_mephi_titlepage.tex:*

- Добавлена титульная страница - пародия на ГОСТ:

| без логотипа                                         | с логотипом                                         |
| ---------------------------------------------------- | --------------------------------------------------- |
| ![титульник без логотипа](/assets/title-page-01.jpg) | ![титульник с логотипом](/assets/title-page-02.jpg) |
