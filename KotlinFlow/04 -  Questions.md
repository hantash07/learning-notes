
### 1. Why don't we use LiveData instead of Kotlin Flow?
- Kotlin Flow and LiveData serve similar purposes in Android. The main difference is that Kotlin Flow is more powerful, coroutine-based, and platform-independent.
- Platform-independent means that Kotlin Flow can work outside of the UI layer, and it is not Android-specific. LiveData is lifecycle-aware and can't work outside the UI layer.

#### 1. Flow is a part of Kotlin Coroutines - LiveData is not
- **Flow** is built into Kotlin's **coroutines**, giving you powerful, asynchronous, and non-blocking stream handling.
- **LiveData** is Android-specific and lifecycle-aware (Totally linked with the Android framework).
- You can use **Flow** in **shared code (e.g., KMM, backend, or non-UI layers)** — LiveData is not usable outside of Android UI contexts.

#### 2. Flow is more flexible and powerful
- Both can emit multiple values.
- Flow supports more powerful operators.
- LiveData is only a hot stream. Some flows are cold, and some types are hot streams.
- Flow can handle exceptions with `catch`
- Flow works with advanced constructs, such as channels, StateFlow, and SharedFlow.

#### 3. Lifecycle-awareness isn't always needed
- LiveData automatically stops updating when the lifecycle is stopped — great for UI.
- But in **ViewModels, Repositories, or UseCases**, you don’t always want to tie your logic to a lifecycle. **Flow lets you control that yourself**.

#### 4. Usage
- Use LiveData for simple UI.
- Use Flow for Shared/business logic, advanced operations, and Multiplatform/Kotlin-only projects.

### 2. Hot Flows vs. Cold Flows In Kotlin - When to Use What?

#### Cold Flows
- The flow starts emitting when the collector is available.
- Each time you **collect**, the flow **starts over from scratch**.
- **Each collector runs the flow independently**.

#### Hot Flows
- Even if there is no collector, the flow starts emitting immediately
- If **multiple collectors are listening at the same time**, they’ll all receive the same emitted values from a **hot** flow, like **SharedFlow** or **StateFlow**.
- If a collector comes **after the first emission**, it **misses "First"**.

#### When to Use What?
| Use Case                                                 | Use                               |
| -------------------------------------------------------- | --------------------------------- |
| **Cold stream of values** per collector                  | `flow {}`                         |
| **UI state** that should be preserved and observed       | `StateFlow`                       |
| **Broadcasting events** to many consumers                | `SharedFlow`                      |
| **Single-use streams**, replaying to each                | Cold Flow                         |
| **Debounce/search/transform input**                      | Cold Flow (`flatMapLatest`, etc.) |
| **Progress/state sharing between screens or components** | StateFlow or SharedFlow           |

### 3. `SharingStarted.WhileSubscribed(5000)` – What does it mean?

```
val notesStateFlow: StateFlow<List<Note>> = noteRepository.notesFlow  
    .stateIn(  
        viewModelScope,  
        SharingStarted.WhileSubscribed(5000),  
        emptyList()  
    )
```

- stateIn() is used to convert the flow into StateFlow or SharedFlow.
- It’s part of how `**stateIn**` and `**shareIn**` handle **when to start and stop collecting** the upstream `Flow`.
- It keeps the state fresh and up to date when needed.
- Practical Example: A user is on the screen and starts collecting, then the user leaves that screen. If the user comes back to that screen within 5 seconds, then the collection process will resume otherwise it will restart from fresh.