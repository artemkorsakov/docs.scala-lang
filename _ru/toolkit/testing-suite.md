---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как писать тесты?
type: section
description: Основы написания тестового набора с помощью MUnit
language: ru
num: 3
previous-page: testing-intro
next-page: testing-run
---

{% include markdown.html path="_markdown/_ru/install-munit.md" %}

## Написание тестового сценария

Группа тестов в одном классе называется тестовым классом или набором тестов.

Каждый тестовый набор проверяет определенный компонент или функцию программного обеспечения.
Обычно мы определяем один тестовый набор для каждого исходного файла или класса, который мы хотим протестировать.

{% tabs munit-unit-test-2 class=tabs-build-tool %}
{% tab 'Scala CLI' %}

В Scala CLI тестовый файл может находиться в той же папке, что и реальный код,
но имя файла должно заканчиваться на `.test.scala`.
Ниже представлен тестовый файл `MyTests.test.scala`.

```
example/
├── MyApp.scala
└── MyTests.test.scala
```

Другие допустимые структуры и соглашения описаны в [документации Scala CLI](https://scala-cli.virtuslab.org/docs/commands/test/#test-sources).

{% endtab %}

{% tab 'sbt' %}

В sbt исходные тексты тестов находятся в папке `src/test/scala`.

Например, ниже приведена файловая структура проекта `example`:

```
example
└── src
    ├── main
    │   └── scala
    │       └── MyApp.scala
    └── test
        └── scala
            └── MyTests.scala
```

{% endtab %}

{% tab 'Mill' %}

В Mill исходные тестовые данные помещаются в папку `test/src`.

Например, ниже приведена файловая структура модуля `example`:

```
example
└── src
|   └── MyApp.scala
└── test
    └── src
        └── MyTests.scala
```

{% endtab %}
{% endtabs %}

В исходном файле теста определите сценарий, содержащий один тест:

{% tabs munit-unit-test-3 class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala
package example

class MyTests extends munit.FunSuite {
  test("sum of two integers") {
    val obtained = 2 + 2
    val expected = 4
    assertEquals(obtained, expected)
  }
}
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
package example

class MyTests extends munit.FunSuite:
  test("sum of two integers") {
    val obtained = 2 + 2
    val expected = 4
    assertEquals(obtained, expected)
  }
```

{% endtab %}
{% endtabs %}

Тестовый набор — это Scala класс, который расширяет `munit.FunSuite`.
Он содержит один или несколько тестов, каждый из которых определяется вызовом метода `test`.

В предыдущем примере у нас есть один тест `"sum of integers"`, который проверяет, что `2 + 2` равно `4`.
Мы используем метод утверждения `assertEquals`, чтобы проверить, что два значения равны.
Тест проходит, если все утверждения верны, и не выполняется в противном случае.

## Утверждения

Важно использовать утверждения в каждом тесте, чтобы описать, что проверять.
Основные методы утверждений в MUnit:

- `assertEquals`, чтобы проверить, что то, что вы получаете, соответствует тому, что вы ожидали
- `assert` для проверки логического условия

Ниже приведен пример теста, который использует `assert` для проверки логического условия в списке.

{% tabs assertions-1 class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala
test("all even numbers") {
  val input: List[Int] = List(1, 2, 3, 4)
  val obtainedResults: List[Int] = input.map(_ * 2)
  // проверьте, что все полученные значения являются четными числами
  assert(obtainedResults.forall(x => x % 2 == 0))
}
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
test("all even numbers") {
  val input: List[Int] = List(1, 2, 3, 4)
  val obtainedResults: List[Int] = input.map(_ * 2)
  // проверьте, что все полученные значения являются четными числами
  assert(obtainedResults.forall(x => x % 2 == 0))
}
```

{% endtab %}
{% endtabs %}

MUnit содержит больше методов утверждений, которые вы можете найти в его [документации](https://scalameta.org/munit/docs/assertions.html):
`assertNotEquals`, `assertNoDiff`, `fail` и `compileErrors`.
