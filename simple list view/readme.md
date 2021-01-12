# Simple `ListView` Android Studio

### main_activity
> buat `ListView` dalam file `main_activity.xml`

```
<ListView
    android:id="@+id/listview"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

### ActivitiMain
> buat `array` ber tipedata `String`
>
> buat `listview.Adapter` yang bernilai `ArrayAdapter<String>`
>> nilai dari `ArrayAdapter` : ( `context`, `int`, `array<String>` )
```
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    ...
    
    val listString = listOf<String>("fuad", "mahrus", "fathoni")
    binding.listview.adapter = ArrayAdapter<String>(
        this, 
        android.R.layout.simple_list_item_1, 
        listString
    )
}
```
