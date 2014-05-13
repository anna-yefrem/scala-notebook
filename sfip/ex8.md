Exercises for Chapter 8
=======================

### 1. Extend the following `BankAccount` class to a `CheckingAccount` class that charges $1 for every deposit and withdrawal.

```scala
class BankAccount(initialBalance: Double) {
  private var balance = initialBalance
  def currentBalance = balance
  def deposit(amount: Double) = { balance += amount; balance }
  def withdraw(amount: Double) = { balance -= amount; balance }
}
```

_Ans_:

```scala
class CheckingAccount(initialBalance: Double) extends BankAccount(initialBalance) {
  override def deposit(amount: Double) = super.deposit(amount - 1)
  override def withdraw(amount: Double) = super.withdraw(amount + 1)
}

val account = new CheckingAccount(100)
account.deposit(100) //> res0: Double = 199.0
account.withdraw(50) //> res1: Double = 148.00
```

### 2. Extend the `BankAccount` class of the preceding exercise into a class `SavingsAccount` that earns interest every month (when a method `earnMonthlyInterest` is called) and has three free deposits or withdrawals every month. Reset the transaction count in the earnMonthlyInterest method.

```scala
class SavingsAccount(initialBalance: Double) extends BankAccount(initialBalance) {
  private var countTransaction = 0
  def earnMonthlyInterest(rate: Double) = {
    super.deposit(currentBalance * rate)
    countTransaction = 0
    currentBalance
  }
  
  override def deposit(amount: Double) = {
    countTransaction += 1
    if (countTransaction > 3) super.deposit(amount - 1)
    else super.deposit(amount)
  }
  override def withdraw(amount: Double) = {
    countTransaction += 1
    if (countTransaction > 3) super.withdraw(amount + 1)
    else super.withdraw(amount)
  }
}
val account = new SavingsAccount(100)

account.earnMonthlyInterest(0.01) //> res0: Double = 101.0
account.deposit(100)              //> res1: Double = 201.0
account.withdraw(50)              //> res2: Double = 151.0
account.deposit(200)              //> res3: Double = 351.0
account.withdraw(20)              //> res4: Double = 330.0
account.earnMonthlyInterest(0.01) //> res5: Double = 333.3
account.withdraw(100)             //> res6: Double = 233.3
```

### 3. Consult your favorite Java or C++ textbook that is sure to have an example of a toy inheritance hierarchy, perhaps involving employees, pets, graphical shapes, or the like. Implement the example in Scala.

_OMMITTED_

### 4. Define an abstract class `Item` with methods `price` and `description`. A `SimpleItem` is an item whose `price` and `description` are specified in the constructor. Take advantage of the fact that a `val` can override a `def`. A Bundle is an item that contains other items. Its price is the sum of the prices in the bundle. Also provide a mechanism for adding items to the bundle and a suitable description method.

_Ans_:

```scala
abstract class Item() {
  def price: Double
  def description: String
}

class SimpleItem(override val price: Double = 0.0, override val description: String = "") extends Item {
}

class Bundle() extends Item {
  var itemList = ArrayBuffer[Item]()
  def add(item: Item) = { itemList.append(item); this}
  override def price = { itemList.map(_.price).reduce(_ + _) }
  override def description = itemList.map(_.description).reduce(_ + ", " + _)
}

val ipad = new SimpleItem(2888, "iPad")
val iphone = new SimpleItem(4888, "iPhone")
val bundle = new Bundle()
bundle.add(ipad).add(iphone)
println(bundle.price)                           //> 7776.0
println(bundle.description)                     //> iPad, iPhone

val device = new Bundle()
device.add(bundle).add(new SimpleItem(3888, "TV"))
println(device.price)                           //> 11664.0
println(device.description)                     //> iPad, iPhone, TV
```

### 5. Design a class `Point` whose `x` and `y` coordinate values can be provided in a constructor. Provide a subclass `LabeledPoint` whose constructor takes a label value and `x` and `y` coordinates, such as

```scala
new LabeledPoint("Black Thursday", 1929, 230.07)
```

_Ans_:

```scala
class Point(val x: Double = 0.0, val y: Double = 0.0) {
  override def toString() = "<" + x + "," + y + ">"
}

class LabeledPoint(val label: String = "", x: Double = 0.0, y: Double = 0.0) extends Point(x, y) {
  override def toString() = label + ": <" + x + "," + y + ">"
}

new LabeledPoint("Black Thursday", 1929, 230.07)
  //> res0: LabeledPoint = Black Thursday: <1929.0,230.07>
```

