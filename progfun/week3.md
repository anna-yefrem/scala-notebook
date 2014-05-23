Week 3
======

### 3.1 Class Hierachies

#### define an abstract class containing methods with no implementation

```scala
abstract class IntSet {
  def incl(x: Int): IntSet
  def contains(x: Int): Boolean
}
```

#### implement sets as binary tree

There are 2 types of possible trees:

- a tree for the empty set
- a tree consisting of an integer and two sub-trees.

```scala
class Empty extends IntSet {
  def contains(x: Int): Boolean = false
  def incl(x: Int): IntSet = new NonEmpty(x, new Empty, new Empty)
  override def toString = "."
}

class NonEmpty(elem: Int, left: IntSet, right: IntSet) extends IntSet {
  def contains(x: Int): Boolean =
    if (x < elem) left contains x
	else if (x > elem) right contains x
	else true

  def incl(x: Int): IntSet =
    if (x < elem) new NonEmpty(elem, left incl x, right)
	  else if (x > elem) new NonEmpty(elem, left, right incl x)
	  else this
	override def toString = "{" + left + elem + right + "}"
}
```

- `Empty` and `NonEmpty` are called persistent data structure because the data remains even after change.
- `IntSet` is called *superclass* of `Empty` and `NonEmpty`
- `Empty` and `NonEmpty` are *subclasses* of `IntSet`
- If no superclass is given, the standard class `Object` in Java package `java.lang` is assumed
- The direct or indirect superclasses of a class `C` are called *base classes* of C

#### override 

```scala
abstract class Base {
  def foo = 1
  def bar: Int
}

class Sub extends Base {
  override def foo = 2
  def bar = 3
}
```

#### define objects

```scala
object Empty extends IntSet {
  def contains(x: Int): Boolean = false
  def incl(x: Int): IntSet = new NonEmpty(x, Empty, Empty)
  override def toString = "."
}
```

- `object` defines a *singleton object* named `Empty`
- no other `Empty` instances can be (or needed to be) created
- Singleton objects are values, so `Empty` evaluates to itself

#### create standalone applications in Scala

```scala
object Hello {
  def main(args: Array[String]) = println("hello world!")
}
```

*Exercise*

Write a function `union` to combine two `IntSet`

```scala
abstract class IntSet {
  def union(other: IntSet): IntSet
}
class NonEmpty(elem: Int, left: IntSet, right: IntSet) extends IntSet {
  ...
  def union(other: IntSet): IntSet =
    ((left union right) union other) incl elem
  ...
}
object Empty extends IntSet {
  def union(other: IntSet): IntSet = other
}
```

- to `union` one with another, we take the element from the first list, union its left and right sublist, and union the other, and then include the element
- `{{,1,},2,} union {{,3,},4,}`
  - > `({,1,} union {,,}) union {{,3,},4,} incl 2`
  - >`(({,,} union {,,}) union {,,}) union {{,3,},4,} incl 1 incl 2`
  - > `{{,3,},4,} incl 1 incl 2`
  - > `{{{,1,{,2,}},3,},4,}`

#### dynamic binding

The code invoked by a method call depends on the runtime type of the object that contains the method.

`Empty contains 1` -> [1/x] [Empty/this] false

*Question*: can we implement one concept in terms of the other?

- Objects in terms of higher-order functions?
- Higher-order functions in terms of objects?

### 3.2 How Classes are Organized

#### place a class or object inside a package, use a package clause at the top of source file

```scala
package progfun.examples
object Hello { ... }
```

- run the `Hello` program by `> scala progfun.examples.Hello`
- use `import progfun.examples` to omit typing package name
- use `progfun._` to refer to all the members of package `progfun`

#### forms of import

```scala
import week3.Rational // imports just Rational
import week3.{rational, Hello} // imports both Rational and Hello
import week3._ // imports everything in package week3
```
- the first two forms are called *named imports*
- the last form is called *wildcard import*

#### automatic imports

