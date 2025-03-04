---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как запускать тесты?
type: section
description: Выполнение MUnit тестов
language: ru
num: 4
previous-page: testing-suite
next-page: testing-run-only
---

{% include markdown.html path="_markdown/_ru/install-munit.md" %}

## Запуск тестов

Вы можете запустить все свои тестовые сценарии с помощью одной команды.

{% tabs munit-unit-test-4 class=tabs-build-tool %}
{% tab 'Scala CLI' %}

Используя Scala CLI, следующая команда запускает все тесты в папке `example`:

```
scala-cli test example
# Compiling project (test, Scala 3.2.1, JVM)
# Compiled project (test, Scala 3.2.1, JVM)
# MyTests:
#  + sum of two integers 0.009s
```

{% endtab %}
{% tab 'sbt' %}

В sbt shell следующая команда запускает все тесты проекта `example`:

```
sbt:example> test
# MyTests:
#   + sum of two integers 0.006s
# [info] Passed: Total 1, Failed 0, Errors 0, Passed 1
# [success] Total time: 0 s, completed Nov 11, 2022 12:54:08 PM
```

{% endtab %}
{% tab 'Mill' %}

В Mill следующая команда запускает все тесты модуля `example`:

```
./mill example.test.test
# [71/71] example.test.test
# MyTests:
#   + sum of two integers 0.008s
```

{% endtab %}
{% endtabs %}

Отчет о тесте, напечатанный в консоли, показывает статус каждого теста.
Символ `+` перед названием теста показывает, что тест пройден успешно.

Добавьте и запустите невалидный тест, чтобы увидеть, как выглядит сообщение об ошибке:

{% tabs assertions-1 class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala
test("failing test") {
  val obtained = 2 + 3
  val expected = 4
  assertEquals(obtained, expected)
}
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
test("failing test") {
  val obtained = 2 + 3
  val expected = 4
  assertEquals(obtained, expected)
}
```

{% endtab %}
{% endtabs %}

```
# MyTests:
#   + sum of two integers 0.008s
# ==> X MyTests.failing test  0.015s munit.ComparisonFailException: ./MyTests.test.scala:13
# 12:    val expected = 4
# 13:    assertEquals(obtained, expected)
# 14:  }
# values are not the same
# => Obtained
# 5
# => Diff (- obtained, + expected)
# -5
# +4
#     at munit.Assertions.failComparison(Assertions.scala:274)
```

Строка, начинающаяся с `==> X` указывает на то, что тест с именем `failing test` не пройден.
Следующие строки показывают, где и как он упал с ошибкой.
Здесь показано, что полученное значение равно `5`, хотя ожидалось `4`.
