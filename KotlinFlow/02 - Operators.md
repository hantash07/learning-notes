
### Basic Operator
#### 1.  `map`
- Transforms each emitted value.
```
flowOf(1, 2, 3)
    .map { it * 2 }
    .collect { println(it) } // 2, 4, 6
```

#### 2.  `filter` 
- Emits only values matching the conditions.
```
flowOf(1, 2, 3, 4)
    .filter { it % 2 == 0 }
    .collect { println(it) } // 2, 4
```

#### 3.  `transform` 
- More flexible than a `map`, it can emit **multiple values** for one input or none at all.
```
flowOf(1, 2)
    .transform { value ->
        emit("Before $value")
        emit("After $value")
    }
    .collect { println(it) }
// Before 1, After 1, Before 2, After 2
```

#### 4.  `distinctUntilChanged`
- Avoids emitting the same value consecutively.
```
flowOf(1, 1, 2, 2, 3)
    .distinctUntilChanged()
    .collect { println(it) } // 1, 2, 3
```

#### 5.  `take`
Limits emissions to the first _n_ values
```
flowOf(10, 20, 30, 40)
    .take(2)
    .collect { println(it) } // 10, 20
```

#### 6.  `drop` 
Skips the first _n_ values
```
flowOf(1, 2, 3, 4)
    .drop(2)
    .collect { println(it) } // 3, 4
```


### Combine, Zip, and Merge Operators
- These operators are used to work with multiple flows.

####  1. `combine`
- It waits for at least one value to be emitted from any combined flows. 
- In simple words, if there are any changes in any of the flows, the `combine` function executes.
- Example:  The **login button** should only be enabled when both email and password are valid.
```
val emailFlow = MutableStateFlow("")
val passwordFlow = MutableStateFlow("")

val isLoginEnabled = combine(emailFlow, passwordFlow) { email, password ->
    email.contains("@") && password.length >= 6
}

isLoginEnabled.onEach { enabled ->
    // Update UI button state
    loginButton.isEnabled = enabled
}.launchIn(lifecycleScope)
```
- Emits **immediately** when either input changes.

####  2. `zip`
- It waits for the emissions of all the flows.
- In simple words, if there are two flows zipped, the `zip` function will execute if both the flows change.
- Example: You make two network calls: One fetches **user data**, and the other fetches **user preferences**. Then combine them into a `UserProfile` object.
```
val userFlow = flow { emit(api.getUser()) }
val prefsFlow = flow { emit(api.getUserPreferences()) }

val profileFlow = userFlow.zip(prefsFlow) { user, prefs ->
    UserProfile(user, prefs)
}

profileFlow.onEach { profile ->
    // Update UI with user profile
    showProfile(profile)
}.launchIn(viewModelScope)
```

####  3. `merge`
- With `combine` and `zip`, we can combine values from the values returned. But in merge, it does not combine values. 
- The `merge` functions get called whenever there are any changes from the flows, but the values returned cannot be combined.
- Example:
```
val uiEvents = flow {
    emit("UI clicked button A")
    delay(500)
    emit("UI clicked button B")
}

val systemEvents = flow {
    delay(300)
    emit("Battery low")
}

val apiEvents = flow {
    delay(100)
    emit("API returned error 404")
}

val allEvents = merge(uiEvents, systemEvents, apiEvents)

allEvents.onEach { event ->
    Log.d("AppEvents", event)
}.launchIn(viewModelScope)
```

### Terminal Flow Operator
- It takes all the results of the flow and terminates it.
#### 1. **`count`**
Count the values that match specific values.
```
val count = myFlow
	.count {
		it % 2 == 0
	}
println(count)
```

#### 2. **`reduce`**
Reduce the values into a single one.
```
flowOf(1, 2, 3, 4)
	.reduce { acc, value -> acc + value } // 10
```

#### 3. **`fold`**
Similar to `reduce`, but with an initial value.
```
flowOf(1, 2, 3)
	.fold(10) { acc, value -> acc + value } // 16
```

