---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как изменить JSON?
type: section
description: Изменение JSON с использованием uPickle.
language: ru
num: 18
previous-page: json-parse
next-page: json-deserialize
---

{% include markdown.html path="_markdown/_ru/install-upickle.md" %}

`ujson.read` возвращает изменяемое представление JSON, которое можно обновлять.
Поля и элементы можно добавлять, изменять или удалять.

Сначала вы разбираете строку JSON, затем обновляете её в памяти и, наконец, записываете обратно.

{% tabs 'modify' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc
// Разбор строки JSON
val json: ujson.Value = ujson.read("""{"name":"John","pets":["Toolkitty","Scaniel"]}""")

// Обновление
json("name") = "Peter"
json("nickname") = "Pete"
json("pets").arr.remove(1)

// Запись обратно в строку
val result: String = ujson.write(json)
println(result)
// выводит: {"name":"Peter","pets":["Toolkitty"],"nickname":"Pete"}
```

{% endtab %}
{% endtabs %}

---
