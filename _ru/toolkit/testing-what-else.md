---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Что ещё может делать MUnit?
type: section
description: Неполный список возможностей MUnit
language: ru
num: 9
previous-page: testing-resources
next-page: os-intro
---

{% include markdown.html path="_markdown/_ru/install-munit.md" %}

## Добавление подсказок для получения лучшего отчета об ошибках

Используйте `clue` внутри `assert` для получения более точного отчета об ошибке,
если утверждение не выполняется.

{% tabs clues %}
{% tab 'Scala 2 and 3' %}

```scala
assert(clue(List(a).head) > clue(b))
// munit.FailException: assertion failed
// Clues {
//   List(a).head: Int = 1
//   b: Int = 2
// }
```

{% endtab %}
{% endtabs %}

Подробнее о подсказках читайте в [документации MUnit](https://scalameta.org/munit/docs/assertions.html#assert).

## Написание тестов, специфичных для конкретной среды

Используется `assume` для написания тестов, специфичных для среды.
`assume` может содержать логическое условие.
Вы можете проверить операционную систему, версию Java, свойство Java, переменную среды или что-либо еще.
Тест пропускается, если одно из его предположений не выполняется.

{% tabs assumption class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala
import scala.util.Properties

test("home directory") {
  assume(Properties.isLinux, "this test runs only on Linux")
  assert(os.home.toString.startsWith("/home/"))
}
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
import scala.util.Properties

test("home directory") {
  assume(Properties.isLinux, "this test runs only on Linux")
  assert(os.home.toString.startsWith("/home/"))
}
```

{% endtab %}
{% endtabs %}

Подробнее о фильтруемых тестах читайте в [документации MUnit](https://scalameta.org/munit/docs/filtering.html).

## Пометка нестабильных тестов

Вы можете пометить тест тегом `flaky`, чтобы отметить его как нестабильный.
Нестабильные тесты можно пропустить, установив переменную окружения `MUNIT_FLAKY_OK` на `true`.

{% tabs flaky class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala
test("requests".flaky) {
  // Тяжелые тесты ввода-вывода, которые иногда терпят неудачу
}
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
test("requests".flaky) {
  // Тяжелые тесты ввода-вывода, которые иногда терпят неудачу
}
```

{% endtab %}
{% endtabs %}

Подробнее о нестабильных тестах читайте в [документации MUnit](https://scalameta.org/munit/docs/tests.html#tag-flaky-tests).
