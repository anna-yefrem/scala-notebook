Exercises for Chapter 6
=======================

### 1. Write an object `Conversions` with methods `inchesToCentimeters`, `gallonsToLiters`, and `milesToKilometers`.

_Ans_:

```scala
object Conversions {
  def inchesToCentimeters(n: Double) = n * 2.54
  def gallonsToLiters(n: Double) = n * 3.785
  def milesToKilometers(n: Double) = n * 1.609
}
```

### 2. The preceding problem wasnot very object-oriented. Provide a general superclass UnitConversion and define objects InchesToCentimeters, GallonsToLiters, and MilesToKilometers that extend it.

_Ans_:

```scala
class UnitConversion(val rate: Double) {
  def convert(n: Int) = n * rate
}

object InchesToCentimeters extends UnitConversion(2.54) {
}

object GallonsToLiters extends UnitConversion(3.785) {
}

object MilesToKilometers extends UnitConversion(1.609) {
}
   
InchesToCentimeters.convert(10)
  //> res0: Double = 25.4
GallonsToLiters.convert(20)
  //> res1: Double = 75.7
```

### 3. Define an Origin object that extends `java.awt.Point`. Why is this not actually a good idea? (Have a close look at the methods of the `Point` class.)

_Ans_: There are methods like `setLocation(int x, int y)` of class `java.awt.Point`. Consider that the Origin is a singleton, which should be immutable, the methods should be private and not allow to be invoked.

```scala
object Origin extends java.awt.Point(0, 0) {
}
```

### 4. Define a `Point` class with a companion object so that you can construct Point instances as `Point(3, 4)`, without using new.

_Ans_:

```scala
class Point (val x: Int = 0, val y: Int = 0) {
  override def toString() = "<" + x + "," + y + ">"
}

object Point {
  def apply(x: Int, y: Int) = new Point(x, y)
}

Point(1, 2) //> res0: Point = <1,2>
```

### 5. Write a Scala application, using the `App` trait, that prints the command-line arguments in reverse order, separated by spaces. For example, scala `Reverse Hello World` should print `World Hello`.

_Ans_:

```scala
object Test extends App {
  if (args.length > 0) {
    println(args.reverse.mkString(" " ))
  } 
}
```

### 6. Write an enumeration describing the four playing card suits so that the toString method returns ♣, ♦, ♥, or ♠.

_Ans_:

```scala
object Suit extends Enumeration {
  val CLUB = Value(9827.toChar.toString)    // ♣
  val DIAMOND = Value(9830.toChar.toString) // ♦
  val HEART = Value(9829.toChar.toString)   // ♥
  val SPADE = Value(9824.toChar.toString)   // ♠
}
```

### 7. Implement a function that checks whether a card suit value from the preceding exercise is red.

_Ans_:

```scala
def isRed(color: Value) = color == HEART || color == DIAMOND
```

### 8. Write an enumeration describing the eight corners of the RGB color cube. As IDs, use the color values (for example, 0xff0000 for Red).

_Ans_:

```scala
object Color extends Enumeration {
  val BLACK     = Value(0x000000)
  val BLUE      = Value(0x0000ff)
  val GREEN     = Value(0x00ff00)
  val CYAN      = Value(0x00ffff)
  val RED       = Value(0xff0000)
  val MAGENTA   = Value(0xff00ff)
  val YELLOW    = Value(0xffff00)
  val WHITE     = Value(0xffffff)
}
```
