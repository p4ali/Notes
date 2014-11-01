## Map
A map of type Map[Key,Value] is a data structure that associates keys of type Key with values of type Value.
```scala
Map('I'->1,'J'->2)
```
### Maps are functions
```scala
val capitalOfCountry = Map("US"->"Washington","Switzerland"->"Bern")
capitalOfCountry("US') //"Washington"
capitalOfCountry("Andorra") // NoSuchElementException
capitalOfCountry get "US" // Some("Washinton")
capitalOfCountry get "Andorra" // None
```
