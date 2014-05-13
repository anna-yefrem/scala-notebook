Week 2
======

### 2.1 Higher-Order Functions

* common pattern for sum_(n=a)^b{f(n)}:

```scala
def sum(f: Int => Int, a: Int, b: Int):
  if (a > b) 0
  else f(a) + sum(f, a + 1, b)

def sumInts(a: Int, b: Int)	 		= sum(id, a, b)
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

Note that `Int => Int => Int` is equivalent to `Int => (Int => Int)`

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
def factorial(n: Int) = product(x => x)(1, x)
```

3. Write a more general function, which generalizes both `sum` and `product`
