---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как запустить процесс?
type: section
description: Запуск внешних подпроцессов с помощью OS-Lib
language: ru
num: 14
previous-page: os-write-file
next-page: os-what-else
---

{% include markdown.html path="_markdown/_ru/install-os-lib.md" %}

## Запуск внешнего процесса

Чтобы настроить процесс, используйте `os.proc`, а затем, чтобы запустить его, `call()`:

{% tabs 'touch' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc:compile-only
val path: os.Path = os.pwd / "output.txt"
println(os.exists(path))
// печатает: false
val result: os.CommandResult = os.proc("touch", path).call()
println(result.exitCode)
// печатает: 0
println(os.exists(path))
// печатает: true
```

{% endtab %}
{% endtabs %}

Обратите внимание, что `proc` принимает как строки, так и `os.Path`-ы.

## Чтение выходных данных процесса

> Конкретные команды в следующих примерах могут отсутствовать на некоторых компьютерах.

Выше мы видели, что `call()` вернул `os.CommandResult`.
Мы можем получить доступ ко всему выводу результата с помощью `out.text()`,
или в виде строк с помощью `out.lines()`.

Например, мы могли бы использовать `bc` для выполнения некоторых математических расчетов:

{% tabs 'bc' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc:compile-only
val res: os.CommandResult = os.proc("bc", "-e", "2 + 2").call()
val text: String = res.out.text()
println(text.trim.toInt)
// печатает: 4
```

{% endtab %}
{% endtabs %}

Или `cal` для показа календаря:

{% tabs 'cal' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc:compile-only
val res: os.CommandResult = os.proc("cal", "-h", "2", "2023").call()
res.out.lines().foreach(println)
// печатает:
//   February 2023
// Su Mo Tu We Th Fr Sa
//          1  2  3  4
// ...
```

{% endtab %}
{% endtabs %}

## Настройка процесса

`call()` принимает множество необязательных аргументов,
слишком много, чтобы попытаться описать их здесь по отдельности.
Например, вы можете задать рабочий каталог (`cwd = ...`),
задать переменные окружения (`env = ...`)
или перенаправить ввод и вывод (`stdin = ..., stdout = ..., stderr = ...`).
Дополнительную информацию о методе `call` можно найти в [README OS-Lib](https://github.com/com-lihaoyi/os-lib#osproccall).
