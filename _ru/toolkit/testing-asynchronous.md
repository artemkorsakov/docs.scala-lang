---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как писать асинхронные тесты?
type: section
description: Написание асинхронных тестов с использованием MUnit
language: ru
num: 7
previous-page: testing-exceptions
next-page: testing-resources
---

{% include markdown.html path="_markdown/_ru/install-munit.md" %}

## Асинхронные тесты

В Scala асинхронный метод часто возвращает `Future`.
MUnit предлагает специальную поддержку для `Future`-ов.

Например, рассмотрим асинхронный вариант метода `square`:

{% tabs 'async-1' class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala mdoc
import scala.concurrent.{ExecutionContext, Future}

object AsyncMathLib {
  def square(x: Int)(implicit ec: ExecutionContext): Future[Int] =
    Future(x * x)
}
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
import scala.concurrent.{ExecutionContext, Future}

object AsyncMathLib:
  def square(x: Int)(using ExecutionContext): Future[Int] =
    Future(x * x)
```

{% endtab %}
{% endtabs %}

Тест сам по себе может возвращать `Future[Unit]`.
MUnit будет ждать в фоновом режиме завершения результата `Future`,
завершая тест с ошибкой, если какое-либо утверждение не выполняется.

Поэтому вы можете написать тест следующим образом:

{% tabs 'async-3' class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala mdoc
// Импорт глобального контекста выполнения, необходимого для вызова асинхронных методов
import scala.concurrent.ExecutionContext.Implicits.global

class AsyncMathLibTests extends munit.FunSuite {
  test("square") {
    for {
      squareOf3 <- AsyncMathLib.square(3)
      squareOfMinus4 <- AsyncMathLib.square(-4)
    } yield {
      assertEquals(squareOf3, 9)
      assertEquals(squareOfMinus4, 16)
    }
  }
}
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
// Импорт глобального контекста выполнения, необходимого для вызова асинхронных методов
import scala.concurrent.ExecutionContext.Implicits.global

class AsyncMathLibTests extends munit.FunSuite:
  test("square") {
    for
      squareOf3 <- AsyncMathLib.square(3)
      squareOfMinus4 <- AsyncMathLib.square(-4)
    yield
      assertEquals(squareOf3, 9)
      assertEquals(squareOfMinus4, 16)
  }
```

{% endtab %}
{% endtabs %}

Тест вначале асинхронно вычисляет `square(3)` и `square(-4)`.
После завершения вычислений, если они оба успешны, тест переходит к вызовам `assertEquals`.
Если какое-либо из утверждений не выполняется, не выполняется результат `Future[Unit]`,
и MUnit приведет к сбою теста.

Подробнее об асинхронных тестах можно прочитать в [документации MUnit](https://scalameta.org/munit/docs/tests.html#declare-async-test).
Там показано, как использовать другие асинхронные типы помимо `Future`.
