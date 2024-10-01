## Classes and Objects

### Classes

- The class declaration consists of the class name, the class header (specifying its type parameters, the primary constructor, and some other things), and the class body surrounded by curly braces. Both the header and the body are optional; if the class has no body, the curly braces can be omitted.

#### Kotlin has two types of constructors
1. Primary Constructor
2. Secondary Constructor

- A class in Kotlin can have a primary constructor and one or more secondary constructors. The primary constructor is a part of the class header, and it goes after the class name.
- It's not mandatory to have primary constructor. A classs can also have secondary contructors and no primmary constructor.

#### Primmary Constructor
- If the primary constructor does not have any annotations or visibility modifiers, the constructor keyword can be omitted.
  ```
  class Person(firstName: String) { /*...*/ }
  ```
- The primary constructor cannot contain any code. Initialization code can be placed in initializer blocks prefixed with the `init` keyword. It can only contain parameters,
- During the initialization of an instance, the initializer blocks `init` are executed in the same order as they appear in the class body.
- Primary constructor parameters can be used in the initializer blocks. They can also be used in property initializers declared in the class body.
- In kotlin, we have init block it is used to further perform initializations or add any code before the program starts.
- We can also assign a defaulft value into the constructor's parameters.
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
- If a class has a primary constructor, each secondary constructor needs to delegate or invoke the primary constructor either implicitly or explicitly.
- Delegation to another constructor of the same class is done using the `this` keyword.
  ```
  class Person(val name: String) {
    val children: MutableList<Person> = mutableListOf()
    constructor(name: String, parent: Person) : this(name) {
        parent.children.add(this)
    }
  }
  ```
- Code in initializer blocks effectively becomes part of the primary constructor. The code in all initializer blocks and property initializers is executed before the body of the secondary constructor.
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
- Functionns
- Properties
- Nested and inner classes
- Object Declarationns


### Inheritance

- All classes in kotlin are derived from a common superclass `any`.
- A class `any` has three methods `equals()`, `hashCode()`, and `toString()`. Thus, these methods are defined for all Kotlin classes.
- By default, Kotlin classes are final they can't be inherited. To make a class inheritable, mark it with the `open` keyword.
- If the derived class has a primary constructor, the base class must be initialized first in that sub class's primary constructor.
- If the derived class has no primary constructor, each of its secondary constructor has to initialize the base class constructor by using the `super` keyword. In this case the derived class secondary constructor can call different constructors of a base class.
  ```
  class MyView : View {
    constructor(ctx: Context) : super(ctx)

    constructor(ctx: Context, attrs: AttributeSet) : super(ctx, attrs)
  }
  ```

#### Overriding methods
- Kotlin requires explicit modifier for all overridable members.
- In order, to override a function from base class, that function must be `open` and the derived class must explicitly define the override modifier.
- If the modifier is not define explicitly the compiler will complain.
- Declaring a method with same signature is not allowed in sub class or derived class.
- Final class is a class wothout `open` modifier.
- The `open` modifier has no effect when added to members of a final class.
- A member marked with `override` is a `open` itself. So it may be override in to further subclasses. In order to stop further re-overriding use `final` keyword.
  ```
  open class Rectangle() : Shape() {
    final override fun draw() { /*...*/ }
  }
  ```

#### Override Properties
- The overriding also works with properties the same way it works with the methods.
- Properties declared on a superclass that are then redeclared on a derived class must be prefaced with `override`
  ```
  open class Shape {
    open val vertexCount: Int = 0
  }

  class Rectangle : Shape() {
    override val vertexCount = 4
  }
  ```
- You can also override a `val` property with a `var` property, but not vice versa. This is allowed because a `val` property essentially declares a get method, and overriding it as a var additionally declares a set method in the derived class.
- Note that you can use the override keyword as part of the property declaration in a primary constructor.

- During the construction of a new instance of a derived class, the base class initialization is done as the first step which means that it happens before the initialization logic of the derived class is run.
-  When the base class constructor is executed, the properties declared or overridden in the derived class have not yet been initialized.

#### Calling the superclass implementation
- A derived class can call it's super class members by using `super` keyword.
- If a derived class has a inner class, and that inner class wants to access the members of supper class, then a `super` keyword with superclass name is used like `super@superClassName`

#### Overriding rules
- If a class inherit multiple implementation of a same member, the derived class must override the member and provide the same implementationn. Mutiple implementation means a class derived from a class and implement an interface.
- If a class inherit multiple implementation of a same member, in order to class the base class override method is to define the class name with `super` keyword `super<ClassName>.member`. 
  ```
  open class Rectangle {
    open fun draw() { /* ... */ }
  }

  interface Polygon {
      fun draw() { /* ... */ } // interface members are 'open' by default
  }

  class Square() : Rectangle(), Polygon {
    // The compiler requires draw() to be overridden:
    override fun draw() {
        super<Rectangle>.draw() // call to Rectangle.draw()
        super<Polygon>.draw() // call to Polygon.draw()
    }
  }
  ```
  
