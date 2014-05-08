Chapter 8 Inheritance
=====================

### 8.1 Extending a Class

* extend a class in Scala just like in Java

```scala
class Employee extends Person {
  var salary = 0.0
  ...
}
```

### 8.2 Overriding Methods

* must `override` modifier when override a method that isn't abstract

```scala
public class Person {
  ...
  override def toString = getClass.getName + "[name = " + name + "]"
}
```

_TIPS_

The `override` modifier is useful:

1. When misspell the name of the method
2. When provide wrong parameters
3. When introdue a new method in a superclass that clashes with a subclass method

* invoke a superclass method

```scala
public class Employee extends Person {
  ...
  override def toString = super.toString + "[salary = " + salary + "]"
}
```

### 8.3 Type Checks and Casts

* use `isInstanceOf[]` method to test whether an object belongs to a given class
* if test succeeds, use `asInstanceOf[]` method to convert a refence to a subclass reference

```scala
if (p.isInstanceOf[Employee]) {
  val s = p.asInstanceOf[Employee] // s has type Employee
    // if p referes to an object of class Employee or its subclass, then p.isInstanceOf[Employee] test succeeds
	// if p is null, then p.isIstanceOf[Employee] is false and p.asInstanceOf[Employee] returns null
	// if p is not an employee, then p.asInstanceOf[Employee] will throw an exception
  ...
}
```

* test if an instance refer to an object of a class but not its subclass

```scala
if (p.getClass == classOf[Employee]) // classOf is defined in the scala.Predef object
```

* use pattern matching to do type check and cast

```scala
p match {
  case s: Employee => ... // process s an Employee
  case _ => ... // p wasn't a Employee
}
```

### 8.5 Superclass Construction

```scala
class Employee(name: String, age: Int, val salary : Double) extends
Person(name, age)
```

is equivalent to

```java
public class Employee extends Person { //Java
  private double salary;
  public Employee(String name, int age, double salary) {
    super(name, age);
    this.salary = salary;
  }
}
```

### 8.6 Overriding Fields

* override a `val` and `def` using `override` modifier

```scala
class Person(val name: String) {
  override def toString = getClass.getName + "[name=" + name + "]"
}

class SecretAgent(codename: String) extends Person(codename) {
  override val name = "secret" //Don’t want to reveal name . . . 
  override val toString = "secret" // . . . or class name
}
```

* override a `val` in parameter

```scala
abstract class Person { // See abstract classes
  def id: Int // Each person has an ID that is computed in some way
  ...
}

class Student(override val id: Int) extends Person
// A student ID is simply provided in the constructor
```

_TIP_

* A `def` can only override another `def`.
* A `val` can only override another `val` or a parameterless def.
* A `var` can only override an abstract `var` See "Abstract Classes").

### 8.7 Anonymous Subclasses

* make an instance of an _anonymous_ subclass if you include a block
with definitions or overrides

```scala
val alien = new Person("Fred") {
  def greeting = "Greetings, Earthling! My name is Fred."
    // create an object of a structural type as Person{def greeting: String}
}
```

### 8.8 Abstract Classes

* define a class which can not be instantiated

```scala
abstract class Person(val name: String) {
 def id: Int // No method body — this is an abstract method
}
```

* need not use the `override` keyword to define a method
that was abstract in the superclass

```scala
class Employee(name: String) extends Person(name) {
  def id = name.hashCode // override keyword not required
}
```

### 8.9 Abstract Fields

* define an abstract class with abstract fields which is simply a field without an initial value

```scala
abstract class Person {
  val id: Int 
    // No initializer—this is an abstract field with an abstract getter method
  var name: String 
    // Another abstract field, with abstract getter and setter methods
}
  // The class defines abstract getter methods for the idand name fields, and an abstract setter for the namefield.
  // The generated Java class has no fields
```

* define a concrete subclass which concrete fields  

```scala
class Employee(val id: Int) extends Person { //Subclass has concrete id property
  var name = "" //and concrete name property
}
```

* customerize an abstract field by using an anonymous type

```scala
val fred = new Person {
val id = 1729
var name = "Fred"
}
```

### 8.10 Early Definitions

```scala
class Creature {
  val range: Int = 10
  val env: Array[Int] = new Array[Int](range)
}

class Ant extends Creature {
  override val range = 2
}
  // superclass constructor runs before subclass construction, thus length of env is set to range = 10 but not 2

class Bug extends {
  override val range = 3
} with Creature
  // initialize val fields of a subclass before the superclass is executed
```

### 8.12 Object Equality

```scala
class Item(val description: String, val price: Double) {
  final override def equals(other: Any) = {
    val that = other.asInstanceOf[Item]
    if (that == null) false 
    else description == that.description && price == that.price
  }
}
```

_TIP_

* define the method as `final` because it's difficult to extends equality in a subclass. For example, we want a.equals(b) to have the same result as b.equals(a), even when b belongs to a subclass
* define the `equals` method with parameter type `Any`
