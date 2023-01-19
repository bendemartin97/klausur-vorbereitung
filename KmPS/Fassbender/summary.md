# 💣 Important

## Scala

### Tail-rekursion
Tail-Rekursion ist eine Form der Rekursion, bei der der **rekursive Aufruf die letzte von der Funktion ausgeführte Operation ist**. Bei tail-recursiven Funktionen ist der rekursive Aufruf nicht in andere Ausdrücke eingebettet, so dass der Compiler die Funktion optimieren kann, indem er den rekursiven Aufruf durch eine Schleife ersetzt. 
**Dies kann effizienter sein** als die Standardrekursion, da 
man einen __konstanten Platzaufwand__ benötigt, weil durch die Tail-Rekursion immer der gleiche Stack-Frame verwendet werden kann.

#### Example 1
Fakultät einer gegebenen Zahl:
```Scala
def factorial(n: Int): Int = { 
	@tailrec 
	def factorialHelper(n: Int, acc: Int): Int = { 
		if (n <= 1) acc 
		else factorialHelper(n-1, n*acc) 
	} 
	factorialHelper(n, 1) }
```

#### Example 2
Summe der ersten n natürlichen Zahlen an
```Scala
def sumfirstn(n:Int) : Int = sumfirstnTR(n,0) 
def sumfirstnTR(n:Int,res:Int) : Int = 
	if (n==0) res else sumfirstnTR(n-1,res+n)
```

### Auswertungsstrategien
![[Pasted image 20230119123459.png]]

#### Call-by-Value 
berechnet den Wert des Argumentausdrucks **vor dem Aufruf der Funktion**. Wenn derselbe Ausdruck mehrmals in einer Funktion verwendet wird, wird er **jedes Mal neu berechnet**. Dies führt dazu, dass die Ausführung des Programms möglicherweise **länger dauert**, da der Ausdruck mehrmals berechnet wird, aber sicherstellt, dass der Wert des Ausdrucks innerhalb der Funktion immer **gleich bleibt.**

#### Call-by-Name 
berechnet den Wert des Argumentausdrucks erst, **wenn er innerhalb der Funktion verwendet wird**. Wenn derselbe Ausdruck mehrmals in einer Funktion verwendet wird, wird er **nur einmal berechnet.** Dies führt dazu, dass die Ausführung des Programms **schneller** sein kann, da der Ausdruck nur einmal berechnet wird, aber es besteht die Möglichkeit, dass der Wert des Ausdrucks innerhalb der Funktion **variiert**.
_Left-most_ -> linker Ausdruck zuerst
_Right-most_ -> rechter Ausdruck zuert


### Currying
Currying ist eine Technik in der Funktionale Programmierung, bei der eine Funktion, die **mehrere Argumente nimmt, in eine Kette von Funktionen umgewandelt wird, die jeweils nur ein Argument entgegennehmen.** Jede dieser Funktionen gibt eine neue Funktion zurück, die das nächste Argument erwartet, bis alle Argumente verarbeitet wurden und das endgültige Ergebnis zurückgegeben wird.

``` Scala
def multiply(x: Int)(y: Int): Int = x * y
val result = multiply(5)(6)
```


### Higher-Order-Funtions
#### Map
wendet eine **Funktion** auf jedes Element der Liste an und speichert es in neuer Liste.

**auf IntegerListen in Scala-Notation.**
```Scala
def map(list: List[Int], func: Int => Int) : List[Int]
```

#### Filter
wendet eine bestimmte **Bedingung** auf jedes Element der Liste an und speichert nur die Elemente, welche die Bedingung erfüllen in einer neuen Liste.

##### Example 1
**auf IntegerListen in Scala-Notation.**
```Scala
def filter(list: List[Int], p: Int => Boolean): List[Int] = { 
list match { 
case Nil => Nil 
case head :: tail => 
					if (p(head)) head :: filter(tail, p) 
					else filter(tail, p) 
			} 
}
```
In diesem Beispiel wird die Liste mittels pattern matching untersucht. Wenn die Liste leer ist, wird eine leere Liste zurückgegeben. Wenn die Liste nicht leer ist, wird das Prädikat (p) auf den Kopf der Liste angewendet (head). Falls das Prädikat true ist, wird der Kopf der Liste in eine neue Liste aufgenommen, die durch den rekursiven Aufruf der Funktion filter auf den Rest der Liste (tail) gebildet wird. Falls das Prädikat false ist, wird der rekursive Aufruf der Funktion filter nur auf den Rest der Liste ausgeführt.

#### Reduce

``` Scala
val numbers = List(1, 2, 3, 4, 5) 
val sum = numbers.reduce((x, y) => x + y) 
println(sum) // Prints 15
```
In diesem Beispiel wird die Methode reduce mit der Liste numbers aufgerufen und erhält eine Binärfunktion als Argument. Die binäre Funktion nimmt zwei ganze Zahlen, x und y, und gibt deren Summe zurück. Die Methode reduce wendet diese Funktion auf die Elemente der Liste an, **beginnend mit den ersten beiden Elementen, dann das Ergebnis und das nächste Element usw., bis sie beim letzten Element angelangt ist.**


### Lazy Evaluation
Lazy evaluation is a feature that allows certain **expressions to be evaluated only when their values are needed.** This can help to improve performance and reduce memory usage by avoiding unnecessary computation.



## Übungen

### Übung 1
### Übung 2
### Übung 3
### Übung 4
### Übung 5
### Übung 6