# RecyclerViews 
https://developer.android.com/guide/topics/ui/layout/recyclerview

## Definition
Component used to display list of data on the screen to a user in an efficient way. You provide the data and the 
views that the data will go into. RecyclerView **recylces** those views as the user scrolls so that they are not 
consistently redrawn, but rather the content *within* those views is merely swapped out for new content. This 
saves CPU, increasing app performance. 

![recyclerview_visual](./images/recyclerview-visual.png)

## Components 
RecyclerView contains 3 parts: 
* Items held in the recyclerview 
* Adapter - takes data and prepares it for a recyclerview
* View Holder - a pool of views for the recyclerview to use and display affirmations

## How to implement

How to add a RecyclerView: 
1. Add the XML component in your activity in a view group
   > 
   > ```xml
   > <!-- don't use match_parent for recyclerview otherwise each item will take up the full screen -->
   >  <androidx.recyclerview.widget.RecyclerView
   >    android:id="@+id/recycler_view"
   >    android:layout_width="wrap_content"
   >    android:layout_height="match_parent"
   >    android:scrollbars="vertical"
   >    app:layoutManager="LinearLayoutManager" />
   > ```
   > The XML of a RecyclerView should have a `layoutManager`. This manager handles how to display the items (ex: linearly or a grid). Some popular ones are `GridLayoutManager`, `LinearLayoutManager`, `StaggeredGridLayoutManager`.
   > <br>
   > <br>
   > To be able to scroll on a RecyclerView, add the following field: 
   > `android:scrollbars="vertical"`
   
2. Implement an Adapter: 
> Your app needs something to take data from the source and format it so each item can be displayed on the recyclerview. 
> An **Adapter** is a design pattern that **adapts** data for this purpose. 
> The RecylcerView uses an adapter to figure out how to display data on the screen

An adapter has multiple parts: 
* 2a. create an XML element for whatever item your recyclerview is holding 
* 2b. create an `ItemAdapter` class
  > ```kotlin
  > import com.example.affirmations.model.Affirmation
  > 
  > class ItemAdapter(
  >    private val context: Context, 
  >    private val dataset: List<Affirmation>
  > ) {
  > }
 * 2c.  Create a view holder:
> You can add this in the adapter class (aka nested class) bc it will only be used by the adapter:
> ```kotlin
> class ItemAdapter(
>    private val context: Context,
>    private val data: List<Affirmation>
>   ) {
> 
>       @param view - view/layout you are going to insert into the recyclervie
>     class ItemViewHolder(private val view: View)
>       // sometimes this will just be of type View and other times it might be view binding 
>       // type of the view you want to add to your recyclerview 
> }

The view holder "holds" the views (list items) in memory and the views get recycled. It holds views and gets ready to add the next one and so forth

* 2d. Connect the view holder and adapter. 
    * Make your adapter class extend the `RecyclerView.Adapter` class 
    * List your view holder class as the view holder type
    * Implement the abstract methods associated with extending `RecyclerView.Adapter`: 
        * `getItemCount()`: returns size of your data set
        * `onCreateViewHolder()`: called by the layout manager to create new ViewHolders when there are no existing view holders to be used.  
        * `onBindViewHolder()`: called by layout manager to replace the contents of a list item view. 
          
Note: `onCreateViewHolder` will use a layout inflater. Layout Inflater

3. Add the set up the recyclerview in your activity: 
```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val data = DataSource().loadData()
        val myRecyclerView = findViewById<RecyclerView>(R.id.recyclerview)
        myRecyclerView.adapter = ItemAdapter(this, data)
    }
}
```

## How Does it Work? 
A Note on RecyclerView Adapter and how things work: 
> When you run the app, RecyclerView uses the adapter to figure out how to display your data on screen. 
> RecyclerView asks the adapter to create a new list item view for the first data item in your list. 
> Once it has the view, it asks the adapter to provide the data to draw the item. 
> This process repeats until the RecyclerView doesn't need any more views to fill the screen. 
> If only 3 list item views fit on the screen at once, the RecyclerView only asks the adapter to prepare those 3 list item views (instead of all 10 list item views).
> Via [link](https://developer.android.com/codelabs/basic-android-kotlin-training-recyclerview-scrollable-list?authuser=1&continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-kotlin-unit-2-pathway-3%3Fauthuser%3D1%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-training-recyclerview-scrollable-list#3)