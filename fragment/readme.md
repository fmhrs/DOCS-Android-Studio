# Android Fragment

### Frame Layout

> Pemasangan `FrameLayout` di pasangkan ke dalam xml yang ingin tampilannya ingin di timpa oleh tampilan fragment kita
>> jangan lupa untuk menambahkan `@+id` pada `FrameLayout<br>agar bisa dilakukan pemasangan fragment di sisi pemrogramanya.

```
<FrameLayout
android:id="@+id/framelayout"
android:layout_width="match_parent"
android:layout_height="match_parent" />
```

### Fragment
> pada fragment ada 3 bentuk overide function yang harus di perhatikan
>> `onCreate` : di saat fragment itu terbentuk
>>
>> `onCreateView` : fungsi pembentukan tampilan pada android
>>
>> `onViewCreated` : fungsi bahwa pembentukan fragment telah selesai, sama serpeti `Activity#onCreate`
>>
>>> mulai dari `onViewCreated` kita sudah bisa memasangkan algoritma programing kita ke dalam file `fragment`<br> yang attribute xml nya telah memiliki `id`.
>>>
>>> untuk fungsi bawa'an `fragment` selain yang tertera di atas <br> 
kalian bisa melihat penjelasan lebih detailnya di [dokumentasi fragment android studio](https://developer.android.com/reference/android/app/Fragment).
```
class FragmentA: Fragment() {
    lateinit var  binding : FragmentABinding
    
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        binding = FragmentABinding.inflate(layoutInflater)
        return binding.root
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        binding.textview.setOnClickListener {
            binding.textview.text = "plus ultraa"
        }
    }
}
```
 
### Main Activity
> tidak seperti attribut `TextView` yang mengganti text nya dengan fungsi `.text` <br>
ataupun `RecyclerView` yang memilik fungsi `.adapter`. <br>
>
> disini `FrameLayout` hanya sebagai wadah yang keberadaanya akan di `replace` oleh `fragment`
>> untuk penggantian tampilan `FrameLayout` kita memerlukan bantuan funsi dari `beginTransaction` pada `supportFragmentManager`
>>
>> pada fungsi `replace` kita memerlukan `FrameLayout` id pada xml, dan juga class `Fragment` yang akan menggantikan tempat `FrameLayout` <br> 
>>
>>>untuk mendapatkan `FarmeLayout` id ada 2 cara <br>
yang pertama menambahkan `.id` pada id framelayout xml ( `binding.framelayout.id` ) <br>
yang kedua bia menggunakan `R.id.` lalu di susul dengan nama id pada frame layout (`R.id.framelayout`)
>>>
>>> untuk class `Fragment` jangan lupa di berikan buka dan tutup kurung di akhiran nama class nya.


```
supportFragmentManager.beginTransaction
    .replace(
        binding.framelayout.id,
        FragmentA()
    ).commit
```
