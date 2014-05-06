Week 1 Functions & Evaluations: Recursion
=========================================

### Exercise 1: Pascal's Triangle

**Instruction**

The following pattern of numbers is called Pascal’s triangle.

```
    1
   1 1
  1 2 1
 1 3 3 1
1 4 6 4 1
   ...
```

The numbers at the edge of the triangle are all 1, and each number inside the triangle is the sum of the two numbers above it. Write a function that computes the elements of Pascals triangle by means of a recursive process.

Do this exercise by implementing the `pascal` function in `Main.scala`, which takes a column `c` and a row `r`, counting from 0 and returns the number at that spot in the triangle. 

	def pascal(c: Int, r: Int): Int

**Example**

	pascal(0,2)
	  // res0: Int = 1
	pascal(1,2)
	  // res1: Int = 2
	pascal(1,3)
	  // res2: Int = 3

**Solution**

The `pascal` function is a recursive one which the value of column `c` and row `r` is the sum of two nubmers at column `c-1` and `c` of row `r-1`. The termination condition is given by the number `1` at left or right edge, of column `0` and `r`, respectively and returns 1. The numbers of column `-1` and `r+1` will be ignored by returning 0.

**Code**

```scala
/**
 * Exercise 1
 */
def pascal(c: Int, r: Int): Int = {
  if (c == 0 || c == r) 1
  else if (c < 0 || c > r) 0
  else pascal(c - 1, r - 1) + pascal(c, r - 1)
}
```


### Exercise 2: Parentheses Balancing

**Instruction**

Write a recursive function which verifies the balancing of parentheses in a string

	def balance(chars: List[Char]): Boolean

**Example**

	balance(":-)".toList)
	  // res0: Boolean = false
	balance("())(".toList) = false
	  // res1: Boolean = false

**Solution**

The `balance` function takes a list of `Char` and output if the parentheses are equal. It uses a recursive function to count the unbalanced number `pCount` of `(` and that in the remaining part. The termination conditions are

1. found `)` with no prior `(`
2. no remaining part with `pCount` = 0
3. no remaining part with `pCount` != 0.
4. in other cases, increase `pCount` where the first `Char` in remaining part is `(`, or decrease at `)`. Simply use the `tail` as remaining if the first `Char` is neither `(` or `)`.

**Code**
	
```scala
/**
 * Exercise 2
 */
def balance(chars: List[Char]): Boolean = {
  def balanceIter(part: List[Char], pCount: Int): Boolean = {
    if (pCount < 0) false
    else if (part.isEmpty) { if (pCount == 0 ) true else false}
    else if (part.head == '(') balanceIter(part.tail, pCount + 1)
    else if (part.head == ')') balanceIter(part.tail, pCount - 1)
    else balanceIter(part.tail, pCount)
  }
  balanceIter(chars, 0)
}
```

### Exercise 3: Counting Change

**Instruction**

Write a recursive function that counts how many different ways you can make change for an amount, given a list of coin denominations.

	def countChange(money: Int, coins: List[Int]): Int
	
**Example**

	countChange(4, List(1, 2))
	  // res0: Int = 3

**Solution**

The `countChange` function uses a recursive function to count the combinations of denominations for the remaining money for change. It takes two inputs `residue` of remaining money to change and `demon` of possible coins to choose from, and terminates at `residue` equal 0, which means the combination gives one possible solution, and at `residue` small than 0, which means the combination is too big for the residue, or when `residue` is large than 0 while there is no denominations to choose from, which terminates at null demonination. For recursion, it returns the sum of the counts of `residue` subtracting the first denomination using the full list of denominations, and the counts of `residue` using the rest of the denominations apart from the first.

For example, the combinations of countChange(4, List(1, 2)) can be:
	countChange(4, List(1, 2))
	countIter(3, List(1, 2)) + countIter(4, List(2))
	countIter(2, List(1, 2)) + countIter(3, List(2)) + countIter(2, List(2)) + countIter(4, List())
	countIter(1, List(1, 2)) + countIter(2, List(2)) + countIter(1, List(2)) + countIter(3, List()) + countIter(0, List(2)) + countIter(2, List()) + 0
	...

**Code**

```scala
/**
 * Exercise 3
 */
def countChange(money: Int, coins: List[Int]): Int = {
  def countIter(residue: Int, denom: List[Int]): Int = {
    if (residue == 0) 1
    else if (residue < 0) 0
    else if (denom.isEmpty) 0
    else countIter(residue - denom.head, denom) + countIter(residue, denom.tail)
  }
  countIter(money, coins)
}
```

### Conclusion

Recusive functions should define terminations conditions for all its parameters. The recusive part will then update the parameters for the remaining of the choices of parameters.

In abstract, recusive function look like:

```scala
def func(input: Any*) = {
  def iter(param: Any*) = {
    if (termCond1) 1
    else if (termCond2) 0
    else iter(updateParam(param))
  }
  
  def termCond1(param: Any*): Boolean = {
    // Code
  }
  
  def termCond2(param: Any*): Boolean = {
    // Code
  }
  
  def updateParam(param: Any*) = {
    // Code
  }
  
  iter(input)
}
