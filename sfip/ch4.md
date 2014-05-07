Maps and Tuples
===============

### 4.1 Constructing a Map

* create a map

```scala
val scores = Map("Alice" -> 10, "Bob" -> 3, "Cindy" -> 8
  // immutable Map[String, Int]
val scores = scala.collection.mutable.Map("Alice" -> 10, "Bob" -> 3, "Cindy" -> 8)
  // mutable Map
val scores = new scala.collection.mutable.HashMap[String, Int]
  // empty mutable HashMap
val scores = Map(("Alice", 10), ("Bob", 3), ("Cindy", 8))
  // ("Alice", 10) is same as "Alice" -> 10
```

### 4.2 Accessing Map Values

```scala
val bobsScore = scores("Bob") // Like scores.get("Bob") in Java
val bobsScore = if (scores.contains("Bob")) scores("Bob") else 0
  // use contain to check whether there is a key with given value
val bobsScore = scores.getOrElse("Bob", 0)
  // if the map contains the key "Bob", return the value; otherwise, return 0
```

### 4.3 Updating Map Values

* update mutable map

```scala
scores("Bob") = 10 
  // updates the existing value for the key "Bob" (assuming scoresis mutable)
scores("Fred") = 7 
  // adds a new key/value pair to scores (assuming it is mutable)
scores += ("Bob" -> 10, "Fred" -> 7)
  // multiple associations
scores -= "Alice"
  // remove a key
```

* update immutable map

```scala
val newScores = scores + ("Bob" -> 10, "Fred" -> 7) // New map with update
```

* update immutable map using variable
```scala
var scores = ...
scores = scores + ("Bob" -> 10, "Fred" -> 7)
scores = scores - "Alice"
```

### 4.4 Iterating over Maps

```scala
for ((k, v) <- map) process k and v
```

* process key or value set

```scala
scores.keySet //A set such as Set("Bob", "Cindy", "Fred", "Alice")
for (v <- scores.values) println(v) //Prints 10 8 7 10or some permutation thereof
```

* reverse a map

```scala
for ((k, v) <- map) yield (v, k)
```

### 4.5 Sorted Maps

* use a sortedMap to visit the keys in sorted order

```scala
val scores = scala.collection.immutable.SortedMap("Alice" -> 10, "Fred" -> 7, "Bob" -> 3, "Cindy" -> 8)
```

* use a LinkedHashMap to visit the keys in insertion order

```scala
val months = scala.collection.mutable.LinkedHashMap("January" -> 1,
"February" -> 2, "March" -> 3, "April" -> 4, "May" -> 5, ...)
```

### 4.7 Tuples

* unlike array or string positions, the component positions of a tuple start with 1,
not 0

```scala
val t = (1, 3.14, "Fred")
val second = t._2
  // second = 3.14
```

* use pattern matching

```scala
val (first, second, third) = t // sets first to 1, second to 3.14, third to "Fred"
val (first, second, _) = t // ignore components
```

* generate pairs

```scala
"New York".partition(_.isUpper)
  // yields the pair ("NY", "ew ork")
```

# 4.8 Zipping

```scala
val symbols = Array("<", "-", ">")
val counts = Array(2, 10, 2)
val pairs = symbols.zip(counts)
  // Array(("<", 2), ("-", 10), (">", 2))
for ((s, n) <- pairs) Console.print(s * n)
  // prints <<---------->>
```
