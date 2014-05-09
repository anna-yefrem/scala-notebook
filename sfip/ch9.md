Chapter 9 Files and Regular Expressions
=======================================

### 9.1 Reading Lines

* read all lines from a file

```scala
import scala.io.Source
val source = Source.fromFile("myfile.txt", "UTF-8")
// The first argument can be a string or a java.io.File
// Y ou can omit the encoding if you know that the file uses
// the default platform encoding
```

* process file

```scala
val lineIterator = source.getLines
for (l <- lineIterator) process l

val lines = source.getLines.toArray // process lines as array of String

val contents = source.mkString // process whole file as a string
```

### 9.2 Reading Charactors

* read individual charactor by using a `Source` object which extends Iterator[Char]

```scala
for (c <- source) process c
```

### 9.3 Reading Tokens and Numbers

```scala
val tokens = source.mkString.split("\\s+")

val numbers = for (w <- tokens) yield w.toDouble
  // same as val numbers = tokens.map(_.toDouble)
```

* read from console

```scala
print("How old are you? ")
  // Console is imported by default, so you don’ t need to qualify print and readInt
val age = readInt()
  // Or use readDouble or readLong
```

### 9.4 Reading from URLs and Other Sources

```scala
val source1 = Source.fromURL("http://horstmann.com", "UTF-8")
val source2 = Source.fromString("Hello, World!") 
// Reads from the given string—useful for debugging
val source3 = Source.stdin
// Reads from standard input
```

### 9.5 Reading Binary

```scala
val file = new File(filename)
val in = new FileInputStream(file)
val bytes = new Array[Byte](file.length.toInt)
in.read(bytes)
in.close()
```

### 9.6 Writing Text Files

* use `java.io.PrintWriter` to write a text file

```scala
import java.io.PrintWriter

val out = new PrintWriter("numbers.txt")
for (i <- 1 to 100) out.println(i)
out.close()
```

* use `print` and `format` method to replace `PrintWriter.printf`

```scala
out.print("%6d %10.2f".format(quantity, price))
```

### 9.7 Visiting Directories

* process all files via an iterator through all subdirectories of a directory

```scala
import java.io.File
def subdirs(dir: File): Iterator[File] = {
  val children = dir.listFiles.filter(_.isDirectory)
  children.toIterator ++ children.toIterator.flatMap(subdirs _)
}

for (d <- subdirs(dir)) process d
```

### 9.8 Serialization

```scala
@SerialVersionUID(42L) class Person extends Serializable
```

is equivalent to

```java
public class Person implements java.io.Serializable { // Java
  private static final long serialVersionUID = 42L;
  ...
}
```

_TIP_

The Serializabletrait is defined in the  scalapackage and does not require an import

* use serialized object

```scala
val fred = new Person(...)
import java.io._
val out = new ObjectOutputStream(new FileOutputStream("/tmp/test.obj"))
out.writeObject(fred)
out.close()

val in = new ObjectInputStream(new FileInputStream("/tmp/test.obj"))
val savedFred = in.readObject().asInstanceOf[Person]
```

### 9.9 Process Control

The `sys.process` package contains an implicit conversion from strings to `ProcessBuilder` objects.
* The `!` operator _executes_ the `ProcessBuilder` object.
* The `!!` operator _executes_ and return the output as a string

```scala
import sys.process._
"ls -al .." !

val result = "ls -al .." !!
```

```scala
"ls -al .." #| " grep sec" !
  // pipe the output by #|
"ls -al .." #> new File("output.txt") !
  // redirect the output to a file by #>
"ls -al .." #>> new File("output.txt") !
  // append the output to a file by #>>
"grep sec" #< new File("output.txt") !
  // redirect input from file by #<
"grep Scala" #< new URL("http://horstmann.com/index.html") !
  // redirect input from URL by #< and URL object
```

### 9.10 Regular Expressions

* use `r` method of the string class to construct a regular expression object

```scala
val numPattern = "[0-9]+".r
```

* use `"""..."""` to allow _raw_ string syntax for `\` and `"`

```scala
val val wsnumwsPattern = """\s+[0-9]+\s+""".r
  // A bit easier to read than "\\s+[0-9]+\\s+".r
```

* use `findAllIn` to return an iterator through all matches
```scala
for (matchString <- numPattern.findAllIn("99 bottles, 98 bottles"))
process matchString
```

* turn iterator into an array

```scala
val matches = numPattern.findAllIn("99 bottles, 98 bottles").toArray
// Array(99, 98)
```

* check whether the beginning of a string matches, use `findPrefixOf`

```scala
numPattern.findPrefixOf("99 bottles, 98 bottles")
// Some(99)
wsnumwsPattern.findPrefixOf("99 bottles, 98 bottles")
// None
```

* replace the first match, or all matches

```scala
numPattern.replaceFirstIn("99 bottles, 98 bottles", "XX")
// "XX bottles, 98 bottles"
numPattern.replaceAllIn("99 bottles, 98 bottles", "XX")
// "XX bottles, XX bottles"
```

### 9.11 Regular Expression Groups

* use parentheses around subexpression

```scala
val numitemPattern = "([0-9]+)([a-z]+)".r
```

* use the regular expression object as an extractor

```scala
val numitemPattern(num, item) = "99 bottles"
  // Sets numto "99", itemto "bottles"
```

* use `for` expression and extacted groups from multiple expressions
for (numitemPattern(num, item) <- numitemPattern.findAllIn("99 bottles, 98 bottles")) 
process num and item
