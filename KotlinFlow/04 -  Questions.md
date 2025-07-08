
### Why don't we use LiveData instead of Kotlin Flow?
- Kotlin Flow and LiveData serve similar purposes in Android. The main difference is that Kotlin Flow is more powerful, coroutine-based, and platform-independent.
- Platform-independent means that Kotlin Flow can work outside of the UI layer, and it is not Android-specific. LiveData is lifecycle-aware and can't work outside the UI layer.

#### 1. Flow is a part of Kotlin Coroutines - LiveData is not
- **Flow** is built into Kotlin's **coroutines**, giving you powerful, asynchronous, and non-blocking stream handling.
- **LiveData** is Android-specific and lifecycle-aware (Totally linked with the Android framework).
- You can use **Flow** in **shared code (e.g., KMM, backend, or non-UI layers)** — LiveData is not usable outside of Android UI contexts.

#### 2. Flow is more flexible and powerful
- Both can emit multiple values.
- Flow supports more powerful operators.
- LiveData is only a hot stream. Flow can either be a hot or cold stream.
- Flow can handle exceptions with `catch`
- Flow works with advanced constructs like **channels, StateFlow, SharedFlow**, etc.

#### 3. Lifecycle-awareness isn't always needed
- LiveData automatically stops updating when the lifecycle is stopped — great for UI.
- But in **ViewModels, Repositories, or UseCases**, you don’t always want to tie your logic to a lifecycle. **Flow lets you control that yourself**.

#### 4. Usage
- Use LiveData for simple UI.
- Use Flow for Shared/business logic, advanced operations, and Multiplatform/Kotlin-only projects.