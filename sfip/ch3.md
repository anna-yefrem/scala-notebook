Chapter 3 Working with Arrays
=============================

### 3.1 Fixed-Length Arrays

* create new fixed-length array

```scala
val nums = new Array[Int](10)
  // An array of ten integers, all initialized with zero
val a = new Array[String](10)
  // Astring array with ten elements, all initialized with null
val s = Array("Hello", "World") 
  // An Array[String] of length 2—the type is inferred
  // Note: No new when you supply initial values
s(0) = "Goodbye"
  // Array("Goodbye", "World") 
  // Use () instead of [] to access elements
```

### 3.2 Variable-Length Arrays: Array Buffers

* create new array buffers

```scala
val b = ArrayBuffer[Int]()
  // Or new ArrayBuffer[Int]
  // An empty array buffer, ready to hold integers
b += 1
  // ArrayBuffer(1)
  // Add an element at the end with +=
b += (1, 2, 3, 5)
  // ArrayBuffer(1, 1, 2, 3, 5)
  // Add multiple elements at the end by enclosing them in parentheses
b ++= Array(8, 13, 21)
  // ArrayBuffer(1, 1, 2, 3, 5, 8, 13, 21)
  // You can append any collection with the ++=operator
b.trimEnd(5)
  // ArrayBuffer(1, 1, 2)
  // Removes the last five elements
```

* insert and remove elements

```scala
b.insert(2, 6)
  // ArrayBuffer(1, 1, 6, 2) 
  // Insert before index 2
b.insert(2, 7, 8, 9)
  // ArrayBuffer(1, 1, 7, 8, 9, 6, 2)
  // You can insert as many elements as you like
b.remove(2)
  // ArrayBuffer(1, 1, 8, 9, 6, 2)
b.remove(2, 3)
  // ArrayBuffer(1, 1, 2)
  // The second parameter tells how many elements to remove
```

* traverse arrays

```scala
for (i <- 0 until a.length)
  println(i + ": " + a(i))
  // 0 until 10 returns Range(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
  // 0 until (a.length, 2) returns Range(0, 2, 4, ...)
```

* transform arrays

```scala
val a = Array(2, 3, 5, 7, 11)
val result = for (elem <- a) yield 2 * elem
  // result is Array(4, 6, 10, 14, 22)
for (elem <- a if a % 2 == 0) yield 2 * elem
```

* Remove all but the first negavie number

```scala
val indexes = for (i <- 0 until a.length if a(i) < 0) yield i
for (j <- (1 until indexes.length).reverse) a.remove(indexes(j))
```

* other operations

```scala
ArrayBuffer("Mary", "had", "a", "little", "lamb").max
  // return "little"
val b = ArrayBuffer(1, 7, 2, 9)
val bSorted = b.sorted(_ < _)
  // b is unchanged; bSorted is ArrayBuffer(1, 2, 7, 9)
a.mkString(" and ")
  // "1 and 2 and 7 and 9"
a.mkString("<", ",", ">")
  // "<1,2,7,9>"
```

### 3.7 Multidimensinal Arrays

* fixed-length 2-d array

```scala
val matrix = Array.ofDim[Double](3, 4)
  // 3 by 4 matrix
```

* variable-length 2-d array

```scala
val triangle = new Array[Array[Int]](10)
for (i <- 0 until triangle.length) 
  triangle(i) = new Array[Int](i + 1)
```
