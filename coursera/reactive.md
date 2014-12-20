## JSON in Scala
```scala
// given 
{"firstname": "John",
  "lastname": "Smith",
  "address": {
    "streetAddress": "21 2nd street",
    "state": "NY",
    "postalCode": 10021
  },
  "phoneNumbers": [
    {"type":"home","number": "212 555-1234"},
    {"type":"fax", "number": "646 555-4567"}
  ]
}

// define following classes
abstract class JSON
case class JSeq (elements: List[JSON]) extends JSON
case class JObj (bindings: Map[String,JSON]) extends JSON
case class JNum (num: Double) extends JSON
case class JStr (str: String) extends JSON
case class JBool (b: Boolean) extends JSON
case object JNull extends JSON

// then
val data = JObj (Map(
  "firstName"-> JStr("Johne"),
  "lastName" -> JStr("Smith"),
  "address" -> JObj(Map(
    "streetAddress" -> JStr("21 2nd street"),
    "state" -> JSt("NY"),
    "postalCode" -> JNum(10021)
  )),
  "phoneNumbers" -> JSeq(List(
    JObj(Map(
      "type" -> JStr("home), "number" -> JStr("212 555-1234")
    )),
    JObj(Map(
      "type" -> JStr("fax"), "number" -> JStr("646 555-4567")
    ))
  ))
))

```
