# Data in a List

1. Get some data from API 
    - See add [network requests](../components/NetworkRequests.md)

2. Make a view model 
    - Create VM class, extending ViewModel()
    - Add VM to activity via ViewModelProvider 
    - Be sure VM has access to a viewModelScope
        * if you have issues here you might need to import:
            `implementation 'androidx.lifecycle:lifecycle-viewmodel-ktx:2.4.0'` 

3. Call your API from your VM and save the data in some LiveData 
    - need to make a private mutable live data object and a public non mutable live data obj:
    ```kotlin
    class MyViewModel : ViewModel() {
        private lateinit var _data = MutableLiveData<String>()

        public val data : LiveData<String> = _data
    }

    ```

4. Add databinding to your app -- 
    - See [add data binding](../components/DataBinding.md)
    - show some data (like a status string on the screen to ensure it's working)