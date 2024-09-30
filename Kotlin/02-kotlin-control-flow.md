## Control Flow
### Conditions and Loops

- In Kotlin, the `if` is an expression that returns a value. therefore, there is no ternary operator.
  
  ```
  max = if (a > b) a else b
  ```
  
- If you are using `if` for returninng a value or assigning it to a variable, then the `else' block is mandatory.

#### When Expression
- It has conditions with multiple branches. It is similar to `switch` statement.
- `when` can be used either as an expressions or statement.
- An expression produced a value whereas statement performs an action.
- If it is used as an expression, the value of the first matching branch becomes the value of the overall expression. If it is used as a statement, the values of individual branches are ignored.
- If `when` is used as an expression, the else branch is mandatory uncless the compiler matches all the conditions.
  ```
  enum class Bit {
    ZERO, ONE
  }
  val numericValue = when (getRandomBit()) {
    Bit.ZERO -> 0
    Bit.ONE -> 1
    // 'else' is not required because all cases are covered
  }
  ```
  - Same conditions applies if `when` is used as an statement.
    ```
    enum class Color {
    RED, GREEN, BLUE
    }

    when (getColor()) {
      Color.RED -> println("red")
      Color.GREEN -> println("green")
      Color.BLUE -> println("blue")
      // 'else' is not required because all cases are covered
    }

    when (getColor()) {
      Color.RED -> println("red") // no branches for GREEN and BLUE
      else -> println("not red") // 'else' is required
    }
    ```
- To define a common behavior for multiple cases, combine their conditions in a single line with a comma.
- You can use arbitrary expressions (not only constants) as branch conditions
  ```
  when (x) {
    s.toInt() -> print("s encodes x")
    else -> print("s does not encode x")
  }
  ```
- You can also check a value for being in or !in a range or a collection:
- Another option is checking that a value is or !is of a particular type. Note that, due to smart casts, you can access the methods and properties of the type without any extra checks.
  ````
  fun hasPrefix(x: Any) = when(x) {
    is String -> x.startsWith("prefix")
    else -> false
  }
  ````
- `when` can also be used as a replacement for an if-else if chain. If no argument is supplied, the branch conditions are simply boolean expressions, and a branch is executed when its condition is true:
  ```
  when {
      x.isOdd() -> print("x is odd")
      y.isEven() -> print("y is even")
      else -> print("x+y is odd")
  }
  ```

##### If you want to iterate through an array or a list with an index, you can do it this way:
```
for (i in array.indices) {
    println(array[i])
}
```

```
for (i in array.indices) {
    println(array[i])
}
```

### Returns and Jumps
Kotlin has three structural jump expressions:
- `return` by default returns from the nearest enclosing function or anonymous function.
- `break` terminates the nearest enclosing loop.
- `continue` proceeds to the next step of the nearest enclosing loop.

- Any expression in Kotlin may be marked with a label. Labels have the form of an identifier followed by the @ sign, such as abc@ or fooBar@. To label an expression, just add a label in front of it.
  ````
  loop@ for (i in 1..100) {
      for (j in 1..100) {
          if (...) break@loop
      }
  }
  ````

- In Kotlin, functions can be nested using function literals, local functions, and object expressions. `return` allow us to return from an outer function by using lammda expressionn.
  ```
  fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return // non-local return directly to the caller of foo()
        print(it)
    }
    println("this point is unreachable")
  }
  Output: 12
  ```

  - Explicit Label
  ```
  fun foo() {
    listOf(1, 2, 3, 4, 5).forEach lit@{
        if (it == 3) return@lit // local return to the caller of the lambda - the forEach loop
        print(it)
    }
    print(" done with explicit label")
  }
  Output: 1245 done with explicit label
  ```
  
  - Implicit Label
  ```
  fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return@forEach // local return to the caller of the lambda - the forEach loop
        print(it)
    }
    print(" done with implicit label")
  }
  Output: 1245 done with explicit label
  ```
- Alternatively, you can replace the lambda expression with an anonymous function. A return statement in an anonymous function will return from the anonymous function itself.
  ```
  fun foo() {
    listOf(1, 2, 3, 4, 5).forEach(fun(value: Int) {
        if (value == 3) return  // local return to the caller of the anonymous function - the forEach loop
        print(value)
    })
    print(" done with anonymous function")
  }
  Output: 1245 done with explicit label
  ```

### Exceptions

- All exception classes in Kotlin inherit the Throwable class. Every exception has a message, a stack trace, and an optional cause.
- To throw an exception object, use the throw expression:
  ```
  throw Exception("This is an Exceptionn!")  
  ```
- There may be zero or more catch blocks, and the finally block may be omitted. However, at least one catch or finally block is required.
- try is an expression, which means it can have a return value
- Kotlin does not have checked exceptions
- `throw` is an expression in Kotlin, so you can use it, for example, as part of an Elvis expression:
  ```
  val s = person.name ?: throw IllegalArgumentException("Name required")
  ```
- The throw expression has the type Nothing. This type has no values and is used to mark code locations that can never be reached. 

  
