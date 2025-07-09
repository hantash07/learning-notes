
### Use Case: Displaying User Input and Showing a Toast on Submit
##### Requirements:
- Track current user input (like in a search field).
- Show a Toast when the user presses "Submit".

#### Option 1: `LiveData` in ViewModel (Traditional)
##### ViewModel:
```
class MyViewModel : ViewModel() {
    private val _inputText = MutableLiveData<String>()
    val inputText: LiveData<String> = _inputText

    private val _submitEvent = MutableLiveData<Unit>()
    val submitEvent: LiveData<Unit> = _submitEvent

    fun onInputChange(text: String) {
        _inputText.value = text
    }

    fun onSubmit() {
        _submitEvent.value = Unit
    }
}
```
##### UI:
```
viewModel.inputText.observe(viewLifecycleOwner) { text ->
    binding.inputField.setText(text)
}

viewModel.submitEvent.observe(viewLifecycleOwner) {
    Toast.makeText(requireContext(), "Submitted!", Toast.LENGTH_SHORT).show()
}
```

#### Option 2: `StateFlow` for State + `SharedFlow` for Events
##### ViewModel:
```
class MyViewModel : ViewModel() {
    private val _inputText = MutableStateFlow("")
    val inputText: StateFlow<String> = _inputText

    private val _submitEvent = MutableSharedFlow<Unit>()
    val submitEvent: SharedFlow<Unit> = _submitEvent

    fun onInputChange(text: String) {
        _inputText.value = text
    }

    fun onSubmit() {
        viewModelScope.launch {
            _submitEvent.emit(Unit)
        }
    }
}
```
##### UI:
```
lifecycleScope.launchWhenStarted {
    viewModel.inputText.collect { text ->
        binding.inputField.setText(text)
    }
}

lifecycleScope.launchWhenStarted {
    viewModel.submitEvent.collect {
        Toast.makeText(requireContext(), "Submitted!", Toast.LENGTH_SHORT).show()
    }
}
```

#### Observations:
- If you rotate the screen or come back to a fragment, `LiveData` can **re-emit old values**, causing **duplicate events** (like showing Toasts again). That’s why `SharedFlow` (without replay) is better for **one-time triggers**.
- Use **`StateFlow`** to hold and expose **UI state** (like form input, loading state).
- Use **`SharedFlow`** for **one-time events** (navigation, Toasts, dialogs).
- Use **`LiveData`** when working in **Android UI only**, and you need simple lifecycle-aware observation (but it's now less recommended).