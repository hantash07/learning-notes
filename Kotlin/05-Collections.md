## Collections
- It contains list of items or objects with the same type. These items are stored in a certain order.
- Example of mutable list:
```
val myList = mutableListOf(11, 22, 33, 44, 55)
myList.add(66)
myList.remove(66)
```
- Example of immutable list:
```
val myList = listOf(11, 22, 33, 44, 55)
```

### List
- Ordered collections of items. Duplicate items are allowed
### Set 
- Unordered collections of items. Duplications of items are not allowed
### Map
- Collections of key-value pair items, Duplication of items are allowed but the key need to be unique

### Immutable Collections
- These collections cannot be modified. They are read only.
- Examples:
	- `listOf`
	- `setOf`
	- `mapOf`
```
val immutableList = listOf(1, 2, 3)
val immutableSet = setOf(1, 2, 3)
val immutableMap = mapOf("key1" to "value1", "key2" to "value2")

println(immutableList) // Output: [1, 2, 3]
println(immutableSet)  // Output: [1, 2, 3]
println(immutableMap)  // Output: {key1=value1, key2=value2}
```

### Mutable Collections
- These collections can be modified. Modifications can be adding, removing or updating elements.
- Example:
	- `mutableListOf`
	- `mutableSetOf`
	- `mutableMapOf`
```
val mutableList = mutableListOf(1, 2, 3)
mutableList.add(4)
println(mutableList) // Output: [1, 2, 3, 4]

val mutableSet = mutableSetOf(1, 2, 3)
mutableSet.add(4)
println(mutableSet)  // Output: [1, 2, 3, 4]

val mutableMap = mutableMapOf("key1" to "value1", "key2" to "value2")
mutableMap["key3"] = "value3"
println(mutableMap)  // Output: {key1=value1, key2=value2, key3=value3}
```

### Initializing Collections
- You need to define the data type of a collection while initializing
```
val myList = mutableListOf<String>()
val mySet = mutableListOf<Int>()
val myMap = mutableListOf<String, Int>()
```

### Empty Collections
- Initializing a collection with no item or element.
```
val emptyMutableList = mutableListOf<String>()
val emptyMutableSet = mutableSetOf<Int>()
val emptyMutableMap = mutableMapOf<String, Int>()

emptyMutableList.add("Kotlin")
emptyMutableSet.add(42)
emptyMutableMap["key"] = 100

println(emptyMutableList) // Output: [Kotlin]
println(emptyMutableSet)  // Output: [42]
println(emptyMutableMap)  // Output: {key=100}
```
- If you know the collection will be empty, you can also use these functions without arguments: `listOf()`, `setOf()` or `mapOf()` 

### Notes:
- The **read-only interfaces (`List`, `Set`, `Map`)** only prevent changes to the reference. Meaning if you assign the reference of mutable collections into immutable collection, then by modifying the mutable collections the immutable collections will also get updated.
Example: 
```
val list = listOf(1, 2, 3) // Immutable reference
// (Cannot add/remove items)

val mutableList = mutableListOf(1, 2, 3)
val readOnlyList: List<Int> = mutableList // Read-only view
mutableList.add(4)
println(readOnlyList) // Output: [1, 2, 3, 4]
```

- Use **mutable collections** when the data needs to change frequently and **immutable collections** for safer, thread-safe operations.