Exercises of Chapter 2
======================

### 1. The signum of a number is 1 if the number is positive, –1 if it is negative, and 0 if it is zero. Write a function that computes this value.

_Ans_:

```scala
def signum(n: Int) = {
  if (n > 0) 1 else if (n < 0) -1 else 0
}
  //> signum: (n: Int)Int
signum(5)  //> res0: Int = 1
signum(-3) //> res1: Int = -1
signum(0)  //> res2: Int = 0
```

### 2. What is the value of an empty block expression `{}`? What is its type?

_Ans_: `val empty = {}` returns `empty: Unit = ()`.

### 3. Come up with one situation where the assignment `x = y = 1` is valid in Scala. (Hint: Pick a suitable type for `x`.)

_Ans_: `x` should be of type `Unit`

```scala
var x: Unit = () //> x  : Unit = ()
var y: Int = 0   //> y  : Int = 0
x = y = 1
```

### 4. Write a Scala equivalent for the Java loop

```java
for (int i = 10; i >= 0; i--) System.out.println(i);
```

_Ans_:

```scala
for (i <- 0 to 10 reverse) println(i)
```

### 5. Write a procedure `countdown(n: Int)` that prints the numbers from n to 0.

_Ans_:

```scala
def countdown(n: Int) {
  for (i <- 0 to n reverse) println(i)
}
```

### 6. Write a for loop for computing the product of the Unicode codes of all letters in a string. For example, the product of the characters in `"Hello"` is `9415087488L`.

_Ans_: `toInt` method of Char returns the unicode code of the letter

```scala
val s = "Hello"
var product: Long = 1
for (c <- s) product *= c.toInt
product // product: Long = 9415087488
```

### 7. Solve the preceding exercise without writing a loop. (Hint: Look at the StringOps Scaladoc.)

_Ans_: use `foldLeft` to do letter-wise reduce operation. Use `1L` rather than `1` as initial value, for 9415087488 is too large for Int type

```scala
val s = "Hello"
s.foldLeft(1L)(_ * _.toInt) //> res0: Long = 9415087488
```

### 8. Write a function `product(s : String)` that computes the product, as described in the preceding exercises.

_Ans_:

```scala
def product(s: String) = s.foldLeft(1L)(_ * _.toInt)
product("Hello") //> res0: Long = 9415087488
```

### 9. Make the function of the preceding exercise a recursive function.

_Ans_:

```scala
def product(s: String): Long = if (s.length == 0) 1 else s(0).toInt * product(s.drop(1))
product("Hello") //> res0: Long = 9415087488
```

### 10. Write a function that computes x^n, where n is an integer. Use the following recursive definition:

* x^n = y^2 if n is even and positive, where y = x^n / 2.
* x^n = x·x^n – 1 if n is odd and positive.
* x^0 = 1.
* x^n = 1 / x^–n if n is negative.

Don’t use a `return` statement.

_Ans_:

```scala
def power(x: BigInt, n: => Int): BigInt = {
  if (n > 0) {
    if (n % 2 == 0) {val pow2 = power(x, n / 2); pow2 * pow2}
    else x * power(x, n - 1)
  }
  else if (n == 0) 1
  else 1 / power(x, -n)
}
power(2, 1024)
```
