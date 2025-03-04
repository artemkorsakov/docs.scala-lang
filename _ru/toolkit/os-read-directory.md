---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как прочитать директорию?
type: section
description: Чтение содержимого директории с помощью OS-Lib
language: ru
num: 11
previous-page: os-intro
next-page: os-read-file
---

{% include markdown.html path="_markdown/_ru/install-os-lib.md" %}

## Paths

Фундаментальным типом данных в OS-Lib является `os.Path`, представляющий путь в файловой системе.
`os.Path` всегда является абсолютным путем.

OS-Lib также предоставляет `os.RelPath` (относительные пути)
и `os.SubPath` (относительный путь, который не может восходить к родительским каталогам).

Типичными отправными точками для создания путей являются `os.pwd` (текущий рабочий каталог),
`os.home` (домашний каталог текущего пользователя), `os.root` (корень файловой системы)
или `os.temp.dir()` (новый временный каталог).

`Paths` определяет метод `/` добавления сегментов пути.
Например:

{% tabs 'etc' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc
val etc: os.Path = os.root / "etc"
```

{% endtab %}
{% endtabs %}

## Чтение каталога

`os.list` возвращает содержимое каталога:

{% tabs 'list-etc' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc
val entries: Seq[os.Path] = os.list(os.root / "etc")
```

{% endtab %}
{% endtabs %}

Или, если нам нужны только подкаталоги:

{% tabs 'subdirs' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc
val dirs: Seq[os.Path] = os.list(os.root / "etc").filter(os.isDir)
```

{% endtab %}
{% endtabs %}

Чтобы рекурсивно спуститься по всему поддереву, измените `os.list` на `os.walk`.
Чтобы обрабатывать результаты «на лету», а не считывать их все вначале в память, замените на `os.walk.stream`.
