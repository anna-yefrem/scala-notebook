Chatper 6 Objects
=================

### 6.1 Singletons

* define an object

```scala
object Accounts {
  private var lastNumber = 0
  def newUniqueNumber() = { lastNumber += 1; lastNumber }
}
```

### 6.2 Companion Objects

* class with both instance methods (in `class`) and static method (in `object`)

```scala
class Account {
val id = Account.newUniqueNumber()
private var balance = 0.0
def deposit(amount: Double) { balance += amount }
...
}
object Account { // The companion object
private var lastNumber = 0
private def newUniqueNumber() = { lastNumber += 1; lastNumber }
}
```

*TIP*

The class and its companion object can access each other’s private features. They must be located in the same source file

### 6.3 Objects Extending a Class or Trait

```scala
abstract class UndoableAction(val description: String) {
  def undo(): Unit
  def redo(): Unit
}

object DoNothingAction extends UndoableAction("Do nothing") {
  override def undo() {}
  override def redo() {}
}

val actions = Map("open" -> DoNothingAction, "save" -> DoNothingAction, ...)
  // Open and save not yet implemented
```

### 6.4 The `apply` Method

Objects can have `apply` method. Calling `Object(arg1, arg2, ...argN)` will return an object of the companion class

* define an `apply` method

```scala
class Account private (val: Int, initBalance: Double) {
  private var balance = initialBalance
  ...
}

object Account { // The companion object
  def apply(initialBalance: Double) =
    private var lastNumber = 0
    private def newUniqueNumber() = { lastNumber += 1; lastNumber }
	new Account(newUniqueNumber(), initialBalance)
  ...
}

val acct = Account(1000.0)
```

### 6.5 Application Obejcts

* define a `main` method

```scala
object Hello {
  def main(args: Array[String]) {
    println("Hello, World!")
  }
}
```

* define an application object by extending the `App` trait

```scala
object Hello extends App {
  if (args.length > 0)
    println("Hello, " + args(0))
  else
    println("Hello, World!")
}
```

### 6.6 Enumerations

* use `type` alias

```scala
object TrafficLightColor extends Enumeration {
  type TrafficLightColor = Value
  val Red, Yellow, Green = Value
}
```

* use `type` alias

```scala
import TrafficLightColor._
def doWhat(color: TrafficLightColor) = {
  if (color == Red) "stop" 
  else if (color == Yellow) "hurry up" 
  else "go"
}
```

* call `TrafficLightColor.values` yields a set of all values

```scala
for (c <- TrafficLightColor.values) println(c.id + ": " + c)
```

* get `TrafficLightColor.Red`

```scala
TrafficLightColor(0) // Calls Enumeration.apply
TrafficLightColor.withName("Red")
```
