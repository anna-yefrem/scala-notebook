Chapter 4 Classes
=================

## 5.1 Simple Classes and Parameterless Methods

* declare a class

```scala
class Counter { 
private var value = 0 // Y ou must initialize the field
def increment() { value += 1 } // Methods are public by default
def current() = value 
}
```

* use class

```scala
val myCounter = new Counter // Or new Counter()
myCounter.increment() 
println(myCounter.current)
```

* call a parameterless method

```scala
myCounter.current // for an accessor method
myCounter.current() // for a mutator method
myCounter.increment()// Use ()with mutator
println(myCounter.current) // Don’t use ()
```

* enforce parameterless method

```scala
def current = value // No ()in definition
```

## 5.2 Properties with Getters and Setters

* generates a class for the JVM with a private `age` field

```scala
class Person {
  var age = 0
}
```

* the getter and setter methods are called `age` and `age_=`

```scala
println(fred.age) // Calls the method fred.age()
fred.age = 21 // Calls fred.age_=(21)
```

* redefine getter and setter

```scala
class Person {
  private var privateAge = 0 // Make private and rename
  def age = privateAge
  def age_=(newValue: Int) {
    if (newValue > privateAge) privateAge = newValue; // Can’t get younger
  }
}
```

*TIP*

* If the field is private, the getter and setter are private.
* If the field is a `val`, only a getter is generated.
* If you don’t want any getter or setter, declare the field as `private[this]`

### 5.3 Properties with Only Getters

* use `val` to define a field with only a getter but no setter

```scala
class Message { 
  val timeStamp = new java.util.Date
  ...
}
```

* define a field which cannot be set but mutated in some other way

```scala
class Counter { 
  private var value = 0
  def increment() { value += 1 } 
  def current = value // No () in declaration
}

val n = myCounter.current // Calling myCounter.current() is a syntax error
```

*SUMMARY*

1. var foo: Scala synthesizes a getter and a setter.
2. val foo: Scala synthesizes a getter.
3. You define methods foo and foo_=.
4. You define a method foo.

### 5.4 Obejct-Private Fields

```scala
class Counter {
  private var value = 0
  def increment() { value += 1 }
  def isLess(other : Counter) = value < other.value 
    // Can access private field of other object
}
```
### 5.6 Auxiliary Constructors

Auxiliary constructors are very similar to those in Java and C++, with just two differences

1. The auxiliary constructors are called `this`
2. Each auxiliary constructor _must_ start with a call to a previously defined auxiliaryconstructor or the primary constructor.

* class with two auxiliary constructors

```scala
class Person {
  private var name = ""
  private var age = 0
  
  def this(name: String) { //An auxiliary constructor
    this() //Calls primary constructor
    this.name = name 
  }
  
  def this(name: String, age: Int) { //Another auxiliary constructor
    this(name) //Calls previous auxiliary constructor
    this.age = age 
  }
}

val p1 = new Person // Primary constructor
val p2 = new Person("Fred") // First auxiliary constructor
val p3 = new Person("Fred", 42) // Second auxiliary constructor
```

### 5.7 The Primary Constructor

```scala
class Person(val name: String, val age: Int){ 
  // Parameters of primary constructor in parameters (...)
  ...
}
```

Primary Constructor Parameter | Generated Field/Methods
-------------------------------------------------------
`name: String` | object-private field, or no field if no method uses `name`
`private val/var name: String` | private field, private getter/setter
`val/var name: String` | private field, public getter/setter
`@BeanProperty val/var name: String` | private field, public Scala and JavaBeans getters/setters

### 5.8 Nested Classes

In Scala, each `instance` has its own class `Member`, just like each instance has its own field `members`. That is, `chatter.Member` and `myFace.Member` are different classes.

```scala
import scala.collection.mutable.ArrayBuffer
class Network {
  class Member(val name: String) {
    val contacts = new ArrayBuffer[Member]
  }
  
  private val members = new ArrayBuffer[Member]
  
  def join(name: String) = {
    val m = new Member(name)
  members += m
  m
  }
}

val chatter = new Network
val myFace = new Network

val fred = chatter.join("Fred")
val wilma = chatter.join("Wilma")
fred.contacts += wilma // OK
val barney = myFace.join("Barney") // Has type myFace.Member
fred.contacts += barney 
  // No — can’t add a myFace.Member to a buffer of chatter.Memberelements
```

* access the `this` reference of the enclosing class as `EnclosingClass.this`

```scala
class Network(val name: String) { outer =>
  class Member(val name: String) {
  ...
  def description = name + " inside " + outer.name
  }
}
```
