---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как тестировать исключения?
type: section
description: Описание утверждений о перехвате
language: ru
num: 6
previous-page: testing-run-only
next-page: testing-asynchronous
---

{% include markdown.html path="_markdown/_ru/install-munit.md" %}

## Перехват исключения

В тесте вы можете использовать `intercept`, чтобы проверить, выдает ли ваш код исключение.

{% tabs 'intercept-1' class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala mdoc
import java.nio.file.NoSuchFileException

class FileTests extends munit.FunSuite {
  test("read missing file") {
    val missingFile = os.pwd / "missing.txt"

    intercept[NoSuchFileException] {
      // код, который должен выдавать исключение
      os.read(missingFile)
    }
  }
}
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
import java.nio.file.NoSuchFileException

class FileTests extends munit.FunSuite:
  test("read missing file") {
    val missingFile = os.pwd / "missing.txt"
    intercept[NoSuchFileException] {
      // код, который должен выдавать исключение
      os.read(missingFile)
    }
  }
```

{% endtab %}
{% endtabs %}

Параметр типа утверждения `intercept` — ожидаемое исключение.
Вот оно - `NoSuchFileException`.
Тело утверждения `intercept` содержит код, который должен выдать исключение.

Тест считается пройденным, если код выдает ожидаемое исключение, в противном случае тест завершается неудачей.

Метод `intercept` возвращает выдаваемое исключение, на котором можно проверить дополнительные утверждения.

{% tabs 'intercept-2' %}
{% tab 'Scala 2 and 3' %}

```scala
val exception = intercept[NoSuchFileException](os.read(missingFile))
assert(clue(exception.getMessage).contains("missing.txt"))
```

{% endtab %}
{% endtabs %}

Вы также можете использовать более лаконичный метод `interceptMessage` для проверки исключения
и его сообщения в одном утверждении.
Узнайте больше об этом в [документации MUnit](https://scalameta.org/munit/docs/assertions.html#interceptmessage).
