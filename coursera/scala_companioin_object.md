## Companion Object
A companion object is an object with the same name as a class or trait and is defined in the same source file 
as the associate file or trait. A companion object differs from other objects as it has access rights to the
class/trait that other objects do not. In particular it can access methoes and fields that are private in class/trait.

An analog to a companion object in Java is having a class with static methods. In scala you would move the 
staitc method to the companion object.

```scala
class MyString(val s:String) {
  private var  extra=""
  override def toString = s+extra
}

object MyString{
  def apply(base:String,extra:String) = {
    val s = new MyString(base)
    s.extra = extra
    s
  }
  def apply(base:String) = new MyString(base)
}

println(MyString("Hello","World"))
println(MyString("Hello"))
```
