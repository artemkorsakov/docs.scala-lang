---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как управлять ресурсами в тестах?
type: section
description: Описание функциональных возможностей
language: ru
num: 8
previous-page: testing-asynchronous
next-page: testing-what-else
---

{% include markdown.html path="_markdown/_ru/install-munit.md" %}

## `FunFixture`

В MUnit мы используем функциональные фикстуры для управления ресурсами лаконичным и безопасным способом.
`FunFixture` создает один ресурс для каждого теста, гарантируя, что каждый тест выполняется изолированно от других.

В тестовом сценарии вы можете определить и использовать `FunFixture` следующим образом:

{% tabs 'resources-1' class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala mdoc
class FileTests extends munit.FunSuite {
  val usingTempFile: FunFixture[os.Path] = FunFixture(
    setup = _ => os.temp(prefix = "file-tests"),
    teardown = tempFile => os.remove(tempFile)
  )
  usingTempFile.test("overwrite on file") { tempFile =>
    os.write.over(tempFile, "Hello, World!")
    val obtained = os.read(tempFile)
    assertEquals(obtained, "Hello, World!")
  }
}
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
class FileTests extends munit.FunSuite:
  val usingTempFile: FunFixture[os.Path] = FunFixture(
    setup = _ => os.temp(prefix = "file-tests"),
    teardown = tempFile => os.remove(tempFile)
  )
  usingTempFile.test("overwrite on file") { tempFile =>
    os.write.over(tempFile, "Hello, World!")
    val obtained = os.read(tempFile)
    assertEquals(obtained, "Hello, World!")
  }
```

{% endtab %}
{% endtabs %}

`usingTempFile` является фикстурой типа `FunFixture[os.Path]`.
Он содержит две функции:

- Функция `setup` типа `TestOptions => os.Path` создает новый временный файл.
- Функция `teardown` типа `os.Path => Unit` удаляет этот временный файл.

Мы используем фикстуру `usingTempFile` для определения теста, которому нужен временный файл.
Обратите внимание, что тело теста принимает `tempFile` типа `os.Path` в качестве параметра.
Фикстура автоматически создает этот временный файл, вызывает его функцию `setup`
и очищает его после окончания теста, вызывая `teardown`.

В примере мы использовали фикстуру для управления временным файлом.
В общем случае фикстуры могут управлять другими видами ресурсов,
такими как временная папка, временная таблица в базе данных, подключение к локальному серверу и т.д.

## Композиция `FunFixture`

В некоторых тестах вам может понадобиться более одного ресурса.
Вы можете использовать `FunFixture.map2` для объединения двух функциональных фикстур в одно.

{% tabs 'resources-2' class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala
val using2TempFiles: FunFixture[(os.Path, os.Path)] =
  FunFixture.map2(usingTempFile, usingTempFile)

using2TempFiles.test("merge two files") {
  (file1, file2) =>
    // тело теста
}
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
val using2TempFiles: FunFixture[(os.Path, os.Path)] =
  FunFixture.map2(usingTempFile, usingTempFile)

using2TempFiles.test("merge two files") {
  (file1, file2) =>
    // тело теста
}
```

{% endtab %}
{% endtabs %}

Использование `FunFixture.map2` на `FunFixture[A]` и `FunFixture[B]` возвращает `FunFixture[(A, B)]`.

## Другие фикстуры

`FunFixture` является рекомендуемым типом фикстуры, поскольку:

- это явно: каждый тест объявляет ресурс, который ему нужен,
- его безопасно использовать: каждый тест использует свой собственный ресурс изолированно.

Для большей гибкости MUnit содержит другие типы фикстур:
повторно используемая фикстура, ad-hoc фикстура и асинхронная фикстура.
Узнайте больше о них в [документации MUnit](https://scalameta.org/munit/docs/fixtures.html).
