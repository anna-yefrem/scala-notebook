Chapter 2 Control Structures and Functions
==========================================

2.4 Input and Output

Get input by `getInt()` and `getLine()`. Produce output by `print()` and `println()`.

2.5 Loops

* while loop

```scala
while (n > 0) {
  /* code */
}
```

* for loop

```scala
for (i <- 1 to n)
  // code
```

* Use `until` for case n - 1.

```scala
val s = "Hello"
var sum = 0
for (i <- 0 until s.length) // Last value for iis s.length - 1
  sum += s(i)
```

* Use direct loop for characters, lists, etc.

```scala
var sum = 0
for (ch <- "Hello") sum += ch
```
