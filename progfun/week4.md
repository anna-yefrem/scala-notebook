Week 4
======

### 4.1 Functions as Objects

#### function values are treated as objects

function type A => B is just an abbreviation for the class `scala.Function1[A, B]`, which is defined as

```scala
package scala
trait Fucntion1[A, B] {
  def apply(x: A): B
}
```

An anonymous function such as `(x: Int) => x * x` is expended to:

```scala
{ class AnonFun extends Function1[Int, Int] {
    def apply(x: Int) = x * x
  }
  new AnonFun
}
```

or in anonymous class syntax:

```scala
new Function1[Int, Int] {
  def apply(x: Int) = x * x
}
```

A function call, such as `f(a, b)`, where `f` is a value of some class type, is expanded to `f.apply(a, b)`, so the OO-translation of

```scala
val f = (x: Int) => x * x
f(7)
```

would be

```scala
val f = new Function1[Int, Int] {
  def apply(x: Int) = x * x
}
f.apply(7)
```

If `def` is used such as `def f(x: Int): Boolean = ...`, `f` is not itself a function value. But if `f` is used in a place where a Function type is expected, it is converted automatically to the function value `(x: Int) => f(x)`, or expanded

```scala
new Function1[Int, Boolean] {
  def apply(x: Int) = f(x)
}
```

*Exercise*

Define an object List{...} with 3 functions in it so that users can create lists of length 0-2 using syntax

```scala
object List {
  // List(1, 2) = List.apply(1, 2)
  def apply[T](x1: T, x2: T): List[T] = new Cons(x1, new Cons(x2, new Nil))
  def apply[T]() = new Nil
}
```

### Objects Everywhere

> A *pure object-oriented language* is one in which every value is an object.

#### define pure Booleans

```scala
package idealized.scala

abstract class Boolean {
  def ifThenElse[T](t: => T, e: => T): T
  def && (x: => Boolean): Boolean 	= ifThenElse(x, false)
  def || (x: => Boolean): Boolean 	= ifThenElse(true, x)
  def unary_!: Boolean				= ifThenElse(false, true)
  def == (x: Boolean): Boolean		= ifThenElse(x, x.unary_!)
  def != (x: Boolean): Boolean 		= ifThenElse(x.unary_!, x)
}
```

#### define Boolean Constants

```scala
package idealized.scala

object true extends Boolean {
  def ifThenElse[T](t: => T, e: => T) = t
}

object false extends Boolean {
  def ifThenElse[T](t: => T, e: => T) = e
}
```

*Exercise*

Provide an implementation of the comparison operator `<` in class  `idealized.scala.Boolean`. Assume for this that `false < true`.

*Ans*

```scala
package idealized.scala

abstract class Boolean {
  def < (x: Boolean): Boolean = ifThenElse(false, x)
}
```

#### the class `Int`

```scala
class Int {
  def + (that: Double): Double
  def + (that: Float): Float
  def + (that: Long): Long
  def + (that: Int): Int // same for -, *, /, %
  def << (cnt: Int): Int // same for >>, >>>
  def & (that: Long): Long
  def & (that: Int): Int // same for |, ^
  def == (that: Double) : Boolean
  def == (that: Float): Boolean
  def == (that: Long): Boolean
}
```

*Exercise*

Provide an implementation of the abstract class Nat that represents non-negative integers.

```scala
abstract class Nat {
  def isZero: Boolean
  def predecessor: Nat
  def successor: Nat
  def + (that: Nat): Nat
  def - (that: Nat): Nat
}
```

Do not use standard numerical classes in this implementation. Rather, implement a sub-object and a sub-class:

```scala
object Zero extends Nat
class Succ(n: Nat) extends Nat
```

One for the number zero, the other for strictly positive numbers.

*Ans*

```scala
abstract class Nat {
  def isZero: Boolean
  def predecessor: Nat
  def successor: Nat = new Succ(this)
  def + (that: Nat): Nat
  def - (that: Nat): Nat
}

object Zero extends Nat {
  def isZero = true
  def predecessor = throw new Error("0.predecessor")
  def + (that: Nat) = that
  def - (that: Nat) = if (that.isZero) this else throw new Error("negative number")
}

class Succ(n: Nat) extends Nat {
  def isZero = false
  def predecessor = n
  def + (that: Nat) = new Succ(n + that) 
  def - (that: Nat) = if (that.isZero) this else n - that.predecessor
}
```
