## Functions
- Kotlin functions are declared using the `fun` keyword:
```
fun double(x: Int): Int {
	return 2 * x 
}
```

### Parameters
- Function parameters are defined using Pascal notation - name: type.
- Parameters are separated using commas, and each parameter must be explicitly typed:
```
fun powerOf(number: Int, exponent: Int): Int { /*...*/ }
```

### Default Arguments
- Function parameters can have default values, these values are used when you skipped passing values into function arguments.
```
fun read( b: ByteArray, off: Int = 0, len: Int = b.size, ) { /*...*/ }
```
- When overriding a functions that has default parameter values, the default parameter values must be removed from the signature. The default values cannot be redefined because it's not the part of a function signature. In short if the default values are redefined, the compiler cannot decide which will be the default values of the parameters.
```
open class A {
	open fun foo(i: Int = 10) {/*...*/ }
} 
	
class B : A() {
	override fun foo(i: Int) { /*...*/ } // No default value is allowed.
}
```
### Named Arguments
- Adding names into the function parameters so that it can be used while calling a function with arguments.
- This can be helpful when a function has many arguments and it's difficult to associate a value with an argument.
- When you used named arguments, you can change the order of the arguments while calling a functions.
- If the named arguments has a default value, you can skip this type of argument.

### Unit-Returning Functions
- Unit is the return type of a function with no return type.
```
fun printHello(name: String?): Unit {
	if (name != null)
		println("Hello $name")
	else
		println("Hi there!") // `return Unit` or `return` is optional 
}
```

### Single-Expression Functions
- When a function body has single expressions, then the curly braces can be removed and the single expressions will be assigned into the function using `=` symbol.
```
fun double(x: Int): Int = x * 2
```
- Explicitly declaring the return type is optional when this can be inferred by the compiler:
```
fun double(x: Int) = x * 2
```

### Explicit return types
- Functions with block body must always specify return types explicitly.
- Kotlin does not infer return types for functions with block bodies because such functions may have complex control flow in the body.

### Variable number of arguments (varargs)
- If the number of argument is not defined or the arguments are dynamic then `varargs` is used.
- It is useful for functions where the exact number of arguments is not known at compile time.
- You can declare a `vararg` parameter by prefixing the parameter name with the `vararg` keyword:
```
fun greetAll(vararg names: String) {
    for (name in names) {
        println("Hello, $name!")
    }
}

Output:
greetAll("Alice", "Bob", "Charlie")
// Output:
// Hello, Alice!
// Hello, Bob!
// Hello, Charlie!
```

- If you already have an array, you can pass it using the **spread operator (`*`)**:
```
val names = arrayOf("David", "Eve")
greetAll(*names)
// Output:
// Hello, David!
// Hello, Eve!
```

- A function can only have one `varargs` parameter. 
- `varargs` must be the last parameter if there are other parameters.
- You can call a function with `varargs` without passing value in the argument
```
greetAll()
// No output since no names were provided
```
- Inside function, `varargs` parameter behaves like an array. You can access its element using indexing.
- Array must use spread operator(`*`) to pass an array into the function with `varargs` parameter.

### Infix notation
- Special kind of function in kotlin that can be called without parentheses or dots.
- Restrictions on `infix` function:
	- The function must be member of a class or extension function.
	- The function must have exactly one parameter.
	- It cannot be used with `vavarg`, default values or multiple variables.
- Example of an Infix Function:
```
infix fun Int.times(str: String): String {
    return str.repeat(this)
}

fun main() {
    // Using the infix notation
    println(3 times "Hi ")
    // Using the standard method call
    println(3.times("Hi "))
}


Output:
Hi Hi Hi 
Hi Hi Hi 
```

#### Using Infix Functions
- Arithmetic operations.
- Custom operations.
- Logical constructs.
```
class Point(val x: Int, val y: Int) {
    infix fun moveBy(dx: Int): Point {
        return Point(x + dx, y)
    }
}

fun main() {
    val point = Point(1, 2)
    val movedPoint = point moveBy 3 // Infix notation
    println("Moved point: (${movedPoint.x}, ${movedPoint.y})")
}

Output:
Moved point: (4, 2)
```