### Properties
- The full syntax for declaring a property is as follows.
- The initializer, getter, and setter are optional. The property type is optional if it can be inferred from the initializer or the getter's return type
  ```
  var <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
    [<setter>]
  ```
- The full syntax of a read-only property declaration differs from a mutable one in two ways: it starts with val instead of var and does not allow a setter
  ```
  val simple: Int? // has type Int, default getter, must be initialized in constructor
  val inferredType = 1 // has type Int and a default getter
  ```
- You can definne a custom controller or accessors for a property. If you define a custom getter, it will be called everytime you access the property. Same case with setter. If you define custom setter, it will be called everytime you assign a value into it.
  Customm Getter:
  ```
  class Rectangle(val width: Int, val height: Int) {
    val area: Int // property type is optional since it can be inferred from the getter's return type
        get() = this.width * this.height
  }
  ```
  
  You can omit the property type if it can be inferred from the getter:
  ```
  val area get() = this.width * this.height
  ```

  Custom Setter:
  ```
  var stringRepresentation: String
    get() = this.toString()
    set(value) {
        setDataFromString(value) // parses the string and assigns values to other properties
    }
  ```
    - By convention, the name of the setter parameter is value, but you can choose a different name if you prefer.
  
  - If you want to add annotate and or change the visibility of the property controller, you don't need to change its implementation.
  ```
  var setterVisibility: String = "abc"
    private set // the setter is private and has the default implementation

  var setterWithAnnotation: Any? = null
    @Inject set // annotate the setter with Inject
  ```
#### Backing Field
- In kotlin, backing field is used in order to avoid recursive call on the property which can throw stack overflow exception.
- A `field` is only used as a part of a property to hold its value in memory.
- Kotlin generate backing field, when the property uses its default implementations.
  ```
  class User{
    var firstName : String  //backing field generated 
        get() = firstName
        set(value) {firstName = value}
    
   var lastName : String   //backing field generated
        get() = lastName
        set(value) {lastName = value}
  
   val name : String                         //no backing field generated
        get() = "{$firstName $lastName}"    
   
   var address : String = "XYZ"           //^because there is no default
                                            //^implementation of an accessor
                                     
  }
  ```
- In Kotlin, the above code snippet will throw StackOverflow because when we access or set property `firstName` or `lastName` the default accessor will be called. So in Kotlin user.firstName = "value” is same as Java’s user.setFirstName("value").
- So when `set(value) {firstName = "value"}` is called, then a recursive call happens and compiler throws a StackOverflow exception because we are calling setter inside the setter.
- Solution to this problem is to user backing fields. In Kotlin, a backing field can be accessed using 'field' keyword inside accessors.
  ```
  class User{
    var firstName : String  
        get() = field
        set(value) {field = value}
    
   var lastName : String  
        get() = field
        set(value) {field = value}
 
                                     
  }
  ```

#### Backing Properties
- If you don't want to use implicit backinng field, you can use backing properties.
- Backing field is generated by Kotlin, whereas backing properties is generated by user itself.
  ```
  private var _table: Map<String, Int>? = null
  public val table: Map<String, Int>
    get() {
        if (_table == null) {
            _table = HashMap() // Type parameters are inferred
        }
        return _table ?: throw AssertionError("Set to null by another thread")
    }
  ```

#### Compile Time Constant
- If the value of a read-only property is known at compile time, mark it as a compile time constant using the 'const' modifier.
- In order to make property as a compile time constant, property needs to fulfill following requirements.
  - It must be a top-level property, or a member of an `object declaration` or a `companion object`.
  - It must be initialized with a value of type String or a primitive type.
  - It cannot have a custom getter (and, since it’s a constant, it naturally can’t have a setter either)
  ```
  const val SUBSYSTEM_DEPRECATED: String = "This subsystem is deprecated"

  @Deprecated(SUBSYSTEM_DEPRECATED) fun foo() { ... }
  ```

#### Late-initialized properties
- `lateinit` means late initialization. If you do not want to initialize a variable in the constructor instead you want to initialize it later on and if you can guarantee the initialization before using it, then declare that variable with `lateinit` keyword.
- This modifier can be used on non null `var` properties declared inside the body of a class.
- You cannot use val for `lateinit` variable as it will be initialized later on.
- It can also be used if the property does not have custom setter or getter.
- It can't be used in the class constructor.
- It can't be used with primative data type. As primative data type has it's default value.
- Accessing a `lateinit` property before it has been initialized throws a special exception that clearly identifies the property being accessed and the fact that it hasn't been initialized.
- In order to avoid this exception, the `lateinit` property must be initialize before using and you can also check if the `lateinnit` property has been initialized or not by using `.isInitialized` on the reference to that property.
