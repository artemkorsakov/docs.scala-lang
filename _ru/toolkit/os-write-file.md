---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как записать в файл?
type: section
description: Запись файлов на диск с помощью OS-Lib
language: ru
num: 13
previous-page: os-read-file
next-page: os-run-process
---

{% include markdown.html path="_markdown/_ru/install-os-lib.md" %}

## Запись файла целиком за один раз

`os.write` записывает предоставленную строку в новый файл:

{% tabs write %}
{% tab 'Scala 2 и 3' %}

```scala mdoc
val path: os.Path = os.temp.dir() / "output.txt"
os.write(path, "hello\nthere\n")
println(os.read.lines(path).size)
// печатает: 2
```

{% endtab %}
{% endtabs %}

## Перезапись или добавление

`os.write` выдает исключение, если файл уже существует:

{% tabs already-exists %}
{% tab 'Scala 2 и 3' %}

```scala mdoc:crash
os.write(path, "this will fail")
// выбрасывается исключение:
// java.nio.file.FileAlreadyExistsException
```

{% endtab %}
{% endtabs %}

Чтобы избежать этого, используйте `os.write.over` для замены существующего содержимого.

Вы также можете использовать `os.write.append`, чтобы добавить что-то в конец:

{% tabs append %}
{% tab 'Scala 2 и 3' %}

```scala mdoc
os.write.append(path, "two more\nlines\n")
println(os.read.lines(path).size)
// печатает: 4
```

{% endtab %}
{% endtabs %}
