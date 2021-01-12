# Android Studio 4.1 : Data Binding "Activity", **Fragment** & "RV.Adapter"
### setup build gradle
> pengaktifan "viewBinding" dalam build gradle
>> untuk penulisan pada build gradle tidak ada concat, <br>
jadi harus menulis manual dari `buildFeatures` sampai ` viewBinding true`.

```
android {
    ...
    buildFeatures {
        viewBinding true
    }
}
```

<br>

### main activity
> kenapa kok tidak pakai `var bind = ActivityMainBinding.inflate(layoutInflater)` saja ?
>> karena `layoutInflater` baru bisa dipakai setelah `super.onCreate(savedInstanceState)`.

<br>

> lalu kenapa pada `setContentView` menggunakan `bind.root` ?
>> Karena `bind.root` adalah sebuah tempat khusus untuk menyimpan layout pada binding. <br>
jadi sudah tidak pelu menggunakan `R.layout.main_activity` lagi karena sudah diwakilkan oleh `bind.root`



```
class MainActivity : AppCompatActivity() {
    private lateinit var bind : ActivityMainBinding
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        bind = ActivityMainBinding.inflate(layoutInflater)
        setContentView(bind.root)
    }
}
```

<br>


### recycler view
> menginisialisasi `_binding` ber tipe data `ItemPostBinding`.
>> *data type* `ItemPostBinding` diambil dari nama xml `item_post.xml`.

>menginisialisasi `get() = _binding!!` pada variable `bind`
>> `get() =` hanya bisa di lakukan pada `val`, tidak bisa di iemplementasikan pada `var`.

> pada kasus normal, `viewHolder` berparameter `(itemView: View)` <br>
lalu meretrun `(itemView)` menggunakan RecyclerView.ViewHolder.
>> pada `viewBinding` kita menggunakan parameter view class binding `(binding: ItemPostBinding)` <br>
lalu mereturn `(bind.root)` menggunakan ecyclerView.ViewHolder.


```
class RVPosts(val listPost: List<ModelPost>) : RecyclerView.Adapter<RVPosts.viewHolder>() {
    private var _binding: ItemPostBinding? = null
    private val bind get() = _binding!!

    inner class viewHolder(binding: ItemPostBinding) : RecyclerView.ViewHolder(binding.root)
    
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): viewHolder {
        _binding = ItemPostBinding.inflate(LayoutInflater.from(parent.context), parent, false)
        return viewHolder(bind)
    }

    override fun onBindViewHolder(holder: viewHolder, position: Int) {
        bind.iPostTitle.text = listPost[position].title
        bind.iPostBody.text = listPost[position].body
    }

    override fun getItemCount(): Int = listPost.size
}
```

<br>

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
