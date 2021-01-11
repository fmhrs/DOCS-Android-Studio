# Session or Shared Preferences Android Studio

### PreferencesHelper Declare

> `SharedPreferences` memerlukan `context` dan juga nama `SharedPreference` yang tetap <br>
seperti hal nya nama pengambilan data pada suatu tabel harus mengetahui nama tabel nya terleih dahulu.
>> `context` di ambil dari `Activity` yang melakukan pemanggilan class `PreferenceHelper`

> `getSharedPreferences` hanya bisa di gunakan oleh activity <br> 
jadi memerlukan `context` dari activity yang melakukan pemanggilan
>> `context.getSharedPreferences(<nama-shred-pref>, Context.MODE_PRIVATE)`


```
class PreferencesHelper(context: Context) {
    private val preferenceName = "MY_PREFERENCES"
    var sharedPreferences = context.getSharedPreferences(preferenceName, Context.MODE_PRIVATE)
    var sharedEditor = sharedPreferences.edit()
    ...
}
```

### PreferencesHelper Setup

> penginputan data kedalam `SharedPreferences` memerlukan fungsi editor `sharedEditor.put<data-typ.e>(<nama-data>, <nilai-data>)`
>> `<tipe-data>` yang bisa di gunakan pada sharedEditor adalah  `Boolean`, `Int`, `Float`, `Long`, `String`, dan `StringSet`.
>>
>> `<nama-data>` hanya string bebas yang di gunakan untuk membedakan nilai data 1 dan yang lain nya pada penyimpanan shared preverences <br>
simpel nya adalah nama variable pada `shared preverences`.
>>
>> `<nilai-data>` adalah nilai dari variable yang dimasukan ke dalam `String` pada funsing `put<tipe-data>`<br>
apabila `<nama-data>` adalah nama variable pada shared preferences, `<nilai-data>` nilai variable dari '<nama-data>` tersebut.
>>> setelah penulisan code fungsi penginputan data pada shared preferences, jangan lupa menambahkan `.apply()` <br>
bila tidak maka data pada shared preferences tidak ada yang akan terupdate, hal ini juga berpengahu pada fungsi `editor.clear()`

```
// fungsi untuk menambah data bertipe data String kedalam SharedPreferences
fun putString(name: String, value:String){
    sharedEditor.putString(name, value).apply()
}

// fungsi untuk membersihkan seluruh data dalam shared preferences
fun clearSharedPref(){
    editor.clear().apply()
}
```
    
> pengambilan data pada `SharedPreferences` tidak perlu menggunakan `.editor` <br>
jadi cukup di tuliskan dengan`sharedPreferences.get<tipe-data>(<nama-data>, <nilai-kosong>)`
>> `<tipe-data>` yang bisa diambil adalah  `Boolean`, `Int`, `Float`, `Long`, `String`, dan `StringSet`.
>>
>> `<nama-data>` adalah data `String` yang digunakan saat melakukan input ke dalam `SharedPreferences` sebelumnya
>>
>> `<nilai-kosong>` adalah nilai yang di berikan apabila `<nama-data>` yang di cari tidak tersedia di dalam `SharedPreferences`

```
fun getString(name: String): String? {
    return sharedPref.getString(name, null)
}
```

### full code Preferences Helper

```
class PreferencesHelper(context: Context) {
    private val preferenceName = "MY_PREFERENCES"
    var sharedPreferences = context.getSharedPreferences(preferenceName, Context.MODE_PRIVATE)
    var sharedEditor = sharedPreferences.edit()

    fun putString(name: String, value:String){
        sharedEditor.putString(name, value).apply()
    }

    fun getString(name: String): String? {
        return sharedPref.getString(name, null)
    }

    fun clearSharedPref(){
        editor.clear().apply()
    }
}
```
<br>

## Pemanggilan PreferencecsHelper onActivity
> pemanggilan `PreferenceHelper` hanya perlu mengisikan `this` pada parameternya <br>
dari sana class `PreferencesHelper` dapat menggunakan `context` dari class kita.
>> apabila fungsi `this` telah ter override kita berikan anotasi nama class kita di depan nya seperti `this@MainActivity`
```
class MainActivity : AppCompatActivity() {
    // biar bisa di gunakan dalam satu class yang sama
    lateinit var sharedPref: PreferencesHelper
    
    override fun onCreate(savedInstanceState: Bundle?) {
        ...
        // Inisialisasi PreferenceHelper
        sharedPref = PreferencesHelper(this)
        
        // Penginputan data username pada PreferenceHelper
        sharedPref.putString("username", username.toString())
        
        // pengambilan data username pada PreferenceHelper
        bind.tvUsername.text = sharedPref.getString("username")
    }
}
```
