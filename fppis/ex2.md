Week 2: Functional Sets
=====================================

In this assignment, you will work with a functional representation of _sets_ based on the mathematical notion of characteristic functions. The goal is to gain practice with _higher-order functions_.

### Representation

__Instruction__

As an example to motivate our representation, how would you represent the set of all negative integers? You cannot list them all… one way would be so say: if you give me an integer, I can tell you whether it’s in the set or not: for `3`, I say `no`; for `-1`, I say `yes`.

Mathematically, we call the function which takes an integer as argument and which returns a boolean indicating whether the given integer belongs to a set, the _characteristic_ function of the set. For example, we can characterize the set of negative integers by the characteristic function `(x: Int) => x < 0`.

Therefore, we choose to represent a set by its characterisitc function and define a type alias for this representation:

```scala
type Set = Int => Boolean // type alias
```

Using this representation, we define a function that tests for the presence of a value in a set:

```scala
def contains(s: Set, elem: Int): Boolean = s(elem)
```

### 2.1 Basic Functions on Sets

* define a function which creates a singleton set from one integer value: the set represents the set of the one given element.

__Solution__

Since the function returns a `set` which is a type alias of function `(Int) => Int`, the right-hand side should be a anonymous function of form `(x: Int) => ???`. Singleton means that it returns true if `elem` is equal to the parameter `x` of the anonymous founction.

__Example__

```scala
val signletonSet(1) // create a Set which compares if a value is equal to 1
```

__Code__

```scala
def singletonSet(elem: Int): Set = (x: Int) => x == elem
```

* define the functions `union`, `intersect`, and `diff`, which takes two sets, and return, respectively, their union, intersection and differences. `diff(s, t)` returns a set which contains all the elements of the set `s` that are not in the set `t`.

__Solution__

These functions returns a `Set`, which is similar to the previous question. Thus, the right-hand side also takes the form `(x: Int) => ???`. The functions make use of boolean operators like `||`, `&&` and `!`. It's equivalent to replace `s(x)` with `contains(s, x)`.

__Code__

```scala
def union(s: Set, t: Set): Set = (x: Int) => s(x) || t(x)
def intersect(s: Set, t: Set): Set = (x: Int) => s(x) && t(x)
def diff(s: Set, t: Set): Set = (x: Int) => s(x) && !t(x)
```

* define the function `filter` which selects only the elements of a set that are accepted by a given predicate `p`. The filtered elements are returned as a new set.

__Solution__

The function behaviors the same as the `intersect` function

__Code__

```scala
def filter(s: Set, p: Int => Boolean): Set = intersect(s, p)
```

### 2.2 Queries and Transformations on Sets

* define the function `forall` which tests whether a given predicate is true for all elements of the set.

__Solution__

The function makes use of recursion to iterate over all possible values between the lower bound `-bound` and upper bound `bound`. It should return `false` whenever there is a value which belongs to set `s`, but not follows `p`. If all the values in the range doesn't violate the rule, then it returns `true`.

__Code__

```scala
def forall(s: Set, p: Int => Boolean): Boolean = {
  def iter(a: Int): Boolean = {
    if (a > bound) true
    else if (contains(s, a) && !filter(s, p)(a)) false 
    else iter(a + 1)
  }
  iter(-bound)
}
```

* define a function `exists` using `forall` which tests whether a set contains at least one element for which the given predicate is true. Note that the functions forall and exists behave like the universal and existential quantifiers of first-order logic.

__Solution__

The function should check the complementary of whether all values of a set does not follows `p`. 

__Code__

```scala
def exists(s: Set, p: Int => Boolean): Boolean = !forall(s, (x: Int) => !p(x))
```

* write a function `map` which transforms a given set into another one by applying to each of its elements the given function. 

__Solution__

The function returns a new `Set s'`, a function `(y: Int) => ???` which returns `true` if there exists a value `x` in `Set s` such that `y' = f(x)`. Thus we use `exists` function with parameter `s` and an anonymous function `(x: Int) => f(x) == y` where `y` is the parameter of the returning anonymous function.

__Code__

```scala
def map(s: Set, f: Int => Int): Set = (y: Int) => exists(s, (x: Int) => f(x) == y)
```

### Test Cases

* test case for function `contains`

```scala
test("contains is implemented") {
  assert(contains(x => true, 100))
}
```

* put the shared values into a separate trait (traits are like abstract classes), and create an instance inside each test method.

```scala
trait TestSets {
  val s1 = singletonSet(1)
  val s2 = singletonSet(2)
  val s3 = singletonSet(3)
}
```

* use `ignore` to diable test, and exchange `ignore` by `test` to enable test

```scala
ignore("Test Case") {
}
```

* test cases for function `union`

```scala
test("union contains all elements") {
  new TestSets {
    val s = union(s1, s2)
    assert(contains(s, 1), "Union 1")
    assert(contains(s, 2), "Union 2")
    assert(!contains(s, 3), "Union 3")
  }
}
```
