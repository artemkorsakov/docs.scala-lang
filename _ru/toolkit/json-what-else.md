---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Что еще может uPickle?
type: section
description: Неполный список возможностей uPickle
language: ru
num: 22
previous-page: json-files
next-page: http-client-intro
---

{% include markdown.html path="_markdown/_ru/install-upickle.md" %}

## Создание новой JSON-структуры с помощью uJson

{% tabs construct%}
{% tab 'Scala 2 и Scala 3' %}

```scala mdoc
val obj: ujson.Value = ujson.Obj(
  "library" -> "upickle",
  "versions" -> ujson.Arr("1.6.0", "2.0.0", "3.1.0"),
  "documentation" -> "https://com-lihaoyi.github.io/upickle/",
)
```

{% endtab %}
{% endtabs %}

Узнайте больше о создании JSON в [документации uJson](https://com-lihaoyi.github.io/upickle/#Construction).

## Определение пользовательской сериализации JSON

Вы можете настроить `ReadWriter` для вашего типа данных, преобразуя `ujson.Value`, например:

{% tabs custom-serializer class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala mdoc
import upickle.default._

case class Bar(i: Int, s: String)

object Bar {
  implicit val barReadWriter: ReadWriter[Bar] = readwriter[ujson.Value]
    .bimap[Bar](
      x => ujson.Arr(x.s, x.i),
      json => new Bar(json(1).num.toInt, json(0).str)
    )
}

val bar = Bar(5, "bar")
val json = upickle.default.write(bar)
println(json)
// выводит: [5, "bar"]
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
import upickle.default.*

case class Bar(i: Int, s: String)

object Bar:
  given ReadWriter[Bar] = readwriter[ujson.Value]
    .bimap[Bar](
      x => ujson.Arr(x.s, x.i),
      json => new Bar(json(1).num.toInt, json(0).str)
    )

val bar = Bar(5, "bar")
val json = upickle.default.write(bar)
println(json)
// выводит: [5, "bar"]
```

{% endtab %}
{% endtabs %}

Узнайте больше о пользовательской сериализации JSON в [документации uPickle](https://com-lihaoyi.github.io/upickle/#Customization).

---
