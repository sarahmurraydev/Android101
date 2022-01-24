# LiveData
https://developer.android.com/guide/navigation

## Definition 
Jetpack library that makes navigation a lot cleaner and smoother to implement \\n
Using this, means you no longer need to build a fragment factory within your application to handle navigation and you do not need to use intents to launch new activities 

## Components 

## How to implement
1. Create data object in view model: 
    * LiveData objects should be `val` 
    * create `vars` for objects the liveData points to that you can update after some network request or user action
```kotlin
class CheersViewModel: ViewModel() {
    private var _breweries = MutableLiveData<List<Brewery>>()
    private var _apiStatus = MutableLiveData<String>()

    val breweries: LiveData<List<Brewery>> = _breweries
    val apiStatus: LiveData<String> = _apiStatus
    val adapter = BreweryAdapter()
}
```

2. Set up databinding, add viewmodel to XML files
```xml
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <variable
            name="viewModel"
            type="com.example.cheers.CheersViewModel" />
    </data>

    <androidx.constraintlayout.widget.ConstraintLayout
    ...
```
3. Use livedata object tied to your viewmodel in your views
```xml
<!-- in same file as above -->
    <TextView
        android:id="@+id/main_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@{viewModel.apiStatus}"
    />
```