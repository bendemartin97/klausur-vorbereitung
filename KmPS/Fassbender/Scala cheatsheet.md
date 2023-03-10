#### Types

![[Pasted image 20230119115645.png]]

`<Any>` the supertype of all types, also called the top type. It defines certain universal methods such as `equals`, `hashCode`, and `toString`. `Any` has two direct subclasses: `<AnyVal>` and `<AnyRef>`.

`<AnyVal>` represents value types. There are nine predefined value types and they are non-nullable: `Double`, `Float`, `Long`, `Int`, `Short`, `Byte`, `Char`, `Unit`, and `Boolean`. **`Unit`** is a value type which carries no meaningful information. There is exactly one instance of `Unit` which can be declared literally like so: `()`. All functions must return something so sometimes `Unit` is a useful return type.

`<AnyRef>` represents reference types. All non-value types are defined as reference types. Every user-defined type in Scala is a subtype of `<AnyRef>`. If Scala is used in the context of a Java runtime environment, `<AnyRef>` corresponds to `java.lang.Object`.

#### Variables

**Variable**
```Scala
var x = 5
```

**Constant**
```Scala
val x = 5
```

**Explicit type**
```Scala
var x: Double = 5
```


#### Functions

**Define function**
```Scala
def f(x: Int) = { x * x }
```

**Types**
_Good_  
```Scala
def f(x: Any) = println(x)
```

_Bad_  
```Scala
def f(x) = println(x)
```
 
**Type alias**
```Scala
type R = Double
```

**Call-by-value**
```Scala
def f(x: R)
```

_vs.__  

**Call-by-name (lazy parameters)**
```Scala
def f(x: => R)
```

#### Anonymous functions

**Anonymous function**
```Scala
(x: R) => x * x
```

**underscore is positionally matched arg**
```Scala
(1 to 5).map(_ * 2)
```

_vs.__  

```Scala
(1 to 5).reduceLeft(_ + _)
```

**to use an arg twice, have to name it**
```Scala
(1 to 5).map(x => x * x)
```

**block style returns last expression.**
```Scala
(1 to 5).map { x =>
  val y = x * 2
  println(y)
  y
}
```

#### Currying

**obvious syntax**
```Scala
val zscore =
  (mean: R, sd: R) =>
    (x: R) =>
      (x - mean) / sd
```



**obvious syntax**
```Scala
def zscore(mean: R, sd: R) =
  (x: R) =>
    (x - mean) / sd
```

**sugar syntax**
```Scala
def zscore(mean: R, sd: R)(x: R) =
  (x - mean) / sd
```
_call_
```Scala
val normer =
  zscore(7, 0.4) _
```
 
#### Generic type.
```Scala
def mapmake[T](g: T => T)(seq: List[T]) =
  seq.map(g)
```

#### Data structures

**Tuple literal (`Tuple3`)**
```undefined
(1, 2, 3)
```


**Destructuring bind: tuple unpacking via pattern matching** 
```scala
var (x, y, z) = (1, 2, 3)
```

_Bad -> Hidden error: each assigned to the entire tuple._  
```scala
var x, y, z = (1, 2, 3)
```

**List (immutable).**
```scala
var xs = List(1, 2, 3)
xs(2)
```

**Cons.**
```Scala
1 :: List(2, 3)
```


**Range sugar.**
```Scala
1 to 5
```

_same as_

```Scala
1 until 6
```

_same as_

```Scala
1 to 10 by 2
```

#### Control constructs

**Conditional**
```scala
if (check) happy else sad
```

**While loop**
```scala
while (x < 5) {
  println(x)
  x += 1
}
```

**Do-while loop**
```scala
do {
  println(x)
  x += 1
} while (x < 5)
```

**For-loop: iterate including the upper bound**
```Scala
for (i <- 1 to 5) {
  println(i)
}
```

**For-loop: iterate omitting the upper bound**
```Scala
for (i <- 1 until 5) {
  println(i)
}
```

#### Pattern matching

```scala
def matchTest(x: Int): String = x 
match case 1 => "one" 
case 2 => "two" 
case _ => "other" 
matchTest(3) // returns other 
matchTest(1) // returns one
```

