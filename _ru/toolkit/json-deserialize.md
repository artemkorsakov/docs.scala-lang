---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как десериализовать JSON в объект?
type: section
description: Преобразование JSON в пользовательский тип данных
language: ru
num: 19
previous-page: json-modify
next-page: json-serialize
---

{% include markdown.html path="_markdown/_ru/install-upickle.md" %}

## Разбор vs десериализация

Разбор с использованием uJson принимает только валидный JSON, но не проверяет, что имена и типы полей соответствуют ожидаемым.

Десериализация, с другой стороны, преобразует строку JSON в указанный пользователем тип данных Scala,
если все необходимые поля присутствуют и имеют правильные типы.

В этом руководстве мы покажем, как десериализовать JSON в `Map`, а также в пользовательский `case class`.

## Десериализация JSON в `Map`

Для типа `T` uPickle может десериализовать JSON в `Map[String, T]`, проверяя, что все поля соответствуют типу `T`.

Например, мы можем десериализовать JSON в `Map[String, List[Int]]`:

{% tabs 'parsemap' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc
val json = """{"primes": [2, 3, 5], "evens": [2, 4, 6]} """
val map: Map[String, List[Int]] =
  upickle.default.read[Map[String, List[Int]]](json)

println(map("primes"))
// выводит: List(2, 3, 5)
```

{% endtab %}
{% endtabs %}

Если значение имеет неправильный тип, uPickle выбрасывает исключение `upickle.core.AbortException`.

{% tabs 'parsemap-error' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc:reset:crash
val json = """{"name": "Peter"} """
upickle.default.read[Map[String, List[Int]]](json)
// выбрасывает: upickle.core.AbortException: expected sequence got string at index 9
```

{% endtab %}
{% endtabs %}

### Десериализация JSON в пользовательский тип данных

В Scala вы можете использовать `case class` для определения собственного типа данных.
Например, чтобы представить владельца питомца, можно сделать так:

```scala mdoc:reset
case class PetOwner(name: String, pets: List[String])
```

Чтобы прочитать `PetOwner` из JSON, нам нужно предоставить `ReadWriter[PetOwner]`.
uPickle может сделать это автоматически:

{% tabs 'given' class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala mdoc
import upickle.default._

implicit val ownerRw: ReadWriter[PetOwner] = macroRW[PetOwner]
```

Некоторые пояснения:

- `implicit val` — это значение, которое может быть автоматически передано в качестве аргумента в метод или функцию без явного указания.
- `macroRW` — это метод, предоставляемый uPickle, который может генерировать экземпляры `ReadWriter` для case-классов, используя информацию об их полях.

{% endtab %}

{% tab 'Scala 3' %}

```scala
import upickle.default.*

case class PetOwner(name: String, pets: List[String])
  derives ReadWriter
```

Ключевое слово `derives` используется для автоматической генерации given-экземпляров.
Используя знания компилятора о полях в `PetOwner`, он генерирует `ReadWriter[PetOwner]`.
{% endtab %}
{% endtabs %}

Это означает, что теперь вы можете читать (и записывать) объекты `PetOwner` из JSON с помощью `upickle.default.read(petOwner)`.

Обратите внимание, что вам не нужно явно передавать экземпляр `ReadWriter[PetOwner]` в метод `read`.
Тем не менее, он получает его из контекста как "given"-значение.
Подробнее о контекстуальных абстракциях можно узнать в [Scala 3 Book](https://docs.scala-lang.org/ru/scala3/book/ca-contextual-abstractions-intro.html).

Собрав всё вместе, вы получите:

{% tabs 'full' class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala mdoc:reset
import upickle.default._

case class PetOwner(name: String, pets: List[String])
implicit val ownerRw: ReadWriter[PetOwner] = macroRW

val json = """{"name": "Peter", "pets": ["Toolkitty", "Scaniel"]}"""
val petOwner: PetOwner = read[PetOwner](json)

val firstPet = petOwner.pets.head
println(s"${petOwner.name} имеет питомца по имени $firstPet")
// выводит: Peter has a pet called Toolkitty
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
import upickle.default.*

case class PetOwner(name: String, pets: List[String]) derives ReadWriter

val json = """{"name": "Peter", "pets": ["Toolkitty", "Scaniel"]}"""
val petOwner: PetOwner = read[PetOwner](json)

val firstPet = petOwner.pets.head
println(s"${petOwner.name} имеет питомца по имени $firstPet")
// выводит: Peter has a pet called Toolkitty
```

{% endtab %}
{% endtabs %}

---
