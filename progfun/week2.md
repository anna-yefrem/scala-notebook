Week 2
======

### 2.1 Higher-Order Functions

* common pattern for \[sum_(n=a)^b{f(n)}\]:

```scala
def sum(f: Int => Int, a: Int, b: Int):
  if (a > b) 0
  else f(a) + sum(f, a + 1, b)

def sumInts(a: Int, b: Int)	 	= sum(id, a, b)
def sumCubes(a: Int, b: Int)		= sum(cube, a, b))
def sumFactorials(a: int, b: Int) 	= sum(fact, a, b))

def id(x: Int): Int	= x
def cube(x: Int): Int = x * x * x
def fact(x: Int): Int = if (x == 0) 1 else fact (x - 1)
```

* anonymous function can be expressed by

```scala
(x: Int) => x * x * x
(x: Int, y: Int) => x + y
```

* thus previous functions can be written as

```scala
def sumInts(a: Int, b: Int) = sum(x => x, a, b)
def sumCubes(a: Int, b: Int) = sum(x => x * x * x, a, b)
```

_EXERCISE_

```scala
def sum(f: Int => Int, a: Int, b: Int): Int = {
  def loop(a: Int, acc: Int): Int = {
    if (a > b) acc
    else loop(a + 1, acc + f(a))
  }
  loop(a, 0)
}
  
sum(x => x, 1, 2) 
  //> res0: Int = 3
```

### 2.2 Currying

Can we get rid of the same parameters?

```scala
def sum(f: Int => Int): (Int, Int) => Int = {
  def sumF(a: Int, b: Int): Int = 
    if (a > b) 0
	else f(a) + sumF(a + 1, b)
  sumF
}

def sumInts 		= sum(x => x)
def sumCubes		= sum(x => x * x * x)
def sumFactorials 	= sum(fact)

sumCubes(1, 10) + sumFactorials(10, 20)
```

* consecutive stepwise applicaitions

```scala
sum (cube) (1, 10) == (sum (cube)) (1, 10)
```

* multiple parameter lists

```scala
def sum(f: Int => Int)(a: Int, b: Int) = {
  if (a > b) 0 else f(a) + sum(f)(a + 1, b)
}
```

* type of `def sum(f: Int => Int)(a: Int, b: Int): Int = ...` is

```scala
(Int => Int) => (Int, Int) => Int
```

* Note that `Int => Int => Int` is equivalent to `Int => (Int => Int)`

_EXERCISE_

1. Write a `product` function that calculates the product of the values of a function for the points on a given interval.

```scala
def product(f: Int => Int)(a: Int, b: Int): Int = {
  if (a > b) 1
  else f(a) * product(f)(a + 1, b)
}   
```

2. Write `factorial` in terms of `product`.

```scala
def factorial(n: Int) = product(x => x)(1, n)
```

3. Write a more general function, which generalizes both `sum` and `product`

* This is a version a `mapReduce`

```scala
def mapReduce(f: Int => Int, combine: (Int, Int) => Int, zero: Int)(a: Int, b: Int): Int = 
  if (a > b) zero
  else combine(f(a), mapReduce(f, combine, zero)(a + 1, b))
```

* `product` function:

```scala
def product(f: Int => Int)(a: Int, b: Int): Int = mapReduce(f, (x, y) => x * y, 1)(a, b)
```

### 2.3 Example: Finding Fixed Points

```scala
def sqrt(x: Double) = fixedPoint(y => (y + x / y) / 2)(1.0)
```

* use `averageDamp` to computer `sqrt`

```scala
def averageDamp(f: Double => Double)(x: Double) = (x + f(x)) /2
def sqrt(x: Double) = fixedPoint(averageDamp(y => x / y))(1.0)
```

### 2.4 Scala Syntax Summary

* EBNF:
  * | denotes an alternative
  * \[...\] an option (0 or 1)
  * {...} a repetition (0 or more)

* A _type_ can be:
  * numeric type: `Int`, `Double`
  * Boolean type: `true`, `false`
  * String type
  * function type: `Int => Int`, `(Int, Int) => Int`
  
* An _expression_ can be:
  * identifier: `x`, `isGoodEnough`
  * literal: `0`, `1.0`, `"abc"`
  * function application: `sqrt(x)`
  * operator application: `-x`, `y + x`
  * selection: `math.abs`
  * conditional expreesion: `if (x < 0) -x else x`
  * block: `{ val x = math.abs(y) ; x * 2 }`
  * anonymous function: `x => x + 1`
  
