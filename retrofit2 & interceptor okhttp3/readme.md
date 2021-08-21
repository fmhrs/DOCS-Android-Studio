# Retrofit2 & Interceptor OkHttp3

### build gradle
> Original Url untuk retrofit2, okHttp3,
>> retrofit2 : https://square.github.io/retrofit/ <br>
open api : https://jsonplaceholder.typicode.com/ <br>
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
    // retrofit and gson
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'

    // interceptor okhttp3
    implementation 'com.squareup.okhttp3:logging-interceptor:4.9.0'
}
```
<br>

### configurasi retrofit

> configurasi minimum saat menggunakan retrofit adalah java 8.
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

<br>

## code

### Modal Posts

> `data class` berisikan data apa saja yang akan di ambil dari server. <br>
>> penggunaan `data class` tidak menggunakan kurung kurawal `{}` namun kurung biasa `()` !

```
data class ModalPosts (
    var userId: Int? = null,
    var id: Int? = null,
    var title: String? = null,
    var body: String? = null,
)
```

<br>

### Interface Typicode
> `@GET` merupakan metode pengiriman yang di gunakan dalam pemanggilan ke dalam server. <br>
`('posts')` adalah alamat url yang di panggil setelah penulisan base url.


```
interface Typicode {
    @GET ("posts")
    fun getPosts(): Call<List<ModalPosts>>
}
```

> kalau mengirim data gunakan @body untuk get, dan unakan @Field untuk post.
> kalau menggunakan @Field tambahkan juga @FormUrlEncoded
> NB : modul ini hanya info tambahan, bukan termasuk pengambilan data pada typicode
```
@FormUrlEncoded
@POST ("login")
fun login(
    @Field("username") username: String,
    @Field("password") passsword: String
    // @Query untuk get
): Call<dataPengguna>
```

<br>

### Object API Services
> base url merupakan direktori root pada website 
>> baseUrl facebook : https://facebook.com <br>
baseUrl github : https://github.com/ <br>
baseUrl stack overflow : https://stackoverflow.com/

> interceptor berguna untuk mengecek aliran saat melakukan pemangilan data ke server !
>>penulisan interceptor tidak bisa langsung `val interceptor = HttpLoggingInterceptor().Level.BODY` <br>
karena `HttpLoggingInterceptor().Level.BODY` hanya berisi level dari interceptor bukan interceptor itu sendiri.

> `GsonConverterFactory.create()` berguna dalam mengkonversikan data dari server !
>> data yang awalnya memiliki tipe data Json diubah menjadi data yang bisa dibaca oleh pada android studio.

```
object ApiService {
    val baseUrl = "https://jsonplaceholder.typicode.com/"

    val retrofit by lazy {
        val interceptor = HttpLoggingInterceptor()
        interceptor.level = HttpLoggingInterceptor.Level.BODY

        val client = OkHttpClient.Builder().addInterceptor(interceptor).build()

        Retrofit.Builder()
            .baseUrl(baseUrl)
            .addConverterFactory(GsonConverterFactory.create())
            .client(client)
            .build()
    }

    val serviceTypicode by lazy {
        retrofit.create(Typicode::class.java)
    }
}
```

<br>

### Main Activity
> `isLoading()` hanya sekedar function untuk menampilkan `ProgressBar` jadi tidak perlu di tuliskan.

> `enqueue` pada interface `getPosts` berparameter `(object : Callback<List<ModalPosts>> {})` !
>> `Callback` disini harus di import dari retrofit2.


> `Callback` pada retrofit mengharuskan kita untuk mengimplementasi `onResponse` dan juga `onFailure` !peng
>> `onResponse` berarti kita berhasil memanggil server dengan nama `baseUrl` dan `subUrl` yang valid.
>>
>> `onFailure` berarti kita gagal dalam melakukan pemanggilan server <br> 
yang mana kita tidak bisa memanggil server dari `baseUrl` atau `subUrl` yang telah kita buat.

> perbedaan jika tidak `response.isSuccesful` dan `fun onFailure` ?
>> jika tidak `response.isSuccesful` berarti ktia sudah bisa memanggil server <br>namun http response dari server mungkin `400` bukan `200`.
>> 
>> jika `fun onFailure` berarti kita memang benar benar belum bisa memanggil server sama sekali.

> data yang di kirim oleh server akan di simpan pada `response.body()`
>> apabila `Callback` kita berbentuk `array` atau `list` maka harus menggunakan indeks setelah body <br> 
seperti `response.body()[<index>].<nama-variable>`
>>
>> namun pada code dibawah ini mengimplementasikan `RecyclerView.Adapter` yang berparameter `list`<br>
jadi tidak perlu di konversikan data `array` satu satu dahulu, langsung saja dilempar ke dalam `Adapter` menggunakan variable `it`.
```
fun loadApi() {
    isLoading(true)
    
    ApiService.serviceTypicode.getPosts().enqueue(object : Callback<List<ModalPosts>> {
        override fun onResponse(
            call: Call<List<ModalPosts>>,
            response: Response<List<ModalPosts>>
        ) {
            if (response.isSuccessful) {
                response.body()?.let {
                    bind.rvPosts.adapter = AdapterPosts(it)
                    bind.rvPosts.layoutManager = LinearLayoutManager(this@MainActivity)
                }
            } else {
                Toast.makeText(
                    this@MainActivity,
                    "can't rechieve data from server",
                    Toast.LENGTH_SHORT
                ).show()
            }
            isLoading(false)
        }

        override fun onFailure(call: Call<List<ModalPosts>>, t: Throwable) {
            Toast.makeText(
                this@MainActivity,
                "can't call the server",
                Toast.LENGTH_SHORT
            ).show()
            isLoading(false)
        }

    })
}
```
