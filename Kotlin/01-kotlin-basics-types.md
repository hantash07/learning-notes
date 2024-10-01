#### Variables
There are two types of variable in Kotlin.
1. val - Read Only Variable - Immutable
2. var - Mutable Variable - Values can be change

#### String Templates
String Templates is used to convert the data stored in the variable into String. This expressions always start with a dollar sign $.
```
println("There are $customers customers")
//There are 10 customers
```

### Data Types
##### In Kotlin, everything is an object. Each type has it's own member functions and properties.

#### Type Inference
Kotlin has the ability to define the type of a variable. When a variable is assigned a value then the type is defined.

#### Kotlin has the following basic types:
1. Integers  -  `Byte, Short, Int, Long`
2. Unasigned Integers  -  `UByte, UShort, UInt, ULong`
3. Floating Point Number  -  `Float, Double`
4. Boolean  -  `Boolean`
5. Characters  -  `Char`
6. Strings  -  `String`

- When you initialized a variable with no explicit type specification, the compiler automatically infers the type with the smallest range enough to represent the value. If it is not exceeding the range of Int, the type is Int. If it exceeds, the type is Long. To specify the Long value explicitly, append the suffix L to the value. Same method applied for Double and Float.

  ```
  val one = 1 // Int
  val threeBillion = 3000000000 // Long
  val oneLong = 1L // Long
  val oneByte: Byte = 1
  ```

- Unlike some other languages, there are no implicit conversions for numbers in Kotlin. For example, a function with a Double parameter can be called only on Double values, but not Float, Int, or other numeric values:

  ```
  fun main() {
      fun printDouble(d: Double) { print(d) }

      val i = 1
      val d = 1.0
      val f = 1.0f

      printDouble(d)
  //    printDouble(i) // Error: Type mismatch
  //    printDouble(f) // Error: Type mismatch
  }
  ```

#### Kotlin Literal / Constant
Fixed value that does not changed during program execution.

- There are the following kinds of literal constants for integral values:
  1. Decimals - `123`
  2. Longs are tagged by a capital - `L` - `123L`
  3. Hexadecimals - `0x0F`
  4. Binaries - `0b00001011`

- Octal literals are not supported in Kotlin.

- You can use underscores to make number constants more readable:
  ```
  val oneMillion = 1_000_000
  val creditCardNumber = 1234_5678_9012_3456L
  val socialSecurityNumber = 999_99_9999L
  val hexBytes = 0xFF_EC_DE_5E
  val bytes = 0b11010010_01101001_10010100_10010010 
  ```
- On the JVM platform, numbers are stored as primitive types: int, double, and so on. Exceptions are cases when you create a nullable number reference such as Int? or use generics. In these cases numbers are boxed in Java classes Integer, Double, and so on.

#### Explicit Number Conversions
In Kotlin, numbers are not implicitly converted. Smaller types are NOT implicitly converted to bigger types. This means that assigning a value of type Byte to an Int variable requires an explicit conversion:

```
val b: Byte = 1 // OK, literals are checked statically
// val i: Int = b // ERROR
val i1: Int = b.toInt()
```

In many cases, there is no need for explicit conversions because the type is inferred from the context, and arithmetical operations are overloaded for appropriate conversions, for example:
```
val l = 1L + 3 // Long + Int => Long
```

#### Operations on Numbers
Kotlin supports the standard set of arithmetical operations over numbers: `+`, `-`, `*`, `/`, `%`. 

Division between integers numbers always returns an integer number. Any fractional part is discarded.
```
val x = 5 / 2
//println(x == 2.5) // ERROR: Operator '==' cannot be applied to 'Int' and 'Double'
println(x == 2)
```

This is true for a division between any two integer types:
```
val x = 5L / 2
println(x == 2L)
```

To return a floating-point type, explicitly convert one of the arguments to a floating-point type:
```
val x = 5 / 2.toDouble()
println(x == 2.5)
```

#### String
Strings are immutable. Once you initialised a string, you can't change its value or assign a new value to it. All operations that transform strings return their results in a new String object, leaving the original string unchanged:
```
val str = "abcd"
println(str.uppercase()) // Create and print a new String object
println(str) // The original string remains the same
```

Kotlin has two types of String Literals:
1. Escape String 
2. Raw String

##### Escape String
- A string that contain special characters. Escaped characters are those that start with backlash `\`.
- It is used when you need to include special characters in a string.
```
val s = "Hello, world!\n"
```

##### Raw String
- In Kotlin it is created by using triple quote ("""). 
- It ignores the escape string (special characters) and print the string as it is.
- Use this string if you want the string to be printed exactly as written
```
val rawString = """Hello\nWorld! "Kotlin" is awesome."""
println(rawString)

Output: Hello\nWorld! "Kotlin" is awesome.
```

#### Array
Arrays in Kotlin are represented by the Array class. It has get() and set() functions

To create an array, use the function arrayOf() and pass the item values to it, so that arrayOf(1, 2, 3) creates an array [1, 2, 3]. Alternatively, the arrayOfNulls() function can be used to create an array of a given size filled with null elements.

Kotlin also has classes that represent arrays of primitive types without boxing overhead: ByteArray, ShortArray, IntArray, and so on. These classes have no inheritance relation to the Array class, but they have the same set of methods and properties.


#### Type Check and Cast
The operator `is` or `!is` is used to check the type of an object. 

`is` is used for smart cast. Smart casts can work only when the compiler can guarantee that the variable won't change between the check and the usage.
```
fun demo(x: Any) {
    if (x is String) {
        print(x.length) // x is automatically cast to String
    }
}
```

The operator `as` is used for casting

##### Unsafe Cast (as):
If the cast is not possible the cast operator throws an exceptions.

##### Safe Cast (as?)
To avoid exceptions, use the safe cast operator `as?`, which returns `null` on failure.