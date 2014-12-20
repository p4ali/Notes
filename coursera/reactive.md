## Rective Programming
Reactive means *readily respoinsive to stimulus*
* Event-driven
* scalable (react to load)
* resilient (react to failure)
* responsive

### Scalable
* **scale up**: make use of parallelism in multi-core system
* **scale down**: make use of multiple server nodes
* important:
 * minimize shared mutable state
 * location transparency, resilience (scale out)

### resilient
Failure:
* Software failure
* hardware failure
* connection failure
Need:
* loose coupling
* strong encapsulation of state
* pervasive supervisor hierarchies
* replication

### Responsive
To provide rich, real-time interaction with its user even under load and in the presense of failure.

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

## Terminology
* Backpressure - When an IO switch's buffer is full and unable to recieve more data, it still can broadcase false collision detection signals or return data packe to the originator. Such that no additional data packets are transferred until the bottleneck of data has been eliminated or the buffer has been emptied. This is called **backpressure**
