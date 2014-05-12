Exercises for Chapter 4
=======================

### 1. Set up a map of prices for a number of gizmos that you covet. Then produce a second map with the same keys and the prices at a 10 percent discount.

_Ans_:

```scala
val price = Map(("Fix" -> 10.0), ("Top" -> 12.0))
val priceSales = {for ((k, v) <- price) yield (k, v * 0.9)}
  //> priceSales : scala.collection.immutable.Map[String,Double] = Map(Fix -> 9.0, Top -> 10.8)
```

### 2. Write a program that reads words from a file. Use a mutable map to count how often each word appears. To read the words, simply use a `java.util.Scanner`:

```scala
val in = new java.util.Scanner(new java.io.File("myfile.txt"))
while (in.hasNext()) process in.next()
```

At the end, print out all words and their counts.

_Ans_:

```scala
val in = new java.util.Scanner(new java.io.File("test.txt"))
  // Hello World Hello My World
val wordCount = scala.collection.mutable.Map[String, Int]
while (in.hasNext) {
  val word = in.next
  wordCount += (word -> (wordCount.getOrElse(word, 0) + 1))
}
wordCount.foreach(println)
  //> (My,1)
  //| (Hello,2)
  //| (World,2)
```

### 3. Repeat the preceding exercise with an immutable map.

_Ans_:

```scala
val in = new java.util.Scanner(new java.io.File("test.txt"))
  // Hello World Hello My World
val buffer = new scala.collection.mutable.ArrayBuffer[Tuple2[String, Int]]
while (in.hasNext) {
  val word = in.next
  buffer.append((word, 1))
}
val wordCount = buffer.groupBy(x => x._1).mapValues(x => x.length)
  // wordCount: scala.collection.immutable.HashMap[String, Int]
wordCount.foreach(println)
  //> (My,1)
  //| (Hello,2)
  //| (World,2)
```

### 4. Repeat the preceding exercise with a sorted map, so that the words are printed in sorted order.

_Ans_:

```scala
val in = new java.util.Scanner(new java.io.File("test.txt"))
  // Hello World Hello My World
var sortedMap = scala.collection.immutable.SortedMap[String, Int]
while (in.hasNext) {
  val word = in.next
  sortedMap = sortedMap + (word -> (sortedMap.getOrElse(word, 0) + 1))
}
sortedMap.foreach(println)
  //> (Hello,2)
  //> (My,1)
  //| (World,2)
```

### 5. Repeat the preceding exercise with a `java.util.TreeMap` that you adapt to the Scala API.

```scala
val in = new java.util.Scanner(new java.io.File("test.txt"))
  // Hello World Hello My World
val treeMap = new java.util.TreeMap[String, Int]
while (in.hasNext) {
  val word = in.next
  val value = if (treeMap.containsKey(word)) treeMap.get(word) else 0
  treeMap.put(word, value + 1)
}
for (i <- treeMap.entrySet.toArray) println(i)
  //> Hello=2
  //| My=1
  //| World=2
```

### 6. Define a linked hash map that maps `"Monday"` to `java.util.Calendar.MONDAY`, and similarly for the other weekdays. Demonstrate that the elements are visited in insertion order.

_Ans_:

```scala
val map = scala.collection.mutable.LinkedHashMap[String, Any]()

map("Monday") = MONDAY       // 2
map("Tuesday") = TUESDAY     // 3
map("Wednesday") = WEDNESDAY // 4
map("Thursday") = THURSDAY   // 5
map("Friday") = FRIDAY       // 6
map("Saturday") = SATURDAY   // 7
map("Sunday") = SUNDAY       // 1

for (i <- map) println(i)  
```

### 7. Print a table of all Java properties, like this:

```scala
java.runtime.name             | Java(TM) SE Runtime Environment
sun.boot.library.path         | /home/apps/jdk1.6.0_21/jre/lib/i386
java.vm.version               | 17.0-b16
java.vm.vendor                | Sun Microsystems Inc.
java.vendor.url               | http://java.sun.com/
path.separator                | :
java.vm.name                  | Java HotSpot(TM) Server VM
```

You need to find the length of the longest key before you can print the table.

_Ans_:

```scala
val a = System.getProperties().entrySet().toArray.map(_.toString)
val b = scala.collection.mutable.LinkedHashMap[String, String]()
for (i <- a) {
  val elements = i.split("=")
  if (elements.length == 2) b(elements(0)) = elements(1)
}
val maxLength = b.map(x => x._1.length).max
for ((k, v) <- b.take(7)) println(k + " " * (maxLength - k.length) + " |" + v)
  //> java.runtime.name             |Java(TM) SE Runtime Environment
  //| sun.boot.library.path         |C:\Program Files\Java\jre7\bin
  //| java.vm.version               |23.6-b04
  //| java.vm.vendor                |Oracle Corporation
  //| java.vendor.url               |http://java.oracle.com/
  //| path.separator                |;
  //| java.vm.name                  |Java HotSpot(TM) Client VM
```

### 8. Write a function `minmax(values: Array[Int])` that returns a pair containing the smallest and largest values in the array.

_Ans_:

```scala
def minmax(values: Array[Int]) = (values.min, values.max)
minmax(Array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10))
  //> res0: (Int, Int) = (1,10)
```

### 9. Write a function `lteqgt(values: Array[Int], v: Int)` that returns a triple containing the counts of values less than `v`, equal to `v`, and greater than `v`.

```scala
def lteqgt(values: Array[Int], v: Int) = (values.filter(_ > v).length, values.filter(_ == v).length, values.filter(_ < v).length)
lteqgt(Array(1, 2, 3, 4, 5), 2)
  //> res0: (Int, Int, Int) = (3,1,1)
```

### 10. What happens when you zip together two strings, such as `"Hello".zip("World")`? Come up with a plausible use case.

_Ans_: can be used in decrpytion?

```scala
"Hello".zip("World")
  //> res0: scala.collection.immutable.IndexedSeq[(Char, Char)] = Vector((H,W), (e,o), (l,r), (l,l), (o,d))
```
