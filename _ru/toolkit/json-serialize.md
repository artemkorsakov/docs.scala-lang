---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как сериализовать объект в JSON?
type: section
description: Как записать JSON с использованием Scala Toolkit.
language: ru
num: 20
previous-page: json-deserialize
next-page: json-files
---

{% include markdown.html path="_markdown/_ru/install-upickle.md" %}

## Сериализация Map в JSON

uPickle может сериализовать ваши Scala-объекты в JSON, чтобы вы могли сохранять их в файлы или передавать по сети.

По умолчанию он может сериализовать примитивные типы, такие как `Int` или `String`, а также стандартные коллекции, такие как `Map` и `List`.

{% tabs 'array' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc
val map: Map[String, Int] =
  Map("Toolkitty" -> 3, "Scaniel" -> 5)
val jsonString: String = upickle.default.write(map)
println(jsonString)
// выводит: {"Toolkitty":3,"Scaniel":5}
```

{% endtab %}
{% endtabs %}

## Сериализация пользовательского объекта в JSON

В Scala вы можете использовать `case class` для определения собственного типа данных.
Например, чтобы представить владельца с именами его питомцев, можно сделать так:

```scala mdoc:reset
case class PetOwner(name: String, pets: List[String])
```

Чтобы иметь возможность записать `PetOwner` в JSON, нам нужно предоставить экземпляр `ReadWriter` для case-класса `PetOwner`.
К счастью, `upickle` может полностью автоматизировать этот процесс:

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

case class PetOwner(name: String, pets: List[String]) derives ReadWriter
```

Ключевое слово `derives` используется для автоматической генерации given-экземпляров.
Используя знания компилятора о полях в `PetOwner`, он генерирует `ReadWriter[PetOwner]`.

{% endtab %}
{% endtabs %}

Это означает, что теперь вы можете записывать (и читать) объекты `PetOwner` в JSON с помощью `upickle.default.write(petOwner)`.

Обратите внимание, что вам не нужно явно передавать экземпляр `ReadWriter[PetOwner]` в метод `write`.
Тем не менее, он получает его из контекста как "given"-значение.
Подробнее о контекстуальных абстракциях можно узнать в [Scala 3 Book](https://docs.scala-lang.org/ru/scala3/book/ca-contextual-abstractions-intro.html).

Собрав всё вместе, вы получите:

{% tabs 'full' class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala
import upickle.default._

case class PetOwner(name: String, pets: List[String])
implicit val ownerRw: ReadWriter[PetOwner] = macroRW

val petOwner = PetOwner("Peter", List("Toolkitty", "Scaniel"))
val json: String = write(petOwner)
println(json)
// выводит: {"name":"Peter","pets":["Toolkitty","Scaniel"]}"
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
import upickle.default._

case class PetOwner(name: String, pets: List[String]) derives ReadWriter

val petOwner = PetOwner("Peter", List("Toolkitty", "Scaniel"))
val json: String = write(petOwner)
println(json)
// выводит: {"name":"Peter","pets":["Toolkitty","Scaniel"]}"
```

{% endtab %}
{% endtabs %}

---
