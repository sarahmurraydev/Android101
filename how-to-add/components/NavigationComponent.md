# Navigation Component 
https://developer.android.com/guide/navigation
https://developer.android.com/guide/navigation/navigation-getting-started

## Definition 
Jetpack library that makes navigation a lot cleaner and smoother to implement \\n
Using this, means you no longer need to build a fragment factory within your application to handle navigation and you do not need to use intents to launch new activities 

## Packages: 
For kotlin: 
implementation("androidx.navigation:navigation-fragment-ktx:$nav_version")
implementation("androidx.navigation:navigation-ui-ktx:$nav_version")

## Components 

## How to implement
How to add the navigation component to your app: 
1. Add the dependencies you need. 
   ```gradle
   implementation("androidx.navigation:navigation-fragment-ktx:$nav_version")
   implementation("androidx.navigation:navigation-ui-ktx:$nav_version")
   ```
2. Create a navgraph of the navigation flows between fragments / activities in your app. (Touch an XML file at `res/navigation/nav_graph.xml`)
    * Add destinations and actions to connect destinations
    * Be sure to add a start destination 
    * can also define transitions here
    ```xml
        <navigation xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools"
            xmlns:android="http://schemas.android.com/apk/res/android"
            app:startDestination="@id/blankFragment">
            <fragment
                android:id="@+id/blankFragment"
                android:name="com.example.cashdog.cashdog.BlankFragment"
                android:label="@string/label_blank"
                tools:layout="@layout/fragment_blank" >
                <action
                    android:id="@+id/action_blankFragment_to_blankFragment2"
                    app:destination="@id/blankFragment2" />
            </fragment>
            <fragment
                android:id="@+id/blankFragment2"
                android:name="com.example.cashdog.cashdog.BlankFragment2"
                android:label="@string/label_blank_2"
                tools:layout="@layout/fragment_blank_fragment2" />
        </navigation>
    ```
3. Add your NavHost 
    * you can define it in either the XML or in your activity 
    * if defining it in your activity, be sure to make sure it persists despite configuration changes (ex: rotation)
    ```xml
    <!-- in your main activity -->
    <!-- last two attributes are the impt ones! -->
        <androidx.fragment.app.FragmentContainerView
            android:id="@+id/nav_host_fragment"
            android:name="androidx.navigation.fragment.NavHostFragment"
            android:layout_width="0dp"
            android:layout_height="0dp"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintBottom_toBottomOf="parent"

            app:defaultNavHost="true"
            app:navGraph="@navigation/nav_graph" />

    ```
4. Add a NavController    
```kotlin
internal class MyActivity {
    private lateinit var navController: NavController

    override fun onCreate(savedInstanceState: Bundle) {
        super.onCreate(savedInstanceState)

        // set up navigation component: 
        val navhost = supportFragmentManager.findFragmentById(R.id.my_nav_host_fragment) as NavHostFragment 
        navController = navHostFragment.navController
        // note: do this to insure you aren't recreating the nav graph every time app undergoes config changes
        if (savedInstanceState == null) navController.setGraph(R.navigation.my_nav_graph) 
    }
}

```
5. Define navigation events based on other user actions (ex: clicks)
```kotlin
override fun onClick(view: View) {
    val action = FragmentDirections.actionAmountFragmentToConfirmationFragment()
    view.findNavController().navigate(action)
}
```