* A _definition_ can be:
  * function definition: `def square(x: Int) = x * x`
  * value definition: `val y = square(2)`

* A _parameter_ can be:
  * call-by-value (CBV) parameter: `(x: Int)`
  * call-by-name (CBN) parameter: `(y: => Double)`
  
### 2.5 Functions and Data

* define a class for rational number

```scala
class Rational(x: Int, y: Int) {
  def numer = x
  def denom = y
}
```

* create an object by prefix `new`: `new Rational(1, 2)`

* implement rational arithmetic `addRational` function for addition

```scala
def addRational(r: Rational, s: Rational): Rational = 
  new Rational(
    r.numer * s.denom + s.numer * r.denom,
	r.denom * s.denom)
def makeString(r: Rational) =
  r.numer + "/" + r.denom
makeString(addRational(new Rational(1, 2), new Rational(2, 3))) //> 7/6
```

* implement method `add` method for addition

```scala
class Rational {
  def add(that: Rational) =
    new Rational(
      numer * that.denom + that.numer * denom,
	  denom * that.denom)
  override def toString = numer + "/" + denom
}
	
val x = new Rational(1, 2)
val y = new Rational(2, 3)
x.add(y) //> 7/6
```

_EXERCISE_

* define `neg`, `sub` and calculate x - y - z

```scala
def neg: Rational = new Rational(-numer, denom)
def sub(that: Rational) = add(that.neg) // Don't repeat yourself to rewrite
val x = new Rational(1,3)
val y = new Rational(5,7) 
val z = new Rational(3,2) 
x.neg //> res0: Rational = -1/3
x.sub(y).sub(z) //> res1: Rational = -79/42
```

### 2.6 More Fun with Rationals

* define `gcd` function and constructor for `numer` and `denom`

```scala
private def gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
private val g = gcd(x, y)
val numer = x / g
val denom = y / g
```

* define `less` and `max`

```scala
def less(that: Rational) = numer * that.denom < that.numer * denom
def max(taht: Rational) = if (this.less(that)) that else this
```

* use `require` to define preconditions to throw `IlligalArgumentException`

```scala
require(y != 0, "denominator must not be 0")
```

* use `assert` to define assertions to throw `AssertionError`

```scala
val x  = sqrt(y)
assert(x >= 0)
```

_TIP_

* `require` is used to enforce a precondition on the caller of a function
* `assert` is used as to check the code of the function itself

* define constructors: primary and auxiliary

```scala
class Rational(x: Int, y: Int) {
  def this(x: Int) = this(x, 1)
}
```

### 2.7 Evaluation and Operators

* the substitution process of `new C(v1, ..., vm) { def y(x1, ..., xm) }`:

_DIFFICULT_


```scala
new Rational(1, 2).numer //> res0: Int = 1
```

* operators

```scala
r add s // Infix Notation same as r.add(s)
```

* identifier can be
  * alphanumeric: `x1`
  * symbolic: `*`, `+?%&`
  * underscore character '_' counts as a letter: `vector_++`
  * end in an underscore, followed by some operator symbols: `counter_=`

* define `<` for `less`

```scala
def < (that: Rational) = numer * that.denom < that.numer * denom
def max(that: Rational) = if (this < that) that else this
def unary_- : Rational = new Rational(-numer, denom) // unary operator
def - (that: Rational) = this + -that // use unary operator -
```

* Precedence: table in increasing order of priority
  * (all letters)
  * `|`
  * `^`
  * `&`
  * `<` `>`
  * `=` `!`
  * `:`
  * `+` `-`
  * `*` `/` `%`

_EXERCISE_

* provide a fully parenthized version of `a + b ^? c ?^ d less a ==> b | c`

_Ans_:

1. `a + b ^? c ?^ d less a ==> b | c`
2. `a + b ^? (c ?^ d) less a ==> b | c`
3. `(a + b) ^? (c ?^ d) less a ==> b | c`
4. `(a + b) ^? (c ?^ d) less (a ==> b) | c`
5. `((a + b) ^? (c ?^ d)) less (a ==> b) | c`
6. `((a + b) ^? (c ?^ d)) less ((a ==> b) | c)`
