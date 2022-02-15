# DataBinding
link

## Definition 
Two way binding that binds your layout files to UI layer components (Activity/Fragments) so that you can access view elements in Activities/Fragments via camelCasing (viewbinding) and also lets you add data objects to your XML layouts that reference objects that live in these Activities/Fragments. In addition, databinding let's you create your own binding adapters, XML attributes that are are unique to data your app defines

## Components 

## How to implement
1. Add view binding to your gradle file: 
```gradle
android {
   ...
    buildFeatures {
        dataBinding true
    }
}
```
2. Create a ViewModel class

3. Add binding to the activity/fragment
```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding  // add this 1

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)

        binding.lifecycleOwner = this

        binding.viewModel = viewModel
    }
}
```
4. Use the viewModel data in XML files -- right click on the root layout element and convert the file into a databinding layout
![databinding-visual](./images/data-binding-conversion.png)


Then add the variable as a data:
```XML
    <data>
        <variable
            name="viewModel"
            type="com.example.cheers.CheersViewModel" />
    </data>c
```