# Kotlin 
By default classes & methods are "public final" in nature.

### Array 
```Kotlin
val arr = Array(3) { "" }  // equivalent to: arrayOf("","","")
or
val arr = Array<String?>(5) {null}
or
var arr = emptyArray<String>()
```

### Visibility Modifier
>private      : Declarations are not visible outside the current scope\
protected     : Declarations that are protected in a class, can be accessed only in their subclasses\
internal      : Declarations are visible everywhere in the same module\
public        : Declaration is visible everywhere

### Loops
```Kotlin
if (x in 1..y + 1) {
   // Code goes here
}
```
```Kotlin
if (-1 !in 0..list.lastIndex) {
   // Code goes here
}
```
```Kotlin
for (item in items) {
   // Code goes here
}
```
```Kotlin
for (index in items.indices) {
   // Code goes here
}
```
```Kotlin
for (x in 1..5) {
   // Code goes here
}
```
```Kotlin
for (x in 1..10 step 2) {
   // Code goes here
}
```
```Kotlin
for (x in 9 downTo 0 step 3) {
   // Code goes here
}
```
```Kotlin
for (i in 1 until 10) {
   // Code goes here
}
```
### When (Switch statements in Kotlin)
```Kotlin
when (obj) {
  1          -> "One"
  "Hello"    -> "Greeting"
  is Long    -> "Long"
  !is String -> "Not a string"
  else       -> "Unknown"
}
```
### Constructors
```Kotlin
class Customer(name: String, val type: String)
```
- Here **name** is just parameter and can't be used outside the method scope.
- **type** is a declared variable and can be used anywhere in the class scope.

```Kotlin
class Customer constructor(name: String) {
    constructor(type: Int): this(name) {}
    constructor(type: HashMap): this(name) {}
    constructor(type: ArrayList): this(name) {}

    init{}
}
```

The above code implies **Customer** class;
- can have multiple secondary constructors.
- **constructor** is not compulsory to be written.
- **this(name)** needs be to called if the primary constructor exists.


### Interface 
>All methods & properties are public open, but not final
```Kotlin
interface One {
  fun click()
}
```
```Kotlin
interface Two {
  fun click()
}
```
```Kotlin
class Implementation : One, Two {

  // compulsory to overide coz of ambiguity
  click() {
    super<One>.click()
    super<Two>.click()
  }
}
```

### Operators
>For regular class C1, C2\
c1 == c2 // Checks reference

>For data class D1, D2\
d1 == d2 // Compares the values within the class 


### Variances
##### Consider below class structure
```Kotlin
open class GrandParent { }

open class Parent : GrandParent { }

class BabyChild : Parent { }
```
##### Covariance
```Kotlin
// Assuming `TaxPayer<Parent>` is covariant
fun calculateHasNoGrandChildTax(taxPayer TaxPayer<Parent>) { 
  //some interesting calculations for only a Parent and a BabyChild
}

// Usage:
fun main(args: Array<String>) {
  // This will NOT work. Only exact type or subtypes are allowed
  calculateHasNoGrandChildTax(TaxPayer<GrandParent>())
  
  // This will work. It's an exact type.
  calculateHasNoGrandChildTax(TaxPayer<Parent>())

  // This will work. It's a subtype of `Parent`
  calculateHasNoGrandChildTax(TaxPayer<BabyChild>())
}
```

##### Contravariance
```Kotlin
// Assuming `Adult<Parent>` is contravariant
fun drinkBeer(adult: Adult<Parent>) {
  //beer?.drink()
}

// Usage:
fun main(args: Array<String>) {
  // This will work. It's a supertype of `Parent`
  drinkBeer(Adult<GrandParent>())
  
  // This will work. It's an exact type.
  drinkBeer(Adult<Parent>())

  // This will NOT work. Only exact type or supertypes are allowed
  drinkBeer(Adult<BabyChild>())
}
```
##### Invariance
```Kotlin
// Assuming `Customer<Parent>` is invariant.
fun useVoucher(customer: Customer<Parent>) {
  //oddlySpecificVoucher.use(customer)
}

// Usage:
fun main(args: Array<String>) {
  // This will NOT work. Only exact types are allowed.
  useVoucher(Customer<GrandParent>())
  
  // This will work. It's an exact type.
  useVoucher(Customer<Parent>())

  // This will NOT work. Only exact types are allowed.
  useVoucher(Customer<BabyChild>())
}
```
##### Bivariance
```Kotlin
// Assuming `TaxPayer<Parent>` is bivariant.
fun haveFun(person: Person<Parent>) {
  //Everyone can have fun!
}

// Usage:
fun main(args: Array<String>) {
  // This will work. It's an supertype of `Parent`.
  haveFun(Person<GrandParent>())
  
  // This will work. It's an exact type.
  haveFun(Person<Parent>())

  // This will work. It's a subtype of `Parent`
  haveFun(Person<BabyChild>())
}
```
### Object of Anonymous class
```Kotlin
setClickListener( object: ClickListener {
    // Code goes here
})
```
```Kotlin
val adhoc = object {
  var x: Int = 0
  var y: Int = 0
}

print(adhoc.x + adhoc.y)
```

