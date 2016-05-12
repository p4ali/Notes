## Option[T]
**Option[T]** provide a container for zero or one element of a given type. An Option[T] can either be *Some[T]* or *None*.

**None** is an object. There is a single instance of None in your program, so it's kind of like *null*. But *None* has method on it. 
So you can invoke `flatMap, foreach, filter` and so on no matter wether you have *Some* of *None*.

```scala
case class Person(name:String, age: Int, valid: Boolean)

def findPerson(key: Int): Option[Person]
def ageFromPerson(key: Int): Option[Person] = findPerson(key).map(_.age)

def tryo[T](f: => T):Option[T] = try{ Some(f) } catch { case _ => None }
def toInt(s:String): Option[Int] =tryo(s.toInt)
def toBool(s:String) = tryo(JBool.parseBool(s))
def personFromParams(p: Map[String, String]) Option[Person] = 
  for { name <- p.get("name")
        ageStr <- p.get("age")
        age <- toInt(ageStr)
        validStr <- p.get("valid")
        valid <- toBool(validStr)
      }
  yield new Person(name, age, valid)
  
// Useful functions
Some(3).get // Int = 3
None.get // java.util.NoSuchElementException
Some(3).getOrElse(4) // Int = 3
None.getOrElse(4) // 4

// Pattern matching
case class User(
  id: Int,
  firstName: String,
  lastName: String,
  age: Int,
  gender: Option[String])
val user = User(1,"Alice", "Green", 12, None)
val gender:String = user.gender match {
  case Some(gender) => gender
  case None => "Unknown"
}
println(s"Gender:$gender")

```