#### Built-in Infix Functions in Kotlin
- **`to` (for creating pairs)**:
```
val pair = "key" to "value" // Equivalent to Pair("key", "value")
println(pair)
```
- **Set operations**:
```
val set = setOf(1, 2, 3)
println(2 in set) // true (uses `in` as an infix function)
```
- **`shl`, `shr`, etc. (bitwise operators)**:
```
val shifted = 1 shl 2 // Equivalent to 1.shl(2)
println(shifted) // Output: 4
```

### Function scope
- Kotlin functions can be declared at the top level in the file, you do not need to create a class to hold a top level function.
- Kotlin functions can also be declared locally inside the function's member and extension functions.

#### Local functions
- Functions inside the other functions. 
- Kotlin supports this type of functions.
- Local functions can access the local variable of the outer function.
```
fun dfs(graph: Graph) {
	val visited = HashSet<Vertex>()
	fun dfs(current: Vertex) {
		if (!visited.add(current)) return
		for (v in current.neighbors)
			dfs(v)
	}
	dfs(graph.vertices[0])
}
```

#### Member functions
- A member function is function that is defined inside a class or an object.

### Generic Functions
- In kotlin, a generic function is a function that can operate with different data types.
- It ensures type safety.
- In order to define generic function, use type parameters with angle brackets (`<>`) before the function's name.
- Many collection functions like `filter`, `map`, and `sort` are generic.
```
fun <T> genericFunction(item: T): T {
    println("Item: $item")
    return item
}

fun main() {
    genericFunction(42)      // Item: 42
    genericFunction("Hello") // Item: Hello
    genericFunction(3.14)    // Item: 3.14
}
```
- `T` is a type parameters that can be replaced with any type.
- The type is defined when the value is passed on function's arguments.

#### Multiple Type Parameters
- One or more type parameters can be defined by separating them with comma.
```
fun <K, V> printKeyValue(key: K, value: V) {
    println("Key: $key, Value: $value")
}

fun main() {
    printKeyValue(1, "One")       // Key: 1, Value: One
    printKeyValue("Name", "John") // Key: Name, Value: John
}
```

#### Type Constraints
- You can restrict the type of a generic function using type constraints. This is done using `: Type` syntax.
```
fun <T : Number> addNumbers(a: T, b: T): T {
    return (a.toDouble() + b.toDouble()) as T
}

fun main() {
    println(addNumbers(5, 10))       // Output: 15
    println(addNumbers(5.5, 3.3))   // Output: 8.8
    // addNumbers("5", "10") // Error: Type mismatch
}
```

#### Generic Extension Functions
- You can define extension functions with generic parameters.
```
fun <T> List<T>.printAll() {
    for (item in this) {
        println(item)
    }
}

fun main() {
    val list = listOf(1, 2, 3)
    list.printAll() // Output: 1, 2, 3
}
```

#### Generic Functions with Nullable Types
- Generic functions can work with nullable types.
```
fun <T> printIfNotNull(item: T?) {
    item?.let { println("Item: $it") } ?: println("Item is null")
}

fun main() {
    printIfNotNull("Hello") // Output: Item: Hello
    printIfNotNull(null)    // Output: Item is null
}
```

### Tail Recursive Functions
- In Kotlin, **tail recursive functions** are functions that use **tail recursion** to optimize recursive calls. Tail recursion allows recursive functions to execute efficiently by avoiding the buildup of stack frames, which can lead to a stack overflow for deeply recursive calls.
```
tailrec fun factorial(n: Int, accumulator: Int = 1): Int {
    return if (n <= 1) accumulator
    else factorial(n - 1, n * accumulator)
}

fun main() {
    println(factorial(5)) // Output: 120
}
```

