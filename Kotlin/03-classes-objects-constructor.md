## Classes and Objects

### Classes

- The class declaration consist of the following:
	- Name
	- Header - *Parameter types or Primary Constructor*
	- Body

### Kotlin has two types of constructors
1. Primary Constructor
2. Secondary Constructor

- A class in Kotlin can have a primary constructor and one or more secondary constructors. The primary constructor is a part of the class header, and it goes after the class name.
- It's not mandatory to have primary constructor. A class can also have secondary constructors and no primary constructor.

#### Primary Constructor
- If the primary constructor does not have any annotations or visibility modifiers, the constructor keyword can be removed.
- This constructor cannot contain runnable code. If you want to run a code during object creation use the `init` block
- During the initialisation of an instance, the initializer blocks `init` are executed in the same order as they appear in the class body.
```
class InitOrderDemo(name: String) {
    val firstProperty = "First property: $name".also(::println)
    
    init {
        println("First initializer block that prints $name")
    }
    
    val secondProperty = "Second property: ${name.length}".also(::println)
    
    init {
        println("Second initializer block that prints ${name.length}")
    }
}

Output:
First property: hello
First initializer block that prints hello 
Second property: 5 
Second initializer block that prints 5
```

- In kotlin, we have `init` block it is used to further perform initializations or add any code before the program starts.
- We can also assign a default value into the constructor's parameters.
- Properties declared in the primary constructor can be mutable (var) or read-only (val).
- If the constructor has annotations or visibility modifiers, the `constructor` keyword is required and the modifiers go before it.
````
class Customer public @Inject constructor(name: String) { /*...*/ }
````

#### Secondary Constructor
- A class can also declare secondary constructors, with a `constructor` keyword
  ```
  class Pet {
    constructor(owner: Person) {
        owner.pets.add(this) // adds this pet to the list of its owner's pets
    }
  }
  ```

- If a class has a primary constructor, each secondary constructor needs to invoke the primary constructor by using the `this` keyword.
  ```
  class Person(val name: String) {
    val children: MutableList<Person> = mutableListOf()
    constructor(name: String, parent: Person) : this(name) {
        parent.children.add(this)
    }
  }
  ```

- The code inside `init` blocks and property initializer are executed before the secondary constructor. As we know that `init` block is executed during object creation and object creation is done using primary constructor. In short `init` block become the part of primary constructor and it is executed before secondary constructor. 
- Even if the class has no primary constructor, the delegation still happens implicitly, and the initializer blocks are still executed.

- If a non-abstract or concrete class does not declare any constructors (primary or secondary), it will have a generated primary constructor with no arguments. The visibility of the constructor will be public.
- If you don't want your class to have a public constructor, declare an empty primary constructor with private modifier.
  ```
  class DontCreateMe private constructor() { /*...*/ }
  ```
- Kotlin does not have a `new` keyword.
- A class can have one or more initializer `init` block

#### Class Members
- Constructor and initializer blocks
- Functions
- Properties
- Nested and inner classes
- Object Declarations
