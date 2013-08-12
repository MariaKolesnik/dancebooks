﻿# dancebooks-bibtex

Данный проект ставит целью собрание наиболее полной библиографии по историческим танцам. База данных хранится в формате `.bib` и предполагает интеграцию с пакетами обработки языка разметки LaTeX.

Обработка библиографической базы данных в LaTeX выполняется двумя программами: фронтэндом (на стороне LaTeX) и бэкэндом (именно бэкэнд считывает базу данных и преобразует её в формат, понятный LaTeX'у). Теоретически, можно использовать любую существующую связку (фронтэнд + бэкэнд), официально поддерживается `(biblatex + biber)`. Кроме базы данных, реализованы собственные стилевые файлы.

Кроме этого в проект включены некоторые транскрипции танцевальных источников в формате [markdown](http://daringfireball.net/projects/markdown/syntax). Некоторые правила оформления данных источников описаны ниже.

## (biblatex + biber):

Нужно установить `biblatex-2.7`, `biblatex-gost-0.9` и `biber-1.7` (про установку данных пакетов можно будет прочесть ниже. Стилевой файл подключается так:

	\newcommand{\rootfolder}{%folderpath%}
	\usepackage[root=\rootfolder]{\rootfolder/dancebooks-biblatex}
	
Опционально доступен параметр `detailed`, принимающий значения `true` и `false`. При указании значения `true` включается печать служебной информации (наличие транскрипций, уточнение датировки и так далее). Значение по умолчанию – `false`. Пример использования опции:

	\newcommand{\rootfolder}{%folderpath%}
	\usepackage[detailed=true,root=\rootfolder]{\rootfolder/dancebooks-biblatex}
	
Опционально доступен параметр `usedefaults`, принимающий значения `true` и `false`. При указании значения `false` источники танцевальной библиографии (`.bib`-файлы из состава проекта) не подключаются. Позволяет использовать стилевой файл из дистрибутива в других проектах. Пример использования опции:

	\newcommand{\rootfolder}{%folderpath%}
	\usepackage[usedefaults=false,root=\rootfolder]{\rootfolder/dancebooks-biblatex}

После подключения становятся доступны макросы цитирования `\cite`, `\footcite`, `\parencite`, `\nocite`, `\volcite`. Работа макросов описана в [руководстве по biblatex](http://mirrors.ctan.org/macros/latex/contrib/biblatex/doc/biblatex.pdf). Стандартный макрос печати библиографии в `biblatex` -- `\printbibliography`, без параметров. Дополнительные библиографические источники можно добавить стандартной командой `\addbibresource{hello.bib}` (расширение `.bib` необходимо указывать явно).

Порядок компиляции такой:

1. `pdflatex project.tex`
2.	`biber --listsep=\| --namesep=\| --quiet project` (в POSIX окружении)
	
	`biber "--listsep=|" "--namesep=|" "test-biblatex"` (в Windows)
3. `pdflatex project.tex`
4. `pdflatex project.tex`

### Установка дополнительных пакетов. `biblatex`:

Скачать можно [по этому адресу](http://sourceforge.net/projects/biblatex/files/), вот [прямая ссылка на последнюю версию](http://sourceforge.net/projects/biblatex/files/latest/download).

Пакет есть в стандартном репозитории.

### Установка дополнительных пакетов. `biber`:

Скачать бэкэнд можно [по этому адресу](http://sourceforge.net/projects/biblatex-biber/files/biblatex-biber/), вот [прямая ссылка на последнюю версию](http://sourceforge.net/projects/biblatex-biber/files/latest/download). Внимание! Не всякая версия biber подходит для конкретной версии biblatex. Изучите, пожалуйста, информацию о необходимой вам версии biber.

Необходимо положить исполняемый файл в любую папку, после чего добавить эту папку в `%PATH%`, если этого не было сделано раньше.

Для x86-дистрибутивов пакет есть в стандартном репозитории (для MiKTeX он называется `miktex-biber-bin`).

### Установка дополнительных пакетов. `biblatex-gost`:

Скачать последнюю версию гостовских стилей можно [по этому адресу](http://sourceforge.net/projects/biblatexgost/files/), вот [прямая ссылка на последнюю версию](http://sourceforge.net/projects/biblatexgost/files/latest/download).

Скачанный архив нужно распаковать (с сохранением структуры директорий) в любую из корневых папок вашего дистрибутива.

После установки пакета нужно выполнить команду обновления кэша (команда зависит от вашего дистрибутива, для MiKTeX – `initexmf -u`).

Порядок компиляции такой:

1. pdflatex project.tex
2. biber project
3. pdflatex project.tex
4. pdflatex project.tex (в Makefile из библиографии этот пункт отсутствует, поскольку в выходных файлах нет содержания)

## О транскрипциях

Все транскрипции хранятся в формате (`markdown`)[http://daringfireball.net/projects/markdown/syntax].

Файлы транскрипций находятся в кодировке utf-8-with-BOM.

Для просмотра файлов можно установить расширения браузера: [Firefox Markdown Viewer](https://addons.mozilla.org/en-US/firefox/addon/markdown-viewer/) (стоит отметить, что для больших файлов расширение работает довольно медленно), или использовать скрипт `transcriptions/_markdown2.py3k` (потребуется `python3` с установленным модулем `markdown2`).

### Правила расстановки заголовков таковы:

* \# – ставится у названия книги,
* \#\# – ставится у автора книги или авторов, если их несколько,
* \#\#\# – ставится у всех прочих заголовков других уровней.

После символов форматирования в разметке присутствует пробел.

### Правила, применяемые при оформлении транскрипций:

Вот короткий список изменений, которые я провожу с текстом транскрипции:

* удаляются номера страниц и символы переноса внутри слов,
* \<, &lt; и \>, &gt; заменяются на ‹ и › соответственно,
* тире ставятся в соответствии с правилами русской орфографии независимо от языка оригинала,
* в словах (по возможности) ставится буква ё вместо е там, где это необходимо,
* инициалы пишутся через пробел,
* стихи и отрывки из произведений оформляются как цитаты (> ),
* короткие (однострочные) сноски вносятся прямо в текст, остальные ограничиваются горизонтальными линиями сверху и снизу (\*\*\*),
* в конце заголовков удаляются точки, в коцне абзацев — наоборот, добавляются,
* в конце строк обрезаются пробельные символы,
* неочевидная расшифровка аббревиатур и опечаток помещается в круглые скобки,
* транскрипции книг, выпущенных после 1800 года переводятся в современные орфографии.

Возможны и некоторые другие специфичные для каждой транскрипции в отдельности изменения.