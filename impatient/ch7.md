Chapter 7 Packages and Imports
==============================

### 7.5 Package Objects

* define a package object in the _parent_ package

```scala
package object people {
  val defaultName = "John Q. Public"
}

package people {
  class Person {
    var name = defaultName // Aconstant from the package
  }
  ... 
}
```

### 7.6 Package Visibility

* use qualifiers to make method visible in its own package

```scala
package people {
  class Person {
    private[people] def description = "A person with name " + name
    ...
  }
}
```

### 7.7 Imports

* import all members of a package

```scala
import java.awt._
```

* import all members of a class or object

```scala
import java.awt.Color._
val c1 = RED // Color.RED
val c2 = decode("#ff0000") // Color.decod
```

### 7.8 Imports can Be Anywhere

* import inside any scope

```scala
class Manager { 
  import scala.collection.mutable._
  val subordinates = new ArrayBuffer[Employee]
  ...
}
```

### 7.9 Renaming and Hiding Members

* use a _selector_ to import few members from a package

```scala
import java.awt.{Color, Font}
```

* use a _selector_ to rename members

```scala
import java.util.{HashMap => JavaHashMap} // JavaHashMap as util.Hashmap
import scala.collection.mutable._ // HashMap as mutable.HashMap
```

* use `=> _` to hide a member

```scala
import java.util.{HashMap => _, _} // hides java.util.HashMap
import scala.collection.mutable._ // HashMap as mutable.HashMap
```

### 7.10 Implicit Import

Every Scala program implicitly start with

```scala
import java.lang._ 
import scala._ 
import Predef._
```

* import scala packages with shortcut

```scala
import collection.mutable.HashMap // same as scala.collection.mutable.HashMap
```
