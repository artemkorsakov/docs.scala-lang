---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как прочитать файл?
type: section
description: Чтение файлов с диска с помощью OS-Lib
language: ru
num: 12
previous-page: os-read-directory
next-page: os-write-file
---

{% include markdown.html path="_markdown/_ru/install-os-lib.md" %}

## Чтение файла

Предположим, у нас есть путь к файлу:

{% tabs 'path' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc
val path: os.Path = os.root / "usr" / "share" / "dict" / "words"
```

{% endtab %}
{% endtabs %}

Затем мы можем преобразовать весь файл в строку с помощью `os.read`:

{% tabs slurp %}
{% tab 'Scala 2 и 3' %}

```scala mdoc:compile-only
val content: String = os.read(path)
```

{% endtab %}
{% endtabs %}

Чтобы прочитать файл построчно, замените на `os.read.lines`.

Мы можем найти самое длинное слово в словаре:

{% tabs lines %}
{% tab 'Scala 2 и 3' %}

```scala mdoc:compile-only
val lines: Seq[String]  = os.read.lines(path)
println(lines.maxBy(_.size))
// печатает: antidisestablishmentarianism
```

{% endtab %}
{% endtabs %}

Также есть вариант `os.read.lines.stream`, если вы хотите обрабатывать строки "на лету",
а не сразу считывать их все в память.
Например, если мы хотим прочитать только первую строку, наиболее эффективным способом будет:

{% tabs lines-stream %}
{% tab 'Scala 2 и 3' %}

```scala mdoc:compile-only
val lineStream: geny.Generator[String] = os.read.lines.stream(path)
val firstLine: String = lineStream.head
println(firstLine)
// печатает: A
```

{% endtab %}
{% endtabs %}

OS-Lib заботится о закрытии файла после того, как генератор, возвращаемый функцией `stream`, исчерпает свои ресурсы.
