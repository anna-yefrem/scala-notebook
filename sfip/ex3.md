Exercises of Chapter 3
=======================

### 1. Write a code snippet that sets `a` to an array of `n` random integers between `0` (inclusive) and `n` (exclusive).

_Ans_:

```scala
val n = 10
val a = new Array[Int](n)
for (i <- 0 until n) a(i) = Random.nextInt(n)
```

### 2. Write a loop that swaps adjacent elements of an array of integers. For example, `Array(1, 2, 3, 4, 5)` becomes `Array(2, 1, 4, 3, 5)`.

_Ans_:

```scala
val a = Array(1, 2, 3, 4, 5) //> a  : Array[Int] = Array(1, 2, 3, 4, 5)
for (i <- 0 until (a.length, 2) if (i < a.length - 1)) {
  val temp = a(i)
  a(i) = a(i + 1)
  a(i + 1) = temp
}
a //> res0: Array[Int] = Array(2, 1, 4, 3, 5)
```

### 3. Repeat the preceding assignment, but produce a new array with the swapped values. Use `for`/`yield`.

_Ans_:

```scala
val a = Array(1, 2, 3, 4, 5) //> a  : Array[Int] = Array(1, 2, 3, 4, 5)

val seq = for (i <- 0 until a.length) yield {
  if (i % 2 != 0) a(i - 1)
  else if (i == a.length - 1) a(i)
  else a(i + 1)
}
seq.toArray //> res0: Array[Int] = Array(2, 1, 4, 3, 5)
```

### 4. Given an array of integers, produce a new array that contains all positive values of the original array, in their original order, followed by all values that are zero or negative, in their original order.

_Ans_:

```scala
val a = Array(1, 2, 0, -1, -2, 3, -4, 4, 5)
val buffer = new ArrayBuffer[Int]()
buffer.appendAll(for (i <- 0 until a.length if (a(i) > 0)) yield a(i))
buffer.appendAll(for (i <- 0 until a.length if (a(i) <= 0)) yield a(i))
buffer.toArray //> res0: Array[Int] = Array(1, 2, 3, 4, 5, 0, -1, -2, -4)
```

### 5. How do you compute the average of an `Array[Double]`?

_Ans_:

```scala
val a = Array(0.1, 0.2, 0.3)
a.reduce(_ + _) / a.length //> res0: Double = 0.20000000000000004
```

### 6. How do you rearrange the elements of an `Array[Int]` so that they appear in reverse sorted order? How do you do the same with an `ArrayBuffer[Int]`?

_Ans_:

* Array[Int]

```scala
val a = Array(2, 1, 4, 3, 5)
a.sortWith(_ > _) // same as a.toList.sorted.reverse.toArray
  //> res0: Array[Int] = Array(5, 4, 3, 2, 1)
  
val b = ArrayBuffer(2, 1, 4, 3, 5)
b.sortWith(_ > _)
  //> res1: scala.collection.mutable.ArrayBuffer[Int] = ArrayBuffer(5, 4, 3, 2, 1)
```

### 7. Write a code snippet that produces all values from an array with duplicates removed.

_Ans_:

```scala
val a = Array(2, 1, 2, 3, 5)
a.distinct //> res0: Array[Int] = Array(2, 1, 3, 5)
```

### 8. Rewrite the example below. Collect indexes of the negative elements, reverse the sequence, drop the last index, and call `a.remove(i)` for each index. Compare the efficiency of this approach with the two approaches in Section 3.4.

```scala
for (elem <- a if elem % 2 == 0) yield 2 * elem
```

_Ans_: 

```scala
val a = ArrayBuffer(1, -2, -3, 4, -5)
val negSeq = for (i <- 0 until a.length if a(i) < 0) yield i
for (i <- negSeq.reverse.dropRight(1))
  a.remove(i)
a //> res0: scala.collection.mutable.ArrayBuffer[Int] = ArrayBuffer(1, -2, 4)
```

### 9. Make a collection of all time zones returned by `java.util.TimeZone.getAvailableIDs` that are in America. Strip off the `"America/"` prefix and sort the result.

_Ans_:

```scala
val ids = java.util.TimeZone.getAvailableIDs
ids.filter(x => x.take(7).equals("America")).map(x => x.drop(8)).sorted
```

### 10. Import `java.awt.datatransfer._` and make an object of type SystemFlavorMap with the call

```scala
val flavors = SystemFlavorMap.getDefaultFlavorMap().asInstanceOf[SystemFlavorMap]
```

Then call the `getNativesForFlavor` method with parameter `DataFlavor.imageFlavor` and get the return value as a Scala buffer. (Why this obscure class? It’s hard to find uses of `java.util.List` in the standard Java library.)

_Ans_:

```scala
import java.awt.datatransfer._
val flavors = SystemFlavorMap.getDefaultFlavorMap().asInstanceOf[SystemFlavorMap]
flavors.getNativesForFlavor(DataFlavor.imageFlavor).toArray.toBuffer
  // res0: scala.collection.mutable.Buffer[Object] = ArrayBuffer(PNG, JFIF, DIB, ENHMETAFILE, METAFILEPICT)
```
