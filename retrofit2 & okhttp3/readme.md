# Retrofit2 & OkHttp3

### build gradle
> Original Url untuk retrofit2, okHttp3,
>> retrofit2 : https://square.github.io/retrofit/ <br>
oKHttp3 https://github.com/square/okhttp <br>
dummy api : https://jsonplaceholder.typicode.com/ <br>
interceptor : https://github.com/square/okhttp/tree/master/okhttp-logging-interceptor

> Converter pada retrofit
>> Gson: com.squareup.retrofit2:converter-gson <br> 
Jackson: com.squareup.retrofit2:converter-jackson <br>
Moshi: com.squareup.retrofit2:converter-moshi <br>
Protobuf: com.squareup.retrofit2:converter-protobuf <br>
Wire: com.squareup.retrofit2:converter-wire <br>
Simple XML: com.squareup.retrofit2:converter-simplexml <br>
JAXB: com.squareup.retrofit2:converter-jaxb <br>


```
dependencies {
    ...
    implementation 'com.squareup.retrofit2:retrofit:(insert latest version)'

    implementation 'com.google.code.gson:gson:2.8.6'

    implementation "com.squareup.okhttp3:okhttp:4.9.0" 
    implementation "com.squareup.okhttp3:logging-interceptor:4.9.0" 
}
```

### configurasi retrofit

> configurasi minimum saat menggunakan retrofit adalah java 8
>> Url support java 8 : https://developer.android.com/studio/write/java8-support


```
Minimum java 8
android {
  ...
  compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
  }
  kotlinOptions {
    jvmTarget = "1.8"
  }
}
```


## code

### Interface Endpoint

```
API ENDPOINT
interface ApiEndpoint {
    @GET("/posts")
    fun getPosts(): Call<List<ModelPost>>
}
```


### Object API Services
```
object Instance {
    val baseUrl = "https://jsonplaceholder.typicode.com/"

    val retrofit by lazy {
        //* start of OkHttp *//
        // setup chain interceptor using HTTPLoggingInterceptor
        val interceptor = HttpLoggingInterceptor()
        interceptor.level = HttpLoggingInterceptor.Level.BODY

        // setup client
        val client = OkHttpClient.Builder()
            .addInterceptor(interceptor)
            .build()
        //* end of OkHttp *//
    
    
        Retrofit.Builder()
                .baseUrl(baseUrl)
                .client(client)
                .addConverterFactory(GsonConverterFactory.create())
                .build()
    }

    val ApiTypicode by lazy {
        retrofit.create(ApiEndpoint::class.java)
    }
}
```

### Main Activity
```
fun callApi(){
    isLoading(true)

    // NB : use retrofit2 Callback 
    Instance.ApiTypicode.getPosts().enqueue(object : Callback<List<ModelPost>> {
        override fun onResponse( 
            call: Call<List<ModelPost>>,
            response: Response<List<ModelPost>>
        ) {
            if (response.isSuccessful) {
                response.body()?.let {
                    rvPost(it)
                } ?: run {
                    Toast.makeText(
                        this@MainActivity,
                        "Server don't have any data",
                        Toast.LENGTH_SHORT
                    ).show()
                }
            }
            isLoading(false)
        }

        override fun onFailure(call: Call<List<ModelPost>>, t: Throwable) {
            Toast.makeText(this@MainActivity, "fail get data from server", Toast.LENGTH_SHORT)
                .show()
            isLoading(false)
        }
    })
}
```
