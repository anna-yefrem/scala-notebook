Exercises for Chapter 5
=======================

### 1. Improve the Counter class in Section 5.1, “Simple Classes and Parameterless Methods,” on page 49 so that it doesn’t turn negative at `Int.MaxValue`.

_Ans_:

```scala
class Counter {
  private var value = 0 // You must initialize the field
  def increment() { if (value == Int.MaxValue) value else value += 1 }
  def current() = value
}
```

### 2. Write a class `BankAccount` with methods `deposit` and `withdraw`, and a read-only property `balance`.

_Ans_:

```scala
class BankAccount {
  private var value = 0
  def balance = value
  def deposit(money: Int) { value += money }
  def withdraw(money: Int) { value -= money }
}
val a = new BankAccount()
println(a.balance) //> 0
a.deposit(100)
println(a.balance) //> 100
a.withdraw(50)
println(a.balance) //> 50          
```

### 3. Write a class `Time` with read-only properties `hours` and `minutes` and a method `before(other: Time): Boolean` that checks whether this time comes before the other. A `Time` object should be constructed as `new Time(hrs, min)`, where `hrs` is in military time format (between 0 and 23).

_Ans_:

```scala
class Time(hrs: Int, min: Int) {
	val hours = hrs;
	val minutes = min;
	def before(other: Time) = {
	  if (hours < other.hours) true
	  else if (hours == other.hours && minutes < other.minutes) true
	  else false
	}
	override def toString() = hours + ":" + (if (minutes == 0) "00" else minutes)
}

val a = new Time(3, 50)
val b = Array(new Time(4, 30), new Time(2, 00), new Time(3, 55))
b.foreach(x => {
  println(a + " is" + (if (!a.before(x)) " not" else "") + " before " + x)
})
  //> 3:50 is before 4:30
  //| 3:50 is not before 2:00
  //| 3:50 is before 3:55
```

### 4. Reimplement the `Time` class from the preceding exercise so that the internal representation is the number of minutes since midnight (between 0 and 24 × 60 – 1). Do not change the public interface. That is, client code should be unaffected by your change.

_Ans_:

```scala
class Time(hrs: Int, min: Int) {
	private var minSinceMidnight = hrs * 60 + min;
	val hours: Int = minSinceMidnight / 60;
	val minutes: Int = minSinceMidnight - hours * 60;
	def before(other: Time) = {
	  if (hours < other.hours) true
	  else if (hours == other.hours && minutes < other.minutes) true
	  else false
	}
	override def toString() = hours + ":" + (if (minutes == 0) "00" else minutes)
}
```

### 5. Make a class Student with read-write JavaBeans properties name (of type String) and id (of type Long). What methods are generated? (Use javap to check.) Can you call the JavaBeans getters and setters in Scala Should you?

_Ans_:

```scala
class Student (n: String, i: Int) {
  @BeanProperty var name = n
  @BeanProperty var id = i
}
```

or

```scala
class Student ( @BeanProperty var name: String,
                @BeanProperty var id: Int) {
}
```

* call getters and setters

```scala
val a = new Student("Kevin", 1)
println(a.getName) //> Kevin
println(a.getId) //> 1
a.setId(10)
println(a.getId) //> 10
```

### 6. In the `Person` class of Section 5.1, “Simple Classes and Parameterless Methods,” on page 49, provide a primary constructor that turns negative ages to 0.

_Ans_:

```scala
class Person {
  private var privateAge = 0
  def age = privateAge
  def age_=(newValue: Int) {
    if (newValue < 0) privateAge = 0
    else if (newValue > privateAge) privateAge = newValue
  }
}

val fred = new Persons

fred.age = -10
println(fred.age) //> 0

fred.age = 30
fred.age = 21
println(fred.age) //> 30
```

### 7. Write a class Person with a primary constructor that accepts a string containing a first name, a space, and a last name, such as new `Person("Fred Smith")`. Supply read-only properties firstName and lastName. Should the primary constructor parameter be a var, a val, or a plain parameter? Why?

_Ans_: The primary constructor parameter should be a plain parameter.

```scala
class Person(name: String) {
  val first = name.split(" ")(0)
  val last = name.split(" ")(1)
  override def toString() = "First: " + first + " Last: " + last
}

new Person("Fred Smith")
  //> res0: ch1.ex.Person = First: Fred Last: Smith
```

### 8. Make a class `Car` with read-only properties for manufacturer, model name, and model year, and a read-write property for the license plate. Supply four constructors. All require the manufacturer and model name. Optionally, model year and license plate can also be specified in the constructor. If not, the model year is set to -1 and the license plate to the empty string. Which constructor are you choosing as the primary constructor? Why?

_Ans_:

```scala
class Car (val man: String, val name: String, val year: Int = -1, val license: String = "") {
  override def toString() = (if (license.length == 0) "NA" else license) + ": " + man + " " + name + (if (year > 0) " manufactured in " + year + " " else "")
}

new Car("Benz", "S350", 1998, "CA293")
  //> res0: Car = CA293: Benz S350 manufactured in 1998 
new Car("Honda", "Civic")
  //> res1: Car = NA: Honda Civic
new Car("Toyota", "Crown", license = "OH192")
  //> res2: Car = OH192: Toyota Crown
```

### 9. Reimplement the class of the preceding exercise in Java, C#, or C++ (your choice). How much shorter is the Scala class?

_OMITTED_

### 10. Consider the class

```scala
class Employee(val name: String, var salary: Double) {
  def this() { this("John Q. Public", 0.0) }
}
```

Rewrite it to use explicit fields and a default primary constructor. Which form do you prefer? Why?

_Ans_: not sure if is correct

```scala
class NewEmployee {
  private var privateName = "John Q. Public"
  def name = privateName
  var salary = 0.0
  
  def this(newName: String, newSalary: Double) {
    this()
    privateName = newName
    salary = newSalary
  }
}
```
