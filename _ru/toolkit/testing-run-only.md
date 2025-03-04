---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как запустить одиночный тест?
type: section
description: О testOnly в инструменте сборки и .only в MUnit
language: ru
num: 5
previous-page: testing-run
next-page: testing-exceptions
---

{% include markdown.html path="_markdown/_ru/install-munit.md" %}

## Запуск одиночного тестового сценария

{% tabs munit-unit-test-only class=tabs-build-tool %}
{% tab 'Scala CLI' %}

Чтобы запустить один сценарий `example.MyTests` с помощью Scala CLI, используйте опцию `--test-only` команды `test`.

```
scala-cli test example --test-only example.MyTests
```

{% endtab %}
{% tab 'sbt' %}

Чтобы запустить один сценарий `example.MyTests` в sbt, используйте задачу `testOnly`:

```
sbt:example> testOnly example.MyTests
```

{% endtab %}
{% tab 'Mill' %}

Чтобы запустить один сценарий `example.MyTests` в Mill, используйте задачу `testOnly`:

```
./mill example.test.testOnly example.MyTests
```

{% endtab %}
{% endtabs %}

## Запуск одного теста в тестовом сценарии

В файле тестового сценария вы можете выбрать отдельные тесты для запуска, временно добавив `.only`, например:

{% tabs 'only-demo' class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala mdoc
class MathSuite extends munit.FunSuite {
  test("addition") {
    assert(1 + 1 == 2)
  }
  test("multiplication".only) {
    assert(3 * 7 == 21)
  }
}
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
class MathSuite extends munit.FunSuite:
  test("addition") {
    assert(1 + 1 == 2)
  }
  test("multiplication".only) {
    assert(3 * 7 == 21)
  }
```

{% endtab %}
{% endtabs %}

В приведенном выше примере будут запущены только тесты `"multiplication"` (т.е. `"addition"` игнорируются).
Это полезно для быстрой отладки определенного теста в наборе.

## Альтернатива: исключение определенных тестов

Вы можете исключить определенные тесты из запуска, добавив к имени теста `.ignore`.
Например, следующий код игнорирует тест `"addition"` и запускает все остальные:

{% tabs 'ignore-demo' class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala mdoc:reset
class MathSuite extends munit.FunSuite {
  test("addition".ignore) {
    assert(1 + 1 == 2)
  }
  test("multiplication") {
    assert(3 * 7 == 21)
  }
  test("remainder") {
    assert(13 % 5 == 3)
  }
}
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
class MathSuite extends munit.FunSuite:
  test("addition".ignore) {
    assert(1 + 1 == 2)
  }
  test("multiplication") {
    assert(3 * 7 == 21)
  }
  test("remainder") {
    assert(13 % 5 == 3)
  }
```

{% endtab %}
{% endtabs %}

## Используйте теги для группировки тестов и запуска тестов только с определенными тегами

MUnit позволяет группировать и запускать тесты по наборам по тегам, которые являются текстовыми метками.
В [документации MUnit][munit-tags] есть инструкции о том, как это сделать.

[munit-tags]: https://scalameta.org/munit/docs/filtering.html#include-and-exclude-tests-based-on-tags
