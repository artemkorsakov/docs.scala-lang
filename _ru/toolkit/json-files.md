---
layout: multipage-overview
partof: toolkit
overview-name: "Scala инструментарий"
title: Как читать и записывать JSON-файлы?
type: section
description: Чтение и запись JSON-файлов с использованием UPickle и OSLib
language: ru
num: 21
previous-page: json-serialize
next-page: json-what-else
---

{% include markdown.html path="_markdown/_ru/install-upickle.md" %}

## Чтение и запись сырого JSON

Для чтения и записи JSON-файлов можно использовать uJson и OS-Lib следующим образом:

{% tabs 'raw' %}
{% tab 'Scala 2 и 3' %}

```scala mdoc:compile-only
// чтение JSON-файла
val json = ujson.read(os.read(os.pwd / "raw.json"))

// изменение содержимого JSON
json("updated") = "now"

// запись в новый файл
os.write(os.pwd / "raw-updated.json", ujson.write(json))
```

{% endtab %}
{% endtabs %}

## Чтение и запись Scala-объектов с использованием JSON

Для чтения и записи Scala-объектов в JSON-файлы и обратно можно использовать uPickle и OS-Lib следующим образом:

{% tabs 'object' class=tabs-scala-version %}
{% tab 'Scala 2' %}

```scala mdoc:compile-only
import upickle.default._

case class PetOwner(name: String, pets: List[String])
implicit val ownerRw: ReadWriter[PetOwner] = macroRW

// чтение PetOwner из JSON-файла
val petOwner: PetOwner = read[PetOwner](os.read(os.pwd / "pet-owner.json"))

// создание нового PetOwner
val petOwnerUpdated = petOwner.copy(pets = "Toolkitty" :: petOwner.pets)

// запись в новый файл
os.write(os.pwd / "pet-owner-updated.json", write(petOwnerUpdated))
```

{% endtab %}
{% tab 'Scala 3' %}

```scala
import upickle.default.*

case class PetOwner(name: String, pets: List[String]) derives ReadWriter

// чтение PetOwner из JSON-файла
val petOwner: PetOwner = read[PetOwner](os.read(os.pwd / "pet-owner.json"))

// создание нового PetOwner
val petOwnerUpdated = petOwner.copy(pets = "Toolkitty" :: petOwner.pets)

// запись в новый файл
os.write(os.pwd / "pet-owner-updated.json", write(petOwnerUpdated))
```

{% endtab %}
{% endtabs %}

Для сериализации и десериализации Scala case-классов (или перечислений) в JSON нам нужен экземпляр `ReadWriter`.
Чтобы понять, как uPickle автоматически генерирует его,
вы можете прочитать руководства [_Как десериализовать JSON в объект?_](json-deserialize.html)
или [_Как сериализовать объект в JSON?_](json-serialize.html).

---
