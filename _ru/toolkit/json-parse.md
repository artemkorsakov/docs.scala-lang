---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как получить доступ к значениям внутри JSON?
type: section
description: Доступ к значениям JSON с использованием ujson.
language: ru
num: 17
previous-page: json-intro
next-page: json-modify
---

{% include markdown.html path="_markdown/_ru/install-upickle.md" %}

## Доступ к значениям внутри JSON

Чтобы разобрать строку JSON и получить доступ к полям внутри нее, можно использовать uJson, который является частью библиотеки uPickle.

Метод `ujson.read` разбирает строку JSON и загружает ее в память:

{% tabs 'read' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc
val jsonString = """{"name": "Peter", "age": 13, "pets": ["Toolkitty", "Scaniel"]}"""
val json: ujson.Value  = ujson.read(jsonString)
println(json("name").str)
// выводит: Peter
```

{% endtab %}
{% endtabs %}

Чтобы получить доступ к полю `"name"`, мы используем `json("name")`, а затем вызываем `str`, чтобы привести значение к строке.

Для доступа к элементам по индексу в массиве JSON:

{% tabs 'array' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc
val pets: ujson.Value = json("pets")

val firstPet: String = pets(0).str
val secondPet: String = pets(1).str

println(s"Питомцы: $firstPet и $secondPet")
// выводит: Питомцы: Toolkitty и Scaniel
```

{% endtab %}
{% endtabs %}

Вы можете углубляться в структуру JSON настолько, насколько это необходимо, чтобы извлечь любое вложенное значение.
Например, `json("field1")(0)("field2").str` вернет строковое значение `field2` из первого элемента массива в `field1`.

## Типы JSON

В предыдущих примерах мы использовали `str`, чтобы привести значение JSON к строке.
Аналогичные методы существуют и для других типов значений:

- `num` для числовых значений, возвращает `Double`
- `bool` для логических значений, возвращает `Boolean`
- `arr` для массивов, возвращает изменяемый `Buffer[ujson.Value]`
- `obj` для объектов, возвращает изменяемый `Map[String, ujson.Value]`

{% tabs 'typing' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc:reset
import scala.collection.mutable

val jsonString = """{"name": "Peter", "age": 13, "pets": ["Toolkitty", "Scaniel"]}"""
val json = ujson.read(jsonString)

val person: mutable.Map[String, ujson.Value] = json.obj
val age: Double = person("age").num
val pets: mutable.Buffer[ujson.Value] = person("pets").arr
```

{% endtab %}
{% endtabs %}

Если значение JSON не соответствует ожидаемому типу, uJson выбрасывает исключение `ujson.Value.InvalidData`.

{% tabs 'exception' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc:crash
val name: Boolean = person("name").bool
// выбрасывает исключение ujson.Value.InvalidData: Expected ujson.Bool (data: "Peter")
```

{% endtab %}
{% endtabs %}

---
