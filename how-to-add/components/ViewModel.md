# ViewModel 
link 

## Definition 

## Components 

## How to implement
1. Create viemodel class class MyViewModel (or something), and have it be of type `ViewModel` 
```kotlin

class MyAppViewModel : ViewModel {

}
```

2. Tie the ViewModel to an activity or fragment so it has scope 
```kotlin

class MainActivity : AppCompatActivity() {
    private val viewModel: MyViewModel

    override fun onCreate() {
        super.onCreate(savedInstanceState)

        viewModel = ViewModelProvider(this).get(MyViewModel::class.java)
    }
}

```

You can also use the `viewModels()` method in lieu of step two: 
```kotlin
class MainActivity : AppCompatActivity() {
    private val viewModel: MyViewModel by viewModels()

    override fun onCreate() {
        super.onCreate(savedInstanceState)

        viewModel = ViewModelProvider(this).get(MyViewModel::class.java)
    }
}
```

 // TODO: explain the difference between step 2 and the above step -- what are the pros/cons?