### Flattering Operator
- In Kotlin Flow, flattening operators are used to handle flows of flows (nested flows).
- In simple words, if you want to emit another flow from the initial flow, then these operators are used.
- When the emission itself is a flow, and you want to combine or merge it into a single flow.
- Imagine you have:
```
Flow<Flow<T>>
```
- Flattening operators help you **flatten this nested structure** into a simple:
```
Flow<T>
```
#### 1.  `flatMapConcat` 
- It executes the inner flow in a sequential manner, in the order they are emitted.
- Use case Example: Step-by-step tasks like chained calls. Getting the product ID, then calling an api which returns the details about the product.
```
val numbers = (1..3).asFlow()

val result = numbers.flatMapConcat { number ->
    flow {
        emit("$number: A")
        delay(100)
        emit("$number: B")
    }
}

result.collect { println(it) }
```
- Output:
```
1: A
1: B
2: A
2: B
3: A
3: B
```
#### 2.  `flatMapMerge` 
- It executes the inner flow in a concurrent order. 
- The order of executions is not defined.
- merges emissions **as they come**, without preserving order.
- Use case example: Parallel downloads or API calls
```
val numbers = (1..3).asFlow()

val result = numbers.flatMapMerge { number ->
    flow {
        emit("$number: A")
        delay(100)
        emit("$number: B")
    }
}

result.collect { println(it) }
```
- Output:
```
1: A
2: A
3: A
1: B
2: B
3: B
```

#### 3.  `flatMapLatest` 
- It executes the latest inner flow.
- It cancels the previous inner flow if a new value comes from upstream.
- Use `flatMapLatest` when you only care about the latest data and want to cancel the previous data if new data arrives.
- Use case Example: Search input where each value entered triggers a network request.
- Example:
```
fun search(query: String): Flow<String> = flow {
    emit("Searching for $query")
    delay(1000) // Simulate network delay
    emit("Results for $query")
}

fun userInput(): Flow<String> = flow {
    emit("A")
    delay(100)
    emit("Ap")
    delay(100)
    emit("App")
    delay(100)
    emit("Appl")
    delay(100)
    emit("Apple")
}

//MainActivity Class
runBlocking {
    userInput()
        .flatMapLatest { query -> search(query) }
        .collect { result -> println(result) }
}


```
- Output:
```
Searching for Apple
Results for Apple
```


### Concurrency & Context Operators
- These operators control **how data is collected and emitted concurrently**
#### 1.  `buffer` 
- **Adds a buffer** between the producer and the collector.
- It allows the upstream flow to produce (emit) without waiting for the downstream flow to finish (collect).
- Use case: When the upstream is faster than the downstream, and you don't want to avoid blocking the producer.
- Example:
```
flow {
    repeat(3) {
        println("Emitting $it")
        emit(it)
        delay(100) // Producing delay
    }
}
.buffer()
.collect {
    delay(300) // Simulating slow collector
    println("Collected $it")
}
```
- Output: 
```
Emitting 0
Emitting 1
Emitting 2
Collected 0
Collected 1
Collected 2
```
- Without a `buffer()`, the producer would **wait** on each `collect` to finish.
#### 1.  `conflate` 
- **Skips intermediate emissions** if the collector is slow.
- Keeps only the **latest value**.
- Use case: When only the **most recent value** matters (e.g. UI updates).
```
flow {
    repeat(3) {
        println("Emitting $it")
        emit(it)
        delay(100)
    }
}
.conflate()
.collect {
    delay(300)
    println("Collected $it")
}
```
- Output:
```
Emitting 0
Emitting 1
Emitting 2
Collected 0
Collected 2
```
- `1` is skipped because the collector was **busy**.

#### 1.  `collectLatest` 
- Cancels the **previous `collect` block** if a new value is emitted.
- Example:
```
flow {
    emit(1)
    delay(100)
    emit(2)
    delay(100)
    emit(3)
}
.collectLatest { value ->
    println("Start collecting $value")
    delay(300) // Simulate long-running task
    println("Done collecting $value")
}
```
- Output:
```
Start collecting 1
Start collecting 2
Start collecting 3
Done collecting 3
```
