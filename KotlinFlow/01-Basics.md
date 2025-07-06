
## Kotlin Flow
- It is a part of Kotlin Coroutines used for handling asynchronous data streams in a reactive and declarative way.
- It is similar to RxJava, but with a Kotlin-first and coroutine-based design.
- Coroutine suspended function only returns one value once the function is executed. If we want to emit multiple values, then we can use Kotlin Flow.
- It is designed to handle data asynchronously, and it is Cold Flow by default, meaning the code inside the flow does not run until the flow is collected or used.

###   Cold Flow
- In cold flow, it requires the collector to produce or emit data. In simple words, the code inside the flow does not run until the flow is collected.
- It does not start producing data until someone starts collecting it.
- Each collector has its own emitter. 
- It is good for **data pipelines** (e.g., fetching from API or DB)
- Example: `Flow`, `suspend fun` returns `Flow`

### Hot Flow
- In hot flow, it starts emitting data even if there is no collector.
- Multiple collectors share the same source of emissions.
- Emission starts **independently**
- It is good for **state management and shared events** (e.g., UI state, user actions).
- Example: `StateFlow`, `SharedFlow`, `LiveData`

### Suspended Functions
- You can use suspend functions inside Kotlin Flow. 
```
val flow = flow {
    println("Flow started")
    emit(1)
    delay(1000) //Suspended Functions
    emit(1)
}
```

### Flow Builder Functions
- `flow { }`: Basic builder
- `flowOf(1, 2, 3)`: Emits a fixed set of values
- `asFlow()`: Convert a collection to Flow
```
//Converting the list into Flow
listOf(1, 2, 3).asFlow().collect { println(it) }
```