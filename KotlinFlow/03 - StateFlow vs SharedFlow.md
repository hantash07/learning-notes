
- In Kotlin, both StateFlow and SharedFlow are hot flows, meaning they always emit even when there is no collector.

### StateFlow
- State holder
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

# TODO: What is the role of Replay?