### High Order Functions
Functions accepting or returning lambda functions as parameters
```Kotlin
fun display(value: Int, body: () -> Unit) {
  if(value == 10) {
     body()
  }
}
```
Above function can be used as;
```Kotlin
display(10) {
   println("Hello World!")
}
```

### Closures
Values that can be changed from the lambda functions 

### Predicates
```Kotlin
val nums = listOf(2, 3, 4, 5, 6, 23, 90)
nums.count() // Returns numbers of elements
nums.all { it > 1 } // Returns true if all numbers greater than 1
nums.any { it > 1 } // Returns true if any numbers greater than 1
nums.find { it > 6 } // Returns first number than meets the condition
```

### Type check
>if (obj is String)

### Collections - Mutable vs Immutable
Immutable List
```Kotlin
listOf("one", "two", "one")

setOf("one", "two", "one")
```
vs 

Mutable List
```Kotlin
val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
fruits.filter { it.startsWith("a") }
  .sortedBy { it }
  .map { it.toUpperCase() }
  .forEach { println(it) }
```
>Output: APPLE; AVOCADO

### * vs Any
List<*> vs List<Any>

>List<String>  => will accept only String\
>List<Any>     => will accept any type viz. Int, String, etc

### Singleton
#### Method 1:
```Kotlin
object Printer {
  val id: Int = 1
  fun display()
}
```
##### Usage
>Printer.id\
>Printer.display()

#### Method 2:
```Kotlin
class Printer {
  companion object {
    fun display()
  }
  
  object FakeCompanion {
    fun displayTest()
  }
}
```
##### Calling in Java
>Printer.Companion.display()\
>Printer.FakeCompanion.Instance.displayTest()

##### Singleton & Inheritance 
```Kotlin
open class Client() {
  fun display()
}

object Browser: Client()
```

##### Usage 
>Browser.display()

### Null Safety
>Safe call           ?\
Elvis call          ?:\
Non-null Assertions !!

### Delegation
Delegation can be achieved by using **by** clause
```Kotlin
val p: String by Delegate()
```

### Delegation using By-clause
```Kotlin
interface Base {
  fun print()
}

class BaseImplementation(val x:Int) : Base {
  @overide 
  fun print() = print("hello")
}

class Derived(b: Base) : Base by b 

fun main() {
  val bi = BaseImpl(10)
  Derived(bi).print()
}
```

### Custom Delegate Properties
```Kotlin
class Delegate() {
    operator fun getValue(thisRef: Any?, prop: KProperty<*>): String {
        return "$thisRef, thank you for delegating '${prop.name}' to me!"
    }

    operator fun setValue(thisRef: Any?, prop: KProperty<*>, value: String) {
        println("$value has been assigned to ${prop.name} in $thisRef")
    }
}
```

Delegation Properties
========================================================
by lazy()
```Kotlin
val lazyValue: String by lazy {
    println("computed!")
    "Hello"
}
```

### Modifier 
For non-null variable which is initialized at a later stage 

>lateinit - can only be used on var 
The modifier can only be used on var properties declared inside the body of a class (not in the primary constructor)

### Lazy vs Lateinit
>lazy can only be used on val, is used to save memory;
lateinit can only be used on var, must be initialized before use.

### Scope Functions Kotlin
![Scope functions](https://miro.medium.com/max/2860/1*UW1WkqmUnbJc1QEGzm59jg.png)

### Extension Functions
```Kotlin
fun String.removeFirstLastChar(): String =  this.substring(1, this.length - 1)

fun main(args: Array<String>) {
    val myString= "Hello Everyone"
    val result = myString.removeFirstLastChar()
    println("First character is: $result")
}
```
Inner Class 
========================================================
Java: holding reference of outer class 
```Kotlin
class A {
    class B {
    ...
    }
}
```
Kotlin: holding reference of outer class 
```Kotlin
class A {
    inner class B {
    ...
    }
}
```

vs 

Inner static class 
-------------------------------------------------------
Java
```Kotlin
class A {
    static class B {
    ...
    }
}
```

Kotlin 
```Kotlin
class A {
    class B {
    ...
    }
}
```

