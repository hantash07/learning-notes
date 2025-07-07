
- In Kotlin, both StateFlow and SharedFlow are hot flows, meaning they always emit even when there is no collector.

### StateFlow
- State holder
- Designed for state management.
- It is the same as LiveData, in which UI gets updated when the data changes.
- It represents a **state** that survives and **updates** over time.
- It is **lifecycle-aware**.
- Always has a current value.
- An initial value is required.
- A **hot** flow that emits values even if no one is collecting it.
- Example:
```
val _counter = MutableStateFlow(0)
val counter: StateFlow<Int> = _counter

fun increment() {
    _counter.value += 1
}
```
- Collector:
```
lifecycleScope.launch {
    counter.collect { value ->
        textView.text = value.toString()
    }
}
```
#### Use case: 
1. To **emit UI state** (like loading, error, success)
2. To **replace LiveData** in Jetpack Compose with lifecycle-aware collectors


### SharedFlow
- Event broadcaster
- Designed for event emissions.
- Used for **events** like showing a toast or navigating to another screen.
- It is used to **broadcast values to multiple collectors**.
- Unlike `StateFlow`, which holds a single state, `SharedFlow` can emit **a stream of values**, and multiple collectors can observe and **receive these emissions in real time**.
- All active collectors receive values
- Unlike `StateFlow`, it doesn’t hold a default value
- Use case: Toasts/snackbars, Navigation events, and One-shot actions
- Example:
```
val _events = MutableSharedFlow<String>()
val events: SharedFlow<String> = _events

fun sendToast(message: String) {
    viewModelScope.launch {
        _events.emit(message)
    }
}
```
- Collector
```
lifecycleScope.launch {
    events.collect { message ->
        Toast.makeText(context, message, Toast.LENGTH_SHORT).show()
    }
}
```


### Role of `replay` in StateFlow and SharedFlow
- **`replay`** plays an important role in SharedFlow.
- The `replay` allows the SharedFlow to emit or return multiple values, including the old values, into the collectors.
- If the `replay` value is 0, the collector will not receive any previously emitted values. The collector will only receive the latest emitted values.
- SharedFlow uses `replay` as a parameter implicitly.
- StateFlow uses `replay` implicitly and has a fixed value of 1.

#### Example:
- `SharedFlow(replay = 3)` buffers the last 3 emissions.
- `StateFlow` doesn’t buffer — it simply holds one **current state**.
```
val shared = MutableSharedFlow<Int>(replay = 2)
shared.emit(1)
shared.emit(2)
shared.emit(3)

val state = MutableStateFlow(0)
state.value = 1
state.value = 2
state.value = 3
```
- New collector of `shared` receives `2` and `3`
- The new collector of `state` receives only `3`