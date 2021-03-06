Exercises for Chapter 1
======================

### <a name="ex2">2. In the Scala REPL, compute the square root of 3, and then square that value. By how much does the result differ from 3?</a>

_Ans_: 

```scala
import math._
abs(pow(sqrt(3), 2) - 3)
  // res0: Double = 4.440892098500626E-16
```

### <a name="ex3">3. Are the `res` variables `val` or `var`?</a>

_Ans_: `res` variables are `val`.

### <a name="ex4">4. Scala lets you multiply a string with a number—try out `"crazy" * 3` in the REPL. What does this operation do? Where can you find it in Scaladoc?</a>

_Ans_: `"crazy" * 3` returns "crazycrazycrazy". `"Crazy"` is a String, which is defined in `Predef` package that `type
String = java.lang.String`.

### <a name="ex5">5. What does `10 max 2` mean? In which class is the `max` method defined?</a>

_Ans_: `10 max 2` mean `10.max(2)` which returns the larger number 10 of two numbers. The `max` method is defined in `abstract final class Int extends AnyVal` that

```scala
def max(that: Int): Int
  // Returns this if this > that or that otherwise.
```

### <a name="ex6">6. Using BigInt, compute 2^1024</a>

_Ans_:

```scala
var exp: BigInt = 2
for (i <- 1 to 1023)
  exp *= 2
exp
```

_ALTERNATIVE_
```scala
BigInt(2).pow(1024)
```

### <a name="ex7">7. What do you need to import so that you can get a random prime as `probablePrime(100, Random)`, without any qualifiers before `probablePrime` and `Random`?</a>

_Ans_:

```scala
import math.BigInt.probablePrime
import util.Random
```

### <a name="ex8">8. One way to create random file or directory names is to produce a random `BigInt` and convert it to base 36, yielding a string such as `"qsnvbevtomcj38o06kul"`. Poke around Scaladoc to find a way of doing this in Scala.</a>

```scala
BigInt.apply(100, util.Random).toString(36)
```

### <a name="ex9">9. How do you get the first character of a string in Scala? The last character?</a>

_Ans_:

```scala
val s = "Hello"
val first = s(0) // first: Char = H
val last = s(s.length - 1) // last: Char = o
```

### <a name="ex10">10. What do the `take`, `drop`, `takeRight`, and `dropRight` string functions do? What advantage or disadvantage do they have over using substring?</a>


_Ans_: 

* `take(n: Int)` return substring of first n charactors
* `drop(n: Int)` return substring of first n charactors removed
* `takeRight(n: Int)` return substring of last n charactors
* `dropRight(n: Int)` return substring of last n charactors removed

```scala
val s = "Hello"

s.take(2) // "He"
s.drop(2) // "llo"
s.takeRight(2) // "lo"
s.dropRight(2) // "Hel"
```

```comment from stackoverflow
The main benefit is that it allows you to treat a String as a sequential collection of characters, much like any other Seq or List instance.

In fact, these methods (and other important transformational functions like map, flatMap and filter) are not implemented in String itself (which is, in fact, simply the Java String class, not a native-Scala class), but in the StringOps class (which extends StringLike -> ... -> SeqLike), and an implicit conversion ensures that a String is converted into a StringOps whenever you need access to these methods.

This means you can pass a String to a list-manipulation function and the function will receive a StringOps instance, work on it like any other SeqLike entity without needing to know it is actually a String, and hand back the results of the manipulation, which StringOps is designed to present back to you as a String.

If you know an entity is a String in a given piece of code, feel free to use the String-specific methods, but the availability of this implicit conversion means that you can also take advantage of a String's "character sequence"-like nature to treat it like any other list in situations where that may be convenient.
```