### 6. Define an abstract class `Shape` with an abstract method `centerPoint` and subclasses `Rectangle` and `Circle`. Provide appropriate constructors for the subclasses and override the `centerPoint` method in each subclass.

_Ans_:

```scala
abstract class Shape() {
  def centerPoint(): Point
}

class Rectangle(val point1: Point, val point2: Point, val point3: Point, val point4: Point) extends Shape {
  override def centerPoint() = new Point((point1.x + point2.x + point3.x + point4.x)/4,
                                         (point1.y + point2.y + point3.y + point4.y)/4)
}

class Circle(val centroid: Point = new Point(0, 0), val radius: Double = 0) extends Shape {
  override def centerPoint() = centroid
}

val rec = new Rectangle(new Point(1, 1), new Point(4, 4), new Point(1, 4), new Point(4, 1))
rec.centerPoint //> res0: Point = <2.5,2.5>
val circle = new Circle(new Point(3, 3), 2.0)
circle.centerPoint //> res1: Point = <3.0,3.0>
```

or

```scala
abstract class Shape() {
  def centerPoint(): Tuple2[Double, Double]
}

class Rectangle(val x: Double, val y: Double, val width: Double, val height: Double) extends Shape {
  override def centerPoint() = (x + width / 2, y - height / 2)
}

class Circle(val x: Double, val y: Double, val radius: Double) extends Shape {
  override def centerPoint() = (x, y)
}

val rec = new Rectangle(1, 4, 3, 3)
rec.centerPoint //> res0: (Double, Double) = (2.5,2.5)
val circle = new Circle(3, 3, 2) 
circle.centerPoint //> res1: (Double, Double) = (3.0,3.0)    
```

### 7. Provide a class `Square` that extends `java.awt.Rectangle` and has three constructors: one that constructs a square with a given corner point and width, one that constructs a square with corner (0, 0) and a given width, and one that constructs a square with corner (0, 0) and width 0.

_Ans_:

```scala
class Square(x: Int, y: Int, width: Int) extends java.awt.Rectangle(x, y, width, width)  {
  def this() {
    this(0, 0, 0)
  }
  
  def this(width: Int) {
    this(0, 0, width)
  }
}

new Square() //> res0: ch1.Square = Square[x=0,y=0,width=0,height=0]
new Square(5) //> res1: ch1.Square = Square[x=0,y=0,width=5,height=5]
new Square(2, 3, 8) //> res2: ch1.Square = Square[x=2,y=3,width=8,height=8]
```

### 8. Compile the `Person` and `SecretAgent` classes in Section 8.6, “Overriding Fields,” on page 89 and analyze the class files with javap. How many name fields are there? How many name getter methods are there? What do they get? (Hint: Use the -c and -private options.)

_OMITTED_

### 9. In the `Creature` class of Section 8.10, “Construction Order and Early Definitions,” on page 92, replace `val range` with a `def`. What happens when you also use a `def` in the `Ant` subclass? What happens when you use a `val` in the subclass? Why?

_Ans_: Everything remains the same by replacing `val range` with a `def`. The length of array `env` of subclass `Ant` is 2.

```scala
class Creature {
  def range: Int = 10
  val env: Array[Int] = { println(range); new Array[Int](range) }
}

class Ant extends Creature {
  override val range = 2
}

val a = new Creature
a.range      //> res0: Int = 10
a.env.length //> res1: Int = 10
  
val b = new Ant
b.range      //> res2: Int = 2
b.env.length //> res3: Int = 0
```

The length of array `env` changes to `10` after using `def` in the `Ant` subclass.

```scala
class Ant extends Creature {
  override def range = 2
}

val b = new Ant
b.range //> res2: Int = 2
b.env.length //> res3: Int = 2
```

### 10. The file scala/collection/immutable/Stack.scala contains the definition

```scala
class Stack[A] protected (protected val elems: List[A])
```

Explain the meanings of the protected keywords. (Hint: Review the discussion of private constructors in Chapter 5.)

_Ans_: To make the primary constructor private, place the keyword private like this:

```scala
class Person private(val id: Int) { ... }
```

A class user must then use an auxiliary constructor to construct a Person object.

In this question, the `Stack` class uses `protected` keyword, which means that both the constructor and the field is only accessible to its subclass, but not other classes.
