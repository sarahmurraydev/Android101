# Network Requests 
link?

## Definition 

## Components 

## How to implement
1. Add dependency to `build.gradle`
```gradle
// ex: 
 dependencies {
    implementation 'com.squareup.moshi:moshi-kotlin:1.9.3'
    implementation 'com.squareup.retrofit2:converter-moshi:2.9.0'
 }
```
2. Make an API Service interface
    * use Retrofit.builder()
    * Add a converter factory for (de)serialization of response objects
    * `ScalarsConverter` is popular converter factory that supports converting to strings/other primatives
    
```kotlin
// Network File
private val moshi = Moshi.Builder()
    .add(KotlinJsonAdapterFactory())
    .build()

private val retrofit = Retrofit.Builder()
        .addConverterFactory(MoshiConverterFactory.create(moshi))
        .baseUrl("https://api.openbrewerydb.org/")
        .build()

interface BeerService {
    @GET("breweries")
    suspend fun getBreweries(@Query("by_city") city: String) : List<Brewery>
}

// do this so the object is only made once
object BeerServiceApi {
    val service: BeerService by lazy {
        retrofit.create(BeerService::class.java)
    }
}
```
    
3. Make a call to the retrofit service in a coroutine in the viewmodel
4. update live data tied to the ui with your API result
```kotlin
// 3 and 4 in viewmodel: 
class CheersViewModel: ViewModel() {
    private var _breweries = MutableLiveData<List<Brewery>>()
    private var _apiStatus = MutableLiveData<String>()

    val breweries: LiveData<List<Brewery>> = _breweries
    val apiStatus: LiveData<String> = _apiStatus
    val adapter = BreweryAdapter()

    init {
        getBreweriesByCity("philadelphia")
    }

    private fun getBreweriesByCity(city: String) {
        viewModelScope.launch {
            try {
                _apiStatus.value = "SUCCESS"
                _breweries.value = BeerServiceApi.service.getBreweries(city)
                adapter.submitList(breweries.value)
            } catch (e: Exception) {
                _apiStatus.value = "FAILURE"
            }
        }
    }
}
```
5. If live data and databinding were set up correctly, UI will update when the viewmodel live data obj are updated

## Notes 
There are multiple libraries for making network requests in android. Some popular ones are: 
* OkHTTP 
* Retrofit 

JSON Converting libraries: 
* Moshi
