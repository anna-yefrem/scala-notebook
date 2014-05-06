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

Do this exercise by implementing the pascal function in Main.scala, which takes a column c and a row r, counting from 0 and returns the number at that spot in the triangle. 

	def pascal(c: Int, r: Int): Int

**Example**

	pascal(0,2)
	  // res0: Int = 1
	pascal(1,2)
	  // res1: Int = 2
	pascal(1,3)
	  // res2: Int = 3

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