- all members of package `scala`
- all members of package `java.lang`
- all members of the singleton object `scala.Predef`

#### Traits

- single inheritance: a class can only have one superclass
- a trait is declared like an abstract class, just with `trait` instead of `abstract class`

```scala
trait Planar {
  def height: Int
  def width: Int
  def surface = height * width
}

class Square extends Shape with Planar with Movable ...
```

- objects and traits can inherit from at most one class but arbitrary many traits
- traits can contain fields and concrete methods
- traits cannot have (value) parameters, only class can

#### Top Types

- Any: the base type of all types
  - methods: `==`, `1=`, `equals`, `hashCode`, `toString`
- AnyRef: the base type of all reference types
  - alias of `java.lang.Object`
- AnyVal
  - the base type of all primitive types
  
#### Nothing type

- there is no value of type `Nothing`
- to signal abnormal termination
- as an element type of empty collections: `Set[Nothing]`

#### Exceptions

- `throw Exc` aborts evaluation with the exception with `Exc`

```scala
def error(msg: String) = throw new Error(msg) //> error: (msg: String)Nothing
error("test") //> error messages ...
```

#### Null type

- every reference type also has `null` as a value
- the type of `null` is `Null`
- `null` is a subtype of every class that inherits from `Object`
  - it is incompatible with subtypes of `AnyVal`

```scala
val x = null // x: Null = null
val y: String = null // y: String = null
val z: Int = null // error: type mismatch
```

*Exercise*

What is the type of `if (true) 1 else false`?

*Ans*: `AnyVal` becuase `1` and `false` are incompatible types, thus pick the supertype that match both

```scala
if (true) 1 else false //> res:1 AnyVal = 1
```

### 3.3 Polymorphism

#### Cons-List

- a fundamental data structure is the *immutable linked list*
- constructed from two building blocks
  - `Nil`: the empty list
  - `Cons`: a cell containing an element and the remainder of the list
  
```scala
trait IntList ...
class Cons(val head: Int, val tail: IntList) extends IntList ...
class Nil extends IntList ...
```

Note that `class Cons(val head: Int, val tail: IntList) extends IntList` equals

```scala
class Cons(_head: Int, _tail: IntList) extends IntList {
  val head = _head
  val tail = _tail
}
```

#### Type Parameters

```scala
trait List[T] ...
class Cons[T](val head: Int, val tail: List[T]) extends List[T] ...
class Nil[T] extends List[T] ...
```

#### define a `trait`

```scala
trait List[T] {
  def isEmpty: Boolean
  def head: T
  def tail: List[T]
}

class Cons[T](val head: T, val tail: List[T]) extends List[T] {
  def isEmpty = false
}
class Nil[T] extends List[T] {
  def isEmpty = true
  def head: Nothing = throw new NoSuchElementException("Nil.head") // Nothing is a subtype of T
  def tail: Nothing = throw new NoSuchElementException("Nil.tail")
}
```

#### Generic Functions

```scala
def singleton[T](elem: T) = new Cons[T](elem, new Nil[T])
singleton[Int](1)
singleton[Boolean](true)
```

- in most cases, type parameters can be left out
- type parameters do not affect evaluation in Scala
- this is called *type erasure* at compile time

#### Polymorphism

- means that a function type comes *in many forms*
- 2 principal forms of polymorphism:
  - subtyping: instances of a subclass can be passed to a base class
  - generics: instances of a function or class are created by type parameterization
  
*Exercise*

Write a function `nth` that takes an integer `n` and a list and selects the `n`'th element of the list. Elements are numbered from 0. If `index` is outside the range from 0 up to the length of the list minus one, a `IndexOutOfBoundsException` should be thrown.

```scala
  def nth[T](n: Int, xs: List[T]): T = 
    if (xs.isEmpty) throw new IndexOutOfBoundsException
    else if (n == 0) xs.head
    else nth(n - 1, xs.tail)     
  val list = new Cons(1, new Cons(2, new Cons(3, new Nil)))
  nth(2, list) //> res0: Int = 3
```






